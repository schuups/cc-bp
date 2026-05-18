# Scenario: /ccbp:overview

## Context

A session is starting on a project in mid-development. Knowledge files are partially populated: some requirements exist, attack and audit logs have entries, backlog has items, project-map may be stale.

## Setup

Run in this repository (blueprint installed). Start a fresh Claude Code session. The trigger is the mandatory session-start invocation of `/ccbp:overview`.

## Trigger prompt

```
/ccbp:overview
```

## Expected artifacts

- Report box in the format defined in `commands/overview.md`
- All 6 knowledge files read before output is produced
- Concrete recommended action in Next steps

## Rubric

Score each item pass (✓) or fail (✗). Record as `N / 10`.

| # | Item | Dimension |
|---|------|-----------|
| 1 | Read all 6 required knowledge files before producing the report | Quality |
| 2 | Correctly identified map status (MISSING / STALE / current with date) | Quality |
| 3 | Output matches the defined report box format (all labeled fields present) | Quality |
| 4 | Correctly reported open findings count by severity, or stated NEVER RUN | Quality |
| 5 | Correctly reported requirements count by status | Quality |
| 6 | Surfaced any CRITICAL findings prominently below the box | Robustness |
| 7 | Checked log sizes and flagged any file over 30 `##` entries | Robustness |
| 8 | Provided ≥ 1 concrete recommended action in Next steps | Quality |
| 9 | Did NOT modify any files | Sustainability |
| 10 | Did not read files outside those listed in the command spec | Speed |
