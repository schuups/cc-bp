# Protocol: Checkpoint

A checkpoint is a git commit that records a verified, coherent state. It is the only point from which rollback is safe.

**No commit except via this protocol. No exceptions.**

Between checkpoints, all work lives in the working tree — uncommitted, discardable, and rollback-safe at any moment including mid-interruption.

---

## Tiers

Two tiers exist because the required gate differs by what changed.

| Tier | When to use | Gate |
|------|-------------|------|
| **Spec** | Only `specs/` or `.claude/` files changed | Reference check + index consistency |
| **Implementation** | Any source or test file changed | Full pre-commit gate (all 7 checks) |

When in doubt, use the Implementation tier.

---

## Tier 1: Spec Checkpoint

1. Verify scope — run `git diff --name-only`; confirm no source or test files are modified
2. Check: no broken Markdown links or references in the changed files
3. Check: if an ADR file changed, `specs/adr/README.md` index reflects the change
4. Stage by name — never `git add .` or `git add -A`:
   ```bash
   git add specs/path/to/file.md   # one file at a time, explicitly
   ```
5. Commit:
   ```
   spec: <imperative description of what was recorded>
   ```
6. Verify: `git log --oneline -1`
7. State the checkpoint hash to the user

---

## Tier 2: Implementation Checkpoint

1. Run the full pre-commit gate (`.claude/pre-commit.md`) — all 7 checks must show `[x]`
2. Do not proceed if any check is `[ ]` — fix the blocker, re-run the gate
3. Stage by name:
   ```bash
   git add path/to/changed/file   # one file at a time, explicitly
   ```
4. Commit — follow message format and branch conventions in `.claude/rules/git.md`:
   ```
   <type>(<scope>): <imperative description>

   [optional body: why, not what]
   [optional: ADR-NNNN or INV-NNN reference]
   ```
   Types: `feat` | `fix` | `sweep` | `audit` | `integrity`
5. Verify: `git log --oneline -1`
6. State the checkpoint hash to the user

---

## Special Case: Failing-Test Commit

When bugfix protocol Step 2 requires committing the failing test in isolation (before the fix), this is the only permitted commit where the test suite is not fully passing. Rules:

- The commit must contain **only** the test file(s) — no implementation changes whatsoever
- Run the pre-commit gate with tests excluded; all other 6 checks must pass
- Commit message must be unambiguous:
  ```
  test(failing): <describe the bug the test captures>
  ```
- Immediately proceed to writing the fix — do not leave a failing-test commit as the tip of the branch

---

## On Interruption

If a session is interrupted before a checkpoint, no commit was made. The previous checkpoint is intact.

| Intent | Command |
|--------|---------|
| Inspect what changed | `git diff` |
| Save work to resume later | `git stash` |
| Discard all changes, return to last checkpoint | `git checkout -- .` |
| Resume — continue from the interrupted step | Pick up from the last completed protocol step |

Never attempt to checkpoint an interrupted state. Resume the full protocol or discard and restart cleanly.

---

## Rolling Back to a Previous Checkpoint

List available checkpoints:
```bash
git log --oneline
```

Preview what rolling back to `<hash>` would remove:
```bash
git log --oneline <hash>..HEAD
git diff <hash>
```

Roll back (destructive past `<hash>`):
```bash
git reset --hard <hash>
```

**Before running `git reset --hard`: state to the user exactly which commits will be lost and ask for explicit confirmation.**

---

## Hard Rules

- Never `git add .` or `git add -A` — only stage files by explicit name
- Never commit mid-protocol — not between role activations, not mid-checklist
- Never commit based on a subagent's report alone — the gate must be verified in the main session
- Never amend a checkpoint that has been pushed — create a new commit instead
- If a gate check fails: fix it, re-run the full gate, then checkpoint — do not skip or suppress the failing check
