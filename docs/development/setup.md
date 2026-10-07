# Local setup

## Prerequisites

- Bun `1.3.14` (from `.bun-version`), Node.js 22+, and Git
- A Discord bot token and a test guild ([Developer Portal](https://discord.com/developers/applications))

## Steps

1. Clone the repository and run `bun run setup` (installs dependencies and generates Prisma Client).
2. Copy `.env.example` to `.env`, set `DISCORD_TOKEN` to a development bot token, and remove the
   placeholder values for integrations you aren't using (`SENTRY_DSN`, `ROVER_API_KEY`,
   `VIRUSTOTAL_API_KEY`).
3. Run `mkdir -p data` (for the default `DATABASE_URL`), then `bun run db:migrate`.
4. Make sure `azalea.cfg.yml` exists and add a `configs/<guild_id>.yml` for your test guild. Don't
   copy production IDs or settings.
5. Run `bun start`, then `curl http://127.0.0.1:7475/healthz`; it reports `ready: true` once startup
   finishes.

If something fails, see [troubleshooting](troubleshooting.md).
