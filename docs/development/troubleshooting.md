# Troubleshooting

| Symptom                                   | Likely cause                                                                     | Fix                                                                                         |
|-------------------------------------------|----------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| `No token provided` at startup            | `DISCORD_TOKEN` unset or `.env` not loaded                                        | Set it in `.env` or export it.                                                              |
| Startup exits before Discord login        | Missing `azalea.cfg.yml`, no guild config in `configs/`, or invalid YAML/schema  | Read the first config error in the logs.                                                    |
| `SQLITE_CANTOPEN` or migration failure    | `DATABASE_URL` directory missing/unwritable, or migrations not applied           | Create the directory, check permissions, run `bun run db:migrate`.                          |
| Commands or actions fail                  | Missing bot permissions, intents, role position, channel access, or Azalea role permission | Check the Developer Portal intents, role hierarchy, channel overwrites, and guild `permissions`. |
| `/healthz` stays `ready: false`           | Startup hasn't finished                                                          | Check login, config, and command-publishing logs.                                           |
| `/healthz` unreachable or port in use     | Wrong host/port, or another process on 7475                                      | Check `HEALTH_HOST`/`HEALTH_PORT`; keep it on loopback.                                     |
| Errors missing from Sentry                | `SENTRY_DSN` unset/invalid or no network access                                  | Check the environment and Sentry project.                                                   |
| URL scan or Roblox lookup unavailable     | `VIRUSTOTAL_API_KEY` or `ROVER_API_KEY` missing/invalid                          | Configure the key.                                                                          |

For production incidents, see the [runbooks](../operations/runbooks/README.md).
