# Command: /status

Dashboard snapshot of project health. Read-only — no files changed. Runs automatically at session start.

## Step 0 — Onboarding Check

If `.claude/commands/onboard.md` still exists: onboarding has not been completed.
Stop and prompt the user to run `/onboard` before proceeding.

## Step 1 — Project State

Read `.claude/knowledge/health/fidelity-index.md`:
- Coverage distribution: THOROUGH / MODERATE / LOW / NONE counts
- Number of `[!] DIVERGENT` entries
- Date of last sweep

If absent: project has not been swept — note this and recommend running Sweep before proceeding.

## Step 2 — Requirements

Read `.claude/knowledge/context/requirements.md`:
- Count by status: implemented / planned / deferred
- Flag any requirements whose intent or scope is unclear enough to warrant checking with the user before design begins (ambiguous)

## Step 3 — Open Work

Read `.claude/knowledge/health/adversary-findings.md` and `.claude/knowledge/health/audit-log.md`:
- Count open adversary findings by severity: CRITICAL, HIGH, MEDIUM
- Note any audit sign-offs marked FAIL without a subsequent PASS

Read `.claude/knowledge/health/escalations.md`:
- Count open escalations; flag any open longer than 30 days

## Step 4 — Deferred Work

Read `.claude/knowledge/deferred.md`:
- Count entries by urgency: HIGH / MEDIUM / LOW
- Flag any entry whose `Activate when` condition is now met

## Step 5 — In-Progress Work

Check `.claude/knowledge/health/fidelity-index.md` for items with a `Last audited` date but NONE coverage — these indicate an interrupted sweep.

Check `.claude/knowledge/context/adr/` for any ADRs with status `Proposed` — decisions pending adversary review.

## Step 6 — Re-sweep Evaluation

Read the last sweep date from `.claude/knowledge/health/fidelity-index.md`.

Run the following checks and collect all that are true:

| Condition | How to check |
|-----------|-------------|
| >10 distinct files changed since last sweep | `git log --since="<last-sweep-date>" --name-only --pretty=format: \| sort -u \| grep -v '^$' \| wc -l` |
| Last sweep >30 days ago | Compare last sweep date to today |
| `invariants.md` changed since last sweep | `git log --since="<last-sweep-date>" -- .claude/knowledge/context/invariants.md` |
| ≥3 DIVERGENT entries | Count from Step 1 |
| ≥5 LOW-rated entries | Count from Step 1 |

If any condition is true: `Sweep recommended: YES — <one-line summary of all matching reasons>`.
If none: `Sweep recommended: NO`.

## Step 7 — Report

```
─── Project Status ──────────────────────────────────────
Last sweep:          YYYY-MM-DD [or "never"]
Fidelity:            THOROUGH N  MODERATE N  LOW N  NONE N  DIVERGENT N
Requirements:        N implemented · N planned · N deferred · N ambiguous
Open findings:       N CRITICAL · N HIGH · N MEDIUM
Open escalations:    N  [oldest: N days]
Deferred work:       N HIGH · N MEDIUM · N LOW  [N activate-now]
Proposed ADRs:       N
Interrupted sweep:   yes — N items remaining | no
Sweep recommended:   YES — <summary of all matching reasons> | NO
⚠ Ambiguous:         [requirement titles, or "none"]
Recommended action:  [one sentence]
─────────────────────────────────────────────────────────
```

No other output. The user decides what to do next.
