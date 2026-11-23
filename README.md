# tdd-studio

A Claude Code plugin that takes a software project **from a project brief to a tested
build**, the careful way: a teacher-mode architecture discussion, a decision record for
every significant choice, a test-first execution plan, and a build loop that keeps its
progress checked into the repo so any session can pick up where the last one stopped.

It is stack-agnostic: the stack is decided per project, in the architecture discussion.

## The workflow

| Stage | Command | What you get |
|---|---|---|
| 1. Kickoff | `/tdd-studio:kickoff` | A completed `PROJECT_BRIEF.md`; a discussion where every concept is explained in plain language and **you** decide; one ADR per decision in `docs/adr/`; a system-design walkthrough in `docs/ARCHITECTURE.md` |
| 2. Plan | `/tdd-studio:plan` | `docs/EXECUTION_PLAN.md` (milestones, each starting with an expected-to-fail "done when" test, then small test-first tasks with a Verify check), the working agreement in `.claude/CLAUDE.md`, `docs/PROGRESS.md`, `docs/LEARNING_LOG.md`, a README stub, and `docs/DESIGN.md` for UI projects |
| 3. Build | `/tdd-studio:build-next` | Each session: check progress against git and the checks, then the next task or phase, test-first, one commit per task, progress updated every time |

You only need a project idea (or a brief) to start: `/tdd-studio:kickoff`.

## What the build loop insists on

- **Red for the right reason:** new tests run and fail because the behaviour is missing,
  not because of a typo or a server that did not start.
- **Sabotage checks:** break the specific behaviour a test protects and watch the test
  fail, especially for "X never happens" tests and anything security- or data-related.
- **Stability repeats:** run affected tests several times; flaky means not done.
- **Visual review for UI:** screenshots in every theme and at phone width, looked at
  before a UI task is done.
- **Probe before guessing**, fix causes instead of raising timeouts, and report honestly
  (including what could not be verified).
- **The owner stays in charge:** phase plans are approved before they start, ADRs are
  never self-accepted, and destructive or outward-facing actions are asked about first.

These rules came from building a real project (a local-first, real-time collaborative
notes app) with this workflow; `build-next` carries the lessons as a reference.

## Install

In Claude Code:

```
/plugin marketplace add supriya-raj/tdd-studio
/plugin install tdd-studio@tdd-studio
```

Then, in a new or empty project folder: `/tdd-studio:kickoff`.

## Repository layout

```
.claude-plugin/marketplace.json      this repo as a plugin marketplace
plugins/tdd-studio/
  .claude-plugin/plugin.json         the plugin manifest
  skills/kickoff/                    brief → architecture discussion → ADRs → ARCHITECTURE.md
  skills/plan/                       execution plan, working agreement, progress files
  skills/build-next/                 the test-first build loop, milestone close, showcase
```

Each skill has a `SKILL.md` and `references/` with templates and detailed guidance that
load only when needed.

## Licence

MIT
