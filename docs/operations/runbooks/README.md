# Runbooks

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

| Runbook                                              | Trigger                                                               | Severity                              |
|------------------------------------------------------|-----------------------------------------------------------------------|---------------------------------------|
| [Bot unavailable](bot-unavailable.md)                | Bot offline, `/healthz` unreachable, or startup fails                 | High, based on impact                 |
| [Database or migration failure](database-failure.md) | Prisma/SQLite errors, failed migration, suspected corruption          | High; Critical if data loss is active |
| [Deployment failure](deployment-failure.md)          | CI/CD reports failure or PM2/Compose deployment does not become ready | Moderate to High, based on impact     |

Copy [the runbook template](../../_templates/runbook.md) when adding a failure mode with a distinct
diagnosis or recovery procedure.
