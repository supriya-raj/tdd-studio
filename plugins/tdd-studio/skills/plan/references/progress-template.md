# docs/PROGRESS.md template

```markdown
# Progress

## Current phase
**Phase 0: Setup (M0): not started.** Execution plan approved YYYY-MM-DD.

## Next action
State the Phase 0 plan in 5-10 lines and wait for the owner's OK.

## Task status

| Phase | ID | Task | Status |
|---|---|---|---|
| 0 | T01 | Workspace root | todo |
| … | | | |

Statuses: `todo`, `in-progress` (with notes below), `done` (verification passed), `blocked`.

## Decisions log
- YYYY-MM-DD: <decision, one or two lines, with the ADR or reason>.

## Open questions
- <question>: <where it will be answered (spike/task) or "owner decision needed">.

## In-progress notes
None.

## Handoff note
<Per task, appended when done: what exists now (files, commands, names a next session
needs), what was verified and how, gotchas. Enough for a cold start.>
```

## Rules

- **Exactly one next action**, concrete enough to start without reading chat.
- **Decisions log** records small choices made inside tasks (a pinned version, a
  relaxed lint rule, a changed test approach) with the reason, so the owner can review
  them and so later sessions do not re-litigate them.
- **Open questions** carry an owner: a spike, a task, or "owner decision needed".
- Update in the same commit as the work. A task is not done until PROGRESS says so.
- On session start, if PROGRESS disagrees with git or the checks, **trust the repo**,
  fix PROGRESS, and say what was corrected.
