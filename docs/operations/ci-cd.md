# CI/CD

**Owner:** Azalea maintainers | **Last reviewed:** 2026-10-07 | **Status:** Current

## CI

`.github/workflows/ci.yml` runs on pushes and pull requests to `main`, `master`, or `develop`.
Its independent jobs are `lint`, `typecheck`, `migrations` (validate/format the schema, apply
migrations to a fresh SQLite database, detect drift), `docker` (build only, with GitHub Actions
cache), `test`, `audit` (`bun audit --audit-level high`, with one documented lodash exception), and
`actionlint`. Which jobs are required for merge is set in GitHub branch protection, not the repo.

## CD

`.github/workflows/cd.yml` runs after a successful CI run on `main` or `master`:

1. SSH to the host and run `scripts/deploy.sh` (see [deployment](deployment.md)).
2. Create and finalize a Sentry release named `<package name>@<package version>` (best-effort).
3. Post the result to a Discord webhook.

| Setting                             | Kind     | Required                         |
|-------------------------------------|----------|----------------------------------|
| `SSH_HOST`, `SSH_USER`, `SSH_KEY`   | Secret   | Yes                              |
| `SSH_PORT`                          | Secret   | No                               |
| `SENTRY_AUTH_TOKEN`                 | Secret   | No; the release step is skipped without it |
| `SENTRY_ORG`, `SENTRY_PROJECT`      | Variable | With `SENTRY_AUTH_TOKEN`         |
| `DISCORD_WEBHOOK_URL`               | Secret   | No; the notification is skipped without it |

There is no release or tagging cadence; the Sentry release uses the `package.json` version.
