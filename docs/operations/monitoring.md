# Monitoring

**Owner:** Azalea maintainers | **Last reviewed:** 2026-10-07 | **Status:** Current

| Signal               | Where                                  | What it tells you                                                       |
|----------------------|----------------------------------------|-------------------------------------------------------------------------|
| Application logs     | Process/container stdout               | Boot info, lifecycle, cron runs, errors                                 |
| Sentry               | Optional Sentry project                | Exceptions with source tags, traces with Prisma spans, cron check-ins   |
| `/healthz`           | Loopback, default `127.0.0.1:7475`     | Whether startup finished; `startedAt` shows when the process started    |
| PM2/container status | Host or Compose                        | Whether the process is running (not whether Discord or the DB is healthy) |

There are no dashboards, SLOs, or alerts defined in the repository; configure alert routing in
Sentry or the host. See [observability](../components/observability.md) for details and the
[runbooks](runbooks/README.md) for diagnosis.
