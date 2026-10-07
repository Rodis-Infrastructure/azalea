# Dependency policy

**Owner:** Azalea maintainers | **Last reviewed:** 2026-10-07 | **Status:** Current

## Adding a dependency

First check whether Bun, discord.js, or an existing dependency already covers the need. Then review
maintenance activity, security history, license compatibility with CC BY-NC 4.0, and the transitive
dependencies it pulls in. Follow the version pinning style in `package.json` and commit
`bun.lockb`.

## Updates

Renovate (`.renovaterc.json`) automerges minor, patch, and digest updates, runs lockfile
maintenance weekly, and raises vulnerability alerts. Review major upgrades manually.

CI runs `bun audit --audit-level high`, ignoring `GHSA-r5fr-rjxr-66jc` (the affected lodash
template function isn't used). Revisit the exception when the dependency changes or a fix ships;
don't add other exceptions without the same justification.

## Upgrade checklist

- [ ] Read the changelog and migration notes.
- [ ] Update `package.json` and the lockfile together.
- [ ] `bun run verify` and `bun run docker:build` pass.
- [ ] Test Discord-facing behavior in a test guild.
