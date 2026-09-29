# 0001. Single-process bot runtime

**Status:** Inferred from implementation; current | **Date:** 2026-09-29 | **Deciders:** Not
recorded

> This ADR reconstructs the current design from the code and deployment files. It does not claim to
> recover the original decision or its historical rationale.

## Context

The application starts one Bun process, connects a discord.js client, uses Prisma with a local
SQLite database, and registers scheduled jobs inside that process. Production is documented for PM2
or a single Docker Compose service. No external queue, distributed lock, or multi-instance
coordination is present.

## Decision (inferred)

Treat Azalea as a single active bot process using a local SQLite file and in-process cron jobs. Run
one active instance per bot/database unless coordination and storage are deliberately redesigned.

## Consequences

- Runtime and scheduled work have a small operational footprint and share one process and local
  database.
- Process restarts interrupt in-memory work; message cache is flushed on shutdown, but there is no
  durable job queue.
- SQLite file access and backup/restore must be coordinated with the running process.
- Multiple active instances could duplicate cron actions and contend on local state; horizontal
  scaling is not a supported assumption.
- Revisit this decision if availability, throughput, or deployment requirements call for multiple
  workers or hosts.

## Source

`src/index.ts`, `src/events/Ready.ts`, `src/utils/messages.ts`, `prisma/schema.prisma`,
`docker-compose.yml`, and `scripts/deploy.sh`.
