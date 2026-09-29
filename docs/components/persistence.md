# Persistence

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

## Role and technology

Prisma Client provides typed access to a SQLite database. The schema is `prisma/schema.prisma`;
ordered SQL migrations live in `prisma/migrations/`. `DATABASE_URL` selects the SQLite file. Prisma
and `@prisma/client` are pinned to `6.19.3`.

## Stored records

Schema models cover ban/mute requests, cached messages, infractions, message/user reports, temporary
role/message records, reminders, permission overwrite snapshots, and highlights with
patterns/channel scoping. Discord snowflakes are generally stored as strings. Details and indexes
are defined in the Prisma schema.

## Migrations

For schema changes, edit and format the Prisma schema, create a migration using the project's
established Prisma workflow, and commit both schema and migration. CI validates the schema, applies
all migrations to a clean SQLite DB, and checks migration/schema drift.

Commands:

```sh
bun run db:generate
bun run db:validate
bun run db:format
bun run db:migrate
bun run db:check
```

Production applies committed migrations through `prisma migrate deploy` before bot start. Back up
the active database before destructive changes. Migrations are not automatically reversed;
see [rollback](../operations/rollback.md).

## Paths and backups

Docker's default `DATABASE_URL` is `file:data/azalea.db`, stored in a named volume mounted at
`/usr/src/app/data`. Host deployments choose the path in `.env`. The PM2 deploy script specifically
backs up `prisma/azalea.db`; ensure this matches the active DB or update the script. Docker backups
are not automated in this repository.

## Testing and links

Migration checks use an ephemeral SQLite database in CI. Do not use production database copies in
tests. See [data flow](../architecture/data-flow.md), [privacy](../policies/privacy.md),
and [database runbook](../operations/runbooks/database-failure.md).

For design constraints and the reason this deployment assumes one active instance,
see [ADR 0001](../architecture/decisions/0001-single-process-runtime.md).
