# Failure Modes

Known ways the system can fail — including expected, designed-for failures. Distinct from `findings.md` (which logs defects found during review). Failure modes are the system's known failure envelope: how it breaks under stress, bad input, dependency failure, or operator error.

This file is populated and updated by AdvSweep and adversary gates. It is referenced by the adversary role when scoping attack vectors.

---

## How to Use This File

- **Adding a failure mode:** append a new `FM-NNN` block. Never renumber.
- **Resolving a failure mode:** add a `Mitigation` entry with the mechanism. The mode remains documented even when mitigated.
- **Referencing from findings:** use `FM-NNN` in a finding to indicate it exploits a known failure mode.
- **Referencing from ADRs:** a design decision that creates or closes a failure mode should reference it.

---

## Severity Levels

| Severity | Meaning |
|----------|---------|
| CRITICAL | Data loss, security breach, or full system unavailability |
| HIGH | Significant degradation or incorrect results for a material set of inputs |
| MEDIUM | Partial failure; workaround exists |
| LOW | Minor impact; cosmetic or edge-case only |

---

## Template

```
### FM-NNN: <Short title>

**Trigger:** <What condition causes this failure>
**Impact:** <What breaks or degrades when this occurs>
**Severity:** CRITICAL | HIGH | MEDIUM | LOW
**Affected invariants:** <INV-NNN list, or "none">
**Mitigation:** <How it is handled, or "None — unmitigated">
**Detection:** <How this failure mode is observable in production, or "Unknown">
**Added:** YYYY-MM-DD | **Source:** Adversary sweep | ADR-NNNN | Manual
```

---

## Failure Modes

<!--
Replace the template entry below with your first real failure mode.
Add subsequent entries as FM-002, FM-003, etc.
-->

### FM-001: `<Short title — replace this>`

**Trigger:** <!-- what causes this failure -->
**Impact:** <!-- what breaks -->
**Severity:** MEDIUM
**Affected invariants:** none
**Mitigation:** None — unmitigated
**Detection:** Unknown
**Added:** YYYY-MM-DD | **Source:** Manual
