# ADR-NNNN: [Short title — imperative phrase]

> **Purpose:** Records a single architecture decision with enough context that a future reader can understand why it was made and evaluate whether it still holds. The acceptance criteria are the contract between architect and auditor — vague criteria produce shallow reviews.
>
> **Producers:** `architect` role; one ADR per decision; file name matches ID (e.g. `0042-use-postgres.md`).
>
> **Consumers:** `implementer` (reads before coding), `auditor` (verifies acceptance criteria), `attacker` (stress-tests the decision), `architect` (checks for supersession chains).
>
> **Status lifecycle:**
> - `Proposed` — drafted; awaiting `/ccbp:attack` pass; no implementation should proceed
> - `Accepted` — attack pass complete and findings addressed; implementation may proceed
> - `Superseded` — replaced by a newer ADR; add `Superseded by: ADR-NNNN` below the status line and update INDEX.md
> - `Deprecated` — decision no longer applies; add a one-line explanation below the status line and update INDEX.md

**Date:** YYYY-MM-DD
**Status:** Proposed

---

## Context

[One to three sentences: what situation or constraint requires a decision?]

## Decision

[One to two sentences: what was decided?]

---

## Options Considered

### Option A: [Name]

[Description in ≤3 sentences.]

**Pros:**
-

**Cons:**
-

**Invariant impact:** [preserves / risks / introduces] — [which invariant(s), or "none"]

**Consequences Map:**
- Intended: [the stated goal — what should change]
- Unintended first-order: [direct side effects]
- Unintended second-order: [downstream effects from those side effects]

---

### Option B: [Name]

[Description in ≤3 sentences.]

**Pros:**
-

**Cons:**
-

**Invariant impact:** [preserves / risks / introduces] — [which invariant(s), or "none"]

**Consequences Map:**
- Intended: [the stated goal — what should change]
- Unintended first-order: [direct side effects]
- Unintended second-order: [downstream effects from those side effects]

---

## Rationale

[Why was this option chosen? Base the argument on consequence/risk profile, not elegance or speed.]

## Acceptance Criteria

[Explicit, testable conditions that must be true for this decision to be considered correctly implemented.
The Auditor and Adversary use these to evaluate the implementation — vague criteria produce shallow reviews.

**At least one criterion must be grep-verifiable:** it must name a specific symbol, function, class, test name, or configuration key that can be checked mechanically. Examples:
- `grep -r 'RetryHandler' src/` returns ≥ 1 result
- test `test_circuit_breaker_opens_after_threshold` exists and passes
- `config/settings.yml` contains key `max_retries`

Narrative-only criteria ("the system handles errors gracefully") are not acceptable as the sole criterion.]

- [ ] [Criterion 1 — grep-verifiable: e.g. `grep 'SymbolName' src/` or test `test_name` passes]
- [ ] [Criterion 2 — narrative or additional verifiable]

## New Invariants

[List any invariants this decision introduces, or state "None."]

## References

<!-- ADR-NNNN, INV-NNN, or external links -->
