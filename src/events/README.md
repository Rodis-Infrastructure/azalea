# Gateway events

Add one default-exported `EventListener` subclass per `.ts` file. `EventListenerManager` mounts each handler on the Discord client; set `options.once` only for events that should run once per process. Implement `execute` using the event's discord.js arguments.

The manager provides request context and logs/captures rejected handler work. Do not add a second direct client listener for the same behavior. `Ready` owns startup-only scheduled work; see [runtime lifecycle](../../docs/components/discord-runtime.md) and [async data flow](../../docs/architecture/data-flow.md).
