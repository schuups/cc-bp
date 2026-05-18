# Command: /ccbp:map

Generate a fresh, complete project map: files, purposes, dependencies, relationships. Must be consulted before any `global` scope action. Re-run if older than the most recent code change.

---

## Step 0 — Onboarding Check

If `onboard.md` still exists in `.claude/commands/`: onboarding is not complete. Stop and prompt the user to run `/onboard` first.

---

## Step 1 — Scan the Repository

Read and catalogue:
- All source files: language, purpose, key exports/symbols
- All test files: what they cover
- All configuration files: what they control
- All documentation files: what they describe
- `.claude/knowledge/` files: current state of requirements, architecture, ADRs

---

## Step 2 — Build the Map

Produce a structured map covering:

**Components** — named subsystems with their purpose and primary files

**Dependencies** — between components (A calls B, A imports B, A reads B's output)

**Entry points** — where execution begins (CLI, API endpoints, main functions, event handlers)

**External dependencies** — third-party libraries, services, APIs with their role

**Knowledge alignment** — flag any component or relationship that contradicts or is absent from `.claude/knowledge/architecture.md`

---

## Step 3 — Write to Log

Overwrite `.claude/knowledge/project-map.md` with:

```
## Project Map — YYYY-MM-DD HH:MM

### Components
<name> — <purpose>
  Files: <list>
  Depends on: <list>

...

### Entry Points
<list with description>

### External Dependencies
<name> — <role>

### Alignment Notes
<any divergence from architecture.md, or "aligned">
```

---

## Step 4 — Report

State: "Map written to `.claude/knowledge/project-map.md` — `<N>` components, `<N>` entry points." Flag any alignment gaps and ask whether to update `architecture.md`.

---

## Staleness check (for other commands)

Any command operating at `global` scope should run this check before proceeding:

1. Read `.claude/knowledge/project-map.md`. Extract the datetime from `## Project Map — YYYY-MM-DD HH:MM`.
2. Run `git log -1 --format=%ci` to get the last commit timestamp.
3. Evaluate:
   - **Map absent** → ⚠ "No project map found — run `/ccbp:map` before proceeding."
   - **Map older than last commit** → ⚠ "Map may be stale (generated: DATE · last commit: DATE). Run `/ccbp:map` to refresh, or proceed knowing the map may not reflect recent changes."
   - **Map up to date** → proceed silently.
