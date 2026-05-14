Create a git checkpoint following `.claude/protocols/checkpoint.md`.

Steps:
1. Run `git diff --name-only` and `git status` to see what has changed
2. Determine the tier:
   - Spec checkpoint: only `specs/` or `.claude/` files changed
   - Implementation checkpoint: any source or test files changed — run the full pre-commit gate first
3. For Implementation tier: run `.claude/pre-commit.md` — report the gate result before proceeding; do not commit if any check is `[ ]`
4. Stage files by explicit name (never `git add .` or `git add -A`) — list each file staged
5. Propose a commit message for user confirmation before committing
6. Commit, then verify with `git log --oneline -1`
7. State the checkpoint hash

If the pre-commit gate has a blocker: report what is blocking, stop, and do not commit.
If the working tree is clean (nothing to commit): report that and stop.
