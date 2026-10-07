# Observability

## Logging

`src/utils/logger.ts` writes to stdout, including boot environment/version, handler lifecycle, and
cron runs/failures. `LOG_LEVEL` (`debug`, `info`, `warn`, `error`; default `info`) and `LOG_FORMAT`
(`text` or `json`; default `text`) are read once at startup.

## Sentry

Enabled when `SENTRY_DSN` is set; otherwise the bot logs a warning at startup. Sentry receives
exceptions with source tags, traces with Prisma spans, profiles, and cron check-ins. With
`NODE_ENV=production` traces are sampled at 20% and profiles at 10%; otherwise 100%. Secret
environment values are redacted from exception messages and breadcrumbs, and URL query strings are
stripped from breadcrumbs. This is a safeguard, not permission to send sensitive context.

CD can also create a Sentry release (see [CI/CD](../operations/ci-cd.md)).

## Health endpoint

`GET /healthz` (`src/utils/health.ts`) returns `{ ready, pid, startedAt, name, version }`; other
paths return 404. `ready` becomes true once the `Ready` handler has started cron jobs. It doesn't
check ongoing Discord or database health. It binds to `127.0.0.1:7475` by default (`HEALTH_HOST`,
`HEALTH_PORT`), is unauthenticated, and must stay on loopback.
