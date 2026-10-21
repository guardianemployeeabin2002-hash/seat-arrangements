# AGENTS.md

## Architecture
- `public/index.html`: the whole UI, in plain HTML/CSS/JS with no bundler. Seed data (`D`, `C`, `ALG`, `NEWS`, `MOVES`, `UP`) builds the initial directory through `seed()`.
- `netlify/functions/state.mts`: `GET /api/state` returns `{state, site, me}`. `PUT` saves `{state, site}` and only works for manager emails.
- `netlify/functions/identity.mts`: an Identity `userValidate` handler that blocks signups from emails that aren't managers.
- `netlify/lib/managers.mjs`: the list of manager emails (env `MANAGER_EMAILS` overrides it).
- `netlify/database/migrations/`: hand-written SQL. Table `app_state(key, data jsonb, updated_by, updated_at)`, with rows `state` and `site`.

## Notes
- The client loads `@netlify/identity` from esm.sh in a module script and exposes it as `window.NI`. The main classic script waits on `niReady`.
- The whole directory is saved as one JSON document, and the last save wins. If `state` is empty in the database, everyone sees `seed()` until a manager saves.
- localStorage is only an offline cache. The server is the source of truth.
- The client hides edit controls unless `me.manager`, but the real check happens on the server in `state.mts`.
- Never edit an applied migration. Add a new one instead.
