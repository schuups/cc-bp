# ADR Index

> **Purpose:** Master list of all Architecture Decision Records. Must be kept in sync with the files under `adr/` — every ADR file needs a row here, and every row must have a corresponding file. This index is the first place Claude checks when navigating decisions.
>
> **Producers:** `architect` role adds a row when creating a new ADR; `auditor` and `/ccbp:checkpoint` verify sync.
>
> **Consumers:** `/ccbp:overview` Step 6 (counts Proposed ADRs), `/ccbp:audit` Category 1 (index completeness check), `implementer` (looks up accepted decisions before coding), `attacker` (finds decisions to stress-test).
>
> **Status values** (must match the `Status:` field inside the ADR file):
> - `Proposed` — drafted and under review; an `/ccbp:attack` pass is pending before acceptance; no implementation should proceed
> - `Accepted` — approved and locked in; implementation may proceed against its acceptance criteria
> - `Superseded` — replaced by a newer ADR; the ADR file must name the successor with `Superseded by: ADR-NNNN`
> - `Deprecated` — the decision no longer applies (e.g. the component was removed); the ADR file must explain why

| ID | Title | Status | Date |
|----|-------|--------|------|
