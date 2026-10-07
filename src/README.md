# Source modules

`index.ts` creates the shared clients and runs startup; see [Discord runtime](../docs/components/discord-runtime.md) for the order.

| Directory                             | Responsibility                                      |
|---------------------------------------|-----------------------------------------------------|
| [`commands/`](commands/README.md)     | Slash and context-menu commands                     |
| [`components/`](components/README.md) | Button, select-menu, and modal handlers             |
| [`events/`](events/README.md)         | Discord gateway event listeners                     |
| [`managers/`](managers/README.md)     | Discovery, dispatch, and configuration loading      |
| [`utils/`](utils/README.md)           | Shared domain operations and infrastructure helpers |

These READMEs describe code boundaries; feature behavior is documented in [`docs/`](../docs/README.md).
