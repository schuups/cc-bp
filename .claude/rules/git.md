# Rules: Git Workflow

Rules for git usage in this project. These complement the checkpoint protocol (`.claude/protocols/checkpoint.md`) and are more specific to project conventions.

---

## Branch Naming

```
<type>/<short-description>
```

| Type | When |
|------|------|
| `feat/` | New feature or capability |
| `fix/` | Bug fix |
| `spec/` | Spec-only changes (domain model, invariants, ADRs) |
| `sweep/` | Fidelity or adversary sweep work |
| `refactor/` | Code restructuring with no behavior change |
| `chore/` | Dependency updates, tooling, CI |

Example: `feat/payment-retry-logic`, `fix/null-pointer-on-empty-cart`

---

## Commit Message Format

```
<type>(<scope>): <imperative description>

[optional body: why, not what]
[optional: refs ADR-NNNN or INV-NNN]
```

Types: `feat` · `fix` · `spec` · `sweep` · `refactor` · `chore` · `test`

- Imperative mood: "add retry logic" not "added retry logic"
- ≤72 characters in the subject line
- Body explains motivation, not mechanics — the diff shows what changed
- Reference an ADR when the commit implements a recorded decision

---

## What Goes Directly to Main

<!--
FILL THIS IN per project. Examples below — uncomment the rule that applies.
-->

<!-- Option A: trunk-based, all commits go to main -->
<!-- All commits go directly to main via checkpoint. No long-lived branches. -->

<!-- Option B: feature branches, PR required -->
<!-- All changes go through a feature branch and pull request. Direct commits to main are not permitted except for spec-only hotfixes. -->

<!-- Option C: custom — describe here -->

---

## Pull Request Rules

<!--
FILL THIS IN per project if PRs are used.
-->

- PR title follows the same format as a commit message subject
- PR description includes: what changed, why, how to verify, ADR reference if applicable
- Minimum reviewers: <!-- N -->
- CI must pass before merge
- Squash merge: <!-- yes / no -->

---

## What Never Goes in a Commit

- Credentials, API keys, tokens (see `.claude/guardrails/security.md`)
- Generated files that are in `.gitignore`
- Editor config, OS metadata (`.DS_Store`, `Thumbs.db`)
- Commented-out code without a corresponding `specs/findings.md` entry
- `TODO` / `FIXME` markers without a corresponding `specs/findings.md` entry
- Unresolved merge conflict markers

---

## Merge Conflicts

Resolve conflicts by understanding both sides — do not blindly accept "ours" or "theirs". If the conflict touches a domain invariant, run the adversary role on the resolved file before committing.

---

## Tags and Releases

<!--
FILL THIS IN per project.
-->

- Release tag format: `v<major>.<minor>.<patch>`
- Tags are created only after the full pre-commit gate passes on main
- A release requires a Sweep checkpoint (all entries MODERATE or higher)
