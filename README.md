# Azalea

A Discord moderation and utility bot built with Bun, TypeScript, discord.js, Prisma, and SQLite.

Azalea provides moderation infractions and approval requests, message/user reports, configurable event logging, highlights, reminders, lockdowns, quick mutes, URL scanning, role requests, scheduled messages, auto-publish/reactions/threads, media-channel rules, and server-specific commands/utilities.

## Requirements

- Bun `1.3.14` as pinned in `.bun-version`
- Node.js 22+
- A Discord bot token and access to at least one configured guild

## Quickstart

```sh
git clone https://github.com/Rodis-Infrastructure/azalea-new.git
cd azalea-new
bun run setup
cp .env.example .env
# Replace DISCORD_TOKEN; leave DATABASE_URL at the local default or set your own.
# Remove optional API-key placeholder values if you are not configuring them.
mkdir -p data
bun run db:migrate
# Add azalea.cfg.yml and at least one configs/<guild_id>.yml.
bun start
```

See the [local setup guide](docs/development/setup.md) for prerequisites and verification.

## Documentation

Start at the [documentation index](docs/README.md). Key references:

- [Commands](docs/commands.md)
- [Configuration](docs/operations/config.md)
- [Deployment](docs/operations/deployment.md)
- [Architecture](docs/architecture/overview.md)

## Contributing and security

- [Contributing](CONTRIBUTING.md)
- [Security reporting](SECURITY.md)

Project license: [CC BY-NC 4.0](LICENSE.md).
