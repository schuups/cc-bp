# Command: /ccbp:overview

Snapshot of current project state. Read-only — no files changed. Mandatory at session start — see `.claude/CLAUDE.md`.

---

## Step 0 — Onboarding Check

If `onboard.md` still exists in `.claude/commands/`: onboarding is not complete.
Stop and prompt the user to run `/onboard` before proceeding.

---

## Step 1 — Map Staleness

Read `.claude/logs/map.md`. Extract the datetime from `## Project Map — YYYY-MM-DD HH:MM`.
Compare against `git log -1 --format=%ci` (last commit timestamp). The map is stale if its datetime is earlier than the last commit timestamp.

- **Map absent** → record: `Map: MISSING — run /ccbp:map`
- **Map older than last commit** → record: `Map: STALE (generated: YYYY-MM-DD HH:MM · last commit: YYYY-MM-DD HH:MM) — run /ccbp:map`
- **Map up to date** → record: `Map: YYYY-MM-DD HH:MM`

Do not block; surface the status in the report and recommend a refresh if stale.

---

## Step 2 — Log Freshness

Read `.claude/logs/interaction.md`:
- Are there uncommitted decisions or pending intentions not yet captured in a git commit?
- Flag any entries referencing work not yet checkpointed.
- **Size check:** count `##` section headings. If > 30: record `interaction.md OVER LIMIT (N entries)`.

---

## Step 3 — Requirements

Read `.claude/knowledge/requirements.md`:
- Count by status: implemented / planned / deferred
- Flag any requirements whose scope is ambiguous

---

## Step 4 — Open Findings and Log Staleness

**Staleness thresholds:** warn if the most recent entry is older than **30 days** OR if more than **10 commits** have been made since the `HEAD:` hash recorded in that entry. Either condition alone is sufficient to trigger a warning.

Read `.claude/logs/attack.md`:
- If the file has no entries: record `Attack log: NEVER RUN — run /ccbp:attack`
- Otherwise, find the most recent `## Attack —` entry overall (any target):
  - Extract the date from the section header
  - Extract the `HEAD:` hash (if present); run `git rev-list <hash>..HEAD --count` to get commit distance
  - Apply staleness thresholds; record one of:
    - `Attack log: STALE (N days old) — run /ccbp:attack`
    - `Attack log: STALE (N commits since last run) — run /ccbp:attack`
    - `Attack log: DATE · HEAD <hash>` (up to date)
- For each distinct target, count only rows with `Status = OPEN` by severity: CRITICAL, HIGH, MEDIUM (rows with no Status column are legacy — treat all as OPEN)
- Do not count `RESOLVED` or `ACCEPTED-RISK` rows
- **Size check:** count `## Attack —` section headings. If > 30: record `attack.md OVER LIMIT (N entries)`.

Read `.claude/logs/audit.md`:
- If the file has no entries: record `Audit log: NEVER RUN — run /ccbp:audit`
- Otherwise, find the most recent `## Audit —` entry:
  - Extract the date and `HEAD:` hash; apply staleness thresholds; record one of:
    - `Audit log: STALE (N days old) — run /ccbp:audit`
    - `Audit log: STALE (N commits since last run) — run /ccbp:audit`
    - `Audit log: DATE · HEAD <hash>` (up to date)
- Note any `Overall: FAIL` entry that has no subsequent `Overall: PASS` entry with the same scope
- **Size check:** count `## Audit —` section headings. If > 30: record `audit.md OVER LIMIT (N entries)`.

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
Attack log:           DATE · HEAD <hash> [or STALE / NEVER RUN — run /ccbp:attack]
Audit log:            DATE · HEAD <hash> [or STALE / NEVER RUN — run /ccbp:audit]
Pending log entries:  N  [or "none — log is clean"]
Requirements:         N implemented · N planned · N deferred · N ambiguous
Open findings:        N CRITICAL · N HIGH · N MEDIUM
Audit failures:       N open FAIL  [or "none"]
Deferred work:        N entries  [N activate-now]
Proposed ADRs:        N
Log health:           OK [or WARNING — <file> N entries (limit 30) · run /ccbp:cleanup]
─────────────────────────────────────────────────────────
Recommended action:   [one sentence]
```

If any file is over the limit, append a blank line after the box and print:

```
! Log size warning: <file> has N entries — context load is growing. Run /ccbp:cleanup to archive resolved entries.
```

Print one line per over-limit file.

If there are any `OPEN` CRITICAL findings, surface each one immediately below the box:

```
! CRITICAL: ATK-NNN — <description>
```

Finally, always close with a **Next steps** line: one to three concrete actions derived directly from the report state. Examples: "Run `/ccbp:attack` — log is NEVER RUN", "Resolve ATK-007 (CRITICAL) before proceeding", "Run `/ccbp:map` — map is stale". If everything is healthy, say so: "No action required — project is in good shape."

No other output beyond the box, log size warnings, CRITICAL findings, and next steps.
