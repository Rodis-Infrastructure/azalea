# Configuration subsystem

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

## Role

`ConfigManager` reads YAML global/guild files, validates them with Zod, applies defaults, and caches
`GuildConfig` instances used by commands, event listeners, logging, and scheduled tasks.

## Source of truth and files

- Global configuration: `azalea.cfg.yml`, validated by `globalConfigSchema`.
- Guild configuration: `configs/<guild_id>.yml`, validated by `rawGuildConfigSchema` in
  `src/managers/config/schema.ts`.
- Runtime configuration helpers and defaults: `src/managers/config/GuildConfig.ts`.
- Startup requires a `configs/` directory with at least one `.yml` or `.yaml` file. Invalid
  YAML/schema fails startup; a file for a guild the bot cannot fetch is skipped and does not provide
  usable guild config.

Use the [configuration and secrets reference](../operations/config.md) for environment variables,
config sections, defaults, and permissions. YAML IDs are Discord snowflakes. Durations use
milliseconds unless a field explicitly says otherwise.

## Scoping and access

Channel/role include and exclude rules are validated to prevent the same ID appearing in both. Guild
permission mappings grant Azalea feature permissions to specified roles; they do not grant native
Discord permissions. `manage_guild_config` is for the sibling editor service and is not checked by
the bot itself.

## Change procedure

Add/modify schema fields in `schema.ts`, update the inferred types and any `GuildConfig` behavior,
change representative YAML as appropriate, and add/update config tests.
Update [operations/config.md](../operations/config.md) for the user-facing field reference. Validate
all local config with `bun run test` and the CI schema checks; do not include real production IDs or
credentials in generic examples.

The implementation-derived rationale and tradeoffs are recorded
in [ADR 0002](../architecture/decisions/0002-yaml-guild-configuration.md).
