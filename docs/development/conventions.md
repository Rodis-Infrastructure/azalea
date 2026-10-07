# Conventions

## Structure

| Path                   | Purpose                                                     |
|------------------------|-------------------------------------------------------------|
| `src/commands/`        | Slash and context-menu command classes                      |
| `src/components/`      | Button, select-menu, and modal handlers                     |
| `src/events/`          | Discord gateway event listeners                             |
| `src/managers/`        | Discovery and dispatch for the above; config loading        |
| `src/utils/`           | Shared helpers, moderation persistence, logging, Sentry, health |
| `prisma/`              | Prisma schema and migrations                                |
| `configs/`             | One YAML config per guild ID                                |
| `tests/`               | Bun tests, one `.test.ts` file per area                     |

Command, event, and component files use PascalCase; utility files use camelCase.

## Code style

ESLint (`eslint.config.js`) enforces style: tabs, double quotes, semicolons, no trailing commas. Run
`bun run lint` or `bun run lint:fix`; there is no separate formatter. TypeScript runs in strict
mode. Give exported functions and public methods explicit return types, and use the `@utils/`,
`@managers/`, and `@/` path aliases where the lint rule requires them.

## Patterns

- Extend `Command`, `GuildCommand`, `Component`, or `EventListener` so the managers discover and
  wire the class.
- Read guild behavior from `GuildConfig`, not ad hoc YAML.
- Don't add environment variables, config keys, or database columns without updating their schema
  and docs.
- Log through `Logger`; capture unexpected failures with the Sentry helpers using source tags and
  non-sensitive context. Never log tokens, keys, or unnecessary message content.
- Use `runWithRequestContext` at new event/cron boundaries so context follows logs.
- Avoid fire-and-forget promises unless a helper owns the failure handling.
- Durations are milliseconds unless documented otherwise; `MuteRequest.duration` is stored in
  seconds.
