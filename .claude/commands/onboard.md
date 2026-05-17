# Command: /onboard

Personalises the blueprint for the specific project. Run immediately after copying the blueprint's `.claude` folder into the target project.

## Phase 1 — Analysis

Read and analyse before asking anything. The project folder may be empty (greenfield) — if so, skip directly to Phase 2.

- Source files, package/config files, existing docs, git log, CI/CD configuration
- Determine: project maturity, language(s), frameworks, test structure, project type

## Phase 2 — Discovery

Iterate with the user until all of the following are unambiguous:
- **Purpose and goals**: what the project is, who it serves, what success looks like
- **Feature inventory**: all features conceived so far — implemented, planned, and deferred
- **Execution context**: what systems are involved in running this software (laptop only, HPC cluster, LAN server with GPU, multiple nodes, cloud, etc.)
- **Constraints**: technical, regulatory, organisational

Keep asking and refining until there are no open ambiguities. Do not proceed until this picture is clear.

## Phase 3 — Setup Questions

Ask only what Phases 1–2 did not resolve:
- **Git workflow**: trunk-based vs feature branches + PRs
- **Language(s)**: if not detected

## Phase 4 — Populate Knowledge Files

The root `CLAUDE.md` Project Context template is a starting point, not a fixed contract. During onboarding, adapt the fields to fit the project — add fields that matter, remove ones that don't apply, rename for clarity. The goal is a Project Context that a future Claude session can rely on, not one that matches a template exactly.

1. Root `CLAUDE.md` — fill in (and adapt) the **Project Context** section; at minimum: Vision, Description, Success criteria, Stakeholders, Technologies, Execution context, Key business constraints, Explicitly out of scope
2. `.claude/knowledge/requirements.md` — full functional requirements inventory with status (implemented / planned / deferred)
3. `.claude/knowledge/architecture.md` — system boundaries, components, key decisions
4. `.claude/knowledge/coding-guide.md` — language-specific rules
5. `.claude/knowledge/backlog.md` — items explicitly identified as deferred
6. `.claude/knowledge/domain.md` — domain knowledge captured during discovery (may be sparse initially)
7. `.claude/knowledge/adr/` — initialise INDEX.md if not present

Show a summary and ask the user to confirm before finalising.

## Phase 5 — Self-Cleanup

Once the user confirms onboarding is complete:
1. Remove the `/ccbp:onboard` row from the Utility Commands table in `.claude/CLAUDE.md`
2. Remove the `/onboard` line from the Command Triggers section in `.claude/CLAUDE.md`
3. Delete this file: `.claude/commands/onboard.md`

This command is single-use. Once these three steps are done, no trace of onboarding remains in the blueprint. The presence of this file is the canonical signal that onboarding has not yet run — its absence means onboarding is complete.
