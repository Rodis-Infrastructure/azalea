# Runbook: Deployment failure

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

## Symptoms

CI fails, CD SSH/deploy fails, PM2 does not reload, a Docker build/start fails, or the new process
does not become ready.

## Impact

The previous bot version may still be running, the service may be unavailable, or a new database
migration may have completed despite application startup failure.

## Diagnosis

1. Read the failed GitHub Actions job or remote deploy output; identify whether failure occurred
   before/after migration.
2. On PM2, inspect process logs/status and check Bun version, `ecosystem.config.js`, repository
   state, and runtime environment.
3. On Docker, inspect `docker compose ps` and `docker compose logs bot`; confirm `.env` and volume
   mount.
4. Check `/healthz` locally and compare its `startedAt` with the attempted reload.
5. Confirm the running code version supports the current database schema before rolling back code.

## Mitigation

Do not rerun a failing migration blindly. For an application-only failure with compatible schema,
deploy a known-good revision and reload. If data/schema is involved, preserve the database and
follow the [rollback procedure](../rollback.md).

## Verification

Confirm service readiness, Discord online state, commands, and database access. Review logs/Sentry
for errors and verify the production environment is using the intended database path.

## Escalation

Contact the repository/host maintainer. Use [incident response](../incident-response.md) for
extended outage, data loss, or security impact.
