# Guidelines

- For new DOM/browser API usage, use `app.ownerDocument` and `app.ownerWindow` instead of globals; without `app`, derive them from the mounted node's `ownerDocument` and its `defaultView`.
- When overriding properties of an existing type, prefer `Merge<Base, Overrides>` from `@excalidraw/common/utility-types` over `Omit<Base, keyof Overrides> & Overrides`.

## Base44 sandbox setup

- Run with `docker compose -f docker-compose.base44.yml up -d`. This uses `node:24` + the Vite dev server from `excalidraw-app/` on port 3000. The repo's own `docker-compose.yml`/`Dockerfile` build a production nginx image, so don't use them for development.
- On startup the container runs `yarn install --frozen-lockfile` (Yarn 1 workspaces). `node_modules` lives in the bind-mounted checkout and is gitignored. A cold install takes about 2 minutes, so wait for `VITE ... ready` in the logs.
- Env comes from the committed `.env.development` (public dev endpoints for json/libraries/firebase). No secrets are required. Collab WS (`localhost:3002`) and AI backend (`localhost:3016`) are not run, so live collaboration and AI features won't connect.
- Vite 5.0.12 does no Host checking, so no allowlist is needed. `BROWSER=none` stops `open: true` from trying to launch a browser.
- The dev server's checker plugin prints `ERROR [ESLint] Found 0 error` / `[TypeScript] Found 0 errors`. Those lines are normal and mean it's clean.
- Verify: `curl localhost:3000/` should serve `<div id="root">` with `src="index.tsx"`, and the canvas should render.
