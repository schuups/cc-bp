# Blueprint Benchmarks

Evaluation framework for measuring the effectiveness of the `.claude/` blueprint.

---

## Purpose

The blueprint's value proposition is that it pushes a development team to build **faster, more robust, better quality, and more sustainable products** than they would if working directly with Claude Code without scaffolding.

**Faster** — The phase structure and role contracts surface the right questions upfront, reducing costly rework later. Claude does not jump to implementation before the problem is understood.

**More robust** — Mandatory gates (attack passes, audit passes), adversarial roles, and structured invariants catch problems that would otherwise reach implementation or production.

**Better quality** — Role contracts enforce artifact standards: requirements are verifiable, ADRs capture rationale, architecture reflects reality as built, not design intent.

**More sustainable** — Decisions are recorded durably. Future sessions pick up context without re-deriving it. The state machine prevents context drift across sessions.

---

## What This Measures

### Utility delta

The core measurement is the **utility delta**: the difference in rubric score between a blueprint-equipped session and a naive Claude Code session (the control) running the same trigger prompt.

The **rubric is the measurement instrument**. Each rubric item operationalizes one of the four dimensions above. By running identical trigger prompts in both conditions, the score difference is the observable utility advantage of the blueprint.

- Items the blueprint passes and the control fails → the blueprint's direct contribution
- Items both pass → confirms no regression; not evidence of added value
- Items the blueprint fails → friction or over-engineering signals worth investigating

Control sessions run in an empty directory with no `.claude/` files — the same Claude model, the same trigger prompt, no scaffolding.

### Token load

The second measurement tracks the token cost of the blueprint: constant session load (files always loaded at session start) and per-command load (files loaded when a specific command runs). This number feeds the compaction effort (TODO item 2) — the goal is to reduce token load without reducing utility delta.

---

## Methodology

Each scenario consists of:
- A **trigger prompt** — generic, no domain dependency; simulates entry into a specific phase or command
- A **rubric** — binary checklist scored `pass / fail` per item; total = `N passed / M total`
- A **control variant** — the same trigger prompt run without the blueprint

Scores are recorded in `runs/summary.md` as `N/M` per scenario per condition, keyed to a git commit hash and date+time. This makes before/after comparisons unambiguous across blueprint versions.

### Scenarios covered

| Category | Scenarios |
|----------|-----------|
| Phases | inception, amendment, research, design, architecture, implementation, deployment, maintenance, assess |
| Special modes | autonomous |
| Commands | overview, map, attack, audit, checkpoint |

---

## Evaluation Criteria

See `eval-criteria.md` for the fixed utility threshold, token budget ceiling, and control delta requirement that define a passing run. These criteria are set once and must not change mid-run.

---

## How to Run

1. **Blueprint run**: Open each scenario in `scenarios/`. Run the trigger prompt in this repository (which has the blueprint installed). Score the rubric after the session.
2. **Control run**: Open the matching file in `control/scenarios/`. Run the same trigger prompt in a fresh Claude Code session in an empty directory with no `.claude/` files. Score the same rubric.
3. Record both scores in `runs/summary.md` using the run template at the top of that file.
4. Update `token-analysis.md` with current file token counts.

A full benchmark run covers all scenarios in both conditions plus a token analysis update.

---

## Segregation

This directory is **not part of the blueprint artifact**. When applying the blueprint to a new project, copy only `.claude/` and `CLAUDE.md` — do not copy `benchmarks/`.

Benchmark runs do not write outside this directory. They report findings only. The user decides what action, if any, to take on the results.
