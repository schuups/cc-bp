# Functional Requirements

> **Purpose:** The authoritative inventory of what the system must do. Architecture decisions, invariants, and ADRs are derived from this file. Every requirement must be stated in verifiable terms — not "fast" but "p99 < 200 ms"; not "secure" but "no unauthenticated write access to resource X."
>
> **Producers:** `/onboard` creates the initial inventory; the `analyst` role adds or refines entries as scope evolves; the `implementer` role updates statuses as work completes.
>
> **Consumers:** `auditor` (verifies implementation matches status), `/ccbp:checkpoint` Check 4 (spec consistency), `/ccbp:overview` Step 3 (counts by status), `architect` (derives decisions from requirements).
>
> **Status values:**
> - `implemented` — coded, tested, and verifiable in source; do not mark implemented until the code exists and passes review
> - `planned` — scoped and intended for the current or next cycle; implementation has not started
> - `deferred` — intentionally postponed; a corresponding entry must exist in `backlog.md` explaining why and under what condition it activates

| ID | Requirement | Status | Notes |
|----|-------------|--------|-------|
| REQ-001 | | planned | |

---

## Domain Invariants

> Business rules that must never be violated, regardless of user action or system state. Unlike functional requirements (what the system must do), domain invariants constrain what states the system may never enter. Producers: `analyst` role. Consumers: `auditor`, `architect` (derives validation and enforcement strategy from these).
>
> **Verifiable condition:** must be checkable via a test or observable externally — not "order total is correct" but "order total equals sum of line items; verified by `test_order_total_invariant`".
>
> **Status values:** `verified` — has a passing test or enforcement mechanism · `at-risk` — invariant stated but enforcement has gaps (SHALLOW fidelity or below) · `untested` — no test coverage exists

| ID | Invariant | Verifiable condition | Status |
|----|-----------|---------------------|--------|
|    |           |                     |        |

---

## Assumptions

> Beliefs the design rests on that have not yet been verified. An architecture-invalidating assumption is a CRITICAL risk. Produced by the `analyst` role. Consumers: `architect` (must validate or design around each assumption before committing to an approach), `attacker` (treats unvalidated assumptions as attack surface).
>
> **Status values:** `validated` — confirmed by evidence, test, or external source · `accepted` — unverified; risk consciously accepted with rationale · `unknown` — not yet assessed

| ID | Assumption | Status | Invalidation consequence |
|----|-----------|--------|--------------------------|
|    |           |        |                          |
