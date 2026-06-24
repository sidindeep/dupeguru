# Agent Instructions

This file is the lightweight runtime entrypoint for this project. Detailed shared rules are copied into focused modules under `patterns/AGENTS_RUNTIME/` so agents can load only the context needed for the current task.

## Project

dupeGuru is a cross-platform GUI application for finding duplicate files. The core logic is Python, the desktop UI is Python/PyQt, documentation lives under `help/`, localization files live under `locale/`, package assets live under `pkg/`, and shared helper code lives under `hscommon/`.

## Project Goal

When a new implementation task starts, confirm the concrete goal from the user's request, local summaries, project memory, or relevant docs. If the goal is unclear, ask a focused question before making code changes.

## Loading Contract

- Start with this file.
- Read only the modules needed for the current request.
- Before acting on a concrete task, select and read the matching module(s); this entrypoint alone is enough only for greetings or status-neutral replies.
- If the request contains a GI chat command such as `gi ...`, `ги ...`, or a known mojibake form such as `РіРё ...`, treat it as a concrete task even when the message is short. First read `COMMANDS.md` when present, then read every runtime module routed to that command before acting.
- For state-changing GI commands that start, stop, restart, rebuild, deploy, test, install, reset, update, commit, push, or manage task-manager state, do not execute from memory, old chat examples, or a command name alone. If the command's routed module is unavailable, stop and report the missing path.
- For broad or unclear work, read `patterns/AGENTS_RUNTIME/01-purpose.md`, `patterns/AGENTS_RUNTIME/03-rule-precedence.md`, `patterns/AGENTS_RUNTIME/06-tool-usage-and-token-economy.md`, and the most relevant task module.
- Prefer project-local instructions, runbooks, contracts, project memory, and service guides over shared defaults when they are more specific.

## Restore Context

If the user only sends a short greeting, thanks, acknowledgement, or status-neutral message, reply briefly and ask what they want to do next.

For concrete restore/start tasks, run:

```powershell
.\tools\agent-start.ps1
```

If the startup script is unavailable, read only the smallest useful slices of:

- `AGENTS.md`
- latest relevant file in `tools/summary/`
- `tools/AGENT_WORKING_AGREEMENTS.md`
- `tools/AGENT_RUNBOOK.md`
- relevant notes in `tools/project-memory/`

## Runtime Module Routing

- Repository purpose, RAG startup, project memory, summaries, connected projects, and shared-rule propagation: `patterns/AGENTS_RUNTIME/01-purpose.md`
- Repository map: `patterns/AGENTS_RUNTIME/02-repository-map.md`
- Rule precedence and scope arbitration: `patterns/AGENTS_RUNTIME/03-rule-precedence.md`
- Authoring reusable rules, configuration boundaries, code quality, project info/stack inventory, and batch verification: `patterns/AGENTS_RUNTIME/04-content-and-authoring.md`
- Windows shell and networking policy: `patterns/AGENTS_RUNTIME/05-windows-command-policy.md`
- Token economy, verification command lookup, `gi info`, `gi stack`, `gi refactor`, feature contracts, and large-output handling: `patterns/AGENTS_RUNTIME/06-tool-usage-and-token-economy.md`
- Startup, restore, project goal, bug evidence, PDF inspection, repository cleanup, filesystem boundaries, and first-message handling: `patterns/AGENTS_RUNTIME/07-startup-and-scope.md`
- Config-service, service guide/contract lookup, task manager commands, sprint commands, and web-service port registration: `patterns/AGENTS_RUNTIME/08-config-service-and-task-manager.md`
- Dev/prod online service publication, FTP deploy, restart/reboot, first test, full test, default reset, installer packaging, SQL/vector inspection, and project/RAG rebuild commands: `patterns/AGENTS_RUNTIME/09-project-operation-commands.md`
- Nested repositories, private local app data, product-plan intent signals, and missing required entities: `patterns/AGENTS_RUNTIME/10-private-scope-and-missing-context.md`
- Project, commit, task, and response language preferences: `patterns/AGENTS_RUNTIME/11-language-preferences.md`
- UI focus, app launch focus, and frontend verification expectations: `patterns/AGENTS_RUNTIME/12-ui-and-focus.md`
- Progress-update style: `patterns/AGENTS_RUNTIME/13-progress-updates.md`
- Update intake and `updates/` handling: `patterns/AGENTS_RUNTIME/14-update-intake.md`
- Verification policy: `patterns/AGENTS_RUNTIME/15-verification.md`
- Git policy: `patterns/AGENTS_RUNTIME/16-git-policy.md`

## Durable Memory

Durable project knowledge lives in `tools/project-memory/`. Put product behavior, algorithms, workflow contracts, architecture decisions, verified findings, and long-lived implementation notes there when they should survive chat resets. Keep raw artifacts, generated outputs, large logs, screenshots, private data, and build products out of project memory.

General project documentation belongs in `README.md`, `Windows.md`, `macos.md`, `help/`, and the runbook.

## Common Commands

Install dependencies:

```powershell
python -m pip install -r requirements.txt
python -m pip install -r requirements-extra.txt
```

Build generated modules/resources:

```powershell
python build.py --modules
```

Run the desktop app:

```powershell
python run.py
```

Run tests and quality checks:

```powershell
tox
```

Run tests without tox after installing extra requirements:

```powershell
pytest core hscommon
```

Format/lint checks used by tox:

```powershell
flake8
black --check .
```

## Working Areas

- Source: `core/`, `qt/`, `hscommon/`, `run.py`, `build.py`, `package.py`
- Tests: `core/`, `hscommon/`
- Documentation: `README.md`, `Windows.md`, `macos.md`, `help/`
- Localization: `locale/`
- Packaging: `pkg/`, `package.py`, `setup.py`, `setup.cfg`, `setup.nsi`
- Tools: `tools/`
- Summaries: `tools/summary/`
- Project memory: `tools/project-memory/`

## Local Rules

- Do not revert user changes unless explicitly requested.
- Treat dirty worktrees as normal.
- Keep changes scoped to the current task.
- Ask before destructive operations, broad formatting-only churn, dependency replacements, data migrations, public API or storage contract changes, or unrelated scope expansion.
- Treat this project root as the filesystem boundary for normal work unless the user gives an explicit concrete path and action.
- Preserve text encodings when editing files.
