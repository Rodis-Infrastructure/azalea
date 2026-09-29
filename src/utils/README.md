# Shared utilities

This directory contains helpers reused across handlers, including persistence workflows, message formatting/cache, logging, request context, secrets handling, and observability. Put behavior with domain state in its focused module (for example, infractions or reports) rather than duplicating it in commands and events.

Helpers that access the shared Discord client or Prisma client are process-level operations, not pure utilities. Keep those dependencies and side effects explicit; add or update focused tests under `tests/`. See [data flow](../../docs/architecture/data-flow.md) and [testing](../../docs/development/testing.md).
