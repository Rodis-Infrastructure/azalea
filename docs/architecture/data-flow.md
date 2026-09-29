# Data flow

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

## Request lifecycle

Example: a staff member invokes a moderation slash command.

1. Discord sends the interaction over the gateway; discord.js emits `InteractionCreate`.
2. `src/events/InteractionCreate.ts` identifies the interaction type and dispatches commands,
   context menus, modals, or components through the matching manager.
3. Discord applies the command's registered native-permission default (currently `ManageGuild`
   unless overridden). The handler validates options and applies any feature-specific Azalea role
   permission and runtime Discord permission checks.
4. The command executes using discord.js. Shared moderation utilities coordinate common infraction
   behavior and persistence.
5. Prisma writes infraction, request, or report state to the configured SQLite file. Event logging
   and optional Sentry capture happen along the relevant path.
6. The handler responds to Discord. Temporary/non-ephemeral responses may be deleted according to
   guild `response_ttl`.

## Data stores

| Store                   | Contents                                                                                                             | Retention                                                                                                                                             | Backup                                                                                                                        |
|-------------------------|----------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| SQLite (`DATABASE_URL`) | Infractions, requests, reports, message metadata/cache, reminders, temporary records, lockdown state, and highlights | Feature-specific; cached messages use `database.messages.ttl` (default 7 days); reports may use configured TTLs; other records have no general expiry | PM2 deploy script backs up `prisma/azalea.db` only; Docker uses a named volume and has no automated backup in this repository |
| YAML files              | Global and per-guild configuration                                                                                   | Until changed or removed                                                                                                                              | Back up with deployment/config management; these files are not included in the SQLite backup                                  |
| Sentry (optional)       | Error, trace, profile, and cron-monitoring events                                                                    | Governed by Sentry project settings                                                                                                                   | Managed by Sentry, not this application                                                                                       |

## Async work

Cron jobs are registered in-process after the Discord client is ready. They cover message
caching/expiry, temporary role/message cleanup, reminders, scheduled messages, and configured
review/retention tasks. Sentry cron instrumentation is used when Sentry is enabled. A job failure is
logged/captured and later ticks can run; there is no durable retry queue or dead-letter store.
Process shutdown flushes message cache and disconnects Prisma.
