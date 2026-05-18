# Scenario: /ccbp:audit

## Context

Implementation is complete and the team is preparing to deploy. A full audit pass is required before deployment.

## Setup

Run in this repository (blueprint installed). Start a fresh Claude Code session. Knowledge files should be populated: at least 1 requirement (implemented), 1 ADR, and a project map.

## Trigger prompt

```
/ccbp:audit
```

## Expected artifacts

- Findings appended to `knowledge/audit-log.md` with PASS/FAIL verdict
- Fidelity ratings assigned per component
- HEAD commit hash recorded

## Rubric

Score each item pass (✓) or fail (✗). Record as `N / 10`.

| # | Item | Dimension |
|---|------|-----------|
| 1 | Read `knowledge/audit-log.md` before starting to avoid duplicating open items | Speed |
| 2 | Checked map staleness before proceeding | Quality |
| 3 | Covered all mandatory check categories defined in `commands/audit.md` | Quality |
| 4 | Assigned a fidelity rating (NONE / STUB / SHALLOW / MODERATE / THOROUGH) per component | Quality |
| 5 | Appended findings to `knowledge/audit-log.md` with PASS / FAIL verdict and Fidelity line | Sustainability |
| 6 | Surfaced CRITICAL and HIGH findings prominently | Robustness |
| 7 | Checked for broken markdown links in knowledge files | Quality |
| 8 | Verified ADR index completeness | Quality |
| 9 | Did NOT claim PASS without citing evidence | Robustness |
| 10 | Recorded the HEAD commit hash in the log entry | Sustainability |
