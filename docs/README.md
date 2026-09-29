# Documentation index

**Owner:** Azalea maintainers | **Last reviewed:** 2026-09-29 | **Status:** Current

| Area                           | Purpose                                                      |
|--------------------------------|--------------------------------------------------------------|
| [architecture/](architecture/) | How the system is shaped and why                             |
| [development/](development/)   | Working in the codebase day to day                           |
| [components/](components/)     | Runtime, configuration, persistence, and observability       |
| [operations/](operations/)     | Running, deploying, and recovering the system                |
| [policies/](policies/)         | Dependency and data-handling practices                       |
| [_templates/](_templates/)     | Templates for new component, incident, runbook, and RFC docs |

Also: [ownership.md](ownership.md), [glossary.md](glossary.md)

Quick references: [commands](commands.md), [configuration](operations/config.md),
and [deployment](operations/deployment.md).

## Which page to update

| Change                                                                             | Update these docs                                                                                                                                                                                                        |
|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Add/change a slash or context-menu command, button, modal, or permission check     | [Commands](commands.md); if registration/dispatch behavior changes, [Discord runtime](components/discord-runtime.md)                                                                                                     |
| Add/change an environment variable or YAML field/default/permission/log event      | [Configuration and secrets](operations/config.md), [configuration component](components/configuration.md), and `../.env.example` or representative config                                                                |
| Change a database model, migration, message retention, or persisted behavior       | [Persistence](components/persistence.md), [data flow](architecture/data-flow.md), and [privacy](policies/privacy.md); also update the root [privacy policy](../PRIVACY_POLICY.md) when data/retention disclosures change |
| Change runtime startup, process topology, integrations, or a durable design choice | [Architecture overview](architecture/overview.md), relevant [component](components/), and an [ADR](architecture/decisions/README.md)                                                                                     |
| Change CI, release, deploy, health checks, or recovery                             | Relevant page in [operations](operations/), plus a [runbook](operations/runbooks/README.md) if operator actions change                                                                                                   |
| Change dependencies, test commands, code layout, or contribution workflow          | [Dependency policy](policies/dependencies.md), [testing](development/testing.md), [conventions](development/conventions.md), or [CONTRIBUTING](../CONTRIBUTING.md), as applicable                                        |

Update only the pages affected. Use the linked implementation/schema as the source of truth; don't
copy a full schema into multiple docs. `operations/config.md` is the human-facing config reference;
`components/configuration.md` explains ownership and change procedure.

## Rules

1. Update documentation in the same pull request as behavior, configuration, or interface changes.
2. Keep owner and review-date metadata current on operational and policy pages.
3. Record significant architectural choices as ADRs. Existing ADRs inferred from the code are
   labeled as such; they do not claim historical intent.
4. Re-review docs at least every six months and whenever a documented interface changes.

The existing reference pages in the repository's `docs/` directory are retained while this new
documentation set is developed. If this set is adopted as the root docs, remove this transition
note.
