# Data flow

## Request lifecycle

Example: a staff member runs a moderation slash command.

1. discord.js emits `InteractionCreate`; `src/events/InteractionCreate.ts` dispatches it to the
   matching command, context-menu, modal, or component handler.
2. Discord has already enforced the command's native permission default (`ManageGuild` unless
   overridden). The handler validates options and checks any Azalea role permissions.
3. The handler acts through discord.js; shared moderation utilities persist infraction, request, or
   report state through Prisma and write event logs.
4. The handler replies. Non-ephemeral responses may be deleted after the guild's `response_ttl`.

## Data stores

| Store                   | Contents                                                                                             | Retention                                                                                                       | Backup                                                                             |
|-------------------------|------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| SQLite (`DATABASE_URL`) | Infractions, requests, reports, cached messages, reminders, temporary records, lockdown state, highlights | Cached messages: `database.messages.ttl` (default 7 days). Reports: optional TTL. Others: no general expiry. | PM2 deploy backs up `prisma/azalea.db` only; Docker volume backups are not automated |
| YAML files              | Global and per-guild configuration                                                                   | Until changed                                                                                                   | Not covered by the SQLite backup                                                   |
| Sentry (optional)       | Errors, traces, profiles, cron check-ins                                                             | Sentry project settings                                                                                         | Managed by Sentry                                                                  |

## Async work

The `Ready` handler starts cron jobs for message caching/expiry, temporary role/message cleanup,
review reminders, report removal, scheduled messages, and DM-pause renewal. Reminders are reloaded
from the database at startup and fired with in-process timers. A failed cron tick is logged and
captured; there is no retry queue. Shutdown flushes the message cache and disconnects Prisma.
