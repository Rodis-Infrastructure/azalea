# Environments

**Owner:** Azalea maintainers | **Last reviewed:** 2026-10-07 | **Status:** Current

| Environment | Purpose                               | Data                                         | Notes                                                        |
|-------------|---------------------------------------|----------------------------------------------|--------------------------------------------------------------|
| Local       | Development and tests                 | Developer's SQLite DB and test-guild YAML    | Health endpoint at `http://127.0.0.1:7475/healthz`           |
| CI          | Checks on GitHub Actions              | Throwaway SQLite DB; no production secrets   | Skips the local config validation test                        |
| Test guild  | Manual Discord validation             | Test data; use a separate bot and database   | Configured by the developer                                  |
| Production  | Live bot                              | Real moderation and user data                | PM2 or Docker; credentials kept outside the repository       |

Sentry's environment comes from `NODE_ENV` (default `development`); `production` lowers sampling.
There is no staging bot or preview deployment.
