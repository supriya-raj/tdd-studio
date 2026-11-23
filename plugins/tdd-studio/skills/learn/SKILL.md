---
name: learn
description: On-demand walkthroughs of a tdd-studio project, grounded in its own docs and code. A system-design recap in the shape of a design interview (requirements, API, data model, diagram, deep dive, failure modes, trade-offs), a guided read through the code that traces one flow file by file with real snippets, an ADR explained with its alternatives, a concept shown where it lives in this repo, or a drill of interview-style questions. Read-only. Only runs when the owner invokes /tdd-studio:learn.
argument-hint: "[design | code <flow, task ID or path> | adr <number> | <concept> | drill]"
disable-model-invocation: true
---

# Learn: understand the system you are building, on demand

The other stages stay brief on purpose. This skill is where depth lives, and it only
runs when the owner asks for it. The owner wants a firm grip on the architecture as a
whole and enjoys reading real code, so every answer is anchored in **this repo**:
`docs/ARCHITECTURE.md`, `docs/adr/`, `docs/PROGRESS.md` and the source itself.

## Ground rules

- **Read-only.** Do not edit code, tests, plans or progress. If you spot a bug or a
  stale doc, say so at the end and leave it for `/tdd-studio:implement`.
- **Real code only.** Every snippet is quoted from a file you just read, with
  `path:line`. Keep snippets short (5-20 lines) and cut with `…`. Never invent an
  illustrative version of code that exists.
- **Built vs designed.** Check `docs/PROGRESS.md` and the code before you describe
  something as working. Mark parts that are designed but not built yet, and say where
  the code and the docs disagree.
- **One screen at a time.** Walk in short stops and end each walkthrough with 2-3 places
  to go deeper, so the owner steers.

## Pick the mode from the argument

No argument: show the modes in one line each, with a suggestion based on what was built
most recently (from `git log` and PROGRESS.md).

### `design`: system-design recap

Answer as if this were a system-design interview about the project:

1. **Requirements**: functional, then non-functional (scale, latency, durability,
   offline, privacy), each in one line.
2. **API / interfaces**: the main endpoints, messages or commands, in a table.
3. **Data model**: entities with their key fields, and where each one is stored.
4. **High-level design**: a plain-text diagram of the components and arrows, with one
   line per component.
5. **Deep dive**: the hard part (sync, scheduling, ranking…), step by step with a tiny
   worked example.
6. **Failure modes and limits**: what breaks first, and what happens when it does.
7. **Trade-offs**: each one with its ADR number, the option that lost, and what would
   make us revisit it.

If the owner names a part (`design sync`), do steps 4-7 for that part only.

### `code <flow | task ID | path>`: guided code reading

Trace one path through the code end to end, the way you would read a codebase on your
first day:

1. State the flow in one sentence (e.g. "a keystroke in tab A reaches tab B").
2. Find the entry point and follow the calls. Read every file you cite.
3. Present 5-8 stops. Each stop has `path:line`, a short snippet, and two lines: what
   this code does in the flow, and what to notice (an invariant, a guard, a trade-off,
   the test that pins it down).
4. Close with the tests that prove the flow works (names and paths), and the ADR or
   architecture section it implements.

A task ID (`code T27`) means: the commit(s) for that task (`git log --grep`), the diff
read as a story, and the tests written first.

### `adr <number>`: a decision in context

The problem in this project's terms, the options that were on the table, why this one
won, where it shows up in the code now (`path:line`), and what would trigger an
amendment.

### `<concept>`: a concept where it lives

Explain it in plain language with one analogy, then show where this repo uses it (code
and tests), and what would go wrong without it. If the repo does not use the concept,
say so and keep the answer short.

### `drill`: check your own understanding

Ask 3-5 interview-style questions about this system, one at a time (e.g. "what happens
if the server restarts during a save?", "why not store snapshots per keystroke?"). Wait
for the owner's answer, then give the answer from the docs and code with references,
noting what they got right and what they missed.
