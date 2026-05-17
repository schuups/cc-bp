# Command: /ccbp:cleanup

Scan for obsolete, orphaned, or stale content. Propose removals or updates — never delete or modify without explicit user confirmation.

---

## Step 0 — Map Check

Read `.claude/logs/map.md`. Compare `## Project Map — YYYY-MM-DD` against `git log -1 --format=%ci`.
- **Absent or older than last commit** → warn "Map may be stale — orphan detection and drift checks may miss recently added or removed components. Run `/ccbp:map` to refresh." Continue unless the user opts to refresh first.
- **Up to date** → use the component list in `map.md` as the canonical inventory for Steps 2–3.

---

## Step 1 — Log Staleness

Read `.claude/logs/interaction.md`:
- Entries describing work now committed to git (compare against `git log`) — propose pruning
- Entries referencing files or features that no longer exist — flag as stale

Read `.claude/logs/attack.md` and `.claude/logs/audit.md`:
- Open findings referencing code that has since been changed or deleted — flag for review

---

## Step 2 — Knowledge File Drift

Read `.claude/knowledge/requirements.md`:
- Requirements marked `implemented` with no identifiable implementation in source — flag as suspect
- Requirements describing features no longer in the codebase — flag as stale

Read `.claude/knowledge/backlog.md`:
- Entries whose deferral condition is now met — flag as activate-now
- Entries no longer relevant given the current codebase — flag for removal

Read `.claude/knowledge/adr/`:
- ADRs with status `Proposed` older than 14 days — flag as stalled
- ADRs marked `Superseded` whose successor does not exist — flag as broken chain

---

## Step 3 — Orphaned Files

Scan `.claude/knowledge/` and `documentation/`:
- Files not referenced from any other file and not listed in CLAUDE.md — flag as orphaned
- Markdown links pointing to files that no longer exist — flag as broken

---

## Step 4 — Propose

Present all findings grouped by category. For each:
- State what was found and why it is considered stale or orphaned
- Propose a specific action: delete, update, or archive
- Wait for explicit user confirmation before taking any action

Never batch-delete. Confirm each item or group of clearly related items separately.

## Hard Rules

- If a file appears to contain credentials, API keys, tokens, or PII: (1) stop — do not delete or read sensitive values; (2) surface to the user: "Found what appears to be a sensitive file: `<file>` — not touching it"; (3) wait for explicit user instruction before proceeding
- Never delete files that are referenced by other files without first updating the references
- Confirmation is not transferable — approval to delete one item does not imply approval for any other
- Never execute database operations (DROP TABLE/DATABASE, TRUNCATE, mass DELETE or UPDATE without a WHERE clause, schema migrations on live data) without explicit user confirmation stating: what will be affected, whether the operation is reversible, and what the rollback plan is
