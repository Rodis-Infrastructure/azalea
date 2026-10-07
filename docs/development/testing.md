# Testing

Tests live in `tests/*.test.ts` and run with Bun's built-in runner (`bun test`). `bun run verify`
runs Prisma generation, lint, typecheck, schema validation/format checks, migration drift
detection, and tests.

- Unit tests cover pure helpers (validation, request context, error serialization, secret
  scrubbing, Sentry scopes) using small typed fakes.
- `tests/health.test.ts` starts the health server on a dedicated local port.
- The config test validates the local YAML and is skipped on GitHub Actions.
- There is no automated end-to-end suite; check Discord behavior manually in a test guild.

No coverage threshold is enforced. Keep tests deterministic and independent of live services and
production data.
