# Command: /ccbp:checkpoint

Verify that the current working-tree delta is complete, coherent, and worthy of a permanent snapshot, then record it as a git commit. The bar is: would this commit be acceptable in a well-run open source project?

This command is intentionally thorough and will take time. Do not skip steps, summarise checks, or accept prior-session observations as substitutes for running checks now. Slowness is expected and correct.

This command commits only. Pushing to a remote is a separate, explicit action.

---

## Step 0 — Onboarding Check

If `.claude/commands/onboard.md` still exists: onboarding has not been completed.
Stop and prompt the user to run `/onboard` first.

**Urgency gate:** if the current urgency is `critical`, run `git diff --name-only` and state the files that will be committed and the nature of the change; wait for explicit user confirmation before proceeding to Step 1.

---

## Step 1 — Assess Working Tree

Run `git diff --name-only` and `git status`.

If the working tree is clean: report "Nothing to commit — working tree is clean." and stop.

**Coherence check:** if the diff spans changes that appear unrelated (e.g. a feature addition mixed with an unrelated refactor, or multiple independent bug fixes), warn the user and suggest splitting into separate commits. A commit should represent one logical change.

---

## Step 2 — Determine Tier

**Spec tier** — no source or test files in the diff; only `.claude/`, documentation, or configuration files changed.

**Implementation tier** — any source or test file appears in the diff.

When in doubt, use the Implementation tier.

---

## Spec Tier

Completeness checks:

1. **No broken links** — scan changed files for `[text](path)` Markdown links; verify each target exists on disk
2. **ADR index current** — if any file under `.claude/knowledge/adr/` changed, confirm `.claude/knowledge/adr/INDEX.md` reflects it

If any check fails: state what is broken and stop. Do not commit.

Stage, propose, and commit — see *Staging and Committing* below.
Commit format: `spec: <imperative description>`

---

## Implementation Tier

Run all six checks. Produce the gate report before committing. Any `[ ]` is a blocker — state what must happen to clear it and stop.

**Test tier at commit:** checkpoint enforces the **slow tier** minimum — unit tests + integration tests + linter + type check. The **full tier** (E2E, live-endpoint, long-running load tests) is required pre-merge, not necessarily pre-commit. The **fast tier** (unit tests only) is for between-edit feedback during implementation and is not a checkpoint gate. Specific commands for each tier go in the project's root `CLAUDE.md`; if not defined there, run everything available.

### Check 1 — Tests pass
Run the full test suite now (slow tier minimum — see above). Required: zero failing tests; zero skipped or `xfail` tests without an open entry in `.claude/knowledge/attack-log.md`.

### Check 2 — Lint and type check pass
Run the project's lint and type-check commands now — do not rely on prior session output. Zero errors required. If the project has no lint or type-check configured, note this explicitly.

### Check 3 — No stale or invalid markers; no committed known issues
In changed files only:
- No `TODO`, `FIXME`, or `HACK` markers without an open entry in `.claude/knowledge/attack-log.md`
- No commented-out code blocks
- No debug instrumentation (`console.log`, `print(`, `debugger`, `binding.pry`, `fmt.Println`)
- No merge conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)

Cross-reference with `.claude/knowledge/attack-log.md`: read all open CRITICAL and HIGH findings. For each one, determine whether it touches any file or symbol in the current diff. If it does, block the commit — a known CRITICAL or HIGH issue may not be silently committed without being resolved or explicitly acknowledged with the user.

### Check 4 — Specs, architecture, and documentation consistent

**Requirements:** read `.claude/knowledge/requirements.md`. For each requirement whose status is `implemented` and whose scope overlaps with the diff: verify the implementation actually does what the requirement describes. Flag any requirement that claims `implemented` but whose intent is not visibly satisfied by the code.

**Architecture compliance:** read `.claude/knowledge/architecture.md` and all ADRs with status `Accepted` in `.claude/knowledge/adr/`. Verify the diff does not violate any architectural decision or constraint — check component boundaries, data flow direction, cross-cutting concern handling, and any explicit prohibitions. A diff that violates an ADR is a blocker even if tests pass.

**Knowledge file updates:**
- If the change introduces or modifies domain concepts or features: `.claude/knowledge/requirements.md` is updated
- If the change introduces or modifies architectural decisions: `.claude/knowledge/architecture.md` is updated; consider whether an ADR is warranted

**Documentation:**
- If a public API, public interface, or user-facing behaviour changed: `documentation/` or README is updated to reflect it
- If a CHANGELOG exists in the project: it has an entry for this change

### Check 5 — No stale references
If the diff moves, renames, or deletes any file, directory, function, class, module, configuration key, or exported symbol: all imports, cross-references, README files, and documentation that pointed to the old form are updated. No dead links or references to names or paths that no longer exist.

This includes semantic renames: if a function, class, or variable is renamed in the diff, grep the codebase for all callers using the old name and verify they have been updated.

### Check 6 — Auditor sign-off
Find the most recent PASS entry in `.claude/knowledge/audit-log.md`. Verify both fields against the current working tree:

1. Run `git rev-parse --short HEAD` — must match the entry's `HEAD:` field.
2. Run `git diff HEAD | shasum -a 256 | cut -c1-8` — must match the entry's `Diff:` field (or both must be `clean` if the working tree is clean).

Both must match. If either differs, the working tree has changed since the audit was run — re-run `/ccbp:audit` before proceeding. If the result is FAIL: resolve all findings and re-run the auditor role before proceeding.

Entries without a `Diff:` field are legacy format — treat as valid only if the working tree is clean (`git status --porcelain` returns nothing).

**Fidelity check:** if the PASS entry contains a `Fidelity:` line, extract it. If any component in the diff is rated NONE or STUB: surface this as a risk in the gate report — not a hard blocker, but it must be visible. DIVERGENT mocks in the diff are a hard blocker (HIGH finding — must be resolved or explicitly accepted before committing).

### Gate Report

```
Pre-commit gate:
[ ] Tests pass — N tests, 0 failed, 0 skipped
[ ] Lint and type check pass
[ ] No stale or invalid markers in changed files
[ ] Specs and documentation consistent
[ ] No stale references
[ ] Auditor sign-off: PASS — YYYY-MM-DD
    Fidelity: N THOROUGH · N MODERATE · N SHALLOW · N STUB · N NONE  [risk if NONE or STUB in diff]
```

---

## Staging and Committing

**Sensitive file check:** before staging, check every changed file's name against the list below. If any matches, stop — do not stage. Surface to the user: "Found what appears to be a sensitive file: `<file>`. Not staging. Please confirm how to handle it before proceeding."

Sensitive patterns: `.env` `.env.*` `*.secret` `*.key` `*.pem` `*.p12` `*.pfx` `*.p8` `credentials.*` `secrets.*` `*_credentials.*` `*_secrets.*` `*.token` `auth.json` `config.local.*` `settings.local.*` `id_rsa` `id_ed25519` `id_ecdsa` `id_dsa` `*.crt` `*.cer` `*.sqlite` `*.db` `*.dump` `gcreds.json` `application-prod.yml` `application-prod.yaml` `*-prod.yml` `*-prod.yaml`

Note: this list is a heuristic, not exhaustive — use judgement for files not listed that appear to contain credentials, tokens, or PII.

**Stage:**
1. Stage each changed file by explicit name — never `git add .` or `git add -A`; list each file as it is staged

**Commit message:** must be:
- Imperative mood: "add retry logic" not "added retry logic"
- Subject line ≤72 characters
- Specific — not "fix", "update", "misc", "wip", "changes", or other vague terms
- Format: `<type>(<scope>): <description>` — types: `feat · fix · refactor · docs · audit · test`
- Optional body explaining *why*, not *what* (the diff shows what)

Propose the message and wait for user confirmation before committing.

**After commit:**
1. Run `git log --oneline -1` to verify
2. State: `Checkpoint <hash> — roll back with git reset --hard <hash>`
3. Clear `.claude/knowledge/interaction-log.md`: replace all content with:

```
# Interaction Log

Records every decision and agreed intention. Cleared by `/checkpoint` (which adds `Previous commit: <hash>`). `/pause` appends a session summary.

---

Previous commit: <hash> — cleared N entries on YYYY-MM-DD HH:MM
```

Where N is the count of `##` section headings that existed before clearing, and `<hash>` matches the commit just made.

---

## Special Case — Failing-Test Commit

When project convention requires committing a failing test before the fix:
- The diff must contain **only** test files — no implementation changes
- Run the gate with Check 1 excluded; all other checks must pass
- Commit message: `test(failing): <describe the bug the test captures>`
- Proceed immediately to writing the fix — never leave a failing-test commit as the branch tip

---

## Hard Rules

- Never `git add .` or `git add -A` — stage files by explicit name only
- Always propose the commit message and wait for user confirmation before committing
- Never commit mid-protocol — not between role activations, not mid-checklist
- Never commit based on a subagent report alone — all gate checks must be observed directly in this session
- Never amend a checkpoint that has been pushed — create a new commit instead
- If credentials, API keys, tokens, or PII are found anywhere in the diff: (1) stop — do not commit or log the value; (2) surface to the user: "Found what appears to be a credential in `<file>` — not committing"; (3) wait for explicit user instruction before touching that file
- Never force-push or `reset --hard` without explicit user instruction; these are destructive and irreversible
- Never push to a remote repository without explicit user confirmation — state the target remote and branch and wait

---

## Merge Conflicts

Resolve conflicts by understanding both sides — never blindly accept "ours" or "theirs". For each conflicted file:
1. Read both versions and understand the intent behind each change
2. Resolve by preserving the correct intent from each side
3. If the conflict touches domain logic, `architecture.md`, or any ADR: run `/ccbp:attack` on the resolved file before committing
