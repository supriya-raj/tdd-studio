# EXECUTION_PLAN.md template

```markdown
# <Project>: execution plan

Derived from `PROJECT_BRIEF.md`, the accepted ADRs (`docs/adr/`) and
`docs/ARCHITECTURE.md`. Progress is tracked in `docs/PROGRESS.md`.

## How every task is done (TDD)

1. **Red:** write the task's tests first; run them and show them failing *for the right
   reason* (the missing behaviour, not a typo, a wrong URL or a server that did not start).
2. **Green:** implement the minimum to pass.
3. **Refactor** with tests green.
4. Run the task's **Verify** check. A task is done only when it passes.
   Any task that adds or changes UI also follows `docs/DESIGN.md` and is checked
   visually (screenshots in every theme and at phone width) before it is done.
5. Commit tests and code together: `feat(T23): …` (conventional prefix, task ID).
   Update `docs/PROGRESS.md` in the same commit. Push.

**Test layers** (ADR <n>):
- **<Layer 1, e.g. integration>** (<runner>): <what it exercises; real dependencies?>.
  Main layer for <…>. Files: `<path pattern>`.
- **<Layer 2, e.g. end-to-end>** (<runner>): <real UI/clients against the real system>.
  Files: `<path pattern>`, tagged by milestone in the title (`@M1`, `@M2`, …).
- **<Unit>**: only where a pure function has tricky cases (<list them>).
- Each milestone starts with its **"done when" test marked expected-to-fail**, switched
  to a normal test in whichever task makes it pass (an expected failure that passes
  turns the run red).

**Spikes** (`S` IDs) answer library-behaviour questions: time-boxed (≤ 2 hours), no TDD,
findings written to `docs/LEARNING_LOG.md`, spike code deleted (not committed).

**Re-runnability rules:**
- Check before creating: files, folders, dependencies, config. Reconcile, do not recreate.
- Install dependencies with the package manager (never hand-edit the lockfile).
- Migrations are forward-only and recorded; re-running is a no-op.
- Tests use temp directories, random ports and fresh state; no shared state between runs.
- An interrupted task is marked `in-progress` in PROGRESS.md with notes on what exists.

**Standard commands:**

| Command | Runs |
|---|---|
| `<check>` | lint + format check + typecheck + all fast tests + main e2e browser/target |
| `<test:int>` | integration tests |
| `<e2e>` / `<e2e:all>` | end-to-end on the main target / on every target |
| `<shots>` | (UI) screenshots of every screen for visual review |

**Test hooks:** test builds expose helpers (e.g. `window.__app` in a browser app) to set
up situations a user cannot easily create (inject a remote change, clear a cache, set a
schema version). They are compiled out of normal builds. Servers read test-friendly
timings (debounces, intervals) from environment variables.

Before each phase starts: state the phase plan in 5-10 lines and wait for the owner's OK.

---

## Phase 0: Setup (M0)

**Done when:** `<check>` is green locally on an empty app.

| ID | Goal | Deliverable | Tests first | Verify |
|---|---|---|---|---|
| T01 | Workspace root | Package manifest pinning runtime and package-manager versions | none | install from lockfile succeeds; runtime version matches |
| T02 | Strict types + shared package | Strict compiler config; shared package without platform APIs | none | typecheck passes; using a platform API in shared code fails it |
| T03 | Server skeleton + test runner | Health endpoint | integration: health returns 200 | `<test:int>` green |
| T04 | App skeleton + e2e runner | App shows its name; e2e config starts the real app and server | e2e `@M0`: app loads | `<e2e>` green |
| T05 | Lint and format | Type-aware lint, formatter, `<check>` script | none | lint + format pass |
| T07 | README + learning log | README run steps; LEARNING_LOG.md | none | a fresh clone follows the README to a running app |

## Phase 1: <Milestone 1 name> (M1)

**Done when:** <one sentence a test can check>.

| ID | Goal | Deliverable | Tests first | Verify |
|---|---|---|---|---|
| T10 | M1 acceptance test | `<file>`, expected-to-fail | e2e `@M1-done`: <steps> | reports expected failure |
| T11 | <…> | <…> | <…> | `<e2e> --grep @M1` green (except `@M1-done`) |
| … | | | | |
| T18 | Close M1 | `@M1-done` normal; all targets installed | none | `<check>` green; `<e2e:all> --grep @M1` green |

<More phases…>

## Phase N: CI (optional, often last)
CI that runs `<check>`, nightly cross-target runs, the stability gate in CI.
```

## Writing good task rows

- **Deliverable** names files, modules, endpoints, UI elements: someone could check it.
- **Tests first** says the layer and the behaviour, not the implementation:
  "integration: two clients converge; late joiner gets full state".
- **Verify** is a command plus an expected result.
- A task that is bigger than a sitting gets split. A task with no test gets a Verify
  that a person can still check (a fresh clone runs, a doc exists).
- Add tasks the owner asks for mid-phase with a free ID in that phase's range and a
  note in PROGRESS.md (e.g. "T19 Visual design baseline (added at the owner's request)").
