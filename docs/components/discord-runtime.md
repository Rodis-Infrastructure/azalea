# Discord runtime

The bot uses discord.js (`^14.26.4`) to connect to Discord, publish application commands, receive
gateway events, and send API actions. Bun runs the TypeScript directly; there is no build step. Bun
is pinned by `.bun-version` (CI and the deploy script read it; the Dockerfile pins the same version).

## Startup and shutdown

Order in `src/index.ts` matters:

1. At module load: start the loopback `/healthz` server, register shutdown handlers, create the
   Prisma and Discord clients.
2. `main()`: check `DISCORD_TOKEN`, initialize Sentry if configured, cache components, log in, load
   global and guild configs, cache commands, mount event listeners, publish commands, emit
   `ClientReady`.
3. The `Ready` handler starts cron jobs and marks `/healthz` ready.

On shutdown the bot stores queued messages, destroys the client, disconnects Prisma, and flushes
Sentry. Startup failures exit non-zero; handler errors are logged and captured without stopping the
process.

## Permissions

Discord scopes, intents, bot permissions, role hierarchy, and channel overwrites must match the
enabled features. Commands default to Discord's `ManageGuild` permission; some actions also check
Azalea's per-guild role permissions. See [commands](../commands.md).

## Testing

Unit tests don't connect to Discord. Verify gateway/API behavior with a dedicated bot and test
guild, and keep the token out of fixtures and CI logs.
