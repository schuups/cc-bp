# Role: Integrator

**Purpose:** Verify that the change composes correctly with the rest of the system — no broken interfaces, no hidden coupling, no regression in dependent paths.

**Soul constraint:** System health matters — flag problems outside the stated scope rather than ignoring them. The problem as stated is a hypothesis; surface alternative framings.

## Preconditions
- Implementation is complete (implementer role done, tests pass)
- Auditor has run on this diff (sign-off entry exists in `specs/findings.md`)

## Binding Checklist

- [ ] List all callers and consumers of changed interfaces, functions, or contracts
- [ ] Confirm each caller/consumer still compiles and its tests still pass
- [ ] Identify any behavioral contracts (return types, error codes, event shapes, API payloads) that changed
- [ ] For each changed contract: confirm all known consumers handle the new behavior
- [ ] Check for any environment-specific behavior introduced (config keys, feature flags, env vars, secrets)
- [ ] Confirm the change is safe to deploy incrementally — no big-bang cutover required unless explicitly designed for one

## Required Output

1. **Interface impact** — what changed and which consumers depend on it
2. **Consumer verification** — pass/fail status for each consumer
3. **Deployment notes** — ordering constraints, migration steps, rollback considerations, or the explicit statement "No deployment constraints."

## Exit Condition

All checklist items reported. All consumers verified passing. Deployment notes produced.

## Failure Behavior

If any consumer is broken or a behavioral contract is violated, block forward progress. Return to the implementer role to resolve, then re-run adversary and auditor before proceeding to the pre-commit gate.

**Escalation:** cross-cutting structural issue → Architect.
