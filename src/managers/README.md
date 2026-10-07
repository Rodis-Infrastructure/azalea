# Managers

Managers handle discovery and dispatch, not feature behavior:

- `commands/`: discovers commands and publishes global or guild-scoped definitions.
- `components/`: routes interaction custom IDs to handlers.
- `events/`: mounts gateway listeners and wraps their execution.
- `config/`: validates and caches global and guild YAML.

Startup order lives in [`index.ts`](../index.ts); see [Discord runtime](../../docs/components/discord-runtime.md) before changing it.
