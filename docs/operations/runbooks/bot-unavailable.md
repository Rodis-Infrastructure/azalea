# Runbook: Bot unavailable

**Owner:** Azalea maintainers | **Last reviewed:** 2026-10-07 | **Status:** Current

**Symptoms:** the bot is offline in Discord, interactions fail, the process keeps restarting, or
`/healthz` is unreachable or stuck at `ready: false`. Commands, events, and scheduled tasks may
all be down; tell guild staff if they need another way to moderate.

## Diagnosis

1. Check PM2/container status and the latest logs for startup errors or restart loops.
2. On the host, run `curl http://127.0.0.1:7475/healthz`.
3. Check `DISCORD_TOKEN`, network access to Discord, the config files, and that the bot is still in
   the configured guilds.
4. If the error is from the database, follow the [database runbook](database-failure.md).

## Mitigation

Fix the credentials, config, or connectivity, then start or reload once and watch the logs. Stop a
crash loop if each restart repeats a harmful action.

## Verification

The process stays up, `/healthz` reports `ready: true`, the bot shows online, and commands
respond.

Escalate to the maintainers, and follow [incident response](../incident-response.md) if moderation
actions, credentials, or data may be affected.
