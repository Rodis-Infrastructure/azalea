# Testing

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

## Strategy

| Layer                 | Covers                                                                                                   | Tooling                                  | Runs                                                       |
|-----------------------|----------------------------------------------------------------------------------------------------------|------------------------------------------|------------------------------------------------------------|
| Unit/pure logic       | Utilities, validation, request context, error serialization, secret scrubbing, Sentry scope construction | Bun test runner                          | Local and CI                                               |
| Component/integration | Health HTTP endpoint and Prisma schema/migration consistency                                             | Bun tests and Prisma CLI                 | Local and CI                                               |
| End to end            | No automated live Discord-guild end-to-end suite is defined in this repository                           | Manual testing in a dedicated test guild | Before changes that need Discord API behavior confirmation |

## Running tests

```sh
bun test
# or
bun run test
```

Tests live in `tests/*.test.ts`. `bun run verify` runs Prisma client generation, lint, typecheck,
schema validation/format checks, migration drift detection, and tests.

## Test data and mocking

Tests use Bun's built-in runner and generally exercise pure helpers with small typed fakes.
`tests/health.test.ts` starts a server on a dedicated local port. The config validation test reads
the local YAML configuration and is skipped on GitHub Actions because deployment config/credentials
are not CI fixtures. There is no shared production database fixture or automated real-Discord
credential setup.

## Coverage and gates

No coverage percentage or coverage gate is configured. CI requires the test job and separately runs
lint, typecheck, migration/schema checks, Docker build, dependency audit, and actionlint. Keep tests
deterministic and independent of live external services.
