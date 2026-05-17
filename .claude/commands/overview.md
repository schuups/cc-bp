# Command: /ccbp:overview

Snapshot of current project state. Read-only — no files changed. Runs automatically at session start.

---

## Step 0 — Onboarding Check

If `onboard.md` still exists in `.claude/commands/`: onboarding is not complete.
Stop and prompt the user to run `/onboard` before proceeding.

---

## Step 1 — Map Staleness

Read `.claude/logs/map.md`. Extract the date from `## Project Map — YYYY-MM-DD`.
Compare against `git log -1 --format=%ci` (last commit).

- **Map absent** → record: `Map: MISSING — run /ccbp:map`
- **Map older than last commit** → record: `Map: STALE (generated: DATE · last commit: DATE) — run /ccbp:map`
- **Map up to date** → record: `Map: DATE`

Do not block; surface the status in the report and recommend a refresh if stale.

---

## Step 2 — Log Freshness

Read `.claude/logs/interaction.md`:
- Are there uncommitted decisions or pending intentions not yet captured in a git commit?
- Flag any entries referencing work not yet checkpointed.

---

## Step 3 — Requirements

Read `.claude/knowledge/requirements.md`:
- Count by status: implemented / planned / deferred
- Flag any requirements whose scope is ambiguous

---

## Step 4 — Open Findings

Read `.claude/logs/attack.md`:
- For each distinct target, find the most recent `## Attack —` entry — that is the authoritative open state
- Count findings (rows in the findings table) from those most-recent entries by severity: CRITICAL, HIGH, MEDIUM

Read `.claude/logs/audit.md`:
- Note any `Overall: FAIL` entry that has no subsequent `Overall: PASS` entry with the same scope

---

## Step 5 — Deferred Work

Read `.claude/knowledge/backlog.md`:
- Count entries
- Flag any whose deferral condition is now met

---

## Step 6 — Pending Decisions

Check `.claude/knowledge/adr/` for any ADRs with status `Proposed` — decisions awaiting an `/attack` pass.

---

## Step 7 — Report

```
─── Project Overview ────────────────────────────────────
Map:                  DATE [or STALE / MISSING — run /ccbp:map]
Pending log entries:  N  [or "none — log is clean"]
Requirements:         N implemented · N planned · N deferred · N ambiguous
Open findings:        N CRITICAL · N HIGH · N MEDIUM
Audit failures:       N open FAIL  [or "none"]
Deferred work:        N entries  [N activate-now]
Proposed ADRs:        N
─────────────────────────────────────────────────────────
Recommended action:   [one sentence]
```

No other output. The user decides what to do next.
