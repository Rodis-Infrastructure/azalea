# Glossary

| Term               | Definition                                                                         | Used in                                         |
|--------------------|------------------------------------------------------------------------------------|-------------------------------------------------|
| Guild              | A Discord server.                                                                  | Config, commands, permissions                   |
| Snowflake          | Discord's numeric string ID for users, guilds, channels, and roles.                | Config and database                             |
| Infraction         | A persisted moderation record (ban, mute, warn, note, etc.).                       | Moderation commands, `Infraction` model         |
| Request            | A ban or mute proposal submitted for staff review.                                 | Request channels, `BanRequest`/`MuteRequest`    |
| Report             | A message or user report awaiting staff review.                                    | Report features and models                      |
| Highlight          | A user's pattern and channel scoping for matching messages.                        | `/highlight`, `Highlight` models                |
| Component          | A handler for a button, select menu, or modal.                                     | `src/components/`                               |
| Cron job           | A recurring in-process task configured by a cron expression.                       | Message cache, cleanup, review reminders, scheduled messages |
| Guild config       | Validated per-server YAML at `configs/<guild_id>.yml`.                             | `GuildConfig`, permissions, automations         |
| Ephemeral response | An interaction response visible only to the invoking user.                         | Interaction responses                           |
| PM2                | Process manager used by the host-based production deploy.                          | Deployment                                      |
