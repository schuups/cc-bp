## Project Context

> If any field contains `<!--`, run `/onboard` first.
> This template is a starting point — during onboarding, Claude may add, remove, or rename fields to better fit the project.

- Vision: <!-- long-term direction and success criteria -->
- Description: <!-- what this project is and why it exists -->
- Success criteria: <!-- what success looks like; measurable outcome or north star -->
- Stakeholders: <!-- primary users and what they care about -->
- Technologies: <!-- languages, frameworks, infrastructure -->
- Execution context: <!-- how and where the software runs -->
- Python environment: <!-- Python binary path and virtual environment root, e.g. `.venv/bin/python` / `.venv` — fill in during onboarding to prevent repeated interpreter errors -->
- Key business constraints: <!-- regulatory, SLA, legacy compatibility, org mandates, budget -->
- Non-obvious technical facts: <!-- third-party quirks, prod/dev differences, legacy limits, known gotchas -->
- Explicitly out of scope: <!-- what this PROJECT intentionally does not do — product and business boundaries, not technical ones. Examples: "does not support mobile platforms", "does not handle billing or payments", "English-only in v1". See architecture.md for what the system component does not own. -->
- Open strategic questions: <!-- unresolved questions affecting project direction -->

## Domain Vocabulary

> One canonical term per concept. Populated by the `analyst` role during inception; updated as the domain evolves. If this section is present, `/ccbp:audit` Category 4 greps exported identifiers against the "Must not use" column and flags drift as a MEDIUM finding.
>
> Leave this section absent entirely if the project has no meaningful domain model (CLI tools, infra repos, build scripts).

| Term | Definition | Must not use |
|------|-----------|--------------|
| <!-- canonical name --> | <!-- what it means in this project's context --> | <!-- synonyms found in the wild that must be eradicated --> |
