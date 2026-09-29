# Environments

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

| Environment | URL                                                                        | Purpose                                                                   | Data                                                                 | Access                                      |
|-------------|----------------------------------------------------------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------|---------------------------------------------|
| Local       | No public URL; health endpoint defaults to `http://127.0.0.1:7475/healthz` | Development and automated tests                                           | Developer-owned SQLite DB and test-guild YAML                        | Developer machine                           |
| CI          | GitHub Actions runners                                                     | Lint, typecheck, tests, migration checks, Docker build, audit, actionlint | Ephemeral CI SQLite DB; test job does not require production secrets | Repository workflow permissions             |
| Test guild  | Discord, configured by developer                                           | Manual integration/command validation                                     | Test Discord data; dedicated bot/config/database recommended         | Test-guild staff and bot maintainers        |
| Production  | Discord guild(s); host URL not published here                              | Live bot service                                                          | Real moderation and user data in SQLite                              | Restricted host/database and Discord access |

## Differences that matter

CI uses a temporary SQLite database for migration checks and skips local config-file validation on
GitHub Actions. Production settings and credentials are provided outside the repository. Sentry
labels its environment from `NODE_ENV` (default `development`); sampling differs for `production`.
Docker persists the database in a named volume, while local/PM2 paths depend on `DATABASE_URL`. No
preview deployment or separate staging bot is configured in the checked-in workflows.
