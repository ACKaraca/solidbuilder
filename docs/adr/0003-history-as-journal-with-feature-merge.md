# 0003 Feature history as journaled operations, merged at feature granularity

- Status: Accepted
- Deciders: maintainers of this repository

## Context

The suite records every change as element-level operations in the `arc-doc` journal. SolidBuilder adds
an ordered feature history per component and promises branching and merging of design history.
Merging whole files or geometry is not meaningful; merging must work on the objects users think in.

## Decision

Features, sketches, sketch constraints, parameters, occurrences, joints and annotations are separate
typed objects with stable IDs in the document store. A feature history is an ordered list of feature
IDs plus a rollback marker; order is data, not implicit in storage. Geometry is never stored as the
source of truth for a history-enabled component: it is a cached, revision-tagged derivative. A merge
replays the change sets of one branch onto another object by object. Edits to different objects merge
automatically; edits to the same property of the same object, or order changes that conflict, become
conflicts that keep both values. After a merge the history is regenerated and any new failures are
reported like any other regeneration failure.

## Consequences

- Merges are deterministic and explainable per object.
- Direct edits on history-enabled components are recorded as features so they merge like other
  features.
- The cached geometry can be discarded and rebuilt; documents may store it to open quickly.
- History-free components merge at body level only; their conflicts are whole-body choices.

## Alternatives considered

- Snapshot-based versions with whole-document choice on conflict: loses work.
- CRDT documents: rejected suite-wide; ordering semantics of a feature history do not fit.
- Merging geometry: not well defined for parametric intent.

## Trigger to reopen

Users report merges that are correct per object but produce invalid designs often enough that a
semantic merge layer (for example sketch-level constraint merging) is required.
