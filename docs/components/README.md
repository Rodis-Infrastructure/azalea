# Components

| Component                   | Role                                                                         | Doc                                      |
|-----------------------------|------------------------------------------------------------------------------|------------------------------------------|
| Discord application runtime | Gateway client, command registration/dispatch, events, interactive components | [discord-runtime.md](discord-runtime.md) |
| Configuration               | YAML loading, Zod validation, defaults, guild scoping                        | [configuration.md](configuration.md)     |
| Persistence                 | Prisma, SQLite schema, migrations                                            | [persistence.md](persistence.md)         |
| Observability               | Logs, Sentry, cron instrumentation, readiness endpoint                       | [observability.md](observability.md)     |

For a new major dependency or subsystem, copy [the component template](../_templates/component.md).
