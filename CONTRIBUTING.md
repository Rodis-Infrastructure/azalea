# Contributing

Azalea is a Bun/TypeScript project. Start with the [local setup guide](docs/development/setup.md) and read the [code conventions](docs/development/conventions.md) before changing runtime behavior.

## Workflow
- Branch naming: `feature/<short-description>`, `fix/<short-description>`, or `chore/<short-description>`. Include an issue number when one exists, for example `fix/123-command-registration`.
- Commit format: Conventional Commits, for example `feat: add infraction logging command`. To use the repository template, run `git config --local commit.template .gitmessage`.
- Keep pull requests focused; the project does not define a numeric size limit.

## Pull requests
- Required CI jobs are lint, typecheck, migrations, Docker image build, tests, dependency audit, and actionlint. Branch-protection settings are configured in GitHub and are not defined in this repository.
- Request review from the project maintainer listed in [CODEOWNERS](.github/CODEOWNERS).
- Docs updated in the same PR when behavior, config, or interfaces change

## Definition of done
- [ ] Relevant tests added or updated and pass locally.
- [ ] `bun run lint`, `bun run typecheck`, and relevant Prisma checks pass.
- [ ] Docs updated when behavior, configuration, or interfaces change.
- [ ] Migration and recovery implications are documented, if applicable.
- [ ] New failure modes have useful logs and, where appropriate, Sentry reporting.

See [testing](docs/development/testing.md), [CI/CD](docs/operations/ci-cd.md), and [conventions](docs/development/conventions.md).
