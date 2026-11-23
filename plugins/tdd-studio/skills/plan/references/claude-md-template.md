# .claude/CLAUDE.md template (the working agreement)

Copy into the project as `.claude/CLAUDE.md` and adapt the bracketed parts. Keep it
short: it is loaded into every session.

```markdown
# <Project>: working agreement

@PROJECT_BRIEF.md
@docs/EXECUTION_PLAN.md
@docs/PROGRESS.md

I (the owner) decide everything significant; never decide silently. Keep explanations
short by default. When I ask ("why?", "explain X"), answer fully, from the actual code
with file:line references. For longer walkthroughs I use `/tdd-studio:learn`.

Design reference: `docs/ARCHITECTURE.md` (system design) and `docs/adr/` (accepted
decisions with their reasons).

## Implementation protocol
**Session start, every session, in order:**
1. Read `docs/PROGRESS.md`. It is the only memory between sessions. Never rely on chat
   history.
2. Verify it against reality: `git status`, `git log --oneline -10`, and run the repo's
   checks. If notes and repo disagree, trust the repo, fix `PROGRESS.md`, and tell me what
   you corrected.
3. Tell me in 3-5 lines: current phase, last completed task, and the single next action.

**Before each phase:** state the plan in 5-10 lines and wait for my OK. [Optional:
"Once I approve a phase, implement all its tasks without pausing between them."]

**Each task (TDD):** write the tests first and show them failing; implement; show them
passing; refactor. A task is done only when its Verify check passes. [Test balance, e.g.
"Tests are integration- and e2e-first, with unit tests only where EXECUTION_PLAN.md says
so."] [Who writes tests, e.g. "You write all tests; I review."]

**Idempotency rules:**
- Tasks must be re-runnable. Check before creating files, folders, dependencies, or config.
- One task per commit, with the task ID in the message (`feat(T12): ...`), conventional
  prefixes (`feat:`, `fix:`, `test:`, `docs:`, `chore:`). Push after committing.
- No destructive operations (deleting files, force pushes, resetting history, dropping
  data) without asking me first.
- If a task is interrupted, mark it `in-progress` in `PROGRESS.md` with notes on what
  exists. On resume, inspect the repo and finish it rather than redoing it.
- New significant technical choices get an ADR and my approval first. Never mark an ADR
  `Accepted` yourself.

**After every task and before a session ends:** update `docs/PROGRESS.md` (task status,
next action, decisions, open questions, handoff note) and commit it with the work.

## Code style
- Small, reviewable steps.
- [UI projects: "UI must look professional and beautiful in every task: follow
  `docs/DESIGN.md` and review screenshots before a UI task is done."]
- [Language rules, e.g. "Strict typing; no `any` without a comment."]
```
