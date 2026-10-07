# Documentation index

| Area                           | Purpose                                                      |
|--------------------------------|--------------------------------------------------------------|
| [architecture/](architecture/) | How the system is shaped and why                             |
| [development/](development/)   | Working in the codebase day to day                           |
| [components/](components/)     | Runtime, configuration, persistence, and observability       |
| [operations/](operations/)     | Running, deploying, and recovering the system                |
| [policies/](policies/)         | Dependency and data-handling practices                       |
| [_templates/](_templates/)     | Templates for new component, incident, runbook, and RFC docs |

Also: [commands](commands.md), [ownership](ownership.md), [glossary](glossary.md).

## Which page to update

| Change                                                                         | Update                                                                                                                                                   |
|--------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Command, button, modal, or permission check                                    | [Commands](commands.md); [Discord runtime](components/discord-runtime.md) if registration/dispatch changes                                               |
| Environment variable or YAML field/default/permission/log event                | [Configuration and secrets](operations/config.md) and `.env.example`                                                                                    |
| Database model, migration, message retention, or persisted behavior            | [Persistence](components/persistence.md), [privacy](policies/privacy.md), and the root [privacy policy](../PRIVACY_POLICY.md) if disclosures change |
| Startup, process topology, integrations, or a durable design choice            | [Architecture overview](architecture/overview.md), the relevant [component](components/), and an [ADR](architecture/decisions/README.md)               |
| CI, release, deploy, health checks, or recovery                                | The relevant [operations](operations/) page, plus a [runbook](operations/runbooks/README.md) if operator actions change                                 |
| Dependencies, test commands, code layout, or contribution workflow             | [Dependency policy](policies/dependencies.md), [testing](development/testing.md), [conventions](development/conventions.md), or [CONTRIBUTING](../CONTRIBUTING.md) |

Link to the implementation or schema as the source of truth rather than copying it into several pages.

## Rules

1. Update docs in the same pull request as the behavior, configuration, or interface change.
2. Keep the owner and review-date header current on operations and policy pages.
3. Record significant architectural choices as ADRs.
4. Re-review docs at least every six months.
