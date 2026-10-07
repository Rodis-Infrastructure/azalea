# Incident response

**Owner:** Azalea maintainers | **Last reviewed:** 2026-10-07 | **Status:** Current

There is no on-call rotation or response SLA. The maintainer running the affected host coordinates
the response and keeps guild staff informed.

## Triage

1. Confirm impact: bot offline, commands failing, wrong moderation actions, data loss, or a
   security/privacy issue.
2. Check process status, logs, `/healthz`, and Sentry; use the [runbooks](runbooks/README.md).
3. Stop harmful or repeated actions first. Disable the bot or the automation if needed, keeping logs
   and database backups.
4. For exposed credentials, rotate them ([config](config.md#rotation)). For exposed data, restrict
   access and follow the [security policy](../../SECURITY.md).
5. Restore service once config, schema, and permissions are confirmed safe.
6. For significant incidents, write up the timeline, impact, cause, and follow-ups using the
   [postmortem template](../_templates/postmortem.md).

## Severity

| Level    | Example                                                                         |
|----------|---------------------------------------------------------------------------------|
| Critical | Unauthorized moderation, credential compromise, or exposure of stored data      |
| High     | Bot unavailable, or a core moderation workflow failing across guilds            |
| Moderate | A non-core feature or a single guild/channel impaired, with no data or security impact |
