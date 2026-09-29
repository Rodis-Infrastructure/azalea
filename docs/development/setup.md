# Local setup

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

## Prerequisites

| Tool                | Version                        | Install                                                                 |
|---------------------|--------------------------------|-------------------------------------------------------------------------|
| Bun                 | `1.3.14` (from `.bun-version`) | [bun.sh](https://bun.sh/)                                               |
| Node.js             | 22+                            | [nodejs.org](https://nodejs.org/)                                       |
| Git                 | Current                        | [git-scm.com](https://git-scm.com/)                                     |
| Discord application | Bot token and a test guild     | [Discord Developer Portal](https://discord.com/developers/applications) |

## Steps

1. Clone the repository and enter the project directory.
2. Run `bun run setup` to install the lockfile-pinned dependencies and generate Prisma Client.
3. Copy `.env.example` to `.env`. Replace the `DISCORD_TOKEN` placeholder with a development bot
   token and set `DATABASE_URL` to a writable SQLite path such as `file:data/azalea.db`. Remove or
   blank optional `SENTRY_DSN`, `ROVER_API_KEY`, and `VIRUSTOTAL_API_KEY` placeholders when those
   integrations are not configured. Create the database parent directory first with `mkdir -p data`.
4. Confirm `azalea.cfg.yml` exists and add or adapt `configs/<guild_id>.yml` for a guild the bot can
   access. This checkout may already contain guild configs; use only IDs/configuration appropriate
   for your test guild, and do not copy production credentials or private operational settings into
   development.
5. Apply migrations with `bun run db:migrate`.
6. Run `bun start`. The bot logs its Discord identity and attempts to publish commands. Optional
   integrations can be left unset; without `SENTRY_DSN`, errors are logged locally and not sent to
   Sentry.
7. Run `curl http://127.0.0.1:7475/healthz` to check the local health endpoint. It reports
   `ready: true` after Discord login and startup initialization.

## Common first-day failures

| Symptom                                    | Cause                                                                                              | Fix                                                                                                       |
|--------------------------------------------|----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| Startup says `No token provided`           | `DISCORD_TOKEN` is unset or `.env` was not loaded by your Bun invocation/environment               | Set it in `.env` or export it before running `bun start`.                                                 |
| Global config or guild configs are missing | `azalea.cfg.yml` is required, as is a `configs/` directory containing at least one YAML guild file | Create valid config files; the Zod validation error identifies invalid fields.                            |
| Database cannot be opened                  | SQLite parent path does not exist or is not writable                                               | Create the directory and ensure the runtime user can write there; check `DATABASE_URL`.                   |
| Bot logs in but commands/actions fail      | The bot lacks guild permissions, intents, channel access, or a configured permission role          | Check Discord Developer Portal intents, role hierarchy, channel overrides, and guild permission mappings. |
| Port 7475 is already in use                | Another process is using the default health endpoint                                               | Set `HEALTH_PORT` to another local port. Keep the service bound to loopback.                              |

See [troubleshooting](troubleshooting.md) for more diagnostic steps.
