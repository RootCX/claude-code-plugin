---
description: Deploy the current RootCX project to the connected Core — installs the manifest, uploads backend and frontend archives, and starts the worker.
---

The user wants to deploy the current project.

1. Verify `manifest.json` exists in the cwd. If not, tell the user to `cd` into the project directory.
2. Run: `rootcx deploy`

The CLI handles everything automatically:
- `bun install` + `bun run build` for the frontend (if `package.json` exists)
- `POST /api/v1/apps` with the manifest (install / update)
- Uploading `backend/` as tar.gz if present
- Uploading `dist/` as tar.gz if present
- Starting the worker if a backend was deployed

**Do NOT run `npm install`, `npm run build`, `bun install`, or `bun run build` manually — the CLI does it.**

Report each step to the user. If any step fails, show the exact error from the CLI output.

After a successful deploy, offer to:
- List installed apps: `rootcx apps`
- Invoke the agent (for agent projects): `rootcx invoke <app_id> "<message>"`
