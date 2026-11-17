# 0002 Persistent naming through feature-derived names with geometric matching

- Status: Accepted
- Deciders: maintainers of this repository

## Context

Sketches, features, joints, annotations, drawing dimensions and toolpaths refer to faces, edges and
vertices of bodies that are regenerated after every upstream edit. Kernel topology indices change on
regeneration, so references based on them attach to the wrong geometry or fail. `arc-brep` provides
persistent topological naming primitives; SolidBuilder must decide how its references use them and
what happens when they fail.

## Decision

Every topological entity produced by a feature receives a name derived from the feature's stable ID
and the generating input (profile edge, tool face, split or merge history), as recorded by `arc-brep`.
A persistent reference stores that name plus a geometric signature (surface type, normal or axis,
area or length, centroid, adjacency). Resolution after regeneration is: exact name match; then name
history through splits and merges; then signature matching among candidates of the same type, accepted
only above a confidence threshold. A reference that resolves to several candidates or none is marked
broken, keeps its last geometry for display, and is listed for repair. The product never reattaches a
reference silently below the threshold.

## Consequences

- Each feature implementation must declare naming for every entity it creates; this is tested.
- A naming regression corpus of models with scripted upstream edits is part of CI; a change that
  lowers its pass rate is rejected.
- Repair tools (one-pick remap, reference audit) are first-class features.
- The details are specified in [feature history](../architecture/feature-history.md).

## Alternatives considered

- Kernel topology indices with heuristics after the fact: the failure mode the decision exists to
  avoid.
- Pure geometric queries evaluated at regeneration (select "the top planar face"): robust for some
  intents, but ambiguous for general picks; kept as an explicit query option for users and scripts.
- Requiring users to reference only datums: good practice, encouraged, but not acceptable as the only
  safe path.

## Trigger to reopen

The naming corpus pass rate stalls below the target in [feature history](../architecture/feature-history.md)
for two consecutive releases, or a kernel change in `arc-brep` provides a stronger naming model.
