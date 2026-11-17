# Plastic parts and molds

Plastic-part features apply a shared set of molding rules to bosses, ribs, snaps and walls so that a
housing stays moldable as it changes. Mold features then derive parting geometry and core and cavity
bodies from the finished part, associatively, so a part edit flows into the tooling.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Plastic rule | Material, nominal wall thickness, minimum and maximum wall, draft angle, rib thickness ratio, boss proportions, fillet radii, shrinkage | Stored in a Document or a rule library; assigned to a Part or Body; can vary by Configuration |
| Plastic feature | Kind (boss, rest, lip and groove, snap fit, grille, rib, rule shell, rule fillet), parameters derived from the Plastic rule, overrides | Entry in the Feature history; references sketch points and faces through Persistent references |
| Pull direction | Vector, parting reference | Shared by draft analysis, parting line and core and cavity features of one part |
| Parting line | Edge loop on the part separating core and cavity sides | Derived from the part Body and Pull direction |
| Parting surface | Sheet bodies extending the parting line to the mold boundary, plus shut-off surfaces | Feature in the mold tooling history; consumed by the split |
| Mold tool body | Kind (core, cavity, side core, lifter, insert), shrinkage-scaled part reference | Solid Body produced by splitting a mold block; linked to the source part |

## Plastic rules

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MOLD-001 | Plastic rule | Defines material, nominal wall, draft angle, rib and boss proportions, default fillet radii and shrinkage in one named rule. Plastic features take their defaults from the rule and regenerate when it changes. | T2 | 4 |
| SB-MOLD-002 | Rule libraries and configurations | Stores plastic rules in shared library documents and assigns a different rule per Configuration. | T2 | 4 |

## Plastic part features

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MOLD-003 | Screw boss | Creates a boss at sketch points with outer diameter, hole for a screw or insert (diameter, depth, chamfer), draft on inside and outside, base fillet and optional support ribs. A matching head-side boss with clearance hole and counterbore is created on the mating part when requested. | T2 | 4 |
| SB-MOLD-004 | Mounting rest | Creates a raised or recessed seat on a face for supporting a mating part, with outline from a sketch, height, draft and clearance. | T2 | 4 |
| SB-MOLD-005 | Lip and groove | Creates a lip on one part and a matching groove on the mating part along selected edges, with width, height, clearance and draft. | T2 | 4 |
| SB-MOLD-006 | Snap fit | Creates cantilever and hook snap fits with beam length, thickness, taper, undercut height, insertion and retention angles, and a matching catch with clearance on the mating part. | T2 | 4 |
| SB-MOLD-007 | Grille | Creates a vent pattern within a boundary sketch with ribs, spars and islands of set thickness, depth and draft, cut through or recessed. | T2 | 4 |
| SB-MOLD-008 | Support ribs and webs | Creates ribs from open sketch lines to the next face with thickness from the rule ratio, draft and a top fillet, and networks of ribs from several lines in one feature. | T2 | 4 |
| SB-MOLD-009 | Rule-based shell | Shells a part to the nominal wall from the rule, removing selected faces, with per-face thickness overrides inside the rule's minimum and maximum. | T2 | 4 |
| SB-MOLD-010 | Rule-based fillet | Fillets edges selected by rules, such as all edges between two features, all concave edges or all edges of a face set, with radius from the rule; edges created by later regeneration are filleted automatically when they match the rule. | T2 | 4 |
| SB-MOLD-011 | Draft from parting line | Applies draft to faces relative to the pull direction from a parting line or neutral plane, in one or both directions with separate angles. | T2 | 4 |
| SB-MOLD-012 | Gussets | Adds triangular gussets between two faces at sketch points or along an edge, with thickness, size and draft. | T3 | 4 |

## Moldability analysis

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MOLD-013 | Wall thickness analysis | Colors the part by local wall thickness against the rule minimum and maximum and lists regions that are too thin or too thick. Analysis recomputes after regeneration. | T2 | 4 |
| SB-MOLD-014 | Draft and undercut analysis | Classifies faces by draft relative to the pull direction against the rule angle and detects undercut regions that cannot release along the pull direction, highlighting them by side. | T2 | 4 |
| SB-MOLD-015 | Sink and boss proportion check | Checks ribs and bosses against the rule's thickness ratios and base fillet sizes and lists features likely to cause sink marks. | T3 | 4 |
| SB-MOLD-016 | Injection molding study link | Passes the part, material and gate locations to an injection molding study as described in [Simulation](26-simulation.md). | T3 | 5 |

## Mold tooling

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MOLD-017 | Shrinkage scaling | Creates a scaled reference of the part by uniform or per-axis shrinkage factors from the material, used as the source for tool bodies. The scaled reference updates when the part changes. | T3 | 4 |
| SB-MOLD-018 | Parting line | Finds the parting line from the pull direction as the silhouette between core-side and cavity-side faces, with manual edge selection where the split is ambiguous. | T3 | 4 |
| SB-MOLD-019 | Parting surface | Extends the parting line outward to a mold block boundary by ruled, swept or planar surfaces and closes through-openings with shut-off surfaces by patch. | T3 | 4 |
| SB-MOLD-020 | Core and cavity split | Splits a mold block around the shrinkage-scaled part along the parting and shut-off surfaces into core and cavity bodies; fails with a list of gaps when the surfaces do not close. | T3 | 4 |
| SB-MOLD-021 | Side cores, lifters and inserts | Splits additional tool bodies from the core or cavity using sketch or surface boundaries for undercut regions, keeping each as a separate body with its own pull direction. | T3 | 5 |
| SB-MOLD-022 | Mold base components | Places mold base plates and standard components such as ejector pins, guide pins and sprue bushings from the standard parts library, sized to the tool bodies. | T3 | 5 |
| SB-MOLD-023 | Runners and gates | Creates runner paths from sketches with a cross-section and gate geometry at selected points, cut into the tool bodies. | T3 | 5 |
| SB-MOLD-024 | Cooling channels | Creates drilled cooling channels from sketch lines with diameter, plugs and clearance checks against tool and part faces. | T3 | 5 |
| SB-MOLD-025 | Associative tooling update | Regenerates parting line, parting surface and tool bodies when the source part changes, and reports which tooling features failed. | T3 | 4 |
