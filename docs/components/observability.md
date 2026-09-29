# Observability

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

## Logging

The shared logger in `src/utils/logger.ts` writes runtime diagnostics to console output, including
boot environment/version, handler lifecycle, and cron execution/failure. Process/container
supervisors collect stdout/stderr. `LOG_LEVEL` accepts `debug`, `info`, `warn`, or `error` (default
`info`); `LOG_FORMAT=json` emits JSON lines, otherwise text is used. Settings are resolved when the
logger module loads.

## Sentry

Set `SENTRY_DSN` to enable Sentry. Without it, the bot warns at startup and errors remain in local
logs. Sentry captures exceptions with source/context tags, traces, Prisma spans, profiles, and cron
check-ins. Production samples traces at 20% and profiles at 10%; non-production is configured at
100%. `beforeSend` redacts secret environment values from exception messages; breadcrumbs redact
known secrets and strip URL query strings. This is not a substitute for avoiding sensitive context.

Optional CD release publication uses `SENTRY_AUTH_TOKEN`, `SENTRY_ORG`, and `SENTRY_PROJECT`;
release tagging is best-effort and does not determine whether deployment succeeded.

## Health endpoint

`src/utils/health.ts` starts a small HTTP server. `GET /healthz` returns JSON with `ready`, `pid`,
`startedAt`, `name`, and `version`; other paths return 404. Readiness starts false and flips true in
the Ready handler after it initiates cron registration; it does not wait for every async
registration or prove Discord/database operations remain healthy. Defaults are host `127.0.0.1` and
port `7475`, configurable with `HEALTH_HOST` and `HEALTH_PORT`. Keep it loopback-only; it is not
authenticated and must not be exposed publicly.

## Limits

The repository defines no SLO, dashboard, paging target, alert threshold, or on-call schedule.
See [monitoring](../operations/monitoring.md)
and [incident response](../operations/incident-response.md).
