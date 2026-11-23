# docs/LEARNING_LOG.md template

```markdown
# Learning log

Short, dated notes on what I learned, surprises, and mistakes. Newest first. Spike
findings are recorded here too.

---

## YYYY-MM-DD: <topic> (<task IDs>)

**<One-line lesson in bold.>** <Two to five sentences: what happened, why, what we do now.>
```

## What deserves an entry

- A surprise from a tool or library (a default that bit, a version incompatibility).
- A test that passed for the wrong reason, and how it was made honest.
- A bug found by looking (screenshots, a probe) rather than by a test.
- A spike's answer, including caveats ("checked in one process, not in real tabs").
- A mistake of process (e.g. code written before its test ran red) and its repair.

## Example entries

**A timeout is not a failure.** An expected-to-fail test waited 30 s for a missing button
and was reported "timed out", not "failed", so the run went red. Fix: per-action
timeouts turn a stuck step into an ordinary failure with a precise message.

**Proving that something did NOT happen needs a barrier.** Checking "no message arrived"
right away proves nothing; make the sender send something after the moment in question
and wait for it (messages on one connection arrive in order).

**Plant the thing that must not leak.** A path-traversal test had no secret file outside
the served folder, so a leaking server still "passed". Put a secret there, send raw
paths, and check it never appears.
