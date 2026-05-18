# Control Scenario: Audit (no blueprint)

## Setup

Run in an **empty temporary directory with no `.claude/` files**. Create a few plain files: `README.md`, a stub `requirements.txt`, and a stub `adr-001.txt`. Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

```
Audit this project for consistency, completeness, and quality. Check that documentation matches what exists, flag anything broken or missing.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/commands/audit.md`. Score each item against bare Claude Code output.

## Scoring notes

Items 1 (reading audit-log first), 4 (fidelity rating), 5 (audit-log format + PASS/FAIL), and 10 (HEAD hash) will reliably **fail**. Items 2–3 (map staleness, category coverage) and 7–9 (broken links, ADR completeness, evidence for PASS) may partially pass. The delta measures whether the blueprint audit command adds systematic coverage and durable findings.
