# Assumptions

Explicit assumptions the design rests on. Distinct from constraints (constraints are known hard requirements; assumptions are things believed to be true that have not been fully verified).

Every assumption carries risk. An unverified assumption that turns out to be wrong is a future bug or design failure. Surface them here so they can be validated or invalidated deliberately.

---

## How to Use This File

- **Adding an assumption:** append a new `ASM-NNN` block. Never renumber existing entries.
- **Validating an assumption:** update its `Validation status` to `Confirmed` with evidence.
- **Invalidating an assumption:** mark it `[INVALIDATED]` with date and consequence. Never delete it.
- **Referencing from ADRs:** use the `ASM-NNN` identifier — a design that rests on an assumption should say so.

---

## Template

```
### ASM-NNN: <Short title>

**Statement:** <What we are assuming to be true>
**Rationale:** <Why this assumption is reasonable / where it comes from>
**Risk if wrong:** <What breaks or changes if this assumption is false>
**Validation status:** Unverified | Confirmed: <evidence> | [INVALIDATED]: <date, consequence>
**Added:** YYYY-MM-DD | **ADR reference:** <NNNN or "none">
```

---

## Assumptions

<!--
Replace the template entry below with your first real assumption.
Add subsequent assumptions as ASM-002, ASM-003, etc.
-->

### ASM-001: `<Short title — replace this>`

**Statement:** <!-- what we assume to be true -->
**Rationale:** <!-- why this is a reasonable assumption -->
**Risk if wrong:** <!-- what breaks -->
**Validation status:** Unverified
**Added:** YYYY-MM-DD | **ADR reference:** none
