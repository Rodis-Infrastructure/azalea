# Runbook: Database or migration failure

**Owner:** Azalea maintainers | **Last reviewed:** 2026-10-07 | **Status:** Current

**Symptoms:** Prisma errors at startup or in commands, SQLite can't open the file, `prisma migrate
deploy` fails, or corruption is suspected. Don't retry deploys until the cause is understood.

## Diagnosis

1. Record the exact error and the deployed revision.
2. Find the active database path: `DATABASE_URL` on PM2 hosts, the `data` volume on Docker.
3. Check that the directory exists, is writable, and has free disk space.
4. Compare the applied migrations with `prisma/migrations/`. Don't edit migration history or delete
   the database to diagnose.

## Mitigation

Pause deploys and copy the database before any repair. Prefer a reviewed forward-fix migration.
Restore a backup only if necessary; see [rollback](../rollback.md).

## Verification

Migrations apply cleanly, `/healthz` reports ready, and representative read/write commands work in
a controlled guild.

Involve the maintainers before manual schema edits or restores, and treat possible corruption or
exposure as an [incident](../incident-response.md).
