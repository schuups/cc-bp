# Control Scenario: Amendment

## Setup

Run in an **empty temporary directory with no `.claude/` files**. Create a plain `requirements.txt` or `README.md` with 5 informal requirement descriptions (no IDs, no status). Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

Same as `benchmarks/scenarios/amendment.md`:

```
We need to add email notification support. When a document changes status (submitted, approved, rejected), the submitter and all assigned approvers should receive an email.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/amendment.md`. Score each item against bare Claude Code output.

## Scoring notes

Items 1 (amendment recognition), 7 (scope document), and 10 (interaction-log) will reliably **fail** — these are blueprint-specific behaviors. Items 3–6 (reading existing state, identifying impacts) may partially pass if Claude reads the provided file. The delta measures the amendment phase scaffolding contribution.
