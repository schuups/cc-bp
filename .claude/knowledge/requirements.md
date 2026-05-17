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
