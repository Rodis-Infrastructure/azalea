# Source modules

`index.ts` creates shared clients and starts the process. Startup order and runtime boundaries are documented in [architecture](../docs/architecture/overview.md) and [Discord runtime](../docs/components/discord-runtime.md).

| Directory                             | Responsibility                                      | Start here                                               |
|---------------------------------------|-----------------------------------------------------|----------------------------------------------------------|
| [`commands/`](commands/README.md)     | Slash and context-menu command implementations      | [Command reference](../docs/commands.md)                 |
| [`components/`](components/README.md) | Button, select-menu, and modal interaction handlers | [Discord runtime](../docs/components/discord-runtime.md) |
| [`events/`](events/README.md)         | Discord gateway event listeners                     | [Data flow](../docs/architecture/data-flow.md)           |
| [`managers/`](managers/README.md)     | Discovery, dispatch, and configuration lifecycle    | [Components](../docs/components/README.md)               |
| [`utils/`](utils/README.md)           | Shared domain operations and infrastructure helpers | [Data flow](../docs/architecture/data-flow.md)           |

When changing a module's external behavior or configuration, update the corresponding canonical page in `docs/`; the local READMEs describe source boundaries, not a second feature reference.
