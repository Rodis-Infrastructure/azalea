# Deployment

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

Supported runtime deployment paths are PM2 on a long-running host and Docker Compose.
See [CI/CD](ci-cd.md) for automation and [rollback](rollback.md) for recovery considerations.

## PM2 host deploy

The CD workflow listens for successful CI runs on `main` or `master` and deploys via SSH. Its remote
script currently runs `git pull origin main` and then calls `scripts/deploy.sh` from the repository
root; deployments from a `master`-only setup would still pull `main` and require a workflow change.
The deploy script pins Bun from `.bun-version`, installs production dependencies, makes up to three
timestamped gzip backups of `prisma/azalea.db`, applies Prisma migrations, then runs
`pm2 reload ecosystem.config.js --only azalea --update-env` from the parent directory.

Required GitHub repository secrets: `SSH_HOST`, `SSH_USER`, and `SSH_KEY`; the workflow also passes
`SSH_PORT`. The host needs Bun, PM2, `~/Projects/azalea`, a parent-directory `ecosystem.config.js`,
and runtime environment variables including `DISCORD_TOKEN` and `DATABASE_URL`. `SENTRY_DSN` and
integration keys are optional.

Manual host deployment:

```sh
cd ~/Projects/azalea
git pull origin main
bash scripts/deploy.sh
```

**Database path caveat:** the PM2 script's backup source is hard-coded to `prisma/azalea.db`, while
`.env.example` and Docker use `file:data/azalea.db`. For PM2, either set `DATABASE_URL` to the
backed-up file or update the script's backup path and verify it before deploying. A backup of a
different path does not protect the active database.

## Docker Compose

```sh
docker compose up -d --build
docker compose logs -f bot
```

Compose builds from `Dockerfile`, reads `.env`, and mounts the `data` named volume at
`/usr/src/app/data`. The container starts with `prisma migrate deploy` followed by `bun start`,
running as the non-root `bun` user. The database persists across rebuilds in that volume; migrations
and schema are part of the image. Back up the volume separately—this repository does not configure
automatic Docker backups.

`docker compose down -v` deletes the named data volume and its database; do not use it for routine
upgrades.

## Ordering rules

Deploy code and matching migrations together. Both deploy paths apply migrations before the bot
starts (PM2 deploy script or Docker entrypoint). Prisma migrations are forward-applied; do not
assume an older application version can safely run against a newer schema. Back up the active
database before high-risk schema changes and document recovery.

## Post-deploy verification

- Confirm the process/container stays running and logs show Discord login and successful startup.
- Check `http://127.0.0.1:7475/healthz` locally on the host; `ready` should be `true`.
- Confirm the bot is online, commands are present, and a low-risk command works in the intended
  guild.
- Check application logs and Sentry (if enabled) for startup, Discord API, database, and cron
  failures.

No fixed observation window or deployment SLO is defined. Observe through at least the next relevant
scheduled job for changes affecting cron behavior.

## Risky changes

There is no feature-flag or canary framework in this repository. Test changes in a dedicated Discord
guild and separate database before production. For schema changes, prefer backward-compatible
expand/migrate/contract steps so the running or rollback version can tolerate the intermediate
schema.
