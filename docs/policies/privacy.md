# Data and privacy

**Owner:** Azalea maintainers | **Last reviewed:** 2026-10-07 | **Status:** Current

What data the bot handles, for engineers. The public statement is the
[privacy policy](../../PRIVACY_POLICY.md); keep the two in sync.

## Data inventory

| Data                                                                                              | Where stored/processed                                       | Purpose                                                                 | Retention                                                                                                                                                     |
|---------------------------------------------------------------------------------------------------|--------------------------------------------------------------|-------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Discord IDs, timestamps, message content, attachment count, sticker/reference IDs, deletion state | SQLite `Message`; in-memory queue before insert              | Message logs, purge/history features, reference context                 | `database.messages.ttl` (default 7 days).                                                                                                                     |
| Infraction action, reason, executor/target IDs, timestamps, expiry/archive/update metadata        | SQLite `Infraction`                                          | Moderation history, search, active sanctions, activity summaries        | No general automatic expiry is defined; staff may archive records.                                                                                            |
| Ban/mute request details, authors/targets/reviewers, reason/status/timing                         | SQLite `BanRequest`, `MuteRequest`                           | Request and approval workflows                                          | No general expiry is defined.                                                                                                                                 |
| Message/user report IDs, content/reason, reporter/target, status and resolution                   | SQLite `MessageReport`, `UserReport`; Discord report channel | Review and resolution of reports                                        | Removed after the configured report TTL, if set; otherwise kept.                                                                                              |
| Reminders, temporary roles/messages, highlight patterns/scoping, lockdown overwrites              | SQLite                                                       | User reminders, timed cleanup, personal highlights, restore permissions | Reminders/temporary records are deleted by feature completion/cleanup; highlights and lockdown state persist until changed/cleared.                           |
| Guild config, role/channel IDs, configured messages and automation rules                          | YAML files on host/repository deployment                     | Configure bot behavior                                                  | Until changed or removed.                                                                                                                                     |
| Error/trace/cron context                                                                          | Sentry, only when `SENTRY_DSN` is configured                 | Diagnostics and performance monitoring                                  | Governed by Sentry project settings.                                                                                                                          |

When enabled, scanned URLs are sent to VirusTotal and guild, Discord, and Roblox IDs to RoVer; their own retention policies apply.

## Deletion and access requests

There is no built-in per-user export or deletion. Requests go to the contacts in the
[privacy policy](../../PRIVACY_POLICY.md). Before promising full deletion, check infractions,
reports, backups, logs, and third-party copies, and back up the database before any manual SQL.

## Engineering rules

- Store only what the feature needs.
- Don't log tokens, keys, or unnecessary message content, and keep message/report content out of
  Sentry unless it's needed and reviewed.
- Never copy production databases or user data into local or CI environments.
- Restrict access to the database, config, and backups.
- Update the privacy policy in the same change as any retention change.
