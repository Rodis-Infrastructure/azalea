# 0002. Validated YAML guild configuration

**Status:** Inferred from implementation; current | **Date:** 2026-09-29 | **Deciders:** Not
recorded

> Reconstructed from the code and deployment files; the original rationale is not recorded.

## Context

The bot reads `azalea.cfg.yml` and one YAML file per guild from `configs/`. Zod schemas validate
them and apply defaults; `ConfigManager` caches the result at startup, and invalid config fails
startup.

## Decision (inferred)

Keep global and guild behavior in YAML, with `src/managers/config/schema.ts` as the validation
contract and `GuildConfig` as the runtime helper.

## Consequences

- Operators configure guild behavior without changing TypeScript.
- Validation and defaults are centralized and typed.
- Config errors block startup, and changes need a restart.
- YAML files contain operational IDs and must be access-controlled like the database.
- Revisit if operators need live editing or centralized config management.

## Source

`src/managers/config/schema.ts`, `src/managers/config/ConfigManager.ts`,
`src/managers/config/GuildConfig.ts`, `configs/`.
