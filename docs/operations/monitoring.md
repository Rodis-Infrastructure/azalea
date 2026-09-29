# Monitoring and alerting

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

## Signals available

| Signal               | Where                                         | What it tells you                                                                                                                                                           |
|----------------------|-----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Application logs     | Process/container stdout                      | Boot environment/version, lifecycle, cron starts/ticks, and operational errors                                                                                              |
| Sentry errors        | Optional Sentry project                       | Captured exceptions with source tags and relevant Discord context                                                                                                           |
| Sentry performance   | Optional Sentry project                       | Traces and Prisma query spans; production sampling is configured at 20% traces and 10% profiles                                                                             |
| Sentry cron monitors | Optional Sentry project                       | Instrumented cron check-ins and failures                                                                                                                                    |
| `/healthz`           | Local HTTP endpoint, default `127.0.0.1:7475` | `ready`, process ID, process start time, app name, and version; `ready` flips after the Ready handler initiates cron registration, not after a full dependency-health check |
| PM2/container status | Host/Compose runtime                          | Whether the process/container is running; this does not alone prove Discord or DB health                                                                                    |

The repository does not define dashboards, numeric SLOs, alert thresholds, paging policies, or an
on-call rotation. Operators should configure alert routing in Sentry/hosting tools and monitor
process logs.

## Diagnostic checklist

1. Check stdout/stderr for startup, Discord API, Prisma, and cron errors.
2. Query `/healthz` over loopback. `ready: false` indicates initialization has not reached the ready
   event.
3. Check whether Discord reports the bot online and whether commands/interactions work.
4. If Sentry is enabled, inspect captured errors and cron monitor status using their tags.
5. For persistence failures, confirm `DATABASE_URL`, filesystem permissions, free disk, and
   migration state.

Keep `/healthz` bound to loopback. Sentry scrubs known environment secrets from exception messages
and URL query strings, but do not rely on scrubbing to make sensitive data safe to send.
