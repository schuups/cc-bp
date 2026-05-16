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

1. `.claude/knowledge/context/project-context.md` — purpose, users, constraints, dependencies, execution context
2. `.claude/knowledge/context/requirements.md` — full functional requirements inventory with status (implemented / planned / deferred)
3. `.claude/knowledge/context/invariants.md` — any invariants identifiable from requirements (may be empty)
4. `.claude/knowledge/context/architecture.md` — system boundaries, components, key decisions
5. `.claude/knowledge/coding-guide.md` — language-specific rules
6. `.claude/knowledge/deferred.md` — items explicitly identified as deferred
7. `.claude/knowledge/context/adr/` — initialise README if not present
8. `.claude/knowledge/health/` — initialise empty template files if not present

Show a summary and ask the user to confirm before finalising.

## Phase 5 — Self-Cleanup

Once the user confirms onboarding is complete:
1. Remove the `/onboard` row from the Commands table in `.claude/CLAUDE_NEXT.md`
2. Delete this file: `.claude/commands/onboard.md`

This command is single-use. Removing it prevents it from loading into context in every future session.
