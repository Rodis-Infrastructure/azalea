# Runbook: Database or migration failure

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

## Symptoms

Startup or commands report Prisma errors, SQLite cannot open the database, migration deployment
fails, or database corruption is suspected.

## Impact

Persistent moderation state and features that rely on SQLite may be unavailable. Avoid repeated
deployment attempts until the failing migration and DB path are understood.

## Diagnosis

1. Record the exact Prisma/SQLite error and deployment revision.
2. Confirm the active `DATABASE_URL` points to a writable file and its parent directory exists.
3. Check filesystem permissions and free disk space.
4. Compare the database migration history with `prisma/migrations/`; do not edit migration history
   or delete the database as a diagnostic shortcut.
5. Identify the active DB path. PM2 backup script targets `prisma/azalea.db`; Docker stores the DB
   in the `data` named volume.

## Mitigation

Pause deployment and preserve a copy of the database before repairs. For a migration defect, prefer
a reviewed forward fix unless rollback compatibility has been established. Restore a verified backup
only if required; restoration discards writes made after the backup. See [rollback](../rollback.md).

## Verification

Run migrations against the intended DB, start the app, check `/healthz`, and verify representative
read/write moderation workflows using a controlled guild.

## Escalation

Involve the maintainer before manual schema edits or data restoration. Treat possible data
corruption/exposure as an incident.
