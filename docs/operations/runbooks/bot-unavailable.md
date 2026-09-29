# Runbook: Bot unavailable

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

## Symptoms

Discord shows the bot offline, interactions fail, the process repeatedly exits, or loopback
`/healthz` cannot connect / remains `ready: false`.

## Impact

Moderation and utility commands, event handling, and scheduled tasks may not be running. Assess
whether guild staff need an alternate moderation path.

## Diagnosis

1. Check PM2/container status and latest stdout/stderr for boot errors or restarts.
2. Query `http://127.0.0.1:7475/healthz` on the host; check bind settings and whether readiness was
   reached.
3. Confirm `DISCORD_TOKEN`, Discord connectivity, global/guild config files, and access to required
   guilds.
4. If boot reaches database access, check `DATABASE_URL`, directory permissions, disk space, and
   migration state.

## Mitigation

Stop a crash loop if it is repeatedly applying an unsafe action. Correct missing credentials/config
or restore host/network connectivity, then start/reload once and observe logs. Keep the health
service private.

## Verification

Verify the process remains online, `/healthz` reports `ready: true`, the bot is online in Discord,
registered commands respond, and no new boot errors appear.

## Escalation

Contact the bot host/repository maintainer. Follow [incident response](../incident-response.md) if
moderation actions, credentials, or stored data may be compromised.
