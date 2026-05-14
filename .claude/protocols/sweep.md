# Protocol: Sweep Mode

Sweep mode systematically audits implementation fidelity against specs. It runs in checkpointed iterations and can be interrupted and resumed at any checkpoint.

---

## Preconditions

- `specs/domain-model.md` and `specs/invariants.md` are populated
  - If absent: run Design mode first to produce a baseline before sweeping
- `specs/fidelity-index.md` exists (create it from the template if absent)
- `specs/findings.md` exists (create it from the template if absent)

---

## Steps (repeat per item)

### 1. Load Checkpoint
Read `specs/fidelity-index.md`. Identify the first open `[ ]` item.

If no open items remain: sweep is complete. Report summary to user and stop.

### 2. Fidelity Audit for Current Item
Locate the corresponding implementation in the codebase.

Verify: does the implementation match the spec entry (domain model, invariant, or ADR)?

Note divergences precisely — do not fix them here. Fixes go through Feature or Bugfix mode.

### 3. Adversary Audit
Run `.claude/roles/adversary.md` scoped to the current item's implementation only — not the entire codebase.

### 4. Record Findings
Append all findings to `specs/findings.md` (including the explicit "no findings" statement if the adversary found nothing).

For any CRITICAL or HIGH finding: also append to `specs/escalations.md` and surface to the user immediately. **An unacknowledged escalation blocks the sweep from advancing.**

### 5. Update Checkpoint

Assign a **coverage rating** and **status flag** for the current item in `specs/fidelity-index.md`:

**Coverage rating** (choose one):

| Rating | Assign when |
|--------|-------------|
| `THOROUGH` | Strong coverage; implementation matches spec; edge cases handled |
| `MODERATE` | Reasonable coverage; some gaps; no critical divergences |
| `LOW` | Minimal coverage; significant gaps; improvement needed |
| `NONE` | Effectively no coverage found |

**Status flag:**

| Flag | Assign when |
|------|-------------|
| — | No divergence |
| `[!] DIVERGENT` | Implementation diverges from spec, even if tests pass |

Update the `Coverage` and `Status` columns in the fidelity-index row.

Then run `.claude/protocols/checkpoint.md`, **Tier 1 (Spec)**, for the fidelity-index and findings updates.

**Commit before moving to the next item.** This is what makes the sweep resumable after an interruption — each item's audit result is durable, not held only in the working tree.

### 6. Loop
Return to Step 1.

---

## Sweep Does Not Fix

Sweep mode is audit-only. Every `[!] DIVERGENT` entry is a task for a subsequent Feature or Bugfix session. Do not attempt to fix divergences inline during a sweep — it breaks the checkpoint integrity.

---

## Interrupted Sweep

A sweep can be stopped at any checkpoint. The fidelity index records progress precisely. To resume, use **Resume** mode — it reads the fidelity index and continues from the first `NONE`-rated item that has no `Last audited` date, or from where the last checkpoint left off.

---

## When to Re-Sweep

A sweep is not a one-time setup. Run a new sweep when:

- Major refactoring is completed
- More than 50 commits have accumulated since the last sweep checkpoint
- Before a release or significant milestone
- A domain invariant changes
- Trust is lost — tests pass but something feels wrong

---

## Sweep Completion Report

When all items have a coverage rating (no remaining `NONE` without a last-audited date), produce a summary:

```
Sweep complete — YYYY-MM-DD
Items audited:  N
  THOROUGH:     N
  MODERATE:     N
  LOW:          N
  NONE:         N
  DIVERGENT:    N
Open escalations: N (see specs/escalations.md)
```
