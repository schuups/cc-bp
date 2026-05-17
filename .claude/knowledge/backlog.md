# Deferred Work

> **Purpose:** Items that are explicitly out of scope for now but are not abandoned. Every entry represents a deliberate decision to defer — not an oversight. Claude reads this file to understand what is intentionally off the table and under what conditions it should be reconsidered.
>
> **Producers:** Any role that identifies work to set aside; `/onboard` for items out of scope for the initial build; the `analyst` role when re-scoping.
>
> **Consumers:** `/ccbp:overview` Step 5 (counts entries, flags activate-now), `/ccbp:cleanup` Step 2 (checks whether activation conditions are now met), `analyst` (before scoping new work, to avoid re-deferring already-deferred items).
>
> **Column definitions:**
> - `ID` — sequential identifier prefixed `DEF-` (e.g., `DEF-001`)
> - `Item` — concise description of the deferred work
> - `Urgency` — priority level once the activation condition is met:
>   - `HIGH` — address in the next cycle once activated
>   - `MEDIUM` — address within a few cycles once activated
>   - `LOW` — address opportunistically; no time pressure after activation
> - `Activate when` — the specific condition, event, or date that should trigger reconsideration; must be concrete and testable (e.g., "user count exceeds 1 000", "after v2.0 ships", "when payment module is implemented")
> - `Notes` — rationale for deferral; cross-references to ADRs or requirements if relevant

| ID | Item | Urgency | Activate when | Notes |
|----|------|---------|---------------|-------|
| DEF-001 | | LOW | | |
