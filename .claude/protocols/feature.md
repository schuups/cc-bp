# Protocol: Feature Mode

Feature mode follows the Double Diamond model (see `.claude/process-model.md`): Discover→Define in the problem space, then Develop→Deliver in the solution space. Complete the first diamond before entering the second; loop back explicitly when findings invalidate prior assumptions.

---

## Diamond 1: Problem Space

### Phase 1 — Discover (Diverge)

Goal: Understand the problem as it actually exists, not as it was described. Cast wide.

Run `.claude/roles/analyst.md` with an expansive posture:

- Explore the codebase, `specs/domain-model.md`, `specs/invariants.md`, and all related ADRs
- Surface adjacent problems and related failure modes — even those outside the stated scope
- Look actively for evidence that challenges the initial problem framing
- Do not propose solutions; do not evaluate options — that is Phase 3

**Divergence requirement:** At least one finding must be something not mentioned in the original problem statement. If everything discovered was already known, the discovery pass was not wide enough.

---

### Phase 2 — Define (Converge)

Goal: Converge on the right problem. The defined problem may differ from the one originally stated.

Continue with `.claude/roles/analyst.md` for the reframe check and non-linear check (see role file).

#### Consequences Preview (required before entering Diamond 2)

Before the problem definition is finalized, map what changes if this problem is solved:

| Dimension | Question |
|-----------|----------|
| Intended changes | What is the stated goal — what should change? |
| Unintended first-order | What else in the system changes as a direct side effect? |
| Unintended second-order | What changes because of those changes? |
| Invariant fragility | Which invariants become harder to enforce even if not broken? |
| Reversibility | If this turns out to be wrong, how hard is it to undo? |

This mapping must be visible in the response before proceeding to Phase 3.

#### Loopback Gate — Define

> Do the Discover findings suggest a different, broader, or more fundamental problem than the one originally stated?

- **Yes** → Surface the alternative framing to the user. Get explicit scope confirmation before entering Diamond 2.
- **No** → Proceed.

#### Spec Checkpoint (if any specs were updated)
Run `.claude/protocols/checkpoint.md`, Tier 1 (Spec), for any `specs/` changes made during Phases 1–2.

---

## Diamond 2: Solution Space

### Phase 3 — Develop (Diverge)

Goal: Explore the solution space broadly. Generate options. Do not commit to a direction here.

Run `.claude/roles/architect.md`:

- Produce ≥2 options (see role file for option diversity requirements)
- For each option: produce a Consequences Map (intended + unintended changes)
- Run `.claude/roles/adversary.md` against each option — not only the preferred one

**Divergence requirement:** At least one option must challenge the most obvious approach. The "obvious" solution and a lateral alternative must both be present in the option set.

#### Loopback Gate — Develop

> Do any of the options, their tradeoffs, or their consequences reveal that the problem definition from Phase 2 was incomplete or wrong?

- **Yes** → Return to Phase 2 (Define). State what changed. Rerun the Consequences Preview with the new understanding before re-entering Phase 3.
- **No** → Proceed.

---

### Phase 4 — Deliver (Converge)

Goal: Select the best option and implement it completely.

#### 4.1 Select and Record

Choose the option with the most favorable consequence/risk profile — not the most elegant or fastest.

- Finalize ADR (status: `Accepted`) → `specs/adr/NNNN-<title>.md`
- Update `specs/adr/README.md` index
- Add new invariants (if any) to `specs/invariants.md`
- Add feature entry to `specs/fidelity-index.md` (Coverage: `NONE`, Status: —)

**Spec Checkpoint** — run `.claude/protocols/checkpoint.md`, Tier 1, before writing any code.

#### 4.2 Implement

Run `.claude/roles/implementer.md`.

**Adversary gate** — run `.claude/roles/adversary.md` against the implementation diff.
HARD GATE: no CRITICAL findings may remain before proceeding.

#### 4.3 Verify

Run `.claude/roles/auditor.md`.
HARD GATE: PASS sign-off appended to `specs/findings.md` before the pre-commit gate.

Run `.claude/roles/integrator.md`.
Output required: interface impact, consumer verification, deployment notes.

#### 4.4 Pre-commit Gate + Implementation Checkpoint

Run `.claude/pre-commit.md` — all seven checks must show `[x]`.

Run `.claude/protocols/checkpoint.md`, Tier 2 (Implementation).

Do not claim work is done until the checkpoint hash is reported.

---

## Done Criteria

Before claiming the feature is complete, verify all of the following:

- [ ] All tests pass (full suite, no skips without findings entries)
- [ ] Fidelity coverage rating on **all areas this feature touches** is `THOROUGH`
- [ ] No new `[!] DIVERGENT` entries introduced (or existing ones explicitly accepted in `specs/findings.md`)
- [ ] Adversary signed off — no unresolved CRITICAL findings
- [ ] Auditor PASS sign-off recorded in `specs/findings.md`
- [ ] Implementation Checkpoint hash reported

If any item is not met: do not claim done. Identify the blocker and resolve it.

## Mode Exit

Update `specs/fidelity-index.md`: set `Coverage` to `THOROUGH` for all entries this feature implements.

Run a final Spec Checkpoint for the fidelity-index update.
