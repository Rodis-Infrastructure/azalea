# Commands

One default-exported `Command` subclass per file; `CommandManager` discovers them at startup. Extend `GuildCommand` instead when the command depends on a guild's `GuildConfig`; it's instantiated once per configured guild.

Define the command data, implement `execute`, and return a `CommandResponse`. Commands default to guild-only with Discord's `ManageGuild` permission unless they override it. Azalea role permissions are separate and must be checked explicitly. Put follow-up interactions in `../components/`. See the [command reference](../../docs/commands.md).
