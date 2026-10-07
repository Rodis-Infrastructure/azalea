# Commands

Commands are guild-only and require Discord's `ManageGuild` permission unless overridden. Some
actions also check Azalea's per-guild role permissions, which don't grant Discord permissions. The
source of truth is `src/commands/` and `src/components/`.

## Slash commands

### Testing and administration

| Command                    | Purpose                                                                                                                                                                                                                                |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `/create-testing-template` | Destructively rebuild a small test guild with sample channels/roles and a generated config. Owner-only; requires fewer than 10 members, exact confirmation, and bot `Manage Channels`, `Manage Roles`, and `Manage Guild` permissions. |

**Warning:** it deletes every channel and every non-managed role (except `@everyone`) first. Never
run it in a real server.

### Moderation

| Command                                                         | Purpose                                                    |
|-----------------------------------------------------------------|------------------------------------------------------------|
| `/ban`, `/kick`, `/mute`, `/unmute`, `/unban`, `/warn`, `/note` | Apply or record moderation actions.                        |
| `/purge all`, `/purge user`                                     | Remove messages in a channel or by a specified user.       |
| `/lockdown start`, `/lockdown end`                              | Apply or restore configured channel permission overwrites. |
| `/censor nickname`                                              | Replace a member nickname according to guild config.       |

### Infractions and activity

| Command                                      | Purpose                                             |
|----------------------------------------------|-----------------------------------------------------|
| `/infraction search`, `/infraction info`     | Search infractions and inspect an infraction by ID. |
| `/infraction duration`, `/infraction reason` | Update an infraction's expiry or reason.            |
| `/infraction archive`, `/infraction restore` | Archive or restore an infraction.                   |
| `/infraction active`                         | Show a user's active mute or ban.                   |
| `/infraction copy-history`                   | Transfer infractions between users.                 |
| `/moderation activity`                       | View moderation activity with month/year filters.   |

### Highlights and reminders

| Command                                                                           | Purpose                                                             |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| `/highlight pattern add`, `/highlight pattern remove`, `/highlight pattern clear` | Manage keyword/pattern subscriptions.                               |
| `/highlight channel add`, `/highlight channel remove`, `/highlight channel clear` | Scope highlights to channels.                                       |
| `/highlight list`, `/highlight erase`                                             | List own highlights or erase a user's highlights (admin operation). |
| `/reminders add`, `/reminders list`, `/reminders remove`, `/reminders clear`      | Create, view, and remove personal reminders.                        |

### Information and utilities

| Command                           | Purpose                                                       |
|-----------------------------------|---------------------------------------------------------------|
| `/search`, `/user info`           | Find users and show user details; Roblox lookup is optional.  |
| `/role members`                   | List members with specified roles.                            |
| `/rule`, `/faq`                   | Display configured rules or quick responses.                  |
| `/config guild`, `/config global` | View guild/global configuration.                              |
| `/list-permissions`               | Show bot permissions available in a channel.                  |
| `/process info`                   | Display process diagnostics such as uptime, ping, and memory. |
| `/scan url`                       | Scan a URL with VirusTotal; requires `VIRUSTOTAL_API_KEY`.    |

## Context menu commands

| Menu item          | Type    | Purpose                                          |
|--------------------|---------|--------------------------------------------------|
| Purge messages     | User    | Purge messages from a selected user.             |
| User info          | User    | Show user information.                           |
| Search infractions | User    | Search a selected user's infraction history.     |
| Censor nickname    | User    | Censor a selected member's nickname.             |
| Report user        | User    | Open a user report form.                         |
| Quick mute (30m)   | Message | Quick-mute the message author for 30 minutes.    |
| Quick mute (1h)    | Message | Quick-mute the message author for one hour.      |
| Report message     | Message | Submit a report for the selected message.        |
| Store media        | Message | Store/log attachments from the selected message. |

Other features run from buttons, select menus, modals, or configured reactions rather than commands.
