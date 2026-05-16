# REFACTOR.md — Blueprint Refactoring Working Document

A collaborative scratchpad. Observations are mine; decisions are ours.

**Legend:** 🔴 OPEN — still thinking · 🟠 VETTED — confirmed, not yet in CLAUDE.md · ✅ DONE — implemented in CLAUDE.md with user approval

---

## Purpose Statement

Copy the blueprint's `.claude/` folder into any project (replacing whatever `.claude/` was there before), then run `/onboard`.
Onboarding is always the first step — it is not optional.
All subsequent development work runs inside the framework the blueprint provides.

Use cases to support (primary first):
1. **Functional requirements** — user brings a new requirement; blueprint guides analysis → design → build → verify
2. **Greenfield** — scaffolding a new software product from scratch
3. **Feature/refactoring** — adding large features to a blueprint-started project
4. **Brownfield extension** — extending a pre-existing project developed by others
5. **Understanding** — exploring and asking questions about an existing codebase

---

## Category B — Structural clarity

### 🔴 B1. No table of contents
At 1900+ lines, there's no way to navigate the file without scrolling.
→ Add a TOC at the top. Defer until content is stable (after R5 flow redesign).

### 🔴 B2. Flat heading hierarchy
Everything uses `#` or `##` with no clear nesting. Major conceptual groups (Soul, Roles, Protocols, Rules, Guardrails, Knowledge) are invisible without reading.
→ Establish consistent hierarchy. Defer until content is stable.

### 🔴 B3. Design mode and Feature mode duplicate Phase 1–2
Feature mode includes a full Discover → Define description, which is also the entire content of Design mode.
→ Revisit when R5 (flow methodology redesign) is complete. Design mode may be recast or eliminated.

---

## Category C — Blueprint usability

### 🔴 C1. No "how to use this blueprint" section
A developer copying the blueprint has no guide for what to do first, what to fill in, what to leave alone.
→ Resolved conceptually by R1 (onboarding command). The quick-start in CLAUDE.md becomes: "copy, then run `/onboard`".
A brief human-readable quick-start still belongs in the blueprint README (see R4).

### 🔴 C2. Fill-in sections are not visually distinct
`Project Context` and `Architecture Knowledge` sections mix template scaffolding with live-content appearance.
→ These sections will be populated by `/onboard` (R1). Template markers can be simplified once onboarding exists.

### 🔴 C3. Git rules section has fill-in instructions invisible in rendered Markdown
HTML-comment options (`<!-- FILL THIS IN per project -->`) vanish when rendered.
→ Default: ask during onboarding (E4). Replace with a visible placeholder that `/onboard` overwrites.

### 🔴 C4. Python coding guide is too specific for a generic blueprint
The guide implies Python is the default language. Language-specific content should not be hardcoded.
→ Remove Python guide from blueprint core (per E2). Replace with a generic placeholder. `/onboard` generates `.claude/knowledge/coding-guide.md` for the actual project language.

### 🔴 C5. No specification template files provided
The blueprint references many files in `specs/` that must exist before protocols can run; none are in the repo.
→ Subsumed into R2. Path changes: `specs/` → `.claude/knowledge/specifications/` (per E3). All templates to be created there.

---

## Category D — Content gaps

### 🔴 D1. No onboarding path for Brownfield projects
The blueprint handles Brownfield state conceptually but gives no specific guidance for the initial session on an existing codebase.
→ Addressed by R1. The `/onboard` command's analysis phase covers brownfield: reads existing code, docs, package files, git log, and adapts questions accordingly.

### 🔴 D2. No guidance on what "Greenfield" means for the spec workflow
A Greenfield project has no code yet — most gate checks can't run. The bootstrap problem is undefined.
→ Addressed by R1. Onboarding detects greenfield and adjusts protocol guidance: gate checks become N/A until code exists.

### 🔴 D3. `/status` auto-run conflicts with fresh install
On a fresh install all `specs/` files are missing; the status report fails or produces meaningless output.
→ Status command must detect "blueprint not yet onboarded" and redirect to `/onboard`.
Add a pre-onboarding state to the mode selection table.

---

## Category E — Design decisions ✅

All decided. Captured here for traceability.

| | Decision |
|-|----------|
| E1 | Single-file CLAUDE.md is **transitional**. Split into subfiles only when refactoring is fully approved by user. |
| E2 | Language-specific coding guides **removed from blueprint core**. Generated per-project by `/onboard`. |
| E3 | `specs/` eliminated. Specifications move to **`.claude/knowledge/specifications/`**. |
| E4 | Blueprint project: **main branch** (direct commits). Destination projects: ask during `/onboard`. |

---

## Category F — Polish (after structural work)

### 🔴 F1. Mode selection table: Review vs Audit distinction unclear
`Review` and `Audit` look similar; could use a two-sentence example or clarifying note.

### 🔴 F2. Sweep and AdvSweep completion report formats differ subtly
Align the two report formats for consistency.

### 🔴 F3. Pre-commit gate check #5 has a redundant skip note
Check #5 says "skip for Spec-only and Sweep checkpoints" but the gate is defined as running only on Feature/Bugfix. The note is redundant noise.

### 🔴 F4. Fill-in pattern inconsistency
`<!-- FILL THIS IN -->` pattern in Pull Request Rules differs from other fill-in patterns throughout the file. Harmonize.

---

## New Requirements

### 🔴 R1. Startup/Onboarding Command (`/onboard`)

Personalises the blueprint for the specific project. Always run immediately after copying the blueprint.

**Phase 1 — Analysis** (before any questions):
- Read all source files, package/config files, existing docs, git log
- Detect: language(s), test framework, existing CI, project type signals
- Identify what's already answerable vs what needs to be asked

**Phase 2 — Adaptive questioning** (only what analysis can't determine):
- Project type: greenfield / adding features / brownfield extension / understanding-only
- Primary language(s) if not detected → used to generate `.claude/knowledge/coding-guide.md`
- Git workflow for this project: trunk-based (main branch) vs feature branches + PRs
- Minimum reviewers for PRs (if feature branch workflow chosen)
- Any other project-specific constraints surfaced by analysis

**Phase 3 — Populate knowledge files**:
- Write/update `.claude/knowledge/context/project-context.md`
- Write/update `.claude/knowledge/context/architecture.md`
- Write `.claude/knowledge/coding-guide.md` (language-specific rules)
- Initialise empty templates in `.claude/knowledge/specifications/`

Key constraint: questions must be adaptive — don't ask what analysis already answered.

---

### 🔴 R2. Knowledge Folder Structure

Target layout after onboarding:

```
.claude/
  knowledge/
    project-context.md            ← project description, users, constraints
    architecture.md               ← system boundaries, components, decisions
    coding-guide.md               ← language-specific rules (generated by /onboard)
    specifications/
      fidelity-index.md
      findings.md
      escalations.md
      deferred.md
      domain-model.md
      invariants.md
      assumptions.md
      ubiquitous-language.md
      failure-modes.md
      adr/
        README.md
        0001-template.md
```

Actions required:
- Create all template files under `.claude/knowledge/specifications/`
- Update all `specs/` references in CLAUDE.md to `.claude/knowledge/specifications/`

---

### 🔴 R3. Documentation Rebuild Command (`/docs`)

Rebuilds user-facing artifacts from `.claude/knowledge/specifications/`:
1. `documentation/` folder at project root — user-facing documentation
2. Root `README.md` — project overview with brief blueprint mechanics section

Behaviour: diffs current docs against specs before rewriting (incremental, not full rewrite).

🔴 Open (R3a): The outer loop ends with step 6 (documentation update). Should Claude run `/docs` automatically as part of every feature flow — after the Tier 2 implementation checkpoint commits the code — or should the user call it explicitly when they want docs updated?
- **Automatic**: docs always stay in sync, but generates output the user may not want yet (e.g. mid-series of related features)
- **Explicit**: user decides when docs are ready to be regenerated, at the cost of potentially forgetting

---

### 🟠 R4. README Distinction

| File | Purpose | Status |
|------|---------|--------|
| `README.md` in this blueprint repo | How to use the blueprint: copy instructions, what `/onboard` does, quick-start | Missing — must be created |
| `README.md` in destination projects | About the project; brief section on blueprint mechanics | Generated by `/docs` |

The blueprint's own README must be created as part of this refactoring.

---

### 🔴 R5. Flow Methodology Redesign

**Problem**: The current CLAUDE.md flow is hard to follow. The primary workflow is: "I have a new functional requirement — help me analyse, design, build, and verify it."

**Two triggers for the outer loop:**
- **New requirement** — user brings a functional requirement to implement
- **Bug report** — user has discovered and is reporting a defect (routes to Bugfix mode at step 4, skipping design)

**Target outer loop** (new requirement path):
```
Trigger: new requirement stated by user
  │
  1. Requirements clarification
  │   Analyst: understand, question, reframe
  │   → USER APPROVES before proceeding
  │
  2. Design
  │   Architect: options, consequences, ADR draft
  │   Adversary: attack the design
  │   → USER APPROVES ADR before proceeding
  │
  3. Acceptance criteria defined
  │   Captured in ADR before any code is written
  │   → USER APPROVES criteria before proceeding
  │
  4. Implementation
  │   Implementer: code per ADR
  │   Adversary gate: attack the implementation (hard block on CRITICAL)
  │   Auditor gate: verify changeset against ADR and acceptance criteria
  │   Integrator: verify composition with rest of system
  │
  5. Verification
  │   Pre-commit gate (7 checks) + checkpoint
  │
  6. Documentation update
      /docs
```

**On user approval gates:** At steps 1, 2, and 3 Claude presents its output and explicitly asks the user to approve or provide feedback before proceeding. Feedback loops back into the current step — not forward. No step begins until the previous one is approved.

**On definition of done:** The adversary/auditor/integrator are the DoD evaluation mechanism. Their effectiveness depends on the ADR capturing explicit acceptance criteria before implementation begins (step 3). Without this, the auditor can only verify against tests and the diff — not against intent. The ADR template needs an `Acceptance Criteria` section added to make this concrete.

**On bug trigger:** Bugs bypass steps 1–3 and enter at step 4 via Bugfix mode (test-first: reproduce → failing test → fix → adversary → auditor → pre-commit gate → checkpoint). User approval is still required before the fix is committed.

The Double Diamond is a useful sub-model within steps 1–4, not the top-level framing.

🔴 Open: explicit `Requirements` mode, or requirements-first step inside Feature mode? (R5a)

---

### 🔴 R9. Organise `specifications/` into subfolders, create templates, update CLAUDE_NEXT

`specifications/` currently has no internal structure. It should be split into named subfolders reflecting what lives there:
- **domain/** — domain knowledge (domain model, ubiquitous language)
- **constraints/** — system invariants
- **health/** — project health tracking (findings, escalations, fidelity index, failure modes, assumptions)

For each subfolder: create the files, provide templates where useful, and update the Knowledge Folder section in `CLAUDE_NEXT.md`.

---

### 🔴 R7. Each command, mode, and role must reference its knowledge files

When porting each command, mode, and role into `CLAUDE_NEXT.md`, explicitly state which `.claude/knowledge/` files it reads and which it updates. This ensures no file is silently ignored and makes context requirements visible per element.

---

### 🔴 R14. Verify that `adversary-findings.md` and `audit-log.md` are actually maintained in practice

**Concern:** these files may be stale by design. The Adversary role is supposed to write to `adversary-findings.md` and the Auditor role to `audit-log.md`, but the role instructions in CLAUDE.md still reference the old `specs/findings.md`. Until the roles are ported to CLAUDE_NEXT.md with explicit file references, nothing reliably writes to these files.

**Questions to answer before porting:**
- Are the role instructions specific enough that Claude will reliably write to these files during Feature/Bugfix flows, or will it omit the step unless explicitly reminded?
- Should the role definitions include a concrete append-format template (like `audit-log.md` already has) so the output is structured and checkable?
- Is there a structural risk that both files accumulate entries but are never read — making them write-only logs that provide no value?

**Action:** when porting Adversary and Auditor roles into CLAUDE_NEXT.md, verify the write step is explicit and test that it happens in practice before marking the roles as vetted.

---

### 🔴 R6. Drift Prevention — make it visible

Mechanisms already present: Sweep, AdvSweep, Integrity Audit, fidelity index, pre-commit gate.
These are good but buried. They need to appear as explicit ongoing activities in the reorganised flow, not just protocols to invoke on demand.

---

### 🔴 R10. Testing and performance measurement of the blueprint

The blueprint is a set of instructions that shapes Claude's behaviour. Testing it is different from testing code: the output is probabilistic, quality requires human judgement, and the cost of running scenarios is non-trivial. Below is a breakdown of approaches.

#### A. Behavioural / functional testing (does Claude do what the blueprint says?)

- **Scenario library**: define a canonical set of input scenarios covering every mode and trigger — greenfield project, brownfield extension, new feature, bug report, understanding-only session, onboarding on an empty folder, onboarding on a large existing codebase
- **Protocol compliance**: for each scenario, verify the correct mode is selected, the correct role sequence is activated, and gates block when they should (e.g. CRITICAL finding prevents commit)
- **Checklist execution**: verify roles actually execute their full checklists rather than skipping items silently
- **Gate integrity**: deliberately inject a CRITICAL finding and confirm forward progress is blocked

#### B. Output quality testing (does Claude produce good outputs?)

- Per-role rubrics: define what a high-quality output looks like for each role
  - Analyst: problem statement uses no solution language; open questions are genuine blockers
  - Adversary: findings are actionable, not hypothetical; all nine attack vectors reported
  - Auditor: sign-off references the ADR; FAIL findings are specific and traceable
- Human-evaluated spot checks rather than automated pass/fail
- Compare outputs across multiple runs of the same scenario to assess consistency

#### C. Context efficiency measurement (is the blueprint token-lean?)

- Measure tokens consumed by CLAUDE.md per session (baseline before and after each refactoring pass)
- Identify sections loaded in every session that are rarely or never invoked in practice
- Track token cost vs. behavioural benefit ratio when adding or removing sections
- Target: onboard command self-deletion is the first example of this principle — measure its effect

#### D. Regression testing (do blueprint changes break existing behaviour?)

- Before merging any CLAUDE.md change, run a fixed set of scenarios and compare outputs
- Define a small set of invariant behaviours that must never change regardless of blueprint edits (e.g. adversary always runs before commit, no `git add .` ever)
- Automated: token counts, presence of required output sections; Manual: quality of reasoning

#### E. Onboarding-specific testing

- Run `/onboard` against at least four project archetypes: empty folder, single-language repo, multi-language repo, existing project with docs and CI
- Verify adaptive questioning skips what analysis already answered
- Verify all knowledge files are populated correctly and consistently

#### F. Metrics to track over time

| Metric | What it measures |
|--------|-----------------|
| Mode selection accuracy | Does Claude pick the right mode for the stated intent? |
| Gate compliance rate | Does Claude block on CRITICAL findings without being reminded? |
| Checklist completion rate | Are role checklists fully executed, not summarised? |
| Tokens per session (by section) | Which sections cost the most context? |
| Output consistency | Same input → same structure across runs? |
| Scenario pass rate | % of canonical scenarios producing an acceptable outcome |

#### G. Infrastructure needed

- A dedicated test project repository with the canonical scenario inputs and expected output rubrics
- A simple before/after comparison script for token counts when CLAUDE.md changes
- A log of spot-check results keyed to the blueprint version (git hash) at time of test

---

### 🔴 R12. Command disambiguation — avoid conflicts with plugins and MCP servers

**Problem**: Claude Code loads commands from multiple sources (project `.claude/commands/`, user-level, MCP plugins). If two sources define `/onboard` or `/docs`, Claude has no reliable way to choose the right one.

**Options considered:**

- **A — Namespace prefix** (`/bp:onboard`, `/bp:docs`): unambiguous by construction; ergonomically worse; the `bp:` prefix becomes meaningless once deployed to a specific project
- **B — Explicit resolution rule in CLAUDE.md**: add an instruction that commands matching the Commands table always resolve to `.claude/commands/`; simple but relies on Claude following the instruction reliably
- **C — Project-scoped prefix set by `/onboard`**: during Phase 4, rename commands to a project-specific prefix (e.g. `/forex:docs`); unambiguous and project-meaningful; adds complexity and makes commands inconsistent across projects

🔴 Open: which option? Or a combination?

---

### 🔴 R13. Literature review step in the development process

The development flow must include an optional but explicitly surfaced literature review step: before committing to a design, review what others have tried, what results they achieved, and what the current state of the art is.

**Applicability:**
- **Research projects**: essential — cannot responsibly design without knowing prior work
- **General software**: valuable even for web development — surfaces new libraries, patterns, deprecations, and trends that challenge assumptions about what we "already know"
- **Trivial tasks** (e.g. a single utility function): may be skipped, but the decision to skip must be explicit

**Where it fits in the flow** (relative to R5 outer loop): between requirements clarification (step 2) and design (step 3). The literature review informs design options; it does not produce them.

**Output and human involvement:** findings from the review must be presented to the user before design begins. The user decides how to act on them — Claude does not auto-incorporate findings into design choices. Results often require human judgement: a paper may describe a method that is theoretically superior but impractical given constraints.

**TODO:** define the triggering condition (when to run vs skip), the scope of the review (academic papers, GitHub repos, docs, blog posts), and how findings are recorded in `.claude/knowledge/`.

---

### 🔴 R11. Cleanup instructions — removing stale references, code, and deployments

The blueprint must guide cleanup when things are removed or changed, not just when things are added. This covers:
- Documentation references to removed features, APIs, or components
- Dead code, unused dependencies, obsolete configuration
- Infrastructure that no longer applies (e.g. an ingress, a service, a deployment no longer needed)

TODO: define how cleanup is triggered, what roles/modes are responsible, and what "done" looks like for a removal.

---

### 🔴 R15. Implement Sweep mode in CLAUDE_NEXT.md

Sweep is listed as ⬜ PROPOSED in ELEMENTS.md but has no implementation file and is not referenced in CLAUDE_NEXT.md.

Sweep is the primary mechanism for verifying that requirements and architecture are actually implemented as intended — the check that `/integrity` explicitly does not perform. Without it, there is no structured way to close the loop between specs and code.

**Actions required:**
- Port the Sweep mode protocol from CLAUDE.md into CLAUDE_NEXT.md, updated for the `.claude/knowledge/` path structure
- Verify that `fidelity-index.md` template supports the checkpoint-per-item workflow described in the protocol
- Consider whether Sweep warrants a `/sweep` command file, or whether intent-signal detection in CLAUDE_NEXT.md is sufficient
- Mark Sweep as ✅ VETTED in ELEMENTS.md once ported and reviewed

---

### 🔴 R8. Final step: replace CLAUDE.md with CLAUDE_NEXT.md *(last action of all)*

Once CLAUDE_NEXT.md is complete and approved:
1. Delete `.claude/CLAUDE.md`
2. Rename `.claude/CLAUDE_NEXT.md` → `.claude/CLAUDE.md`
3. Search the entire repo for any remaining occurrence of the string `CLAUDE_NEXT` and remove or correct each one

---

## Open Decisions

| | ID | Question |
|-|----|----------|
| 🔴 | R3a | `/docs`: run automatically after Feature checkpoint, or explicit call only? |
| 🔴 | R5a | Explicit `Requirements` mode, or requirements-first step inside Feature mode? |
