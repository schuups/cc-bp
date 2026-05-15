# Domain Model

<!--
Replace this template with the actual domain model for this project.
Run Design mode with the Analyst and Architect roles to produce the initial content.
Each section below is a template — copy and repeat as needed.
-->

---

## Entities

<!-- An entity has identity that persists across state changes. Two entities with the same fields but different identities are not equal. -->

### `<EntityName>`

**Identity:** <!-- What uniquely identifies this entity? e.g. UUID, composite key -->
**State:** <!-- What mutable state does it carry? List fields with types and constraints. -->
**Lifecycle:** <!-- How is it created, updated, and destroyed? Who is responsible for each transition? -->
**Invariants enforced:** <!-- Which INV-NNN from invariants.md does this entity enforce locally? -->

---

## Value Objects

<!-- A value object has no identity — equality is determined entirely by its field values. Value objects are immutable. -->

### `<ValueObjectName>`

**Fields:** <!-- List fields, their types, and validity constraints -->
**Validity rules:** <!-- What makes this value object valid? What values are rejected? -->
**Immutability:** <!-- How is immutability enforced? (constructor-only, frozen, record type, etc.) -->

---

## Aggregates

<!-- An aggregate clusters entities and value objects behind a single root for consistency enforcement. External code only interacts with the root. -->

### `<AggregateName>` Aggregate

**Root:** `<EntityName>` <!-- Which entity is the aggregate root? -->
**Members:** <!-- Which entities and value objects belong inside this aggregate boundary? -->
**Consistency boundary:** <!-- What invariants does crossing this boundary enforce? -->
**External references:** <!-- Which other aggregates does this one reference by identity only (not by value)? -->

---

## Domain Events

<!-- Events that represent something meaningful that happened in the domain. Named in past tense. -->

| Event | Triggered by | Payload fields | Consumers |
|-------|-------------|----------------|-----------|
| `<EventName>` | | | |

---

## Relationships

| From | Relationship | To | Cardinality | Notes |
|------|--------------|----|-------------|-------|
| | | | | |

---

## Ubiquitous Language

See `specs/ubiquitous-language.md` — maintained as a separate file for independent enforcement and reference.
