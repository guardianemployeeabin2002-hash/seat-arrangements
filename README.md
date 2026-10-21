# Store Capsules

A directory of monitored stores grouped into "capsules". Each store has its owner, address, phone, camera app, employee allowance and a dated list of updates. Employees can browse and search without signing in. Only assigned managers can sign in and edit.

## Technology
- `public/index.html`: one static page with no build step (the original Store Capsules app)
- Netlify Database (Postgres): stores the shared directory and site settings
- Netlify Identity (`@netlify/identity`): manager email login
- Netlify Functions: `/api/state` (public read, manager-only write) and a signup guard

## Managers
Assigned manager emails are listed in `netlify/lib/managers.mjs`. You can override them with a comma-separated `MANAGER_EMAILS` environment variable. Only these emails can create an account, and only these accounts can save changes. The server checks this on every save.

First-time setup for a manager: open the site, tap **Manager login → Create account** with the assigned email, click the confirmation link in the email, then sign in.

## Run locally
```bash
npm install
netlify dev
```
