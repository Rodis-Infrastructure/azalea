# Interaction components

One default-exported `Component` subclass per file. `ComponentManager` routes button, select-menu, and modal interactions by `customId`, matched exactly or by prefix, suffix, substring, or regex. Avoid patterns that overlap with existing handlers.

Implement `execute`, return a `CommandResponse`, and check any Azalea role permissions in the handler.
