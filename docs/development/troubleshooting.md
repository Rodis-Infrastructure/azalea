# Troubleshooting

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

| Symptom                                   | Likely cause                                                                                  | Fix                                                                                                                      |
|-------------------------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| Startup fails before Discord login        | Missing `DISCORD_TOKEN`, config file, config directory, or invalid YAML/schema                | Check required files and environment; read the first Zod/YAML error in the logs.                                         |
| Discord rejects commands or actions       | Missing bot permissions, intents, role hierarchy, or access to a configured channel           | Check the bot's OAuth scopes/permissions, enabled intents, channel overwrites, and role position.                        |
| `SQLITE_CANTOPEN` or migration failure    | `DATABASE_URL` points to a missing/unwritable directory or migrations are not applied         | Verify the resolved SQLite path and write permissions; run `bun run db:migrate` before startup.                          |
| `/healthz` returns `ready: false`         | Discord `Ready` processing has not completed or a new process has not finished initialization | Check bot login/config/command registration logs. The endpoint does not provide a detailed dependency diagnosis.         |
| `/healthz` cannot connect                 | Wrong host/port or process failed to start endpoint                                           | Defaults are `127.0.0.1:7475`; check `HEALTH_HOST`, `HEALTH_PORT`, and port availability. Keep the endpoint private.     |
| Errors are missing from Sentry            | `SENTRY_DSN` is unset/invalid or network access is unavailable                                | Confirm the runtime environment variable and Sentry project configuration; local logs remain available.                  |
| URL scan or Roblox linking is unavailable | Optional `VIRUSTOTAL_API_KEY` or `ROVER_API_KEY` is missing/invalid                           | Configure the relevant key or treat the integration as disabled.                                                         |
| PM2 deploy does not reload                | Host Bun/PM2 path, ecosystem config, migration, or file permissions issue                     | Inspect the remote deployment log; confirm pinned Bun, `pm2` install, `ecosystem.config.js`, and writable database path. |

See
the [runbook index](../operations/runbooks/README.md), [deployment](../operations/deployment.md),
and [rollback](../operations/rollback.md).
