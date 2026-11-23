# ADR template

File: `docs/adr/NNN-short-kebab-title.md`, numbered in order of decision (001, 002, …).
One decision per ADR. Short is good: a reader should get the decision and its reasons in
two minutes.

```markdown
# ADR NNN: <Decision title>

- **Status:** Proposed | Accepted (YYYY-MM-DD) | Superseded by ADR MMM
- **Date:** YYYY-MM-DD

## Context

<The problem in this project's terms: forces, constraints, what the brief requires.
Link earlier ADRs this builds on.>

## Options

### A. <Option>
- Pros: …
- Cons: …
- Right when: …

### B. <Option>
…

## Decision

<What we chose, stated so it can be implemented and tested. Include the concrete
parameters (limits, versions, names) that were agreed.>

## Consequences

- <What becomes easier, what becomes harder, what is now forbidden or required.>
- <Follow-ups: spikes, later ADRs, things to revisit (with a trigger, e.g. "if builds
  get slow").>
```

## Rules

- **Never mark an ADR Accepted yourself.** The owner accepts, explicitly; record the date.
- **Amend, do not rewrite.** When an accepted decision changes, append:

  ```markdown
  ## Amendment (YYYY-MM-DD): <what changed>

  <Why (often a finding from a spike or a bug), what changes, what does not.
  If not yet approved: "(Proposed, awaiting owner approval)".>
  ```

  Then update every document the change touches.
- **Conditions are decisions.** If the decision is conditional ("use X if it is stable in
  the pinned runtime, otherwise Y"), the plan must include the check, and the outcome is
  recorded in the progress log with evidence (e.g. the stability label from the docs).

## Example (shortened)

```markdown
# ADR 005: Sync topology

- **Status:** Accepted (2026-01-12)
- **Date:** 2026-01-11

## Context
Notes must work offline and merge concurrent edits (ADR 001: a CRDT). Clients need a
way to exchange changes and a place where notes survive when every browser is closed.

## Options
### A. Peer-to-peer (WebRTC)
- Pros: no server for content. Cons: needs signalling anyway; peers must be online at
  the same time; no durable copy. Right when: ephemeral sessions.
### B. Central relay server that also persists
- Pros: durable copy; simple mental model. Cons: a server to run; operator can read
  content. Right when: notes must outlive sessions.

## Decision
B. One in-memory room per open note; changes relayed to the others and saved
(debounced) to SQLite. The server confirms durable saves with an `ack`.

## Consequences
- The server is a single point of storage: backups matter.
- Plaintext on the server: stated in the README (privacy honesty).

## Amendment (2026-01-12): ack after durable save
The `ack` is sent only after the write returns and carries the delete set as well as
the state vector (state vectors miss deletions).
```
