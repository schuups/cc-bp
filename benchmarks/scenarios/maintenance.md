# Scenario: Maintenance

## Context

The document approval system has been live for 3 months. Intermittent WebSocket connection drops are observed in production logs — roughly 5% of connections fail silently after ~10 minutes.

## Setup

Run in this repository (blueprint installed). Start a fresh Claude Code session.

## Trigger prompt

```
We're seeing intermittent connection drops in production — about 5% of WebSocket connections silently fail after roughly 10 minutes. The logs show no errors, just missing heartbeats. Investigate and fix.
```

## Expected artifacts

- Diagnosis of likely cause before any fix is proposed
- Fix scoped to the affected component only
- No ADR or full architecture review initiated

## Rubric

Score each item pass (✓) or fail (✗). Record as `N / 8`.

| # | Item | Dimension |
|---|------|-----------|
| 1 | Recognized `maintenance` phase without requiring an explicit declaration from the user | Quality |
| 2 | Applied the maintenance mode exemption — did not demand full role transition formality for local scope | Speed |
| 3 | Diagnosed the likely cause (e.g. missing keepalive / proxy timeout) before proposing a fix | Robustness |
| 4 | Proposed a fix scoped to the affected component | Speed |
| 5 | Did NOT propose a cross-cutting architectural change for a local, reproducible bug | Quality |
| 6 | Did NOT produce an ADR for a local implementation fix | Speed |
| 7 | Did not add features or refactor code beyond what the stated symptom requires | Speed |
| 8 | Fix specifically addresses the heartbeat / keepalive mechanism | Robustness |
