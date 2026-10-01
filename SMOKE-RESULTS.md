# Smoke cycle results

Date: 2026-10-01. Branch: `smoke`, based on `6c939e9` (bootstrap commits, not yet merged to `main`).
Steps: `BOOTSTRAP.md` steps 7 and 8.

Baseline before any break: `run_all.py` 9/9 passed, `pytest -q tests` 169 passed.
Each break was tested on its own and undone before the next one. After the last undo, 9/9
checks passed and 169 tests passed again.

## Step 7: hooks

`git config core.hooksPath .githooks` was run. Git wrote it to the shared repository config
(`/home/claude/appliance-dryrun/.git/config`), not to this worktree alone. See note 2.

## Step 8: smoke cycle

"Refused" means the named check exited 1 when run alone and `run_all.py` exited 1 with 8/9 passed.

| # | Check | Breaking change | Setup created | Refusal message | Refused |
| --- | --- | --- | --- | --- | --- |
| 1 | E10 | Added `docs/design/INTENT-001-smoke-misplaced.md`, a valid draft intent in the design folder. | The artifact file. | `E10 docs/design/INTENT-001-smoke-misplaced.md: type 'intent' belongs in docs/case/` | yes |
| 2 | SEATS | Added `docs/case/INTENT-001-smoke-self-approved.md` with `author_seat: Intent Owner` and `approver: Intent Owner`. | The artifact file. | `SEATS docs/case/INTENT-001-smoke-self-approved.md: author and approver are the same seat` | yes |
| 2b | SEATS | Same file with `author_seat: Engineering Lead`, the same holder as the approver. Variant, see note 8. | Same file. | `SEATS docs/case/INTENT-001-smoke-self-approved.md: author and approver seats share holder 'Human 1'` | yes (single check only) |
| 3 | DOR | Deleted row `C3: {status: open}` from `docs/dor/checklist.yml`. | None. | `DOR docs/dor/checklist.yml: floor row C3 is missing; floor rows cannot be deleted` | yes |
| 4 | E3 | Added `tests/test_smoke_early_return.py`, a test with `if True: return` before a failing assert. | The test file. No product test folder exists. | `E3 tests/test_smoke_early_return.py:3: return inside test 'test_smoke_early_return'; an early return passes silently` | yes. Pytest alone reported the test as passed. |
| 5 | E11 | Appended two valid near-miss entries, RG-001 and RG-003, to `docs/registry.md`. | Two entries, since the registry was empty. | `E11 docs/registry.md: RG-003: expected RG-002; ids are ascending without gaps` | yes |
| 6 | E14 | Added `contracts/loan-due.yml` with `consumers: []`. Every other field was valid. | The `contracts/` folder and one contract. | `E14 contracts/loan-due.yml: missing 'consumers'` and `E14 contracts/loan-due.yml: no consumers: output no one reads is a dead contract (STD-002 clause 2)` | yes |
| 7 | E15 | Committed `migrations/0001_create_loans.sql` (commit `2f2918e`), used that commit as the base ref, then committed an added `ALTER TABLE` line to the same file. | `migrations/` and one migration, plus two temporary commits. Both were removed by `git reset --hard 6c939e9`. | `E15 migrations/0001_create_loans.sql: merged migration edited; migrations are append-only, add a new one` | yes, with `--base 2f2918e` or `APPLIANCE_BASE=2f2918e`. Plain `run_all.py` passed 9/9. See note 5. |
| 8 | E16 | Added `docs/design/ARCH-001-smoke-no-data-model.md`, built from the architecture template, `status: in-review`, with all seven layers filled and the data-model block removed. | That file, plus a draft parent `docs/case/INTENT-001-smoke-parent.md`. | `E16 docs/design/ARCH-001-smoke-no-data-model.md: missing required diagram 'data-model' (Entities, attributes and relationships with cardinality)` | yes. Control: at `status: draft` the same file passed. |

## Pre-push hook

Break in place: item 1, the misplaced intent, as an untracked file.
Command: `../.githooks/pre-push origin https://example.invalid`, run from `docs/` to confirm the
hook resolves the repository root in a linked worktree.
Result: blocked, exit 1. Output was the E10 refusal above and `8/9 checks passed`. Because of
`set -e`, pytest did not run after the failed checks.

## Notes: unclear, missing or contradictory instructions, and guesses

1. **Order of steps.** `STATE.md` says steps 5 (challenge) and 6 (approve and merge) have not
   happened. `BOOTSTRAP.md` puts the smoke cycle after the merge. This run used the unmerged
   bootstrap commits as instructed. `origin/main` still holds the bare template, and E15
   compares against `origin/main` by default.
2. **Hook install leaves the worktree.** In a linked worktree, `git config core.hooksPath`
   writes to the shared repository config, so it applies to every worktree of the clone. The
   relative path resolves per worktree, so it is harmless here. The task said to touch nothing
   outside this worktree, but step 7 as written cannot be done without this. A per-worktree
   install needs `extensions.worktreeConfig` and `git config --worktree`.
3. **"Discard the branch" conflicts with "record results in STATE.md".** Step 8 records results
   in `STATE.md`, then discards the branch, which discards the record. This task said to
   commit to `smoke`. The record has to be carried to `bootstrap` or `main` some other way,
   for example by cherry-picking the results commit. No instruction says which.
4. **Which command must refuse is not stated.** Step 8 says "confirm it refuses" but does not
   name the single check, `run_all.py`, the hook or CI. All three local paths were run. CI and
   branch protection were not exercised, because nothing could be pushed. Under the rule "a gate
   that has not been seen to refuse is not trusted", CI is still unseen.
5. **E15 cannot be smoked as written, and the default base hides it.** A "merged migration"
   needs a merge to `main`. A local commit was used as the base instead. Without `--base`,
   `run_all.py` and the pre-push hook compare against `origin/main`, so they pass this break.
   That is correct, because the fixture was never merged, but it means the hook alone cannot
   show E15 refusing. Two more gaps follow. An uncommitted edit to a merged migration passes,
   because the diff is `base..HEAD`. A push to a lane other than `main` is compared with
   `origin/main`, not with the lane's own base.
6. **The pre-push hook checks the working tree, not the commits being pushed.** It ignores the
   refs on stdin. It blocked an untracked file that would never have been pushed. In the other
   direction, a broken commit with an uncommitted fix in the working tree would pass the hook
   and be pushed. CI would still catch it.
7. **E14 reports an empty consumer list twice.** It says `missing 'consumers'` even though the
   key is present, then gives the correct "no consumers" message.
8. **Item 2 wording.** "Author is also its approver" can mean the same seat or the same holder.
   Both were tested, and both refuse. Under `single_principal: true`, `Human 1` holds both human
   seats, so a human-authored artifact can never have a human approver.
9. **E10 does not see artifacts outside the artifact folders.** A valid intent placed at
   `docs/misc/INTENT-001-smoke-stray.md` passed E10, because the check only scans folders named
   in `docs/artifact-types.yml`. The smoke item (wrong artifact folder) refuses. An artifact in
   a non-artifact folder is silently ignored.
10. **Setup folders.** `contracts/` and `migrations/` do not exist yet. `appliance.yml` says
    the first increment creates them. Both were created for the smoke run and then removed.
    E14 and E15 pass silently while the folders are absent.
11. **Holder names.** `CLAUDE.md` names `PublicEnemage` for both human seats. `docs/roles.yml`
    names the holder `Human 1`. These may be meant to match.
