# Contributing

Start with the [local setup guide](docs/development/setup.md) and the [code conventions](docs/development/conventions.md).

## Workflow
- Branch names: `feature/<short-description>`, `fix/<short-description>`, or `chore/<short-description>`, with an issue number when one exists (e.g. `fix/123-command-registration`).
- Commits follow Conventional Commits (e.g. `feat: add infraction logging command`). Optional template: `git config --local commit.template .gitmessage`.
- Keep pull requests focused, and request review from [CODEOWNERS](.github/CODEOWNERS).

## Definition of done
- [ ] Relevant tests added or updated and passing.
- [ ] `bun run verify` passes (lint, typecheck, Prisma checks, and tests).
- [ ] Docs updated in the same PR when behavior, configuration, or interfaces change.
- [ ] Migration and recovery implications documented, if applicable.
- [ ] New failure modes produce useful logs and, where appropriate, Sentry reports.

See [testing](docs/development/testing.md) and [CI/CD](docs/operations/ci-cd.md).
