# Conventions

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

## Structure

| Path                       | Purpose                                                         |
|----------------------------|-----------------------------------------------------------------|
| `src/commands/`            | Slash and context-menu command classes                          |
| `src/components/`          | Buttons, selects, and modal interaction handlers                |
| `src/events/`              | Discord gateway event listeners                                 |
| `src/managers/commands/`   | Command discovery, publication, and dispatch                    |
| `src/managers/components/` | Component discovery and dispatch                                |
| `src/managers/events/`     | Event listener discovery and mounting                           |
| `src/managers/config/`     | Config schemas, loading, defaults, guild runtime helpers        |
| `src/utils/`               | Shared helpers, moderation persistence, logging, Sentry, health |
| `prisma/`                  | Prisma schema and ordered SQL migrations                        |
| `configs/`                 | One YAML configuration per Discord guild ID                     |
| `tests/`                   | Bun tests, one `.test.ts` file per area                         |

Command, event, and component files use PascalCase. Utility filenames use camelCase.

## Code style

ESLint is configured in `eslint.config.js`; TypeScript settings and aliases are in `tsconfig.json`.
Use tabs, double quotes, and semicolons, consistent with nearby code. ESLint forbids trailing
commas. `bun run lint` checks the tree and `bun run lint:fix` applies safe ESLint fixes. There is no
separate formatter command.

TypeScript strict mode is enabled. Add explicit return types to exported functions and public
methods; use `@utils/`, `@managers/`, and `@/` aliases instead of relative paths where the lint rule
requires them.

## Commit messages

Use Conventional Commits (`feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`, or `ci:`).
The root `.gitmessage` provides a subject/body/footer prompt; enable it per repository with
`git config --local commit.template .gitmessage`.

## Patterns to follow

- Extend the relevant base class (`Command`, `GuildCommand`, `Component`, or `EventListener`) so the
  manager can discover and wire it.
- Keep guild-specific behavior driven by the validated `GuildConfig` rather than reading YAML ad
  hoc.
- Use Prisma's generated client and update `prisma/schema.prisma` plus a migration when changing
  persistent data.
- Capture unexpected operational errors through the existing logger/Sentry helpers with useful
  source tags and non-sensitive context.
- Use `runWithRequestContext` in new event/cron boundaries when context should follow emitted logs.

## Patterns to avoid

- Do not add undeclared environment variables, config keys, or database columns without updating
  their source-of-truth schema and documentation.
- Do not log tokens, API keys, raw credentials, or unnecessary private message content.
- Avoid fire-and-forget promises except where a documented helper owns failure handling; unhandled
  rejections are reported globally but should not be normal control flow.
- Do not modify an accepted ADR in place; supersede it if the architectural decision changes.

## Error handling and logging

Use `Logger` for operational messages and Sentry capture helpers for unexpected failures when
available. Event and cron managers wrap callbacks and report errors so one handler/tick failure does
not become an unhandled rejection. Expected Discord API misses may be handled locally; do not
silently suppress unexpected failures. Sentry scrubs configured secret environment values and
removes URL query strings from breadcrumbs, but this is defense in depth: do not include secrets in
logs or error context in the first place.

Time values in code/config are milliseconds unless explicitly documented otherwise; the
`MuteRequest.duration` database field is stored in seconds.
