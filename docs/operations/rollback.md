# Rollback

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

## Decision rule

Roll forward for application defects when the active database schema remains compatible and a safe
fix is available. Roll back code only when the previous code version supports the already-migrated
schema. Restore a database backup only when corruption or destructive migration effects make it
necessary; restoration loses writes made after that backup. Stop automated deploy/restart loops
before restoring state.

## Procedures

| Target               | Steps                                                                                                                                                                                                                                                                                 | Time to complete                            |
|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|
| PM2 application code | Deploy/re-checkout the known-good revision on the host and run `bun install --frozen-lockfile --production`; reload with `(cd .. && pm2 reload ecosystem.config.js --only azalea --update-env)`. Confirm schema compatibility before switching code.                                  | Depends on host/network and migration state |
| PM2 SQLite backup    | Stop/reload safely to avoid writes while restoring. Choose a verified archive in `prisma/backups/`, then `gunzip -c prisma/backups/azalea.db.<timestamp>.gz > prisma/azalea.db`; start/reload and verify `/healthz` and bot behavior. Confirm this path is the active `DATABASE_URL`. | Depends on database size                    |
| Docker application   | Rebuild/deploy a known-good image with `docker compose up -d --build`; verify logs and health. This does not roll back or restore database contents.                                                                                                                                  | Depends on build time                       |
| Docker SQLite volume | Stop the service and restore a separately created volume/database backup using the operator's backup procedure, then bring the service up and verify. This repository defines no automated volume-restore command.                                                                    | Operator-defined                            |

## Irreversible changes

Deployment uses `prisma migrate deploy` and does not automatically undo migrations. Treat
schema/data migrations as forward-only unless a tested reverse procedure exists. Back up the active
SQLite file/volume before destructive migrations; prefer additive, backward-compatible steps. The
PM2 script retains the newest three gzip backups of the fixed `prisma/azalea.db` path. Docker volume
backups must be arranged separately.
