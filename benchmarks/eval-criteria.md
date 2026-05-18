# Evaluation Criteria

Fixed criteria for benchmark runs. Set once; must not change mid-run or retroactively.
To revise these criteria, create a new dated version section and leave the prior version intact.

---

## Version 1 — 2026-05-18

### Baselines (measured at commit b0f766a)

| Metric | Current value |
|--------|--------------|
| Constant session load | 5,316 tokens |
| Heaviest command load (checkpoint) | 2,744 tokens |
| Overview total load (cmd + knowledge files) | 2,627 tokens |
| Average role file | 970 tokens |

Token estimates use `chars ÷ 4` approximation. See `token-analysis.md` for per-file breakdown.

---

### Utility Criteria

**Per-scenario pass threshold:** ≥ 80% of rubric items scored `pass`.

**Minimum rubric depth:** Each scenario must have at least 8 rubric items. Rubrics with fewer items are too shallow to be meaningful.

**Suite pass threshold:** ≥ 90% of scenarios must individually meet the per-scenario threshold. With 15 scenarios (9 phases + autonomous + 5 commands), at most 1 scenario may fail.

**Control delta requirement:** Blueprint average score across all scenarios must exceed control average by ≥ 25 percentage points. The blueprint must also score ≥ control on every individual scenario — no scenario where control wins.

---

### Token Budget Criteria

These are **ceiling** values. Exceeding a ceiling is a finding, not an automatic failure — but it must be noted and justified.

| Budget | Ceiling | Rationale |
|--------|---------|-----------|
| Constant session load | 7,500 tokens | ~41% above current baseline (5,316); baseline must pass before compaction |
| Any single command load | 8,000 tokens | ~49% above current max (checkpoint: 5,359); baseline must pass before compaction |
| Any single role file | 2,000 tokens | ~106% above current average (970); wide margin, roles are loaded on demand |

**Compaction target** (aspirational, for tracking TODO item 2 progress):

| Budget | Target |
|--------|--------|
| Constant session load | ≤ 3,500 tokens |
| Any single command load | ≤ 2,000 tokens |
| Any single role file | ≤ 800 tokens |

---

### Re-run Trigger

Re-run the full benchmark suite after any commit that modifies:
- `.claude/CLAUDE.md`
- any file in `.claude/commands/`
- any file in `.claude/roles/`

A partial re-run (token analysis only, no scenario execution) is sufficient for commits that only change knowledge file templates.

---

### Pass / Fail Summary

A benchmark run **passes** when all four conditions hold:
1. Per-scenario utility: ≥ 80% rubric items per scenario
2. Suite utility: ≥ 90% of scenarios pass individually
3. Control delta: blueprint average ≥ control average + 25 pp; blueprint ≥ control on every scenario
4. Token load: no ceiling exceeded (or any exceedance is documented with justification)
