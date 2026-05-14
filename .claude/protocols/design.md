# Protocol: Design Mode

Design mode works the **first diamond** of the Double Diamond model (see `.claude/process-model.md`): Discover then Define. The output is a committed problem definition — an ADR and updated specs — that Feature mode's second diamond acts on.

**No solution is chosen here. No code is written here.** Looping back from Define to Discover is expected and correct.

---

## Phase 1 — Discover (Diverge)

Goal: Understand the problem as it actually exists, not as it was described. Cast wide.

Run `.claude/roles/analyst.md` with an expansive posture:

- Explore the codebase, `specs/domain-model.md`, `specs/invariants.md`, and all related ADRs
- Surface adjacent problems, related failure modes, and prior decisions that this problem touches
- Look actively for evidence that challenges the initial problem framing
- Do not propose solutions — that is out of scope for this mode

**Divergence requirement:** At least one finding must be something not mentioned in the original problem statement. If everything found was already known, the discovery pass was not wide enough.

All open questions must be resolved or explicitly escalated to `specs/escalations.md` before proceeding to Phase 2.

---

## Phase 2 — Define (Converge)

Goal: Converge on the right problem. The defined problem may differ from the one originally stated.

Continue with `.claude/roles/analyst.md` for the reframe check and non-linear check (see role file).

#### Consequences Preview (required before finalizing the problem definition)

Before declaring the problem defined, map what changes if it is solved:

| Dimension | Question |
|-----------|----------|
| Intended changes | What is the stated goal — what should change? |
| Unintended first-order | What else changes as a direct side effect of solving this? |
| Unintended second-order | What changes because of those changes? |
| Invariant fragility | Which invariants become harder to enforce even if not broken? |
| Reversibility | If this turns out to be wrong, how costly is it to undo? |

This mapping must be visible in the response before the ADR is written.

#### Loopback Gate — Define

> Do the Discover findings suggest a different, broader, or more fundamental problem than the one originally stated?

- **Yes** → Surface the alternative framing to the user. Get explicit scope confirmation before finalizing.
- **No** → Proceed.

#### Finalize

- Write the ADR (status: `Accepted`) to `specs/adr/NNNN-<title>.md`
  - The ADR captures the problem definition and the decision to proceed with Feature mode
  - Implementation details are TBD — that is Phase 3 (Develop)
- Update `specs/adr/README.md` index
- Update `specs/domain-model.md` if new domain concepts were identified
- Update `specs/invariants.md` if new invariants were identified or existing ones clarified

#### Spec Checkpoint

Run `.claude/protocols/checkpoint.md`, Tier 1 (Spec).

Commit all spec changes. The working tree must be clean before the mode exits.

---

## Mode Exit

Design mode is complete when:
- The problem is precisely defined and the Consequences Preview is documented
- The Loopback Gate has been explicitly evaluated
- The ADR exists with status `Accepted`
- The Spec Checkpoint hash has been reported

Transition to Feature mode starting at Phase 3 (Develop) to explore solutions.

---

## What Design Mode Does Not Do

- Does not explore, evaluate, or select solutions
- Does not write production code
- Does not claim "done" based on a problem definition alone — a defined problem is an input to implementation, not a shipped feature
