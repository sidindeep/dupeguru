# Connected Projects

This register lists external projects, repositories, services, libraries, docs sites, upstream tools, cloned examples, and sibling workspaces that this project depends on, researches, vendors, or regularly interacts with.

Agents should read this file before touching integrations, nested repositories, external project folders, or cross-project service contracts.

## General Instructions

- Purpose: Source used to bootstrap local AI-agent instructions for this repository.
- Business or architectural role: reusable startup, memory, verification, and agent-collaboration conventions.
- Local folder: none committed; temporary cache may exist under `%TEMP%`.
- Canonical Git/package/docs URLs: `https://github.com/Dimosfil/general-instructions.git`
- Service ID or runtime endpoints: none.
- Owner or source of truth: `Dimosfil/general-instructions`.
- Data/API contract: copied templates, runtime modules, patterns, and helper scripts are project-owned after bootstrap.
- Setup, sync, build, test, or update commands: use `tools/check-instruction-kit-updates.ps1` and accepted migrations from the shared source.
- Version, branch, or update cadence: installed baseline `2026.06.24.5`.
- Privacy, secret, license, and access boundaries: do not add the shared repository as a dependency, package, submodule, symlink, or runtime reference.
- Status and caveats: bootstrapped locally; `updates/` in the shared source is maintenance intake and is not startup/runtime instruction content.
- Reason this dependency still exists: update provenance for the copied local instruction kit.

## dupeGuru Upstream

- Purpose: Product source repository identity from package metadata.
- Business or architectural role: application being maintained in this workspace.
- Local folder: current repository root.
- Canonical Git/package/docs URLs: `https://github.com/arsenetar/dupeguru`
- Service ID or runtime endpoints: none mapped.
- Owner or source of truth: upstream dupeGuru maintainers.
- Data/API contract: Python package metadata, application source, tests, packaging files, docs, and localization.
- Setup, sync, build, test, or update commands: use normal Git workflow and project commands documented in `README.md` and `tools/AGENT_RUNBOOK.md`.
- Version, branch, or update cadence: verify from Git before release or upstream-sync work.
- Privacy, secret, license, and access boundaries: do not assume network access or private maintainer credentials.
- Status and caveats: current workspace; verify upstream policy before changing compatibility, release, packaging, or translation workflows.
- Reason this dependency still exists: canonical identity for the project.
