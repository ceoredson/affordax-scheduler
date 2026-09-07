# ADMP Scheduler

This public repository contains only GitHub Actions schedules for the private
ADMP-MW backend. It does not contain application source code or database
credentials.

## Repository secrets

Configure these secrets under **Settings → Secrets and variables → Actions**:

```text
MAINTENANCE_URL=https://your-app.onrender.com
MAINTENANCE_TOKEN=<same-long-token-configured on Render>
```

The backend requires the token in an `Authorization: Bearer` header and rejects
unauthorized or overlapping maintenance requests.

GitHub schedule times are UTC. The schedules intentionally avoid minute zero,
which reduces the chance of competing with the global start-of-hour workload.

Each workflow has its own concurrency group. A manual invocation and scheduled
invocation of the same workflow cannot run concurrently; an in-flight request
is never cancelled by a newer run. Backend locking remains necessary because
other schedulers or callers can invoke maintenance independently.
