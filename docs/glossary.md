# Glossary

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

| Term               | Definition                                                                         | Used in                                         |
|--------------------|------------------------------------------------------------------------------------|-------------------------------------------------|
| Guild              | A Discord server.                                                                  | Config, commands, permissions                   |
| Snowflake          | Discord's numeric string identifier for users, guilds, channels, and roles.        | Config and database                             |
| Infraction         | A persisted moderation record, including actions such as ban, mute, warn, or note. | Moderation commands and `Infraction` model      |
| Request            | A ban or mute proposal submitted for staff review.                                 | Request channels and `BanRequest`/`MuteRequest` |
| Report             | A message or user report awaiting staff review/resolution.                         | Report features and report models               |
| Highlight          | A user's configured pattern and channel-scoping rule for matching messages.        | `/highlight`, `Highlight` models                |
| Component          | A Discord interactive control handler (button, select menu, or modal).             | `src/components/`                               |
| Cron job           | A recurring in-process task configured by a cron expression.                       | Cleanup, reminders, scheduled messages          |
| Guild config       | Validated per-server YAML configuration at `configs/<guild_id>.yml`.               | `GuildConfig`, permissions, automations         |
| Ephemeral response | A Discord interaction response visible only to the invoking user.                  | Interaction response scoping                    |
| PM2                | Process manager used by the host-based production deploy path.                     | Deployment                                      |
