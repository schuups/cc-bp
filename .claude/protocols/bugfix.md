# Protocol: Bugfix Mode

Bugs are fixed test-first. No fix is accepted without a failing test that captures the bug and was confirmed failing before the fix was written.

---

## Steps

### 1. Reproduce
Identify the exact failing condition: input, state, or sequence that triggers the bug.

If it cannot be reproduced, do not proceed. Append an entry to `specs/escalations.md` and surface it to the user.

### 2. Write a Failing Test
Write a test that captures the bug.

Confirm the test fails *for the right reason* — not due to a test setup error or wrong assertion. State the failure output explicitly.

If the project convention requires it, commit the failing test separately before writing the fix — use the Failing-Test Commit special case in `.claude/protocols/checkpoint.md`.

### 3. Fix
Implement the minimum change that makes the failing test pass.

Do not expand scope beyond the stated bug. Run `.claude/roles/implementer.md` binding checklist.

### 4. Confirm
- The new test passes
- No previously passing tests now fail
- Run full test suite: `<test-command>` (e.g. `pytest`, `npm test`, `go test ./...`)
- State the test results explicitly — do not infer from code

### 5. Adversary
Run `.claude/roles/adversary.md` against the fix diff.

**HARD GATE: No CRITICAL findings before proceeding.**

If CRITICAL findings exist: return to Step 3.

### 6. Auditor
Run `.claude/roles/auditor.md`.

**HARD GATE: Must produce a PASS sign-off appended to `specs/findings.md` before committing.**

### 7. Pre-commit Gate
Run `.claude/pre-commit.md`. All seven checks must pass.

### 8. Implementation Checkpoint
Run `.claude/protocols/checkpoint.md`, **Tier 2 (Implementation)**.

Do not claim the bug is fixed until the checkpoint hash is reported.

---

## Done Criteria

- [ ] The failing test that was red before the fix is now green
- [ ] Full test suite passes — no regressions
- [ ] Fidelity coverage rating on the fixed area is at least `MODERATE`; if it was `LOW` or `NONE`, improve it to `MODERATE` as part of this fix
- [ ] No new `[!] DIVERGENT` entries introduced
- [ ] Adversary signed off — no unresolved CRITICAL findings
- [ ] Auditor PASS sign-off recorded in `specs/findings.md`
- [ ] Implementation Checkpoint hash reported

## If the Root Cause Is Larger Than the Fix

Do not silently expand scope. Instead:

1. Fix only the immediate symptom in this commit
2. Append a HIGH or CRITICAL finding to `specs/findings.md`
3. Append an entry to `specs/escalations.md`
4. Surface the escalation to the user before claiming work is done
