# Agent Runbook

Every command should be run from the project root unless noted otherwise.

## Install

```powershell
python -m pip install -r requirements.txt
python -m pip install -r requirements-extra.txt
```

## Run

```powershell
python run.py
```

The GUI requires a working Qt/PyQt environment.

## Test

```powershell
tox
```

Targeted test command after installing `requirements-extra.txt`:

```powershell
pytest core hscommon
```

## Build

```powershell
python build.py
```

Build only generated modules/resources, as tox does:

```powershell
python build.py --modules
```

## Smoke Check

```powershell
python build.py --modules
pytest core hscommon
```

Expected result:

```text
Build finishes, then pytest reports passing tests for core and hscommon.
```

## Packaging

```powershell
python build.py --clean
python package.py
```

Packaging can require platform-specific dependencies; see `Windows.md`, `macos.md`, and `README.md`.

## Logs

No project-local log command is currently documented. For command failures, capture only the relevant tail or error section in chat.

## Environment Notes

- Python 3.7+ is supported by project metadata.
- PyQt5 is required for the desktop UI.
- `tox.ini` defines `py37`, `py38`, `py39`, `py310`, and `py311` environments with `skip_missing_interpreters = True`.
