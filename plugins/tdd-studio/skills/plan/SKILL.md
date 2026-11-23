---
name: plan
description: Turn an agreed architecture (PROJECT_BRIEF.md plus accepted ADRs and docs/ARCHITECTURE.md) into a test-first execution plan and the files that let any future session pick up exactly where the last one stopped. Writes docs/EXECUTION_PLAN.md (milestones with expected-to-fail "done when" tests, phases of small tasks with tests-first and verify columns, spikes), the working agreement in .claude/CLAUDE.md, docs/PROGRESS.md, docs/LEARNING_LOG.md, a README stub and, for UI projects, docs/DESIGN.md. Use this when the architecture is settled and someone asks for an execution plan, milestones, a roadmap, a TDD plan, task breakdown, or "how should we get this implemented". Part of the tdd-studio workflow (kickoff → plan → build-next).
---

# Plan: from an agreed architecture to a test-first execution plan

Second stage of the tdd-studio workflow. Inputs: `PROJECT_BRIEF.md`, accepted ADRs in
`docs/adr/`, `docs/ARCHITECTURE.md` (from `/tdd-studio:kickoff`). Outputs:

| File | Purpose |
|---|---|
| `docs/EXECUTION_PLAN.md` | How every task is done, and the phases of tasks |
| `.claude/CLAUDE.md` | The working agreement every future session follows |
| `docs/PROGRESS.md` | The **only** memory between sessions: status, decisions, handoff |
| `docs/LEARNING_LOG.md` | Dated lessons, surprises, mistakes, spike findings |
| `README.md` | Stub: one-liner, status, how to run, layout, doc links |
| `docs/DESIGN.md` | UI projects only: the design system and how UI is reviewed |

The next stage is `/tdd-studio:build-next`, which executes the plan task by task.

## Why the plan looks the way it does

- **Each milestone starts with its "done when" test, written first and expected to
  fail.** It turns a fuzzy goal into something checkable, and the test runner reports
  "expected to fail, but passed" the moment the milestone works. (Timeouts are not
  failures to most runners: give individual actions their own short timeouts so a
  missing feature fails as a failure.)
- **Tasks are small, test-first, and each has a Verify check.** A task is done only when
  its Verify passes. One task per commit, with the task ID in the message, so history
  maps to the plan.
- **Spikes answer library questions before tasks depend on them.** Time-boxed, findings
  go to the learning log, spike code is deleted.
- **Progress lives in the repo, not in chat.** Sessions end, context is summarised,
  machines change. `PROGRESS.md` plus git is enough to resume.
- **The owner approves each phase plan before it starts,** and can grant standing
  autonomy within an approved phase.

## Process

### 1. Sketch milestones with the owner

Before writing the plan file, propose in chat (brief, scannable):

- 4-7 milestones, each a user-visible capability, with a one-line **done when** that an
  end-to-end (or top-level) test can check;
- for each milestone, the main tests you would write (by layer);
- the test balance and who writes tests;
- what is deferred (deployment, CI, polish) and where it lands;
- any stability gate (e.g. "the merge test passes 5 times in a row").

Ask the owner to adjust. Common adjustments: "keep unit tests minimal", "I will not write
tests myself", "no deployment", "CI at the end, local checks first", "reduce the gate to
N runs". Record their answers in the brief's working preferences.

### 2. Write docs/EXECUTION_PLAN.md

Use `references/execution-plan-template.md`. Essentials:

- **How every task is done (TDD):** red (run the new tests, show them failing for the
  right reason), green (minimum code), refactor, Verify, commit with task ID, update
  PROGRESS in the same commit, push. UI tasks also follow DESIGN.md and are reviewed
  visually.
- **Test layers** chosen for this stack (from the testing ADR): what each layer is for,
  where its files live, naming/tagging by milestone (e.g. `@M2` in test titles).
- **Spikes**, **re-runnability rules**, **standard commands** (one `check` command that
  runs everything the gate needs), **test hooks** (test-only helpers compiled out of
  production builds, used to set up situations users cannot easily create).
- **Phases:** Phase 0 is setup (toolchain, strict types, test runners, lint/format,
  README stub, one smoke test per layer), done when the single check command is green
  on an empty app. Then one phase per milestone. Table columns:
  `ID | Goal | Deliverable | Tests first | Verify`. IDs: `T` + number for tasks, `S` +
  number for spikes, numbered by phase (T10-T19 for phase 1, …) so inserted tasks fit.
  The first task of each milestone phase writes the done-when test (expected to fail);
  the last closes the milestone (all tests green, cross-platform/browser run if
  relevant, stability gate).

### 3. Write the working agreement: .claude/CLAUDE.md

Use `references/claude-md-template.md`. It imports the brief, plan and progress (so they
load every session), sets the teacher role, the session-start protocol, the per-phase
approval, the per-task TDD rules, idempotency rules and code style. Keep it short: it is
loaded into every session.

### 4. Create PROGRESS.md, LEARNING_LOG.md, README stub, DESIGN.md (UI)

From `references/progress-template.md`, `references/learning-log-template.md`, and for UI
projects `references/design-template.md`. PROGRESS.md lists every task from the plan
with status `todo`. If the owner has a quality bar for UI ("professional and
beautiful"), DESIGN.md is where it becomes concrete: tokens, components, light/dark,
phone width, and a screenshot-review step that every UI task's Verify includes.

### 5. Clean up and hand over

Remove what is no longer relevant from the docs (draft notes, superseded text). Commit
(`docs: execution plan and working agreement`). Push only with the owner's agreement.
Then state the Phase 0 plan in 5-10 lines and wait for the owner's OK, or suggest
`/tdd-studio:build-next`.

## Reference files

- `references/execution-plan-template.md`: plan skeleton with a worked phase table
- `references/claude-md-template.md`: the working agreement (copy and adapt)
- `references/progress-template.md`: PROGRESS.md skeleton and status rules
- `references/learning-log-template.md`: learning log format with example entries
- `references/design-template.md`: DESIGN.md for UI projects (tokens, components, review)
