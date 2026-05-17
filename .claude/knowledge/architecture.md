# Architecture

> **Purpose:** Describes the system as it currently exists — reality as built, not design intent. For the rationale behind decisions, see `.claude/knowledge/adr/`. This file is the reference point for compliance checks: if the code diverges from what is written here, either the code or this file is wrong.
>
> **Producers:** `/onboard` creates the initial structure; the `architect` role updates it after accepted ADRs; the `implementer` role flags discrepancies and requests updates when implementation reveals gaps.
>
> **Consumers:** `auditor` (compliance and doc-freshness checks), `/ccbp:checkpoint` Check 4 (architecture compliance), `/ccbp:map` Step 2 (alignment notes), `implementer` (component boundaries and constraints before coding).
>
> **Conventions:**
> - **System Boundaries** — list only what this system *owns*; delegation and exclusions are equally important to state explicitly
> - **Component Map** — each row must correspond to a real, currently-existing component; remove rows when components are deleted
> - **Key Technical Decisions** — non-obvious choices only; obvious or standard choices do not belong here; full rationale lives in the linked ADR
> - **Known Technical Debt** — active liabilities, not aspirational cleanup; remove entries when debt is resolved
> - **Cross-cutting Concerns** — describe the *current* approach, not the intended one; if the approach is "none yet", say so explicitly

---

## System Boundaries

**This system owns:**
- <!-- list responsibilities -->

**Delegates to:**
- <!-- external systems, services, or libraries this system relies on -->

**Explicitly out of scope:**
- <!-- what this SYSTEM COMPONENT intentionally does not own or handle — architectural and technical boundaries, not product ones. Examples: "authentication is delegated to the auth service", "does not own user profile storage", "email delivery is handled by the notification service". For product-level scope see root CLAUDE.md. -->

---

## Component Map

| Component | Responsibility | Key interfaces | Primary files |
|-----------|---------------|----------------|---------------|
| | | | |

---

## Key Technical Decisions

Non-obvious choices. For full decision records see `.claude/knowledge/adr/INDEX.md`.

| Decision | Rationale | Alternative rejected | ADR |
|----------|-----------|---------------------|-----|
| | | | |

---

## Known Technical Debt

| Area | Nature | Deferred because | Resolve when |
|------|--------|-----------------|-------------|
| | | | |

---

## Infrastructure and Environment

<!-- Non-obvious runtime facts: required env vars, startup order, prod/dev differences, deployment targets -->

### CI/CD

> **Purpose:** Captures CI/CD conventions discovered during onboarding or added as the project evolves. Populated by `/onboard` from detected CI/CD configuration; updated manually as pipelines change.

| Aspect | Detail |
|--------|--------|
| Platform | <!-- e.g. GitHub Actions, GitLab CI, CircleCI, Jenkins --> |
| Config file(s) | <!-- e.g. .github/workflows/*.yml --> |
| Trigger conditions | <!-- e.g. push to main, PR opened, tag push --> |
| Build steps | <!-- compile, lint, test, package — in order --> |
| Test command | <!-- exact command used in CI --> |
| Deploy targets | <!-- staging / production; how promotion works --> |
| Secrets management | <!-- where secrets are stored and how they reach the pipeline --> |
| Non-obvious behaviour | <!-- flaky steps, required env vars, ordering constraints, manual gates --> |

---

## Performance and Scale Constraints

<!-- Known bottlenecks, capacity limits, or scale assumptions the design rests on -->

---

## Cross-cutting Concerns

| Concern | Current approach |
|---------|-----------------|
| Authentication | |
| Authorization | |
| Logging | |
| Error handling | |
| Feature flags | |
