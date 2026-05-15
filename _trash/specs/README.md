# Specs

The living specification for this project. All implementation, design, and audit work references this directory as ground truth.

## Contents

| File / Directory | Purpose |
|------------------|---------|
| `domain-model.md` | Entities, value objects, aggregates, domain events, and relationships |
| `ubiquitous-language.md` | Domain terms with precise meanings — maintained separately for enforcement |
| `invariants.md` | Rules that must hold at all times — never violated by any change |
| `assumptions.md` | Explicit assumptions the design rests on — tracked and validated over time |
| `failure-modes.md` | Known ways the system can fail, populated by adversary sweeps |
| `features/` | *(optional)* BDD scenarios as living spec — populate if the project uses Gherkin/BDD |
| `architecture/` | *(optional)* Module map, dependency graph, interfaces, error taxonomy — for multi-module projects |
| `adr/` | Architecture Decision Records — one file per decision, immutable once Accepted |
| `fidelity-index.md` | Coverage tracker: THOROUGH / MODERATE / LOW / NONE per spec entry, updated by Sweep |
| `findings.md` | Append-only log of adversary and auditor findings across all sessions |
| `escalations.md` | Blockers and unresolved issues requiring user decision |
| `deferred.md` | Triaged work items explicitly set aside — not blocked, not forgotten, not now |

## Core Conventions

**Language-agnostic.** Specs describe behavior and intent, not implementation details or technology choices.

**No stale specs.** If the implementation diverges from a spec entry, that is a finding to record — not a reason to leave the spec outdated, and not a reason to silently update the spec to match the code.

**ADR decisions are immutable.** A superseded ADR is marked `Superseded by NNNN` and kept for history. Never delete or overwrite a past decision.

**Findings are append-only.** Never edit a past finding entry. Add a follow-up entry that references the original.

**Escalations require acknowledgment.** An escalation stays open until the user explicitly closes it. An open escalation blocks the mode that raised it from advancing.

## Starting from Scratch (Brownfield → Baselined)

1. Run **Design mode** to produce `domain-model.md`, `invariants.md`, and `ubiquitous-language.md`
2. Run **Sweep mode** to populate `fidelity-index.md` and surface divergences into `findings.md`
3. Run **AdvSweep** to populate `failure-modes.md` and find structural weaknesses
4. Triage `findings.md` — resolve CRITICAL and HIGH findings via Feature or Bugfix mode sessions
5. When `fidelity-index.md` has no `NONE`-rated items, the project is **Baselined**

## When to Re-Sweep

A sweep is not a one-time setup. Run a new Sweep (and optionally AdvSweep) when:

- Major refactoring is completed
- More than 50 commits have accumulated since the last checkpoint
- Before a release or significant milestone
- A domain invariant changes
- Trust is lost — tests pass but something feels wrong

## Adapting to an Existing Docs Structure

If the project already has a `docs/` directory with ADRs, architecture diagrams, or runbooks, this `specs/` directory coexists with it. `specs/` holds the behavioral ground truth (what the system must do and must never do); `docs/` holds explanatory and reference material (how it works, how to operate it). Do not merge them.
