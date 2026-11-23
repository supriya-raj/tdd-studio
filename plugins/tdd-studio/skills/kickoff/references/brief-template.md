# PROJECT_BRIEF.md template

Copy into the project root as `PROJECT_BRIEF.md`. Fill it with the owner; keep their
wording. Delete prompts (the italic lines) once a section is written.

```markdown
# <Project name>

<One sentence: what it is and who it is for.>

## Goals
*What must be true for this project to be a success? 3-6 bullets, concrete.*

## Learning goals
*What does the owner want to understand by building it? (e.g. "how CRDTs merge
concurrent edits", "how a block editor integrates with sync"). Drives how much the
build explains, and which parts are hand-built instead of bought.*

## Non-goals
*What we are deliberately not doing (now). Prevents scope creep later.*

## Users and use cases
*Who uses it, the 3-5 core journeys, in one line each.*

## Features
*Grouped by milestone if the owner already has an order in mind; otherwise a flat list
to be grouped during planning.*

## Constraints
*Platform, offline needs, data sensitivity, performance targets, budget, deployment
(or "local only for now"), deadlines.*

## Stack
*Filled during kickoff from the accepted ADRs: one line per area, with the ADR number.*

## Milestones
*Filled during planning: each with a one-line "done when" that a test can check.*

## Working preferences
*Who writes tests (the owner or Claude), test balance (e2e/integration/unit),
how often to pause for review, commit/push conventions, design standard for UI.*

## Deferred
*Ideas parked on purpose, with a word on why.*
```
