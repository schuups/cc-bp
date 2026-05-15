# BDD Feature Specifications

<!--
OPTIONAL. Populate this directory if the project uses Behavior-Driven Development.
If not using BDD, leave this directory empty or delete it.
-->

BDD scenarios are living specifications — human-readable AND executable. When present, they become the primary evidence for fidelity ratings and the canonical definition of "done" for Feature mode.

## Convention

- One `.feature` file per domain concept or user-facing capability
- Filename mirrors the concept: `booking-confirmation.feature`, `payment-retry.feature`
- Scenarios are written in Gherkin (Given / When / Then)
- Scenarios live here; test step implementations live in the test suite

## Done Criteria (when BDD is active)

Feature mode done requires:
- All scenarios in affected `.feature` files pass
- No new scenarios are skipped or pending without a `specs/findings.md` entry
- Fidelity rating on changed areas is THOROUGH

## Relationship to fidelity-index.md

Each `.feature` file should have a corresponding row in `specs/fidelity-index.md`. Coverage ratings apply at the scenario level: THOROUGH means all scenarios pass with no pending steps; MODERATE means happy path passes but edge cases are missing.

## Format

```gherkin
Feature: <domain concept or capability>

  Background:
    Given <shared precondition>

  Scenario: <specific behaviour>
    Given <precondition>
    When  <action>
    Then  <expected outcome>

  Scenario: <edge case or failure path>
    ...
```
