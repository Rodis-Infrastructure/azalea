# 0001. Single-process bot runtime

**Status:** Inferred from implementation; current | **Date:** 2026-09-29 | **Deciders:** Not
recorded

> Reconstructed from the code and deployment files; the original rationale is not recorded.

## Context

The application runs one Bun process with a discord.js client, Prisma over a local SQLite file, and
in-process cron jobs. Production uses PM2 or a single Docker Compose service. There is no external
queue, distributed lock, or multi-instance coordination.

## Decision (inferred)

Run one active instance per bot/database, with in-process scheduled work.

## Consequences

- Small operational footprint: one process, one local database.
- Restarts interrupt in-memory work; the message cache is flushed on shutdown, but there is no
  durable job queue.
- SQLite backup/restore must be coordinated with the running process.
- Multiple instances would duplicate cron actions and contend on local state.
- Revisit if availability or throughput requires multiple workers or hosts.

## Source

`src/index.ts`, `src/events/Ready.ts`, `src/utils/messages.ts`, `prisma/schema.prisma`,
`docker-compose.yml`, `scripts/deploy.sh`.
