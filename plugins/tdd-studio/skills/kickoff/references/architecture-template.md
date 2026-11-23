# ARCHITECTURE.md outline

Audience: someone who missed every discussion (including the owner, two weeks later).
Style: a system-design interview answer. Plain-text diagrams, tables, numbered flows.
Mention libraries in passing; explain the design.

```markdown
# <Project>: architecture

## 1. What we are building
Requirements in 6-10 bullets: functional, then non-functional (offline, latency,
durability, privacy, scale target).

## 2. Vocabulary
Table of terms used below (domain terms and technical ones), one line each.

## 3. The big picture
A plain-text diagram of the components and how they talk. One paragraph per component.

## 4. <The core technique>
How the hard part works (e.g. how merging works, how scheduling works), with a small
worked example. Deep enough that a reader could rebuild it.

## 5. Data model
### 5.1 Inside the <unit> (e.g. a note's document)
### 5.2 Server storage (tables: name, columns with types and meaning, indexes)
### 5.3 Client storage
Say which data is derived (rebuildable cache) and which is not (must never be dropped).

## 6. Interfaces
### 6.1 Realtime/protocol: connect URL, handshake checks in order, message types, who
may send what, close/error codes with the client's reaction
### 6.2 REST/RPC: table of method + path, who may call it, purpose, response; error codes

## 7. Server internals
Lifecycle of the main in-memory objects, robustness rules (races, dead connections,
slow consumers), durability and save correctness.

## 8. Flows
Numbered step-by-step walkthroughs: the happy path for each core journey, then the
hard ones (offline then reconnect, revoked access, deletion, upgrade).

## 9. Failure modes
Table: what fails, what the user sees, what is lost (if anything), recovery.

## 10. Scale and limits
Current limits with numbers, where it breaks first, how you would scale later.

## 11. Security and privacy
Threats considered, mitigations, what is explicitly not protected.

## 12. Open items
Questions for spikes or later ADRs.

## 13. ADR index
| ADR | Topic | Decision (one line) | Status |
```

Keep it current: when an ADR is amended, update the affected sections the same day.
