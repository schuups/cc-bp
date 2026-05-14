# Protocol: AdvSweep Mode

An adversary-led sweep of the codebase for attack surfaces, failure modes, and structural weaknesses.

Distinct from Sweep mode (auditor-led fidelity audit):

| | Sweep | AdvSweep |
|---|---|---|
| Led by | Auditor | Adversary |
| Question | Does the implementation match the spec? | Where are the actual problems? |
| Output | Coverage ratings in fidelity-index | Findings + failure modes |
| Priority signal | Spec entries | LOW/NONE fidelity areas get higher attention |

**Run Sweep first when possible** — LOW and NONE coverage areas become the highest-priority targets for AdvSweep. Together: *where tests are shallow* AND *where things are broken*.

---

## Preconditions

- `specs/fidelity-index.md` exists and has at least a partial sweep checkpoint
- `specs/findings.md` exists
- `specs/failure-modes.md` exists

If preconditions are absent, run Sweep mode first, then return to AdvSweep.

---

## Steps (checkpointed per chunk)

### 1. Scope and Prioritize

Read `specs/fidelity-index.md`. Build a prioritized target list:

| Priority | Criteria |
|----------|----------|
| 1 — Highest | `[!] DIVERGENT` entries — known broken |
| 2 | `LOW` or `NONE` coverage — thin tests, high risk |
| 3 | `MODERATE` coverage with security-sensitive logic |
| 4 | `THOROUGH` coverage — still sweep, lower depth |

Divide into chunks of reasonable size (one module, one aggregate, one layer). State the chunk order before starting.

### 2. Adversary Review — Current Chunk

Run `.claude/roles/adversary.md` scoped to the current chunk only.

Execute all nine attack vectors (see role file), including the unintended-consequences vector. Report findings per vector before moving to the next chunk.

### 3. Record Findings

Append all findings to `specs/findings.md` — including an explicit "no findings" statement if the adversary found nothing for this chunk.

For any CRITICAL or HIGH finding: also append to `specs/escalations.md` and surface to the user immediately.

**An unacknowledged CRITICAL escalation blocks the sweep from advancing to the next chunk.**

### 4. Update Failure Modes

If the adversary review surfaces a previously undocumented way the system can fail, add it to `specs/failure-modes.md` as a new `FM-NNN` entry.

### 5. Checkpoint

Run `.claude/protocols/checkpoint.md`, Tier 1 (Spec), for the updated `specs/findings.md` and `specs/failure-modes.md`.

**Commit before advancing to the next chunk.** This is what makes the sweep resumable.

### 6. Loop

Return to Step 2 for the next chunk. If all chunks are done, proceed to the completion report.

---

## Completion Report

Append to `specs/findings.md` and report to user:

```
AdvSweep complete — YYYY-MM-DD
Chunks reviewed:       N
New findings:          N CRITICAL · N HIGH · N MEDIUM · N LOW · N INFO
New failure modes:     N (see specs/failure-modes.md)
Open escalations:      N (see specs/escalations.md)
Highest-risk areas:    [list top 3 by finding severity]
```

---

## AdvSweep Does Not Fix

Every finding goes to `specs/findings.md`. Fixes go through Feature or Bugfix mode with the full Double Diamond protocol.
