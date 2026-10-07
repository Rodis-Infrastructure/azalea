# Deployment

**Owner:** Azalea maintainers | **Last reviewed:** 2026-10-07 | **Status:** Current

Production runs either under PM2 on a host or with Docker Compose. Both apply migrations before
starting the bot. See [rollback](rollback.md) for recovery.

## PM2 host

CD deploys automatically after CI passes (see [CI/CD](ci-cd.md)). Over SSH it runs
`cd ~/Projects/azalea && git pull origin main && bash scripts/deploy.sh`, which:

1. Installs the Bun version in `.bun-version` if it differs.
2. Runs `bun install --frozen-lockfile --production`.
3. Backs up `prisma/azalea.db` to `prisma/backups/` (gzip, newest three kept).
4. Runs `bun run db:migrate`.
5. Runs `pm2 reload ecosystem.config.js --only azalea --update-env` from the parent directory.

The host needs Bun, PM2, the repository at `~/Projects/azalea`, an `ecosystem.config.js` in the
parent directory, and the runtime environment variables. To deploy manually, run the same command on
the host.

The CD script always pulls `main`, even when triggered from `master`.

**Database path:** the backup step only copies `prisma/azalea.db`, while `.env.example` uses
`file:data/azalea.db`. On PM2 hosts, point `DATABASE_URL` at the backed-up file or update the
script; otherwise the backup doesn't cover the live database.

## Docker Compose

```sh
docker compose up -d --build
docker compose logs -f bot
```

Compose reads `.env` and stores the database in the `data` named volume at `/usr/src/app/data`.
The container runs as the non-root `bun` user and starts with `prisma migrate deploy && bun start`.
Volume backups aren't automated. `docker compose down -v` deletes the volume and the database.

## After deploying

- Logs show Discord login and no startup errors.
- `curl http://127.0.0.1:7475/healthz` on the host reports `ready: true`.
- The bot is online and a low-risk command works.
- For cron changes, watch the next scheduled run.

There are no feature flags or canaries; test risky changes in a separate guild and database first.
For schema changes, prefer additive steps the previous code version can still run against.
