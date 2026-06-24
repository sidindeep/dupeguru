# Result Marking Contract

dupeGuru result rows are markable files, not only non-reference duplicates.

## Behavior

- Every visible file row in the result table can show and toggle the marked checkbox, including the current group reference row.
- Mark All, Mark None, Invert Marking, and Mark Selected operate on all markable result files in their current filtered scope.
- The result stats line counts marked files and total markable files, including references.
- Actions over marked files, including delete, move, copy, and remove from results, operate on the exact marked file set.
- When a marked reference is deleted with "link deleted files" enabled, dupeGuru must not create a replacement link from the deleted reference path to itself.
- Removing a reference from results may remove its whole group when the group no longer has enough files to be a duplicate result.

## Verification

- Targeted regression coverage lives in `core/tests/results_test.py` and `core/tests/app_test.py`.
- The focused verification command is `python -m pytest core/tests/results_test.py core/tests/app_test.py`.
