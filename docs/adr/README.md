# Architecture decision records

Decisions specific to SolidBuilder. Suite-wide decisions (repository topology, UI toolkit, geometry
tiers, document model and package, command model, regeneration, units, interoperability, rendering,
collaboration, extensibility, licensing, assistance, robustness, localization) are recorded in
[arc-suite/docs/adr](https://github.com/ACKaraca/arc-suite/tree/main/docs/adr) and are not restated
here.

## Index

| Number | Title | Status |
|---|---|---|
| [0001](0001-document-structure-by-choice.md) | Parts, assemblies and drawings in one document or separate documents | Accepted |
| [0002](0002-persistent-naming.md) | Persistent naming through feature-derived names with geometric matching | Accepted |
| [0003](0003-history-as-journal-with-feature-merge.md) | Feature history as journaled operations, merged at feature granularity | Accepted |
| [0004](0004-one-assembly-solver.md) | One solver for joints and assembly constraints | Accepted |
| [0005](0005-canonical-text-form.md) | Stable modeling API and canonical text form of the feature history | Accepted |
| [0006](0006-external-solvers-and-owned-toolpath-kernels.md) | External open solvers, owned toolpath kernels and a documented post-processor format | Accepted |

## Template

```markdown
# NNNN Title

- Status: Proposed | Accepted | Superseded by NNNN
- Deciders: maintainers of this repository

## Context

The forces at play: requirements, constraints, what makes the decision necessary.

## Decision

What is decided, stated so it can be checked.

## Consequences

What becomes easier, what becomes harder, what must now be built or tested.

## Alternatives considered

Each alternative with the reason it was not chosen.

## Trigger to reopen

The observable condition that would make this decision worth revisiting.
```
