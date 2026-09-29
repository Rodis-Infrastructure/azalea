# Incident response

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

The repository does not define an on-call rotation, severity SLA, incident channel, or
customer-communication cadence. The maintainer/operator for the affected bot host is responsible for
coordinating response and communication with impacted server staff.

## Triage

1. Confirm impact: bot offline, commands failing, incorrect moderation actions, data loss, or a
   security/privacy concern.
2. Check host/container status, logs, `/healthz`, Discord API access, database path/migrations, and
   Sentry if enabled.
3. Prioritize stopping harmful or repeated actions. Disable the bot or affected automation if needed
   while preserving evidence and database backups.
4. For suspected credential exposure, rotate the relevant Discord/API/GitHub credential and review
   access. For suspected data exposure, limit access and follow the repository privacy/security
   contacts.
5. Restore service only after confirming the configuration, schema, and bot permissions are safe.
6. Record a concise timeline, impact, root cause, and follow-up actions. Use
   the [postmortem template](../_templates/postmortem.md) for material incidents.

## Severity guidance

Assign severity based on actual impact and urgency; no fixed response times are promised:

| Level    | Example                                                                                                |
|----------|--------------------------------------------------------------------------------------------------------|
| Critical | Ongoing unauthorized moderation, credential compromise, or confirmed exposure of sensitive stored data |
| High     | Production bot unavailable or a core moderation workflow failing across its guilds                     |
| Moderate | A non-core feature or one configured guild/channel is impaired without data/security impact            |

Contact: use the private reporting route in the repository [security policy](../../../SECURITY.md)
for vulnerabilities; for service incidents contact the bot maintainer through the repository.
