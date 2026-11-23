---
name: implement
description: Continue building a tdd-studio project: run the session-start check against docs/PROGRESS.md and git, then execute the next task or phase from docs/EXECUTION_PLAN.md test-first (red for the right reason, green, refactor, sabotage check, stability repeats, visual review for UI), commit one task per commit, push, and update PROGRESS.md so the next session can resume cold. Also closes milestones and, when asked, records README showcase media. Use this whenever a repo has docs/EXECUTION_PLAN.md and docs/PROGRESS.md and the owner says things like "continue", "next task", "start phase 2", "resume", "go ahead with the next milestone", "implement everything in this phase", or asks to close a milestone or showcase the project. Part of the tdd-studio workflow (kickoff → plan → implement).
---

# Implement: the test-first build loop

Third stage of the tdd-studio workflow. The repo already has a brief, ADRs, an
architecture doc, `docs/EXECUTION_PLAN.md`, `.claude/CLAUDE.md` and `docs/PROGRESS.md`
(from `/tdd-studio:plan`). The working agreement in `.claude/CLAUDE.md` is the
authority; this skill is how to carry it out well.

## 1. Session start (every session)

1. Read `docs/PROGRESS.md`: it is the only memory between sessions. Do not rely on chat
   history or summaries.
2. Verify it against reality: `git status`, `git log --oneline -10`, fetch and compare
   with the remote, and run the check command. If notes and repo disagree, trust the
   repo, fix PROGRESS.md and say what you corrected.
3. Tell the owner in 3-5 lines: phase, last completed task, the single next action.

If the environment itself is broken (permissions, missing runtime, an unreachable
folder), say exactly what fails and what the owner can do; do not try to work around
safety settings.

## 2. Phase boundaries

Before a phase starts, state its plan in 5-10 lines (tasks, new libraries, decisions you
expect to make, anything that needs the owner) and wait for the owner's OK. Within an
approved phase, follow the owner's preference: some want a check-in after each task,
some want the whole phase done without pauses. Either way, still ask before destructive
operations, outward-facing actions, and new ADR-level choices; record smaller choices in
the decisions log and report them at the end.

## 3. One task, end to end

Read `references/tdd-loop.md` for the full loop. In short:

1. **Re-read the task row** (deliverable, tests first, verify) and the ADR/architecture
   sections it touches. Check what already exists (tasks must be re-runnable).
2. **Red:** write the tests first and run them. Read *why* they fail. Red for the wrong
   reason (typo, wrong selector, server not started, a test-only setup gap) is fixed
   before any implementation. If you ever write code before its test ran red, take the
   code away (stash), show red, then restore.
3. **Green:** the minimum implementation. New libraries: check maintenance, version,
   licence and compatibility first; say what you checked.
4. **Refactor** with tests green.
5. **Sabotage check:** break the specific behaviour (remove the guard, reorder the
   steps, disable the feature) and watch the relevant test fail; then restore. Mandatory
   for tests that passed before the implementation existed ("guards"), for negative
   assertions ("X never happens"), and for anything security- or data-loss-related.
6. **Stability:** repeat the affected tests (3-5×). Flaky means not done.
7. **UI tasks:** capture screenshots (every theme, desktop and phone) and look at them.
   Fix what they show. Follow `docs/DESIGN.md`.
8. **Verify** exactly as the task row says, then run the full check.
9. **Record:** PROGRESS.md (status, next action, decisions, open questions, handoff
   note), LEARNING_LOG.md for lessons, docs touched by the change (README, ADR
   amendments, architecture). Commit with the task ID, push.

Report each task in a few lines (what changed, what the tests prove, anything odd). Do
not lecture; if the owner asks "why?" or "explain X", answer from the code, and suggest
`/tdd-studio:learn` for a longer walkthrough.

## 4. When things go wrong

Read `references/hard-won-rules.md` when a test fails unexpectedly, flakes, or passes
suspiciously. The core habits:

- **Probe before guessing.** Capture the console and page errors, dump the DOM or the
  messages, run the failing step alone, repeat it 15×. One probe usually ends a guessing
  spiral.
- **Fix causes, not timeouts.** Raise a time budget only when the cause is outside the
  project and per-step limits still catch real hangs; say so explicitly.
- **Rule out the app before blaming it, and the test before trusting it.**
- **Never weaken a test to make it pass** without telling the owner why the old
  assertion was wrong.

## 5. Spikes

Time-boxed (≤ 2 hours), throwaway code in a scratch location or temporary files that are
reverted, findings in LEARNING_LOG.md with caveats, nothing committed but docs. A spike
that contradicts an accepted ADR produces a **proposed** amendment for the owner, plus
any fix that merely makes the accepted decision actually hold.

## 6. Closing a milestone

Read `references/phase-close.md`: the done-when test is a normal test (switched in the
task that made it pass), the full check is green, every target (browsers, platforms) is
green with repeats, README status updated, PROGRESS.md says the phase is complete and the
next action is "state the next phase plan and wait for the owner's OK".

## 7. Showcase (optional, when the owner asks)

Read `references/showcase.md`: recording demo GIFs and screenshots from the real app
with self-checking scripts, and a README that sells the project honestly (including
privacy statements the ADRs require).

## Reference files

- `references/tdd-loop.md`: the per-task loop in detail, with commit and PROGRESS examples
- `references/hard-won-rules.md`: debugging and test-honesty rules learned the hard way
- `references/phase-close.md`: milestone close checklist
- `references/showcase.md`: demo recording and README showcase
- `references/example-typescript-web.md`: a worked example for a TypeScript web app
  (pnpm, Vite/React, Vitest, Playwright): configs, fixtures and pitfalls
