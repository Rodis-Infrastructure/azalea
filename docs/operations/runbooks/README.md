# Runbooks

**Owner:** Azalea maintainers | **Last reviewed:** 2026-10-07 | **Status:** Current

| Runbook                                              | Trigger                                                         | Severity                       |
|------------------------------------------------------|-----------------------------------------------------------------|--------------------------------|
| [Bot unavailable](bot-unavailable.md)                | Bot offline, `/healthz` unreachable, or startup fails           | High                           |
| [Database or migration failure](database-failure.md) | Prisma/SQLite errors, failed migration, suspected corruption    | High; Critical if data is being lost |
| [Deployment failure](deployment-failure.md)          | CD fails or the new process doesn't become ready                | Moderate to High               |

For a new failure mode, copy [the runbook template](../../_templates/runbook.md).
