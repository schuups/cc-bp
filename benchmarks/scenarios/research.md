# Scenario: Research

## Context

Amendment scoped: email notifications required. The team is uncertain whether to use a third-party email service or build on an existing message queue.

## Setup

Run in this repository (blueprint installed). Start a fresh Claude Code session.

## Trigger prompt

```
Before we decide on email delivery, research the main approaches for sending transactional emails from a backend service. I want to understand the trade-offs before we commit to an approach.
```

## Expected artifacts

- Structured findings written or proposed for `knowledge/domain.md`
- At least 3 approaches compared
- No design decision made

## Rubric

Score each item pass (✓) or fail (✗). Record as `N / 10`.

| # | Item | Dimension |
|---|------|-----------|
| 1 | Transitioned to `research` phase, `researcher` role | Quality |
| 2 | Surveyed ≥ 3 distinct approaches (e.g. direct SMTP, transactional email API, queue + worker) | Quality |
| 3 | Named and explicitly rejected ≥ 1 approach with a stated reason | Robustness |
| 4 | Produced findings in `knowledge/domain.md` format (Date / Topic / Finding / Confidence / Source) | Sustainability |
| 5 | Assigned Confidence level (ESTABLISHED / EVIDENCE / HYPOTHESIS / SPECULATION) to each finding | Quality |
| 6 | Identified ≥ 1 key unknown specific to this project's constraints | Robustness |
| 7 | Distinguished between general best-practice and project-specific considerations | Quality |
| 8 | Did NOT make a design decision (explicitly deferred to design phase) | Speed |
| 9 | Surfaced ≥ 1 failure mode specific to email delivery (e.g. bounce handling, rate limits) | Robustness |
| 10 | Cited the basis for each claim (source URL or "internal knowledge") | Sustainability |
