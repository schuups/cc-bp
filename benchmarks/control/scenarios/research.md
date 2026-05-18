# Control Scenario: Research

## Setup

Run in an **empty temporary directory with no `.claude/` files**. Start a fresh Claude Code session. No blueprint installed.

## Trigger prompt

Same as `benchmarks/scenarios/research.md`:

```
Before we decide on email delivery, research the main approaches for sending transactional emails from a backend service. I want to understand the trade-offs before we commit to an approach.
```

## Rubric

Use the identical rubric from `benchmarks/scenarios/research.md`. Score each item against bare Claude Code output.

## Scoring notes

Item 1 (role transition), items 4–5 (domain.md format, Confidence levels), and item 10 (source citation) will reliably **fail** — these are blueprint-specific. Items 2–3 (surveying approaches, rejecting alternatives) and item 8 (deferring design decisions) may pass if Claude's default research behavior is thorough. The delta measures the research phase scaffolding contribution.
