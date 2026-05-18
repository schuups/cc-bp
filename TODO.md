# TODO

Status: 🔴 open · 🟠 in progress · ✅ closed

Attack sweep completed: 2026-05-17

---

1. 🟠 **Blueprint benchmarking** — design and run a test suite that measures the blueprint's effectiveness at supporting projects across each phase of the lifecycle (inception, research, design, architecture, implementation, deployment, maintenance, assess). Each phase must be evaluated on two dimensions: **utility** (did Claude produce the right artifact, flag the right issues, follow the correct role constraints?) and **tokens** (constant session load and per-command load at that phase). Results move to `backlog.md` with a concrete re-run criterion once the baseline is established.

2. 🔴 **Compaction and simplification** — the blueprint files have grown verbose through iterative additions. Review all command and role files for redundancy, over-specification, and prose that restates what the state machine already enforces. Also identify and strip elements that can be removed entirely. Goal: reduce token load per session without losing any behavioural guarantee.

3. ✅ **Add `assess` phase** — added to phase inventory in `.claude/CLAUDE.md` with entry criteria ("is this right?" not "what shall I change?"), analyst as primary role, findings to `knowledge/domain.md`, and three exit paths: → `amendment`, → `backlog`, or close. Noted as an additional entry point that does not enter the main sequence unless a change is decided.

4. ✅ **Rename and consolidate log files** — all files previously under `.claude/logs/` moved to `.claude/knowledge/`: `attack.md` → `attack-log.md`, `audit.md` → `audit-log.md`, `interaction.md` → `interaction-log.md`, `map.md` → `project-map.md` (not a log — renamed accordingly). `logs/` directory deleted. All path references updated across commands, roles, and `.claude/CLAUDE.md`.

---

## Gap Analysis — Structural / Coverage (evaluated 2026-05-17)

5. ✅ **Add lightweight decision track** — Added a three-tier decision table to `interaction-log.md` logging rules in `.claude/CLAUDE.md`: ADR (cross-component/API/stack impact), mid-tier decision (`## Decision — TITLE` entry in `interaction-log.md` with Choice / Alternatives / Rationale), micro-decision (not logged). Mid-tier format defined inline.

6. ✅ **Address role rigidity during maintenance/debugging** — Added maintenance mode exemption to the role section in `.claude/CLAUDE.md`: when phase is `maintenance`, scope is `local`, and no ADR is pending, analyst / implementer / auditor may interleave without explicit role transitions. Activity tracking and announcements still apply. Attacker and architect always require explicit transition.

7. ✅ **Add model card concept to documentarian role** — Added `:ml-engineer` declination to `roles/documentarian.md` (removing the "No declinations" note). Model card sections defined: model details, intended use (with explicit out-of-scope requirement), training data, performance, known limitations, bias and fairness, failure modes. Four binding checklist additions including cross-reference to `architecture.md` Failure Modes table.

---

## Compaction Analysis (evaluated 2026-05-17)

Goal: reduce constant token load per session without losing behavioural guarantees. Items are prioritized by impact — files loaded most often get addressed first.

### Constant session load (highest impact)

8. 🔴 **Compact `.claude/CLAUDE.md`** — This file is loaded every session and is the largest single source of constant overhead. Specific candidates:
   - **State Transition Triggers / Implicit Triggers section** (~200 tokens): restates what the decision checklist already enforces; redundant.
   - **Within-Response State Tracking section** (~100 tokens): repeats rules already stated in the Activity section above it.
   - **Role inference table** (~100 tokens): useful but could be compressed to a single line per role (remove the prose "Infer when…" column, keep keyword triggers only).
   - **Phase sequence narrative** (~80 tokens): the arrow diagram plus the prose paragraph below it cover the same information twice.
   - Estimated saving: **~400–500 tokens** per session.

9. 🔴 **Compact knowledge file prose headers** — Each knowledge file (requirements.md, architecture.md, domain.md, coding-guide.md, backlog.md) opens with a multi-line `> **Purpose:** / **Producers:** / **Consumers:** / **Column definitions:**` block. These are loaded on every `/ccbp:overview` run (6 files × ~80–120 tokens each). The information in these headers is redundant with `CLAUDE.md`'s Folders and Files section and the command descriptions. Move to a single `knowledge/README.md` read once during onboarding, or strip to a one-line comment. Estimated saving: **~500–600 tokens** per overview invocation.

### On-demand load (lower impact, still worth doing)

10. 🔴 **Compact role file declination boilerplate** — Every declination opens with "Only domain-specific additions; the contract above applies in full." This is implied by the structure and adds ~14 tokens × (7 roles × ~6 declinations) = ~600 tokens across all role files. The boilerplate is invisible when a role isn't active, but cleaning it improves readability and sets a tighter precedent.

11. 🔴 **Resolve attack vector duplication** — The attacker role says "Authoritative list and per-vector guidance: `.claude/commands/attack.md` Step 1. That table is the single source of truth — do not duplicate it here." This is correct design, but the nine vectors are then *named* in the attack command and implicitly referenced in the role's binding checklist. Verify there is no duplication in practice after compaction of CLAUDE.md; if the role's "Nine Attack Vectors" header is empty (it currently redirects), remove the heading entirely.

12. 🔴 **Compress `overview.md` command** — The overview command has 7 steps with detailed sub-bullets. Steps 2–6 are read-only file checks that could be expressed as a compact checklist with file paths and decision rules, rather than prose. Estimated saving: ~300 tokens per invocation (the command file is loaded when `/ccbp:overview` runs).
