# Claude Code Blueprint

A reusable `.claude/` scaffolding that makes Claude a disciplined collaborator on any software project — greenfield or brownfield. Drop it in, populate `specs/`, and every session follows the same structured process: the right problem before the right solution, hard gates before commits, git checkpoints you can always roll back to.

---

## What It Contains

```
.claude/
  CLAUDE.md               Working Agreements — soul, process model, mode selection, gate policy, standing rules
  pre-commit.md           Seven-check gate that must clear before any commit
  roles/
    analyst.md            Discover and Define — extracts the real problem; loads knowledge/ and specs context
    architect.md          Develop — generates options with consequences maps
    adversary.md          Attacks every artifact across 9 vectors before it proceeds
    implementer.md        Deliver — writes code strictly within the agreed design
    auditor.md            Verifies diff against specs, invariants, and testing standards
    integrator.md         Verifies composition across callers and consumers
  protocols/
    feature.md            Full Double Diamond — Discover→Define→Develop→Deliver
    design.md             First Diamond only — Discover→Define, no code
    bugfix.md             Failing test first, minimum fix, full gate
    sweep.md              Checkpointed fidelity audit of the whole codebase
    assess.md             Read-only session-start status check
    advsweep.md           Adversary-led sweep for attack surfaces and failure modes
    checkpoint.md         Git checkpoint procedure — Spec and Implementation tiers
    integrity.md          Repo-wide health audit — five check categories
  commands/
    status.md             /status — run at the start of every session
    checkpoint.md         /checkpoint — create a verified git checkpoint
    integrity.md          /integrity — repo-wide health audit
  guardrails/
    security.md           Data safety — credentials, PII, sensitive file handling
    operations.md         Risky operations — confirm before destructive git/db/fs actions
  rules/
    git.md                Branch naming, commit format, PR workflow
    testing.md            Test requirements, depth standards, mock discipline
  coding/
    python.md             Python coding guide — loaded by Architect and Implementer roles

specs/
  README.md               Convention guide, file listing, brownfield→baselined migration
  domain-model.md         Entities, value objects, aggregates, domain events
  ubiquitous-language.md  Domain terms with precise meanings
  invariants.md           Rules that must never be violated (INV-NNN format)
  assumptions.md          Explicit assumptions tracked and validated (ASM-NNN format)
  failure-modes.md        Known failure modes — populated by AdvSweep (FM-NNN format)
  features/               (optional) BDD scenarios as living spec — populate if using Gherkin/BDD
  architecture/           (optional) Module map, interfaces, dependency graph — for multi-module projects
  adr/
    README.md             ADR index and status vocabulary
    0001-template.md      ADR template with adversary gate record
  fidelity-index.md       Coverage tracker — THOROUGH / MODERATE / LOW / NONE per entry
  findings.md             Append-only adversary and auditor finding log
  escalations.md          Blockers requiring user decision
  deferred.md             Triaged work items explicitly set aside — not blocked, not forgotten, not now
```

---

## Getting Started

### Step 1 — Copy the scaffold

```bash
cp -r /path/to/blueprint/.claude  ./
cp -r /path/to/blueprint/specs    ./
```

### Step 2 — Configure three files

These are the only files that need project-specific content before your first session. Everything else is either template (Claude fills it in during Design mode) or language-agnostic (works as-is).

**`CLAUDE.md`** (root, create it if it doesn't exist) — tell Claude your test and lint commands:

```markdown
## Project Commands

- Test: `pytest` (or `npm test`, `go test ./...`, etc.)
- Lint: `ruff check . && mypy .` (or `tsc --noEmit`, `golangci-lint run`, etc.)
```

**`.claude/knowledge/project-context.md`** — fill in the project purpose, primary users, and any non-obvious constraints. The Analyst role reads this at the start of every Design and Feature session. Without it, Claude works blind to your project's context.

**`.claude/knowledge/architecture.md`** — fill in system boundaries and any non-obvious technical facts (third-party quirks, startup order dependencies, prod/dev differences). Skip this if the project is brand new.

That's it. You're ready to start a session.

> **Do you need to fill in `specs/domain-model.md` and `specs/invariants.md` first?**
> No. If you don't have a domain model yet, use Design mode — Claude will create one from your description or existing code. Pre-populating them manually is only faster if the domain is already well understood.

### Step 3 — Start your first session

Always begin with `/status`. It reads the project state, surfaces open findings and escalations, and tells you whether a sweep is recommended — so you know what you're walking into before acting.

```
/status
```

Then choose your starting prompt based on where you are:

---

**New project — nothing exists yet**

```
This is a task management API for engineering teams. Teams create projects, 
break them into tasks, and assign them to members. Let's define the domain model.
```
Claude enters Design mode: runs Analyst (discovers domain concepts), Architect (proposes structure), Adversary (attacks the design), then produces `specs/domain-model.md`, `specs/invariants.md`, and the first ADR.

---

**Existing codebase — no specs yet**

```
I have an existing Express.js booking system in this repo. 
Create a baseline: read the code and define the domain model and invariants.
```
Claude enters Design mode with Discover phase, reads the codebase, and produces the spec files. Run a Sweep after to populate `fidelity-index.md`.

---

**Ready to build a feature**

```
I want to add webhook notifications — fire an event when a booking is confirmed.
```
Claude enters Feature mode, works through the Double Diamond (Discover the problem, Define it, Develop options, Deliver with gates), and checkpoints at each phase.

---

**Known bug to fix**

```
The booking confirmation email is sent even when payment fails. Fix it.
```
Claude enters Bugfix mode: writes a failing test first, implements the minimum fix, runs the adversary and auditor gates, then checkpoints.

---

**Audit what exists**

```
Run a fidelity sweep of the codebase.
```
Claude enters Sweep mode: audits every entry in `specs/fidelity-index.md` against the implementation, assigns THOROUGH/MODERATE/LOW/NONE ratings, and checkpoints after each item.

---

## How It Works

### Three Layers

| Layer | What it is |
|-------|-----------|
| **Working Agreements** (`.claude/CLAUDE.md`) | Loaded every session. Defines the soul, the process model, mode selection, gate policy, and standing rules. |
| **Roles** (`.claude/roles/`) | Behavioral contracts. When a protocol invokes a role, every item in the role's binding checklist must be executed and reported. A role that has not completed its checklist has not run. |
| **Protocols** (`.claude/protocols/`) | Mode-specific procedures. They direct which roles to run, in what order, and what gates must clear before proceeding. |

### Every Session Starts the Same Way

Claude assesses the project state and maps your intent to a mode before acting. The first line of every response is:

```
Mode: [MODE] · Project: [state] · Role: [role] · [one-phrase reason]
```

Example: `Mode: Feature · Project: Baselined · Role: Analyst · extending payment flow`

If intent is ambiguous, Claude asks rather than guesses.

---

## Key Concepts

| Term | What it means |
|------|--------------|
| **Fidelity** | How closely the implementation matches the spec. Each spec entry is rated: THOROUGH, MODERATE, LOW, or NONE. |
| **Fidelity Index** | `specs/fidelity-index.md` — the live tracker of fidelity ratings across every domain model entry, invariant, and ADR. The hub of the spec system. |
| **Sweep** | A systematic, checkpointed audit of the fidelity index against the implementation. Assigns coverage ratings per entry. Safe to interrupt and resume. Finds divergences — does not fix them. |
| **AdvSweep** | An adversary-led sweep for attack surfaces and failure modes. Uses fidelity ratings to prioritise — LOW/NONE areas get deepest attention. Where Sweep measures coverage, AdvSweep finds actual problems. |
| **Adversary / Adversarial** | A role that actively attacks an artifact — design, code, or spec — across 9 vectors to find gaps, edge cases, and failure modes. Every design and every implementation passes through an adversary gate before proceeding. |
| **Gate** | A hard-blocking condition. Work cannot proceed past a gate until it clears. CRITICAL adversary findings and auditor FAIL both block — no exceptions without explicit user override. |
| **Checkpoint** | A verified git commit created only after the required gate passes. The unit of rollback. All uncommitted work stays in the working tree and is discardable at any moment — that safety is precisely why no ad-hoc commits are allowed. |
| **Baselined** | The project state where `specs/` is fully populated and every fidelity-index entry has been audited at least once. The goal of the first Sweep. |
| **Deferred** | Work that has been identified and triaged but explicitly set aside — not blocked, not forgotten, just not now. Lives in `specs/deferred.md` with a stated condition for re-activation. |

---

## Project States

Claude uses these to calibrate its approach at the start of each session.

| State | Signal |
|-------|--------|
| **Greenfield** | No source files committed; scaffolding only |
| **Brownfield** | Source files exist; `specs/` absent or unpopulated |
| **Baselined** | `specs/domain-model.md` and `specs/invariants.md` are populated |
| **Sweep in progress** | `specs/fidelity-index.md` has `NONE`-rated or unaudited items |

---

## Modes

### Primary Modes

#### Feature — Double Diamond

The most complete mode. Two diverge→converge cycles, one per diamond.

```
Problem Space                   Solution Space
────────────────────────────    ─────────────────────────────
 Discover      →     Define      Develop      →     Deliver
 (diverge)          (converge)   (diverge)          (converge)
```

- **Discover** — cast wide; surface what's actually true about the problem, including things not in the original description
- **Define** — converge on the right problem; produce a Consequences Preview (intended + unintended effects) before crossing into the solution space; Loopback Gate: does the evidence suggest a different problem?
- **Develop** — generate ≥2 options; one must challenge the obvious approach; map consequences per option; adversary runs on each option; Loopback Gate: do the options reveal the problem definition was wrong?
- **Deliver** — select by consequence/risk profile; record ADR; Spec Checkpoint; implement; adversary + auditor + integrator gates; Implementation Checkpoint

Non-linear movement is correct and expected. Findings from any phase can send you back to an earlier one.

#### Design — First Diamond Only

Produces a precise problem definition (ADR + updated specs). No code. Use when the problem space needs mapping before implementation begins, or as a standalone deliverable for a deferred feature.

#### Bugfix — Test-First

1. Write a failing test that captures the bug — confirm it fails for the right reason
2. Implement the minimum fix
3. Adversary gate → Auditor gate → Pre-commit gate → Implementation Checkpoint

No fix is accepted without a test that was red before the fix and green after.

#### Sweep — Fidelity Audit

Systematically audits every item in `specs/fidelity-index.md` against the implementation, assigning THOROUGH / MODERATE / LOW / NONE coverage ratings. Checkpointed after each item — safe to interrupt and resume. Finds divergences; does not fix them.

### Utility Modes

| Mode | Intent signals | What it does |
|------|---------------|-------------|
| **Assess** | "status", "where are we", "summary" | Read-only status check: fidelity coverage, open findings, open escalations, in-progress sweep. Runs via `/status`. |
| **AdvSweep** | "adversary sweep", "security review", "attack surface" | Adversary-led sweep for attack surfaces and failure modes. Uses fidelity ratings to prioritise — LOW/NONE areas get highest attention. Populates `specs/failure-modes.md`. |
| **Review** | "review", "find flaws", "adversary review [X]" | Runs the adversary role against a specific named artifact. State the target before starting. |
| **Resume** | "continue", "pick up", "where did we leave off" | Reads `specs/fidelity-index.md` and continues from the first unaudited item. Routes to Sweep or AdvSweep. |

---

## Roles Are Contracts

Each role file defines a binding checklist. When a protocol invokes a role, Claude executes every item and reports status before exiting. Skipping a checklist item means the role hasn't run.

| Role | When invoked | Core responsibility |
|------|-------------|-------------------|
| **Analyst** | Discover + Define phases | Extract the real problem; reframe check; non-linear check |
| **Architect** | Develop phase | ≥2 options with consequences maps; lateral option required |
| **Adversary** | After Architect; after Implementer | Attack every artifact across 9 vectors; CRITICAL findings block |
| **Implementer** | Deliver phase | Write code strictly within the ADR scope |
| **Auditor** | After Adversary (implementation) | Verify diff against specs; produce PASS/FAIL sign-off |
| **Integrator** | After Auditor | Verify all callers and consumers still pass |

---

## Gate Policy: Hard Blocking

| Gate | Trigger | What it blocks |
|------|---------|---------------|
| Adversary (design) | After Architect, before any code | Implementation |
| Adversary (implementation) | After Implementer | Auditor and commit |
| Auditor | After Adversary clears | Commit |
| Pre-commit | After Auditor PASS | Checkpoint |

CRITICAL adversary findings must be resolved and the adversary must re-run before any gate clears. No exceptions without an explicit user override.

---

## Git Checkpoints

**Rule: no commit except via the checkpoint protocol.**

Between checkpoints all work is in the working tree — uncommitted, discardable, and rollback-safe at any moment including mid-interruption.

### Two Tiers

| Tier | When | Gate required |
|------|------|---------------|
| **Spec** | Only `specs/` or `.claude/` files changed | Reference check + index consistency |
| **Implementation** | Any source or test file changed | Full pre-commit gate (all 7 checks) |

### Invoke with `/checkpoint`

Claude will: determine the tier → run the required gate → stage files by explicit name (never `git add .`) → propose a commit message → commit → report the hash.

### When You Interrupt Mid-Session

No commit was made. The previous checkpoint is intact.

```bash
git diff            # see what changed
git stash           # save for later
git checkout -- .   # discard all, return to last checkpoint
```

### Rolling Back

```bash
git log --oneline                  # find target checkpoint
git log --oneline <hash>..HEAD     # preview what will be lost
git reset --hard <hash>            # roll back (Claude will confirm first)
```

---

## The Specs Directory

The living specification. Everything in `specs/` is ground truth that roles and protocols reference.

| File | Populated when | Purpose |
|------|---------------|---------|
| `domain-model.md` | Design or Feature mode | Entities, value objects, aggregates, domain events |
| `ubiquitous-language.md` | Design mode | Domain terms with precise meanings; enforced across code and docs |
| `invariants.md` | Design or Feature mode | INV-NNN rules that must never be violated |
| `assumptions.md` | Design or AdvSweep | Explicit assumptions tracked and validated over time |
| `failure-modes.md` | AdvSweep | Known failure modes; updated every adversary sweep |
| `adr/` | Every significant design decision | One file per decision; immutable once Accepted |
| `fidelity-index.md` | First Sweep run | THOROUGH/MODERATE/LOW/NONE coverage ratings; the sweep's checkpoint log |
| `findings.md` | Every adversary/auditor run | Append-only log; auditor sign-offs live here |
| `escalations.md` | Any blocking issue | Open until user explicitly closes |

### Getting from Brownfield to Baselined

1. Run **Design mode** to produce `domain-model.md`, `invariants.md`, and `ubiquitous-language.md`
2. Run **Sweep mode** to populate `fidelity-index.md` — aim for MODERATE or higher on all entries
3. Run **AdvSweep** to populate `failure-modes.md` and surface structural weaknesses
4. Triage `findings.md` — resolve CRITICAL and HIGH entries via Feature or Bugfix sessions
5. When all `fidelity-index.md` entries are `MODERATE` or higher: the project is **Baselined**

---

## Slash Commands

| Command | What it does |
|---------|-------------|
| `/status` | Session-start status check: project state, fidelity coverage, open findings, escalations. **Run at the start of every session.** |
| `/checkpoint` | Create a verified git checkpoint. Determines tier, runs gate, stages by name, proposes message, reports hash. |
| `/integrity` | Repo-wide health audit: reference integrity, work item completeness, documentation freshness, spec consistency, open item triage. Appends report to `specs/findings.md`. |

---

## Before Your First Session: Language-Specific Setup

The blueprint's role files reference two placeholders — `<test-command>` and `<lint-command>` — that must be filled in for your project. Beyond those, you can add a language-specific coding guide that the Architect and Implementer roles load automatically.

### Step 1 — Set the test and lint commands

In your project's root `CLAUDE.md` (the one Claude reads at session start), add:

```markdown
## Project Commands

- Test: `pytest` (or `npm test`, `go test ./...`, etc.)
- Lint: `ruff check . && mypy .` (or `tsc --noEmit`, `golangci-lint run`, etc.)
```

### Step 2 — Create a language coding guide

Copy or adapt `.claude/coding/python.md` for your language. Place it at `.claude/coding/<language>.md`.

The file should cover: style conventions, testing patterns, error handling rules, common anti-patterns, and the exact lint/type-check commands to substitute for `<lint-command>`.

### Step 3 — Create the coding guide file

Both `architect.md` and `implementer.md` already contain this precondition:

```
If a language coding guide exists (.claude/coding/<language>.md), read it before writing code
```

All you need to do is create the file. Copy and adapt `.claude/coding/python.md` for your language and place it at `.claude/coding/<language>.md`. Both roles will pick it up automatically.

### Included example

`.claude/coding/python.md` — a complete Python guide covering PEP 8, type hints, pytest patterns, error handling, anti-patterns, and the `ruff` / `mypy` / `black` command trio.

---

## Adapting the Blueprint

The `docs/` directory (if present in your project) coexists with `specs/`. `docs/` holds explanatory and reference material (how it works, how to operate it). `specs/` holds behavioral ground truth (what the system must do and must never do). Do not merge them.
