# Interaction components

Add one default-exported `Component` subclass per `.ts` file. `ComponentManager` discovers and caches these handlers at startup, then dispatches component and modal interactions by `customId`. IDs may be exact strings or match by prefix, suffix, substring, or regular expression; avoid overlapping patterns that could route an interaction to the wrong handler.

Implement `execute` and return a `CommandResponse`. Keep interaction-specific permission checks in the handler; native Discord permissions and Azalea role permissions are distinct. See [Discord runtime](../../docs/components/discord-runtime.md).
