# Dependency policy

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

## Adding dependencies

Dependencies affect a long-running bot with access to Discord and stored moderation data. Before
adding one, check whether the platform, Bun/Node, discord.js, or an existing library already
provides the needed capability. Review maintenance activity, license compatibility with this
repository's CC BY-NC 4.0 project license, security history, transitive dependency impact, and
runtime size. Pin direct dependencies consistently with the existing `../../package.json`
conventions and commit `bun.lockb`.

## Updating

Renovate is configured in `../../.renovaterc.json`. It opens updates and is configured to automerge
minor, patch, and digest updates as branches; vulnerability alerts are enabled, and lockfile
maintenance is scheduled weekly. Major upgrades should be reviewed manually. No separate approval
group is defined in repository files.

CI runs `bun audit --audit-level high` with the documented exception `GHSA-r5fr-rjxr-66jc` (the
affected lodash template function is not used and the dependency has no patched release in the
checked-in workflow comment). Reassess the exception when the dependency chain changes or a fix is
available; do not copy the exception to unrelated advisories.

## Upgrade checklist

- [ ] Read upstream changelog and migration notes.
- [ ] Update manifest and lockfile together.
- [ ] Run `bun run verify` and the Docker image build.
- [ ] Check Prisma, discord.js, Bun, and Node runtime compatibility.
- [ ] Review security advisories and license changes.
- [ ] Test Discord-dependent behavior in a dedicated guild before production.
- [ ] Document behavior or operational changes.
