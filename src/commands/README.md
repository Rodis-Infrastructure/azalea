# Commands

Add one command class per `.ts` file in this directory and default-export it. `CommandManager` discovers these files at startup; classes must extend `Command`. Extend `GuildCommand` when construction or behavior depends on the current guild's `GuildConfig`; it is instantiated separately for each configured guild.

Define Discord command data in the class, implement `execute`, and return a `CommandResponse`. `Command.build()` supplies the default guild context and `ManageGuild` permission unless the command overrides them. For interactions beyond command behavior, use a component handler in `../components/`.

Keep permission checks explicit: Discord permissions and Azalea's configured role permissions are separate. See [command behavior and permissions](../../docs/commands.md) and [code conventions](../../docs/development/conventions.md).
