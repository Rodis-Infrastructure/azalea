# Shared utilities

Helpers shared across handlers: persistence workflows (infractions, reports), message formatting and caching, logging, request context, secret redaction, and Sentry. Put domain logic here instead of duplicating it in commands and events.

Helpers that use the shared Discord or Prisma client have side effects; keep those explicit and cover changes with tests in `tests/`.
