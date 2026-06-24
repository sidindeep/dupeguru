# Архитектура dupeGuru

Последний обзор: 2026-06-24.

## Назначение

dupeGuru - локальное desktop-приложение для поиска дублей файлов. Основной продуктовый поток: пользователь выбирает папки, режим приложения и тип сканирования; приложение собирает подходящие файлы, находит совпадения, группирует их, показывает результаты и позволяет помечать, удалять, перемещать, копировать, экспортировать или сохранять найденные дубли.

## Слои

- `run.py`: старт Qt-приложения, настройка `QApplication`, переводов, сигналов ОС, ресурсов и `qt.app.DupeGuru`.
- `qt/`: PyQt5 слой представления. Создает окна, меню, диалоги, модели Qt и прокидывает пользовательские действия в core.
- `core/`: доменная логика поиска дублей, директории, файлы, результаты, ignore/exclude lists, операции с файлами и mode-specific scanner/file classes.
- `hscommon/`: общие GUI/model/job/util helpers, используемые core и Qt.
- `core/pe/modules/` и `qt/pe/modules/`: C extensions для picture matching и Qt block extraction.
- `help/`, `locale/`, `images/`, `pkg/`: документация, переводы, ресурсы и упаковка.

## Runtime Lifecycle

1. `run.py` создает `QApplication`, выставляет organization/app/version, настраивает logging, `QSettings`, переводы из `locale/`, обработчики SIGINT/SIGTERM/SIGQUIT и Qt resource module `qt.dg_rc`.
2. После установки переводов импортируется `qt.app.DupeGuru`, потому что многие строки переводятся при импорте.
3. `qt.app.DupeGuru` загружает `Preferences`, создает `core.app.DupeGuru(view=self, portable=...)`, затем настраивает actions, окна, диалоги, progress window и recent lists.
4. Core model на старте создает appdata folder, подключает `core.fs.filesdb` к `hash_cache.db`, создает `Directories`, `Results`, `IgnoreList`, `ExcludeList`, core GUI adapters и `ProgressWindow`.
5. `model.load()` восстанавливает `last_directories.xml`, `ignore_list.xml` и `exclude_list.xml` из appdata.
6. При выходе `model.save()` сохраняет директории, ignore/exclude lists, сессию UI и закрывает filesdb.

## Scan Data Flow

1. Пользователь выбирает режим: Standard, Music или Picture. В `core.app.AppMode` это `STANDARD`, `MUSIC`, `PICTURE`.
2. UI сохраняет настройки в `qt.preferences.Preferences`, затем `_update_options()` переносит их в `core.app.DupeGuru.options`.
3. `core.app.DupeGuru.start_scanning()` выбирает scanner через `SCANNER_CLASS`: `core.se.scanner.ScannerSE`, `core.me.scanner.ScannerME` или `core.pe.scanner.ScannerPE`.
4. `Directories.get_files()` или `Directories.get_folders()` рекурсивно собирает элементы, применяя `DirectoryState` и `ExcludeList`.
5. `core.fs.File` лениво читает размер, mtime и digest поля; digest values кэшируются в appdata `hash_cache.db`.
6. Standard и Music scanners используют `core.scanner.Scanner`, который вызывает `core.engine.getmatches()` для fuzzy name/tag matching или `core.engine.getmatches_by_contents()` для content hashing.
7. Picture scanner использует `core.pe.matchblock.getmatches()` для fuzzy block comparison через SQLite block cache и multiprocessing либо `core.pe.matchexif.getmatches()` для EXIF timestamp matching.
8. `core.engine.get_groups()` превращает `Match(first, second, percentage)` в `Group`, scanner фильтрует ref/ref matches, ignore-list pairs, mixed extensions и missing files.
9. `core.results.Results` хранит groups, dupes, marks, filters, sort descriptors, stats and XML load/save.
10. После scan job `core.app.DupeGuru._job_completed()` обновляет results UI, коммитит filesdb и показывает окно результатов или сообщение "No duplicates found."

## Modes

- Standard: `core.se.fs.File`/`Folder`, scan types Filename, Contents, Folders.
- Music: `core.me.fs.MusicFile`, supported audio extensions, mutagen metadata, scan types Filename, Fields, Fields No Order, Tags, Contents.
- Picture: `qt.pe.photo.File` as platform-specific subclass of `core.pe.photo.Photo`, supported image extensions, dimensions/EXIF/block extraction, scan types Contents and EXIF Timestamp.

## Persistence And Config Boundaries

- User preferences use Qt `QSettings` through `qt.preferences` and `qt.util.create_qsettings()`.
- Runtime appdata stores `hash_cache.db`, `cached_pictures.db`, `last_directories.xml`, `ignore_list.xml`, `exclude_list.xml`, debug logs and optional scan profile files.
- Saved scan results are XML files created through `core.results.Results.save_to_xml()`.
- Appdata and generated cache DB files are runtime data, not repository fixtures.
- Local agent memory under `tools/project-memory/` is for agent/project knowledge only and is not part of product runtime.

## Build And Packaging

- `python build.py --modules` builds C extensions through `setup.py build_ext --inplace`.
- Full `python build.py` builds PE modules, localizations, Qt resources `qt/dg_rc.py`, and Sphinx help.
- `Makefile` wraps venv creation, `make modules`, `make run`, localization, install and clean targets.
- `package.py` creates source, Debian/Ubuntu, Arch, Windows or macOS packages depending on platform and flags.
- Windows packaging uses PyInstaller and NSIS; macOS packaging uses PyInstaller; Linux packaging uses Debian/Arch skeletons in `pkg/`.

## Tests And Quality Gates

- `tox.ini` runs `python build.py --modules`, `flake8`, `black --check .`, then `py.test core hscommon`.
- GitHub Actions `default.yml` runs pre-commit, then build modules and `pytest core hscommon` across Linux Python 3.8-3.14 plus Windows/macOS Python 3.12.
- `.pre-commit-config.yaml` uses check-yaml, check-toml, end-of-file-fixer, trailing-whitespace, black, flake8 and commitlint.
- CodeQL runs for Python and C/C++.
- Current automated tests mainly cover `core/` and `hscommon/`; Qt UI behavior is mostly exercised indirectly through model boundaries and manual/runtime usage.

## Change-Risk Notes

- Scanner changes should be tested with `core/tests/scanner_test.py`, `core/tests/engine_test.py`, `core/tests/results_test.py`, and mode-specific tests.
- Changes to file hashing or appdata DB schemas need care around `core.fs.FilesDB.schema_version` and `core.pe.cache_sqlite.SqliteCache`.
- Picture matching depends on compiled modules and multiprocessing, so failures may differ by platform.
- Preference changes must keep `qt.preferences.Preferences.reset()`, `_load_values()`, `_save_values()` and `qt.app.DupeGuru._update_options()` aligned.
- Packaging changes should check `build.py`, `package.py`, `Makefile`, platform docs and CI together.
