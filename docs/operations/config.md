# Configuration and secrets

**Owner:** Azalea maintainers | **Last reviewed:** 2026-10-07 | **Status:** Current

## Environment variables

`.env.example` lists every variable. Never commit `.env` or real credentials.

| Name                 | Purpose                             | Required | Secret? | Notes                                                                                                                         |
|----------------------|-------------------------------------|---------:|--------:|-------------------------------------------------------------------------------------------------------------------------------|
| `DISCORD_TOKEN`      | Discord bot authentication          |      Yes |     Yes | Obtain from Discord Developer Portal.                                                                                         |
| `DATABASE_URL`       | Prisma SQLite connection            |      Yes |      No | `file:` URL to writable DB file; example is `file:data/azalea.db`. Protect the DB as it contains moderation and message data. |
| `SENTRY_DSN`         | Error/performance/cron telemetry    |       No |     Yes | Unset disables Sentry reporting.                                                                                              |
| `ROVER_API_KEY`      | Roblox account linking in user info |       No |     Yes | Unset disables this integration.                                                                                              |
| `VIRUSTOTAL_API_KEY` | `/scan url` lookups                 |       No |     Yes | Unset disables URL scanning.                                                                                                  |
| `HEALTH_HOST`        | Health HTTP bind host               |       No |      No | Defaults to `127.0.0.1`; do not expose publicly.                                                                              |
| `HEALTH_PORT`        | Health HTTP bind port               |       No |      No | Defaults to `7475`.                                                                                                           |
| `NODE_ENV`           | Runtime/Sentry environment label    |       No |      No | Defaults to `development`; `production` lowers Sentry sampling; `test` skips bot startup.                                     |
| `LOG_LEVEL`          | Minimum console log level           |       No |      No | `debug`, `info`, `warn`, or `error`; defaults to `info`.                                                                      |
| `LOG_FORMAT`         | Console log format                  |       No |      No | `text` or `json`; defaults to `text`.                                                                                         |

To add a variable, document it in `.env.example` and this table, set it in each environment, and
make sure logs, errors, and telemetry can't reveal secret values.

## Rotation

Rotate on suspected exposure; there is no fixed interval.

| Secret                     | Procedure                                                                                          |
|----------------------------|----------------------------------------------------------------------------------------------------|
| Discord bot token          | Regenerate in the Developer Portal, update the environment, reload, and verify the bot reconnects. |
| Sentry DSN                 | Update in Sentry and the environment; restart.                                                     |
| RoVer / VirusTotal API key | Regenerate with the provider, update the environment, and verify the feature.                      |
| GitHub SSH/deploy secrets  | Rotate the host's authorized key and the repository secrets together; verify a deploy.             |

Treat the SQLite database, YAML configs, and backups as sensitive and restrict access to them.

## YAML configuration

`azalea.cfg.yml` (global message-cache jobs) and at least one `configs/<guild_id>.yml` are
required. Files are validated at startup and invalid config stops the bot; files for guilds the bot
can't fetch are skipped. Edits take effect after a restart (except `/create-testing-template`,
which hot-loads the config it generates). The Zod schema in `src/managers/config/schema.ts` is the
full reference. IDs are Discord snowflakes (17–19 digits) and durations are milliseconds unless
noted.

Global example:

```yaml
database:
  messages:
    insert_cron: "0 * * * *"
    delete_cron: "0 0 * * *"
    ttl: 604800000 # 7 days, milliseconds
```

Guild-level top-level settings:

| Key                           | Purpose / default                                                                      |
|-------------------------------|----------------------------------------------------------------------------------------|
| `default_purge_amount`        | Purge default, 1–100; default 100                                                      |
| `response_ttl`                | Lifetime of non-ephemeral bot responses in ms; default 3000                            |
| `notification_channel_id`     | Optional notification target                                                           |
| `media_conversion_channel_id` | Optional media conversion target                                                       |
| `auto_publish_announcements`  | Announcement channels to auto-publish                                                  |
| `ban_delete_message_days`     | Days of message history removed by a ban, 0–7; default 0                               |
| `default_mute_duration`       | Default mute duration in ms; defaults to maximum of 28 days                            |
| `auto_pause_dms`              | Renew Discord's pause-DMs server setting hourly; default false; requires Manage Server |

Feature config keys:

| Key                               | Purpose                                                                        |
|-----------------------------------|--------------------------------------------------------------------------------|
| `logging`                         | Event log destinations and default/per-log channel and role scoping            |
| `permissions`                     | Guild roles mapped to bot permissions                                          |
| `ban_requests`, `mute_requests`   | Request queue and optional review-reminder schedule                            |
| `message_reports`, `user_reports` | Report channel, optional expiry/reminder, mentions, and excluded roles         |
| `lockdown`                        | Channels and permission overwrites to apply/revert                             |
| `auto_reactions`                  | Per-channel reactions, excluded roles, and excluded patterns                   |
| `auto_threads`                    | Thread names using `$USERNAME`, `$NICKNAME`, `$USER_ID`; optional role scoping |
| `media_channels`                  | Channels requiring attachments, role scoping, fallback response                |
| `scheduled_messages`              | Cron schedule, Sentry monitor slug, and messages                               |
| `role_requests`                   | Request channel and allowed roles with optional millisecond TTL                |
| `quick_responses`                 | FAQ entries, maximum 25                                                        |
| `rules`                           | Rule entries and optional reference channel                                    |
| `nickname_censorship`             | Excluded roles, response, and replacement nickname using `$RAND`/`$USER_ID`    |
| `stage_event_overrides`           | Stage-related channel permission behavior                                      |
| `user_flags`                      | Flags shown in user information, maximum 18                                    |
| `infraction_reasons`              | Excluded domains and message-link scoping                                      |
| `ephemeral_scoping`               | Channels where default/moderation activity responses are ephemeral             |
| `emojis`                          | Optional reaction and display emoji IDs/Unicode emoji                          |

Example of a simple guild permission map and event-log destination:

```yaml
permissions:
  - roles: [ "123456789012345678" ]
    allow:
      - view_infractions
      - manage_infractions

logging:
  default_scoping:
    include_channels: [ ]
    exclude_channels: [ ]
    include_roles: [ ]
    exclude_roles: [ ]
  logs:
    - events: [ "infraction_create", "infraction_update" ]
      channel_id: "123456789012345679"
      scoping: { }
```

An empty per-log `scoping` inherits `default_scoping`. A channel or role ID can't be in both an
include and an exclude list. Review reminder objects can set `cron`,
`embed` (default true), `count_threshold` (default 25), `age_threshold` (default 3,600,000 ms), and
`mentioned_roles`; omitted cron defaults to hourly.

Bot permission values include `manage_infractions`, `transfer_infractions`, `manage_mute_requests`,
`manage_ban_requests`, `manage_message_reports`, `manage_user_reports`, `manage_highlights`,
`view_infractions`, `view_moderation_activity`, `purge_messages`, `quick_mute`, `report_messages`,
`manage_role_requests`, `manage_roles`, `forward_messages`, and `manage_guild_config`. The last
permission is used by the sibling `azalea-editor`, not the bot.

`stage_event_overrides` entries identify a `stage_id` and non-empty `channels` and `roles` lists.
`lockdown` requires a non-empty channel list and permission overwrites; the bot snapshots prior
state so it can revert the lockdown. Scheduled message `monitor_slug` values must be 1–50 uppercase
letters/underscores.

The current logging event values are: `message_bulk_delete`, `message_delete`, `message_update`,
`message_reaction_add`, `message_publish`, `interaction_create`, `voice_join`, `voice_leave`,
`voice_move`, `thread_create`, `thread_delete`, `thread_update`, `member_join`, `member_leave`,
`media_store`, `infraction_create`, `infraction_archive`, `infraction_restore`, `infraction_update`,
`ban_request_approve`, `ban_request_deny`, `mute_request_approve`, `mute_request_deny`,
`message_report_create`, `message_report_resolve`, `user_report_create`, `user_report_update`, and
`user_report_resolve`.

Highlights are managed with `/highlight`, not YAML: up to 20 patterns (45 characters each), 40
whitelisted channels, and 40 blacklisted channels per user per guild. Unsafe regular expressions
are rejected.
