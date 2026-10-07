# Persistence

Prisma Client (pinned with `prisma` at `6.19.3`) provides typed access to SQLite. The schema is
`prisma/schema.prisma`, migrations are in `prisma/migrations/`, and `DATABASE_URL` selects the file.
Models cover ban/mute requests, cached messages, infractions, message/user reports, temporary
roles/messages, reminders, lockdown permission snapshots, and highlights. Snowflakes are generally stored as
strings.

## Changing the schema

1. Edit the schema and run `bun run db:format`.
2. Create a migration and commit it with the schema.
3. Run `bun run db:validate` and `bun run db:check` (migration/schema drift). CI also applies all
   migrations to a fresh database.

Both deploy paths run `prisma migrate deploy` before starting the bot. Migrations are forward-only;
back up the database before destructive changes and see [rollback](../operations/rollback.md).

## Paths and backups

Docker stores `file:data/azalea.db` in the `data` named volume at `/usr/src/app/data`, with no
automated backup. Host deployments set the path in `.env`; the PM2 deploy script backs up only
`prisma/azalea.db`, so make sure that's the active database.
