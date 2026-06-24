# Pending Tasks

Use this file for active project-wide plans and multi-step work.

Keep entries concise and task-relevant. Do not store full diffs, large logs,
generated outputs, secrets, credentials, or private production data.

## Status Markers

- `[ ]` not started
- `[~]` in progress
- `[x]` done
- `[!]` blocked or needs attention

## Tasks

### Local Sprint: Mark Any Result File

Goal: allow users to mark every row in duplicate search results with checkboxes,
including reference files, so marked actions can operate on exactly the files
the user selected.

Planned changes:

- [x] Make reference result rows markable in core results.
- [x] Let marked delete/move/copy/remove operations include marked references.
- [x] Update result statistics and user-facing wording from duplicates to files.
- [x] Add regression coverage for marking and removing reference files.
- [x] Verify targeted core tests.

Execution order:

- [x] Inspect result marking, deletion, and table model flow.
- [x] Patch core behavior and tests.
- [x] Run targeted verification.

Risks or dependencies:

- [x] Deleting a marked reference must not create a replacement link to itself.
- [x] Removing the last useful file in a group must not leave stale group mappings.

Verification:

- [x] `python -m pytest core/tests/results_test.py core/tests/app_test.py`
