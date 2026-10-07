# Rollback

**Owner:** Azalea maintainers | **Last reviewed:** 2026-10-07 | **Status:** Current

Prefer rolling forward when the database schema is still compatible. Roll back code only if the
previous version supports the already-migrated schema. Restore a database backup only for
corruption or a destructive migration; writes since the backup are lost. Migrations are never
undone automatically.

| Target               | Steps                                                                                                                                                                                                 |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PM2 code             | Check out the known-good revision, run `bun install --frozen-lockfile --production`, then `(cd .. && pm2 reload ecosystem.config.js --only azalea --update-env)`.                                     |
| PM2 database         | Stop the bot, then `gunzip -c prisma/backups/azalea.db.<timestamp>.gz > prisma/azalea.db` (check this is the active `DATABASE_URL`). Start it and verify `/healthz` and bot behavior.                  |
| Docker code          | Check out the known-good revision and run `docker compose up -d --build`. The database is unchanged.                                                                                                 |
| Docker database      | Stop the service, restore the volume from your own backup, and start it again. There is no automated volume backup or restore.                                                                        |

Pause automatic deploys (CD on `main`) before restoring.
