# Core REST API — Jobs

Async job queue managed by Core. Workers enqueue jobs via REST, Core dispatches them back to the worker via IPC.

Base: `/api/v1/apps/{app_id}/jobs`

| Method | Path | Body | Response |
|--------|------|------|----------|
| POST | `/` | `{payload:{...}}` | `{job_id}` (201) |
| GET | `/` | — | `Job[]` (query: `status`, `limit`) |
| GET | `/{job_id}` | — | `Job` |

**Job statuses:** `pending` → `running` → `completed` | `failed`

**Flow:**
1. Worker (or frontend) enqueues: `POST /api/v1/apps/{app_id}/jobs` with `{payload:{...}}` + `Authorization: Bearer {authToken}`
2. Core scheduler claims pending jobs and dispatches to worker via IPC: `{ type: "job", id, payload, caller }` — `caller` has `userId`, `username`, `authToken` (short-lived JWT minted by Core from the enqueuing user)
3. Worker processes and responds: `{ type: "job_result", id, result }` or `{ type: "job_result", id, error }`
4. Use `caller.authToken` in job handlers for authenticated Core API calls (collections, integrations, etc.)

Use jobs for long-running work (bulk fetches, batch imports, async syncs) that would exceed the 30s RPC timeout.

## Core REST API — Crons

Scheduled jobs via pg_cron. Crons fire → pgmq → scheduler → worker `onJob`.

Base: `/api/v1/apps/{app_id}/crons`

| Method | Path | Body | Response |
|--------|------|------|----------|
| POST | `/` | `{name, schedule, payload?, timezone?, overlapPolicy?}` | `CronSchedule` (201) |
| GET | `/` | — | `CronSchedule[]` |
| PATCH | `/{id}` | `{schedule?, payload?, overlapPolicy?, enabled?}` | `CronSchedule` |
| DELETE | `/{id}` | — | `{message}` |
| POST | `/{id}/trigger` | — | `{msgId}` |

**schedule:** 5-field cron (`min hr dom mon dow`) or `"N seconds"` interval (`"10 seconds"`, 1-59). `$` in dom = last day of month. All times GMT unless timezone set. **overlapPolicy:** `"skip"` (default) or `"queue"`. **enabled:** toggle on/off without deleting.

Cron payload arrives in worker `onJob(payload, caller, ctx)` — `payload` contains the cron's configured payload + `cron_id`.
