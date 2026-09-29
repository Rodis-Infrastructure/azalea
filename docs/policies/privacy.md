# Data and privacy

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

This page describes data handled by the software, not legal advice. The
published [privacy policy](../../PRIVACY_POLICY.md) is the external-facing statement; keep it
aligned with deployed configuration and actual feature use.

## Data inventory

| Data                                                                                              | Where stored/processed                                       | Purpose                                                                 | Retention                                                                                                                                                     |
|---------------------------------------------------------------------------------------------------|--------------------------------------------------------------|-------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Discord IDs, timestamps, message content, attachment count, sticker/reference IDs, deletion state | SQLite `Message`; in-memory queue before insert              | Message logs, purge/history features, reference context                 | Global `database.messages.ttl`; checked-in config and schema default are 7 days. Deployments may configure a different TTL.                                   |
| Infraction action, reason, executor/target IDs, timestamps, expiry/archive/update metadata        | SQLite `Infraction`                                          | Moderation history, search, active sanctions, activity summaries        | No general automatic expiry is defined; staff may archive records.                                                                                            |
| Ban/mute request details, authors/targets/reviewers, reason/status/timing                         | SQLite `BanRequest`, `MuteRequest`                           | Request and approval workflows                                          | No general expiry is defined.                                                                                                                                 |
| Message/user report IDs, content/reason, reporter/target, status and resolution                   | SQLite `MessageReport`, `UserReport`; Discord report channel | Review and resolution of reports                                        | Optional per-report TTL can remove records; without a configured TTL, no general expiry is defined. Resolved state alone is not a universal retention period. |
| Reminders, temporary roles/messages, highlight patterns/scoping, lockdown overwrites              | SQLite                                                       | User reminders, timed cleanup, personal highlights, restore permissions | Reminders/temporary records are deleted by feature completion/cleanup; highlights and lockdown state persist until changed/cleared.                           |
| Guild config, role/channel IDs, configured messages and automation rules                          | YAML files on host/repository deployment                     | Configure bot behavior                                                  | Until changed or removed.                                                                                                                                     |
| Error/trace/cron context                                                                          | Sentry, only when `SENTRY_DSN` is configured                 | Diagnostics and performance monitoring                                  | Governed by Sentry project settings.                                                                                                                          |

The bot also sends normal API requests and feature data to Discord. URL scan requests use VirusTotal
when enabled; Roblox linking uses RoVer when enabled. Provider-specific processing and retention are
governed by those providers.

## Deletion and access requests

The repository defines automated retention for message cache and optionally reports, but does not
implement a general per-user export/deletion workflow or service-level turnaround. Requests should
be routed to the server staff/bot maintainer identified in
the [published privacy policy](../../PRIVACY_POLICY.md). Operators must review active infractions,
reports, backups, logs, and provider copies before promising complete deletion. Avoid manual SQL
changes without a tested procedure and an audit/backup.

## Engineering rules

- Collect/store only data required by the enabled feature.
- Do not log tokens, API keys, or unnecessary raw message content. Do not include message/report
  content in Sentry context unless required and reviewed.
- Do not copy production DBs, Discord exports, or user data into local/CI environments. Use a test
  guild and synthetic data.
- Restrict access to SQLite files, configuration, and backups. Encrypt/protect host disks and
  backups according to operator controls.
- Secret scrubbing in Sentry/log serialization is defense in depth, not authorization to place
  sensitive data in events.
- Document retention changes and update `../../PRIVACY_POLICY.md` in the same change.
