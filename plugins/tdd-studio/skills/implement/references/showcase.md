# Showcase: README media and polish (optional)

When the owner wants to show the project, produce media **from the real app** with a
committed script (so it can be re-recorded after UI changes), and a README that explains
the project honestly.

## Recording
- Prefer in-repo tooling over system installs. For browser apps: Chromium's DevTools
  screencast streams timestamped frames; encode GIFs with a small library (e.g. `gifenc`
  + `pngjs`), merging identical consecutive frames (≈1 MB for a 10-second demo).
- One script, one command (e.g. `pnpm demo`), excluded from normal test runs by a tag.
- **Make the demo assert its result** (final text, statuses). A recording that "ran"
  can still show the wrong thing; the first recordings in practice showed text typed
  into the wrong note and text inserted mid-word.
- Save evenly spaced preview frames as PNGs next to the GIF and **look at them**; most
  viewers only show a GIF's first frame.
- Make demos human-paced (typing delay ~35 ms, short pauses) and deterministic (wait for
  signals, not sleeps, except for pacing).
- Stills: capture light and dark; on GitHub use `<picture>` with
  `prefers-color-scheme` sources.
- Seed identities and quiet incidental banners (e.g. stub a "storage not persistent"
  warning) so the media shows the feature, not noise. Never fake the feature itself.

## README structure
1. Logo/name, one-line pitch, hero image.
2. Status line (milestones done, what is next).
3. "What it does": each headline capability with a GIF and two sentences.
4. "And also": smaller features as bullets.
5. Honest statements the ADRs require (privacy, what is not protected).
6. "How it works": a plain-text diagram and 3-5 key ideas, linking ARCHITECTURE.md/ADRs.
7. "Built with": a small table.
8. "How it is tested": the method and real counts (run the commands to get them).
9. Run locally; other commands; roadmap; repo layout; docs links.
Keep personal data out of public READMEs.
