# Hard-won rules

Each rule came from a real failure. The examples are from a TypeScript web app with a
WebSocket sync server, but the rules are general.

## Making tests honest

1. **Red for the right reason.** Read every failure message before implementing.
2. **A test you have never seen fail proves nothing.** Sabotage-check guards and
   negative assertions.
3. **Check absence with a presence check first.** "No Image item in the menu" passes if
   the menu never opened; assert a present item (Heading 1) first.
4. **Plant the thing that must not leak.** A traversal test needs a secret file outside
   the served folder, otherwise a leaking server passes.
5. **Send what an attacker would send.** High-level clients normalise input (`fetch`
   removes `..`, `%2e%2e`); use raw sockets/requests for hostile input.
6. **Prove "did not happen" with a barrier.** After the moment in question, make the
   sender send something else and wait for it; on an ordered channel, anything wrong
   would have arrived first.
7. **Hand-built protocol messages for permission tests.** Attackers do not use your
   client library.
8. **Disable shortcuts in tests that hide bugs.** E.g. same-process clients syncing via
   BroadcastChannel would pass with a broken server: turn it off in server tests.

## Diagnosing

9. **Probe before guessing.** Capture `console` and `pageerror`, dump the DOM or the
   message sequence, read the runner's error context/snapshot, isolate the step, repeat
   it. Write probes as temporary test files and delete them.
10. **Passes alone, fails together → shared load or shared state.** Look at machine load
    (`uptime`), parallel workers, per-test downloads (e.g. a service worker precaching
    megabytes in every fresh browser context). Reduce the shared cost.
11. **Fix causes, not timeouts.** Only raise a total time budget when the cause is
    external and per-step limits remain; say so in the decisions log.
12. **Rule out the app before blaming it, and vice versa.** A demo typing text mid-word
    looked like a collaboration bug; probes showed the app was fine and the script
    clicked mid-word. Keep the probe as a permanent test if it covers a core scenario.
13. **Read the library's real class names / API from its source or docs.** Search
    patterns can cut off prefixes (`collaboration-cursor__label` vs
    `bn-collaboration-cursor__label`).

## Changing code safely

14. **Scripted edits must assert they matched.** Formatters re-wrap lines; a find-and-
    replace that silently matches nothing leaves old code in place (and makes sabotage
    checks meaningless). Prefer exact edit tools; in scripts, fail on no match.
15. **Libraries vs strict settings.** When a strict compiler flag breaks a library's own
    types, scope the relaxation to that package and record it; do not litter casts.
16. **Closures and narrowing.** Type checkers assume a flag checked before an `await`
    keeps its value; read cancellation flags through a function.
17. **CSS scoped by containment leaks into dialogs/portals rendered inside that
    container.** Use dedicated classes for layout tweaks.

## Protocol and data

18. **Order matters across independent channels.** Content and presence (cursors) are
    separate streams; a cursor arriving before its content crashed the editor's cursor
    plugin. Send content first.
19. **State vectors miss deletions.** "Covered" checks need the delete set too (a
    snapshot), or delete-only changes look synced.
20. **Clear the dirty flag before encoding** a save, and send acknowledgements only after
    the write returns.
21. **Do not rebuild a live editor to fix a race**: it drops what the user just typed.
    Fix ordering at the source.
22. **Read-only is not "no writes" for every library.** An editor deleted unknown node
    types from the shared document on render, even when not editable; bind it to a
    disposable copy.

## Working with the owner

23. **Ask before destructive or outward-facing actions** (force push, deleting data,
    publishing, installing system software); prefer in-repo tools over system installs.
24. **When the owner pauses to ask questions, answer from the code.** Read the actual code and answer
    with file:line references; offer the cleaner design if the question exposes a wart.
25. **Report honestly.** Say what failed, what you could not verify ("suspected, not
    confirmed"), and what you changed in tests and why.
