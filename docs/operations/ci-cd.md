# CI/CD

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

`.github/workflows/ci.yml` runs on pushes and pull requests targeting `main`, `master`, or
`develop`. Jobs run independently:

1. Lint with ESLint.
2. Generate Prisma client and typecheck TypeScript.
3. Validate/format schema, apply migrations to CI SQLite, and detect migration drift.
4. Build the Docker image (not pushed to a registry).
5. Run Bun tests.
6. Audit dependencies at high severity (with a documented lodash advisory exception).
7. Run actionlint on workflow definitions.

`.github/workflows/cd.yml` listens for completed CI runs on `main` or `master` and deploys only when
CI concluded successfully. It deploys over SSH with `scripts/deploy.sh`, optionally
creates/finalizes a Sentry release, and optionally sends a Discord webhook notification.

## Required checks

The CI workflow defines `lint`, `typecheck`, `migrations`, `docker`, `test`, `audit`, and
`actionlint`. Which jobs are enforced as branch-protection required checks is controlled by GitHub
settings and is not checked into this repository. The root contribution guide lists the checks
expected for a pull request.

## Caching and speed

The Docker build uses GitHub Actions cache (`type=gha`). Bun setup follows `.bun-version`;
dependencies are installed with the frozen lockfile. No duration guarantees are documented.

## Releases

`package.json` currently identifies the application as `azalea` version `1.0.0`. CD derives the
Sentry release name as `<package-name>@<package-version>`. A formal release/tagging cadence is not
defined. Configure `SENTRY_AUTH_TOKEN`, `SENTRY_ORG`, and `SENTRY_PROJECT` for Sentry release
publication; configure `DISCORD_WEBHOOK_URL` for notifications. SSH deploy requires `SSH_HOST`,
`SSH_USER`, `SSH_KEY`, and may use `SSH_PORT`.
