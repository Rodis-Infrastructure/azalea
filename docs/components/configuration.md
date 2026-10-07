# Configuration subsystem

`ConfigManager` reads the global and guild YAML files, validates them with Zod, and caches
`GuildConfig` instances for commands, listeners, logging, and scheduled tasks. The field reference
is [configuration and secrets](../operations/config.md); the rationale is
[ADR 0002](../architecture/decisions/0002-yaml-guild-configuration.md).

## Files

- `azalea.cfg.yml`: global config, validated by `globalConfigSchema`.
- `configs/<guild_id>.yml`: guild config, validated by `rawGuildConfigSchema`
  (`src/managers/config/schema.ts`). `configs/example.yml` is ignored.
- `src/managers/config/GuildConfig.ts`: runtime helpers and cron setup.

Startup exits if `azalea.cfg.yml` is missing, `configs/` has no `.yml`/`.yaml` file, or any file
fails validation. A file for a guild the bot can't fetch is skipped with a warning.

## Changing a field

1. Edit the schema in `schema.ts` and any `GuildConfig` behavior that uses it.
2. Update representative YAML and config tests.
3. Update [operations/config.md](../operations/config.md). Don't put real production IDs or
   credentials in examples.
