# Worked example: a TypeScript web app (pnpm, Vite/React, Vitest, Playwright)

Use this when a project's ADRs choose a similar stack. Everything here was learned
building a local-first collaborative notes app with a Node WebSocket sync server.

## Workspace (Phase 0)
- pnpm workspaces: `apps/web`, `apps/server`, `packages/shared` (no DOM or Node types,
  verified by a typecheck that fails if shared code touches `document` or `process`).
- Pin the runtime with `.nvmrc` + `engines`, the package manager with `packageManager`.
  If the machine's default runtime differs, run `nvm use` in every command; installing a
  new runtime can silently change nvm's default alias.
- `pnpm-workspace.yaml`: `minimumReleaseAge` (e.g. 3 days, avoids just-published
  malicious releases) and an `allowBuilds` allow-list (pnpm blocks install scripts; allow
  only what needs them, e.g. `esbuild`).
- Strict `tsconfig.base.json` (`strict`, `noUncheckedIndexedAccess`,
  `exactOptionalPropertyTypes`, `verbatimModuleSyntax`, `noEmit`). Some UI libraries'
  types do not compile with `exactOptionalPropertyTypes`: relax it for that package
  only and record it.
- Type-aware ESLint (`typescript-eslint` `strictTypeChecked`): catches floating promises
  (a forgotten `await` on a save). Check its supported TypeScript range before upgrading
  TypeScript majors. Useful options: `no-unused-vars` with `ignoreRestSiblings`,
  `no-confusing-void-expression` with `ignoreArrowShorthand` (for `onClick={() => set(x)}`).
- One `check` script: lint, format check, typecheck, Vitest, Playwright (Chromium).

## Vitest
- Projects: `unit` (`**/src/**/*.test.ts`) and `integration` (`apps/server/test/**/*.int.test.ts`).
- Integration tests start the real server in-process on port 0 (any free port), with a
  temp directory/database, and real clients (e.g. the stock `y-websocket` provider with
  BroadcastChannel disabled).
- Timers: inject a `Timers` interface (setTimeout/setInterval/clear/now) into the server
  and drive a manual clock in tests, instead of global fake timers (which also freeze
  timers inside networking libraries).
- `vi.waitFor` (or a small `eventually` helper) instead of sleeps.

## Playwright
- `use`: `actionTimeout: 10_000`, `navigationTimeout: 10_000`, `trace:
  "retain-on-failure"`, `serviceWorkers: "block"` by default (each fresh context would
  otherwise download the whole precache); offline tests opt in with
  `test.use({ serviceWorkers: "allow" })`.
- `webServer`: build in a `test` mode (adds `window.__app` test hooks, compiled out of
  normal builds via `import.meta.env.MODE`), then start the real server serving the
  build on one origin, with a fresh temp database per run and short save delays via env.
- Projects for chromium, firefox, webkit; `e2e` runs Chromium, `e2e:all` runs all three.
- A `bob` fixture for a second person: a new browser context, closed after the test,
  doubling the test's total budget (two browsers); per-step limits unchanged.
- Network control: `page/context.routeWebSocket(/\/sync\//, …)` to block/unblock sync
  sockets (close open ones on block). Whether `context.setOffline` cuts WebSockets
  differs by browser: spike it.
- Stub browser APIs on the prototype (`StorageManager.prototype.persist`), not the
  instance: WebKit can recreate wrapper objects and drop instance properties.
- Screenshots: a tagged spec run by `pnpm shots` (light/dark × desktop/phone) into a
  git-ignored folder; demos via `pnpm demo` (see showcase.md).
- Wait for app-exposed signals (`data-local-state="saved"`, `data-sync-state="synced"`),
  never for time.

## Web app patterns that paid off
- Expose state for tests as data attributes on the element users see.
- Native `<dialog>` + `showModal()` for modals (focus, Escape, inert background).
- Design tokens as CSS variables; dark mode redefines variables, not components.
- Security headers on every response: CSP with `script-src 'self'` (an e2e test records
  `securitypolicyviolation` events across the main flows and expects none), Referrer-
  Policy `no-referrer`, `nosniff`, HSTS only over TLS or from a trusted proxy.
