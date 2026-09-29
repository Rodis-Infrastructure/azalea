# Components

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

| Component                   | Role                                                                              | Owner              | Doc                                      |
|-----------------------------|-----------------------------------------------------------------------------------|--------------------|------------------------------------------|
| Discord application runtime | Gateway client, command registration/dispatch, events, and interactive components | Azalea maintainers | [discord-runtime.md](discord-runtime.md) |
| Configuration               | YAML parsing, Zod validation, defaults, and guild scoping                         | Azalea maintainers | [configuration.md](configuration.md)     |
| Persistence                 | Prisma ORM, SQLite schema, and migrations                                         | Azalea maintainers | [persistence.md](persistence.md)         |
| Observability               | Logs, Sentry, cron instrumentation, and readiness endpoint                        | Azalea maintainers | [observability.md](observability.md)     |

For a new major dependency or subsystem, copy [the component template](../_templates/component.md).
