# Invariants

Rules that must hold at all times. A change that violates an invariant is a defect regardless of whether any test catches it.

Every adversary and auditor run checks the changeset against this list.

---

## How to Use This File

- **Adding an invariant:** append a new `INV-NNN` block below. Never renumber existing entries.
- **Retiring an invariant:** mark it `[RETIRED]` with a date and reason. Never delete it.
- **Referencing from ADRs:** use the `INV-NNN` identifier.
- **Referencing from code:** add a comment citing `INV-NNN` near the enforcement point.

---

## Template

```
### INV-NNN: <Short title>

**Rule:** <State the invariant as a "must" or "must never" statement — precise enough to test>
**Rationale:** <Why this rule exists — the consequence it prevents>
**Violation consequence:** <What breaks or what risk materializes if this is violated>
**Enforcement:** <Where/how it is enforced: constructor, guard clause, DB constraint, test, etc.>
**Added:** YYYY-MM-DD | **ADR reference:** <NNNN or "none">
```

---

## Invariants

<!--
Replace the template entry below with your first real invariant.
Add subsequent invariants as INV-002, INV-003, etc.
-->

### INV-001: `<Short title — replace this>`

**Rule:** <!-- must / must never ... -->
**Rationale:** <!-- why this rule exists -->
**Violation consequence:** <!-- what breaks -->
**Enforcement:** <!-- where it is enforced in code or infrastructure -->
**Added:** YYYY-MM-DD | **ADR reference:** none
