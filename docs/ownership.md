# Ownership

**Owner:** @Archasion **Last reviewed:** 2026-09-29 **Status:** Current

| Area                                                        | Owner                        | Primary contact                            | Escalation                         |
|-------------------------------------------------------------|------------------------------|--------------------------------------------|------------------------------------|
| Bot source, documentation, releases, and production runtime | Project maintainer           | [@Archasion](https://github.com/Archasion) | Repository maintainer              |
| Discord guild configuration and moderation policy           | Guild administrators         | Guild staff                                | Project maintainer for bot defects |
| Sentry project, if enabled                                  | Sentry project administrator | Configured project owner                   | Project maintainer                 |
| RoVer/VirusTotal credentials, if enabled                    | Integration operator         | Credential owner                           | Project maintainer                 |

The repository history shows @Archasion as the primary contributor across the codebase. Other
contributors have delivered focused changes, but the history does not establish durable area
ownership, so [CODEOWNERS](../../.github/CODEOWNERS) assigns the project maintainer as the default
reviewer rather than implying additional maintainers. There is no on-call rotation. Incident
response and escalation are operator-coordinated;
see [incident response](operations/incident-response.md).
