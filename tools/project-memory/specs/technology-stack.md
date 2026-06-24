# Technology Stack

Last reviewed: 2026-06-24

Canonical source: this file
Linked from: `AGENTS.md`

This is project documentation. Keep business rules, feature algorithms, workflow contracts, state machines, and verification guarantees in project memory; keep stack facts, commands, runtime assumptions, and operational notes here.

## Summary

- Primary stack: Python desktop application with PyQt UI.
- Runtime model: local GUI app plus build/test/package scripts.
- Current confidence: verified from `README.md`, `setup.cfg`, `tox.ini`, and requirements files on 2026-06-24.

## Components

| Layer | Technology | Evidence | Notes |
| --- | --- | --- | --- |
| Language/runtime | Python 3.7+ | `README.md`, `setup.cfg` | `tox.ini` lists py37 through py311 and skips missing interpreters. |
| Frontend | PyQt5 | `README.md`, `requirements.txt`, `setup.cfg` | Qt desktop UI lives under `qt/`. |
| Backend/API | Local Python modules | `core/`, `hscommon/` | Core duplicate-finding behavior is in `core/`; shared helpers are in `hscommon/`. |
| Data/storage | Not fully mapped | pending study | Verify before changing scan results, preferences, or cache behavior. |
| Build/package | `build.py`, `package.py`, setuptools, NSIS | `README.md`, `setup.py`, `setup.cfg`, `setup.nsi` | Platform packaging has Windows/macOS notes. |
| Test/quality | tox, pytest, flake8, black | `tox.ini`, `requirements-extra.txt` | tox runs build modules, flake8, black check, and pytest. |
| Deployment/runtime | Desktop packages | `package.py`, `pkg/`, `Windows.md`, `macos.md` | Packaging dependencies vary by platform. |

## Commands

| Purpose | Command | Evidence |
| --- | --- | --- |
| Install | `python -m pip install -r requirements.txt` | `README.md` |
| Install test extras | `python -m pip install -r requirements-extra.txt` | `README.md` |
| Run | `python run.py` | `README.md` |
| Test | `tox` | `README.md`, `tox.ini` |
| Targeted tests | `pytest core hscommon` | `README.md`, `tox.ini` |
| Build | `python build.py` | `README.md` |
| Build modules | `python build.py --modules` | `tox.ini` |

## External Services

| Service | Role | Evidence | Boundary |
| --- | --- | --- | --- |
| Transifex | Translation project | `README.md` | Do not assume credentials or network access without explicit task need. |

## Gaps

- Confirm current supported Python versions against CI and upstream policy before changing compatibility.
- Map application persistence/configuration behavior before editing scan result storage, preferences, or caches.
- Map release packaging flows per platform before changing installer behavior.
