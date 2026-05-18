# Token Analysis

Per-file and per-command token load estimates for the blueprint. Updated on each benchmark run.
Token estimate: `chars ÷ 4` (English/markdown approximation).

Last measured: 2026-05-18 · commit b0f766a

---

## How to Update

For each file, run `wc -c <path>` and divide by 4. Record the result. For per-command totals,
sum the command file plus all knowledge files that command explicitly reads (see per-command tables below).
Variable loads (target files, source files, ADR contents) are noted separately — do not include them
in the base totals.

---

## Constant Session Load

Files loaded into context for every Claude Code session in this project.

| File | Chars | Tokens |
|------|-------|--------|
| `.claude/CLAUDE.md` | 19,251 | 4,812 |
| `CLAUDE.md` | 2,017 | 504 |
| **Total** | **21,268** | **5,316** |

Ceiling from `eval-criteria.md`: **6,000 tokens**.
Compaction target: **3,500 tokens**.

---

## Per-Command Load

Base load = command file + all knowledge files that command explicitly reads.
Variable load (source files, target files, ADR bodies) is noted but not totalled — it depends on project state.

### `/ccbp:overview`

| File | Tokens |
|------|--------|
| `commands/overview.md` | 1,420 |
| `knowledge/project-map.md` | 17 |
| `knowledge/interaction-log.md` | 42 |
| `knowledge/requirements.md` | 712 |
| `knowledge/attack-log.md` | 24 |
| `knowledge/audit-log.md` | 23 |
| `knowledge/backlog.md` | 389 |
| **Base total** | **2,627** |
| Variable | `knowledge/adr/INDEX.md` (306) + any Proposed ADR files |

### `/ccbp:attack`

| File | Tokens |
|------|--------|
| `commands/attack.md` | 1,929 |
| `knowledge/attack-log.md` | 24 |
| **Base total** | **1,953** |
| Variable | target files (focused mode) or all component files (sweep mode) + `roles/attacker.md` (794) |

### `/ccbp:audit`

| File | Tokens |
|------|--------|
| `commands/audit.md` | 2,069 |
| `knowledge/project-map.md` | 17 |
| `knowledge/audit-log.md` | 23 |
| `knowledge/requirements.md` | 712 |
| `knowledge/architecture.md` | 1,508 |
| `knowledge/attack-log.md` | 24 |
| `knowledge/adr/INDEX.md` | 306 |
| `knowledge/interaction-log.md` | 42 |
| `knowledge/backlog.md` | 389 |
| **Base total** | **5,090** |
| Variable | accepted ADR bodies · source files in scope · `documentation/` files |

### `/ccbp:checkpoint`

| File | Tokens |
|------|--------|
| `commands/checkpoint.md` | 2,744 |
| `knowledge/interaction-log.md` | 42 |
| `knowledge/requirements.md` | 712 |
| `knowledge/architecture.md` | 1,508 |
| `knowledge/adr/INDEX.md` | 306 |
| `knowledge/attack-log.md` | 24 |
| `knowledge/audit-log.md` | 23 |
| **Base total** | **5,359** |
| Variable | changed source files in diff · accepted ADR bodies |

### `/ccbp:map`

| File | Tokens |
|------|--------|
| `commands/map.md` | 615 |
| **Base total** | **615** |
| Variable | all project files (reads everything to build the map) |

### `/ccbp:cleanup`

| File | Tokens |
|------|--------|
| `commands/cleanup.md` | 895 |
| `knowledge/project-map.md` | 17 |
| `knowledge/interaction-log.md` | 42 |
| `knowledge/attack-log.md` | 24 |
| `knowledge/audit-log.md` | 23 |
| `knowledge/requirements.md` | 712 |
| `knowledge/backlog.md` | 389 |
| `knowledge/adr/INDEX.md` | 306 |
| **Base total** | **2,408** |

### `/ccbp:resume`

| File | Tokens |
|------|--------|
| `commands/resume.md` | 553 |
| `knowledge/interaction-log.md` | 42 |
| `knowledge/project-map.md` | 17 |
| `knowledge/requirements.md` | 712 |
| `knowledge/attack-log.md` | 24 |
| `knowledge/audit-log.md` | 23 |
| **Base total** | **1,371** |
| Variable | files relevant to the paused work |

### `/ccbp:pause`

| File | Tokens |
|------|--------|
| `commands/pause.md` | 343 |
| **Base total** | **343** |
| Variable | writes to `knowledge/interaction-log.md` (no additional reads) |

### `/ccbp:onboard`

| File | Tokens |
|------|--------|
| `commands/onboard.md` | 1,694 |
| **Base total** | **1,694** |
| Variable | project source files for brownfield analysis |

### `/ccbp:docs`

| File | Tokens |
|------|--------|
| `commands/docs.md` | 765 |
| **Base total** | **765** |
| Variable | all `knowledge/` files + existing `documentation/` |

---

## Role Files

Loaded when a role activates (on demand, not constant).

| Role | Chars | Tokens |
|------|-------|--------|
| `roles/analyst.md` | 5,596 | 1,399 |
| `roles/implementer.md` | 5,341 | 1,335 |
| `roles/auditor.md` | 4,404 | 1,101 |
| `roles/integrator.md` | 4,045 | 1,011 |
| `roles/architect.md` | 3,786 | 946 |
| `roles/attacker.md` | 3,176 | 794 |
| `roles/researcher.md` | 2,409 | 602 |
| `roles/documentarian.md` | 2,288 | 572 |
| **Average** | **3,880** | **970** |
| **All roles total** | **31,045** | **7,760** |

Ceiling from `eval-criteria.md`: **1,200 tokens per role file**.

---

## Typical Workflow Estimates

Cumulative context load for a typical session, combining constant load + one command + one role.
Does not include variable loads (source files, ADR bodies, target files).

| Workflow | Constant | Command base | Role | Total |
|----------|----------|-------------|------|-------|
| Session start only | 5,316 | — | — | 5,316 |
| Start + overview | 5,316 | 2,627 | — | 7,943 |
| Start + overview + analyst | 5,316 | 2,627 | 1,399 | 9,342 |
| Start + attack (focused) | 5,316 | 1,953 | 794 | 8,063 |
| Start + audit (full) | 5,316 | 5,090 | 1,101 | 11,507 |
| Start + checkpoint | 5,316 | 5,359 | — | 10,675 |

Ceiling from `eval-criteria.md`: **6,000 tokens** constant · **3,200 tokens** any single command.
Note: audit and checkpoint base loads exceed the per-command ceiling — this is a pre-compaction finding.
