# Gateway events

One default-exported `EventListener` subclass per file; `EventListenerManager` mounts it on the client and wraps it with request context and error capture. Set `options.once` only for once-per-process events, and don't add a second raw client listener for the same behavior. Startup-only scheduled work belongs in `Ready.ts`.
