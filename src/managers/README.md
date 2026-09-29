# Managers

Managers own application-wide discovery and dispatch, not feature behavior:

- `commands/` discovers command classes and publishes global or guild-scoped command definitions.
- `components/` resolves interaction custom IDs to handlers.
- `events/` mounts gateway listeners and wraps their execution.
- `config/` validates YAML and provides cached global and guild configuration.

Startup sequencing is centralized in [`index.ts`](../index.ts). Preserve that lifecycle when changing cache or mount requirements. Update the relevant [runtime](../../docs/components/discord-runtime.md) or [configuration](../../docs/components/configuration.md) documentation when a contract changes.
