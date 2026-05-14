# Protocol: ASSESS Mode

Read-only status check. No changes to codebase or specs.

**Goal:** understand where the project is before taking any action. Run automatically at the start of every new session. Also run any time project state is uncertain.

Invoked by `/status`, by intent signals ("status", "where are we", "what's the state"), or at session start.

---

## Steps

### 1. Project State
Read `specs/fidelity-index.md`:
- What is the coverage distribution? (THOROUGH / MODERATE / LOW / NONE per section)
- Are there any `[!] DIVERGENT` entries?
- Are there any items still at NONE — not yet audited?
- Date of last sweep checkpoint

If `specs/fidelity-index.md` is absent or empty: project is Brownfield — note this in the report and recommend running a Sweep before proceeding.

### 2. Open Work
Read `specs/findings.md`:
- Count open findings by severity: CRITICAL, HIGH, MEDIUM
- Identify any unresolved `[!] DIVERGENT` items

Read `specs/escalations.md`:
- Count and list all open escalations
- Flag any escalation open longer than 30 days

### 3. Deferred Work
Read `specs/deferred.md`:
- Count active DEF-NNN entries by urgency (HIGH / MEDIUM / LOW)
- Note any entry whose `Activate when` condition is now met

### 4. In-Progress Work
Check for an interrupted sweep: look for items in `specs/fidelity-index.md` that have a `Last audited` date but coverage rating of NONE — these indicate work that started but was not completed.

Check `specs/adr/` for any ADRs with status `Proposed` — these are decisions pending adversary review and should not be forgotten.

### 5. Re-sweep Evaluation

Read the Sweep History table in `specs/fidelity-index.md` to find the last sweep date.

Run the following checks and flag any that are true:

| Condition | Check |
|-----------|-------|
| >50 commits since last sweep | `git log --oneline --since="<last-sweep-date>" | wc -l` |
| Last sweep >90 days ago | Compare last sweep date to today |
| `specs/invariants.md` changed since last sweep | `git log --oneline --since="<last-sweep-date>" -- specs/invariants.md` |
| ≥3 DIVERGENT entries in fidelity-index | Count from Step 1 |
| ≥5 LOW-rated entries in fidelity-index | Count from Step 1 |

If **any** condition is true: `Sweep recommended: YES — <first matching reason>`.
If none: `Sweep recommended: NO`.

### 6. Status Report

Produce this report (no other output for ASSESS mode):

```
─── Project Status ──────────────────────────────────────
Project state:   [Greenfield | Brownfield | Baselined | Sweep in progress]
Last sweep:      [YYYY-MM-DD or "never"]

Fidelity:
  THOROUGH   N
  MODERATE   N
  LOW        N
  NONE       N
  DIVERGENT  N

Open findings:   N CRITICAL · N HIGH · N MEDIUM
Open escalations: N  [oldest: N days]
Deferred work:   N HIGH · N MEDIUM · N LOW  [N activate-now]
Proposed ADRs:   N unfinalized
Interrupted sweep: [yes — N items remaining | no]

Sweep recommended: YES — <reason> | NO

Recommended next action: [one sentence]
─────────────────────────────────────────────────────────
```

---

## What ASSESS Does Not Do

- Does not change any file
- Does not run checks or gates
- Does not start any protocol
- Does not make commits

ASSESS reads and reports. The user decides what to do next.
