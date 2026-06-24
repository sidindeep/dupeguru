# Known Issues And Cautions

Последний обзор: 2026-06-24.

- Qt UI has limited direct automated coverage in this repository; core and hscommon have the strongest test coverage.
- `tox.ini` still lists Python 3.7-3.11, while GitHub Actions tests Python 3.8-3.14 plus Windows/macOS 3.12. Verify the intended supported Python range before changing compatibility metadata.
- `build.py --modules` requires a working native compiler toolchain for C extensions.
- Full `build.py` also requires `pyrcc5`, localization tooling and Sphinx documentation dependencies.
- Runtime appdata contains product caches and user state; do not treat `hash_cache.db`, `cached_pictures.db`, XML session files or debug logs as repo artifacts.
- `core.me.fs.MusicFile._read_info()` assumes mutagen returns metadata objects with `info` and `tags`; changes around unsupported/corrupt audio should be tested with defensive cases.
- Picture matching has separate core and Qt C extensions, and uses multiprocessing. Verify behavior on the target platform before release-sensitive changes.
