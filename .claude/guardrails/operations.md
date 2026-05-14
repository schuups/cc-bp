# Guardrail: Risky Operations

Operations that are hard to reverse, affect shared state, or could cause data loss require explicit user confirmation before execution. This rule applies regardless of mode, role, or how confident the reasoning is.

---

## Always Confirm Before Executing

### Destructive Git Operations

| Command | Risk | Required action |
|---------|------|----------------|
| `git reset --hard` | Discards all uncommitted changes permanently | State exactly what will be lost; show `git diff`; wait for confirmation |
| `git push --force` or `--force-with-lease` | Overwrites remote history | State which commits will be overwritten; confirm the remote branch; wait |
| `git branch -D` | Deletes a branch that may not be merged | Confirm branch is merged or explicitly abandoned |
| `git clean -f` / `git clean -fd` | Permanently deletes untracked files/directories | List what will be deleted; wait for confirmation |
| `git rebase -i` | Rewrites commit history | Describe the rewrite; confirm on shared branches |
| `git checkout -- .` / `git restore .` | Discards all unstaged changes | State scope; wait for confirmation unless user explicitly asked |

**Never force-push to `main` or `master` without unambiguous user instruction.**

### File System Operations

| Operation | Confirm when |
|-----------|-------------|
| `rm -rf` | Always — state exactly what will be deleted |
| Overwriting a file with content the user has not seen | Show diff first |
| Moving or renaming a directory | Confirm the full set of downstream effects |

### Database / Storage Operations

| Operation | Confirm when |
|-----------|-------------|
| `DROP TABLE` / `DROP DATABASE` | Always |
| `TRUNCATE` | Always |
| Mass `DELETE` or `UPDATE` without a `WHERE` clause | Always |
| Schema migrations on a database with live data | Always — confirm rollback plan |

### External Side Effects

| Action | Confirm when |
|--------|-------------|
| Sending messages (email, Slack, webhooks) | Always, unless clearly part of a test |
| Pushing to a remote repository | Confirm destination and branch |
| Deploying to a staging or production environment | Always |
| Modifying shared configuration (CI/CD, permissions, secrets) | Always |

---

## How to Confirm

State:
1. What operation will run
2. What will be permanently changed or lost
3. That it cannot be undone (or how it can be)

Then wait for explicit user approval. "Go ahead" or "yes" is sufficient. Silence is not.

---

## When in Doubt

If an operation is not listed here but feels risky — pause and ask. The cost of a confirmation is one exchange. The cost of an unintended destructive operation can be hours or data.
