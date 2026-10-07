# Runbook: Deployment failure

**Owner:** Azalea maintainers | **Last reviewed:** 2026-10-07 | **Status:** Current

**Symptoms:** CD fails over SSH, PM2 doesn't reload, the Docker build or container fails, or the new
process never becomes ready. The old version may still be running, or a migration may have applied
even though the app failed to start.

## Diagnosis

1. Read the failed GitHub Actions job or deploy output and note whether migrations ran.
2. PM2: check `pm2 logs azalea`, the Bun version, `ecosystem.config.js`, and the checkout state.
3. Docker: check `docker compose ps` and `docker compose logs bot`, `.env`, and the volume.
4. Compare `startedAt` from `/healthz` with the deploy time to see whether a new process started.

## Mitigation

Don't rerun a failing migration blindly. If only the code is broken and the schema is compatible,
deploy a known-good revision. If the schema or data is involved, back up the database and follow
[rollback](../rollback.md).

## Verification

`/healthz` reports ready, the bot is online, commands work, and the logs show the intended database
path.

Escalate to the maintainers, and use [incident response](../incident-response.md) for extended
outages, data loss, or security impact.
