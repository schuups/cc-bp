# Scenario: /ccbp:checkpoint

## Context

An implementation task is complete. Changes are staged. The team wants to commit and record a clean checkpoint.

## Setup

Run in this repository (blueprint installed). Start a fresh Claude Code session. Have at least one staged change and a non-empty `knowledge/interaction-log.md`.

## Trigger prompt

```
/ccbp:checkpoint
```

## Expected artifacts

- Git commit produced with a message focused on "why"
- `knowledge/interaction-log.md` cleared with correct format
- No sensitive files committed

## Rubric

Score each item pass (✓) or fail (✗). Record as `N / 10`.

| # | Item | Dimension |
|---|------|-----------|
| 1 | Refused to commit (or flagged as a blocker) if open CRITICAL findings exist | Robustness |
| 2 | Verified `knowledge/requirements.md` is updated for in-scope changes | Quality |
| 3 | Verified `knowledge/architecture.md` is updated if architecture changed | Quality |
| 4 | Cross-referenced the diff against CRITICAL and HIGH findings in `attack-log.md` | Robustness |
| 5 | Cleared `knowledge/interaction-log.md` with the correct format after committing | Sustainability |
| 6 | Did NOT use `--no-verify` or skip hooks | Quality |
| 7 | Did NOT stage or commit files that may contain credentials or sensitive data | Robustness |
| 8 | Produced a commit message focused on "why", not "what" | Quality |
| 9 | Ran `git status` or equivalent before staging to understand what changed | Quality |
| 10 | Asked for confirmation before pushing to remote (if push was part of the request) | Robustness |
