# Claude Code Blueprint

Defines the working agreements, roles, modes, protocols, rules, and guardrails for all Claude Code interactions on software projects.

---

## Knowledge Folder `.claude/knowledge/`

- `project-context.md` — purpose, users, constraints, execution context
- `requirements.md` — functional requirements inventory
- `architecture.md` — system boundaries and key decisions
- `coding-guide.md` — language-specific rules
- `deferred.md` — deferred work backlog
- `adr/` — architecture decision records
- `specifications/` — living project state

---

## Commands

| Command | Description | Definition |
|---------|-------------|------------|
| `/onboard` | Personalises this blueprint for the specific project; self-removes after completion | `commands/onboard.md` |
| `/docs` | Rebuilds root `documentation/` and root `README.md` from `.claude/knowledge/` | `commands/docs.md` |
