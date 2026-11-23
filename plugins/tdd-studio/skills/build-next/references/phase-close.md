# Closing a milestone (the last task of a phase)

1. **Done-when test is a normal test.** It was switched in the task that made it pass
   ("expected to fail, but passed" is good news). If it still needs `test.fail`, the
   milestone is not done.
2. **Full check green** locally.
3. **Every target green, with repeats:** all browsers/platforms the plan names, run at
   least twice for the milestone's tags. Investigate every failure: a rare flake is
   either fixed or recorded as an open question with its evidence and an owner (e.g. the
   next phase's spike). Note machine load if timings look off.
4. **Stability gate** if the plan defines one (e.g. 5 consecutive passes).
5. **Docs:** README status line; PROGRESS.md "Phase N: complete (date)"; next action
   "State the Phase N+1 plan in 5-10 lines and wait for the owner's OK"; learning log
   for the phase's big lessons.
6. **Commit** `chore(T38): close M2; all M1 and M2 tests green in <targets>`, push.
7. **Report to the owner:** what the milestone delivers in user terms, decisions made
   without asking (with where they are recorded), open questions, then the next phase
   plan, and wait.
