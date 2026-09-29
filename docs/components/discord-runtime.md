# Discord runtime

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

## Role

The bot process uses discord.js to connect to Discord, publish application commands, receive gateway
events, and send API actions/replies. Bun runs TypeScript directly; this project does not compile a
separate runtime bundle.

## Versions and entry points

- Bun version is pinned by `.bun-version` (`1.3.14` at review time); Docker uses the corresponding
  pinned Bun image.
- `discord.js` is declared as `^14.26.4`.
- `src/index.ts` constructs the health server, Prisma client, and Discord client.
- Commands, event listeners, and interactive component handlers are discovered from `src/commands/`,
  `src/events/`, and `src/components/`.

## Runtime lifecycle

Startup order in `src/index.ts` is significant: the loopback `/healthz` endpoint and shutdown
handlers start at module load; then `main()` validates `DISCORD_TOKEN`, initializes optional Sentry,
caches components, logs into Discord, loads global and guild configs, caches commands, mounts event
listeners, publishes commands, and emits `ClientReady`. That ready handler starts in-process cron
work and marks health ready. Shutdown stores queued messages, destroys the Discord client,
disconnects Prisma, and flushes Sentry.

## Permissions and operation

Discord application scopes, gateway intents, bot permissions, role hierarchy, and channel overwrites
must match enabled features. Commands default to Discord's `ManageGuild` permission unless
explicitly overridden. Selected actions/components also check per-guild Azalea role permissions;
this is a separate permission system. See [commands](../commands.md)
and [configuration](../operations/config.md).

One unhandled request or event should not normally terminate the process: managers report handler
errors, while startup failures exit non-zero. Unexpected global promise/exception handlers log and
capture errors. Do not assume a caught API failure means the requested action succeeded.

## Testing

Unit tests do not connect to a live guild. Validate gateway/API behavior using a dedicated bot and
test guild. Keep the bot token out of test fixtures and CI logs.

## Links

- [discord.js documentation](https://discord.js.org/)
- [Architecture overview](../architecture/overview.md)
- [Bot unavailable runbook](../operations/runbooks/bot-unavailable.md)
