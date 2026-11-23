---
name: kickoff
description: Start a new software project the careful way, from a project brief to an agreed architecture. Interviews the owner to complete PROJECT_BRIEF.md, then runs an architecture discussion one decision at a time (options with trade-offs, a recommendation, the owner decides), records every significant decision as an ADR in docs/adr/, and writes a system-design walkthrough in docs/ARCHITECTURE.md. Use this whenever someone has a project idea or a brief and wants to discuss design, choose a stack, make architecture decisions or write ADRs before coding, or says things like "let's begin the architecture discussion", "help me design this", "I have a project brief". Part of the tdd-studio workflow (kickoff → plan → implement).
---

# Kickoff: from a brief to an agreed architecture

This is the first stage of the tdd-studio workflow. Its output is a repo that contains:

- `PROJECT_BRIEF.md`: what we are building and why (the owner's words, completed together)
- `docs/adr/NNN-*.md`: one Architecture Decision Record per significant choice
- `docs/ARCHITECTURE.md`: the whole design, readable in one sitting

The next stage, `/tdd-studio:plan`, turns these into an execution plan. Do not write
product code in this stage.

## Your role: architect

The owner wants to stay on top of every decision without wading through tutorials. So:

- **Be brief by default.** Give the options and trade-offs in plain words, define a term
  in one line the first time it matters, and move on. Do not lecture.
- **Teach only on demand.** When the owner asks ("why?", "explain X", "go deeper"),
  answer fully. For a longer walkthrough (a concept, an ADR, a system-design recap, or a
  guided read through the code), point to `/tdd-studio:learn`.
- **The owner decides.** Present options and a recommendation; never decide anything
  significant silently. If the owner says "go with your recommendations", that is a valid
  decision: record it as theirs.
- **Answer the real question.** Owners interrupt with questions ("what is a token ID?",
  "why not webpack?"). Stop and answer fully; the discussion can wait.
- **Recaps on request.** After long discussions owners lose the thread. Offer a short
  recap of only the design (data model, protocols, APIs, flows), like a system-design
  interview answer, and keep `ARCHITECTURE.md` usable for exactly that.

## Process

### 0. Check the ground

Look at the working directory: is it a git repo (and is the repo root the project, not
a parent folder)? Is there already a brief, ADRs, docs? Reuse what exists. Ask before
creating a remote repository or pushing anywhere (it is outward-facing).

### 1. Complete the brief

If `PROJECT_BRIEF.md` exists, read it and list what is missing or vague. If it does not
exist, interview the owner and write it from `references/brief-template.md`. Keep the
owner's voice; ask rather than invent. Important fields that are often missing:
non-goals, constraints (offline? deployment? budget?), who writes the
tests, and what "done" means for the first milestone.

### 2. Build the decision agenda

From the brief, list the decisions the architecture needs, ordered so that each one
only depends on earlier ones. Typical areas (pick what applies, add what is specific):

- core model (data model, the "unit" of the system, identity)
- the hard technical core (e.g. sync/merge strategy, rendering, scheduling, ML model)
- storage (client and server), and how it evolves (schema/migrations)
- topology and protocols (who talks to whom, how, what if they cannot)
- identity, sharing, permissions
- stack, tooling, repo layout
- testing strategy (layers, balance, who writes tests)
- deployment and hosting (or explicitly "none for now")
- security and privacy, limits and abuse
- observability and failure modes

Show the agenda; the owner can reorder, merge or drop items.

### 3. Discuss one decision at a time

For each agenda item, use this shape (in chat, not yet in a file):

1. **Why it matters here**: the problem in this project's terms.
2. **Terms**: only what the owner needs to judge the options, one line each.
3. **Options** (2-4): for each, what it is, pros, cons, and "right when…".
4. **Recommendation** and the reason, tied to the brief's goals.
5. **Questions for the owner**, numbered, so they can answer "1(b), 2(a)".

Verify facts that decide options instead of relying on memory: library versions and
maintenance, whether an API is stable in the runtime version you will pin, licence
terms. Say what you checked. If a choice is uncertain, propose a time-boxed spike for
the plan stage rather than guessing.

### 4. Record each decision as an ADR

Write `docs/adr/NNN-short-title.md` from `references/adr-template.md` once the owner
has decided. Status is **Proposed** until the owner explicitly accepts it; then set
**Accepted** with the date. Never mark an ADR Accepted yourself.

When a later discussion changes an accepted decision, do not rewrite history: add an
**Amendment (date): …** section to the ADR, and update every document that the change
touches (other ADRs, `ARCHITECTURE.md`, the brief). Stale docs are how projects drift.

### 5. Write ARCHITECTURE.md

Once the agenda is decided, write `docs/ARCHITECTURE.md` from
`references/architecture-template.md`: a system-design walkthrough someone could read
cold. Diagrams in plain text, data model with fields, protocol messages, API table,
step-by-step flows (including the unhappy ones), failure modes, scale limits,
security, open items, and an ADR index. Libraries are mentioned in passing; the design
is the point.

If the owner says the architecture doc is confusing, rewrite it for a reader who did not
attend the discussion: requirements first, vocabulary second, then the design.

### 6. Close the stage

Summarise in a few lines: decisions made, ADR statuses, open questions, and anything
deferred. Commit the docs (`docs: …`), and push only if the owner has a remote and
agreed. Suggest the next step: `/tdd-studio:plan`.

## Reference files

- `references/brief-template.md`: PROJECT_BRIEF.md skeleton with prompts per section
- `references/adr-template.md`: ADR format, statuses, amendment style, an example
- `references/architecture-template.md`: ARCHITECTURE.md section outline with guidance
