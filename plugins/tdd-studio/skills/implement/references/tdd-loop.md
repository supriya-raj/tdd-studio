# The per-task loop in detail

## 0. Orient
- Read the task row: Goal, Deliverable, Tests first, Verify.
- Read the ADR and ARCHITECTURE sections it implements. If they disagree with what you
  find in the code or the libraries, stop and raise it (an ADR amendment may be needed).
- Check what exists already (files, dependencies, config). Reconcile, do not recreate.
- If the task's test depends on another task's feature, reorder (and say so in
  PROGRESS.md: "T34 done before T33: T33's test needs share links").

## 1. Red
- Write the tests the row asks for, at the layer it names. Test behaviour, not
  implementation: select UI elements by role and accessible name; drive servers through
  their public protocol with real clients.
- Run them and **read the failure**. Good red: "button 'Share' not found", "expected 4401,
  got open". Bad red: syntax errors, a missing import, a typo in a label, a fixture that
  never starts. Fix bad red first.
- Tests that pass before the implementation exists are **guards** (e.g. "never serves
  files outside the folder"). Keep them, but they must be sabotage-checked after green.
- Give the e2e runner per-action timeouts so a missing element fails as a failure, not
  as a whole-test timeout.

## 2. Green
- Minimum code that passes. Prefer boring, well-typed code.
- Small decisions inside the task (a lint option, a version pin, a CSS compromise) go in
  PROGRESS.md's decisions log with the reason. Significant ones need an ADR and the
  owner.
- Lint and typecheck as you go; fix warnings with the narrowest change, and explain any
  rule you relax.

## 3. Refactor
- Extract helpers the tests now share (e.g. `createNote`, `waitForSave`), remove
  duplication, rename for clarity, all with tests green.

## 4. Sabotage check
Remove or reorder exactly the thing the test protects, run the test, see it fail with a
message that points at the problem, restore. Examples:
- remove the containment check in a static file server → traversal test leaks a secret;
- send the acknowledgement before the save → "no ack on failed save" test fails;
- apply a change before checking limits → "oversized change not applied" fails;
- disable the cross-tab channel → "two offline tabs stay in sync" fails;
- reset the save timer on every edit → "continuous typing still saves" fails.
If the test still passes, the test is wrong: strengthen it (see hard-won-rules.md).
Use scripted edits that **assert they matched** (a silent no-op sabotage proves nothing).

## 5. Stability
- Repeat the affected tests (`--repeat-each=5` or a loop) and, for concurrency or
  timing-sensitive code, the whole suite. Any failure is a finding, not noise.

## 6. UI review
- Capture every screen the task touches in each theme and at phone width; look at the
  images; fix and recapture. Add new screens to the capture script.

## 7. Verify, record, commit
- Run the row's Verify, then the full check.
- Update PROGRESS.md: task `done`; next action; decisions; open questions; a handoff
  paragraph naming files, commands and gotchas.
- LEARNING_LOG.md for anything surprising.
- Commit: `feat(T24): in-memory rooms relaying sync and presence over WebSockets`
  (`test(T10)` for an acceptance-test-only task, `fix(T31)` for a fix to a finished task,
  `chore(T38)` for a milestone close, `docs:` for docs-only work). Push.

## Example handoff note

> T26 done: `src/db/notesStore.ts` (load; upsert whole state + schema version). Rooms
> load from SQLite on open; any other change sets `dirty` and schedules a save 2 s after
> the first unsaved change (not reset by later edits); unload and shutdown save first; a
> failed save re-marks dirty and retries. Env overrides: `SAVE_DEBOUNCE_MS`. Tests
> `test/persistence.int.test.ts`; the no-postpone rule verified by sabotage.
