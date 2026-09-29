# 0002. Validated YAML guild configuration

**Status:** Inferred from implementation; current | **Date:** 2026-09-29 | **Deciders:** Not
recorded

> This ADR reconstructs the current design from the code and deployment files. It does not claim to
> recover the original decision or its historical rationale.

## Context

The bot reads `azalea.cfg.yml` and one YAML file per guild from `configs/`. Zod schemas validate the
parsed structures and provide defaults. `ConfigManager` caches config during startup; invalid config
is a startup failure.

## Decision (inferred)

Keep global and guild-specific behavior in YAML files, with `src/managers/config/schema.ts` as the
validation contract and `GuildConfig` as the runtime helper. Treat schema and documentation changes
as part of a config-field change.

## Consequences

- Operators can review/configure guild behavior without changing TypeScript.
- Schema validation and defaults are centralized and typed from Zod.
- Config errors can prevent startup; changes generally require a restart and coordinated deployment
  of compatible config/code.
- YAML files contain operational IDs and messages and must be access-controlled with the database
  and backups.
- Revisit if operators need live editing, centralized secret/config management, or stronger config
  versioning.

## Source

`src/managers/config/schema.ts`, `src/managers/config/ConfigManager.ts`,
`src/managers/config/GuildConfig.ts`, and `configs/`.
