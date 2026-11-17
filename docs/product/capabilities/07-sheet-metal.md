# Sheet metal

Sheet metal parts are modeled as folded bodies of constant thickness governed by a sheet metal rule,
and always carry a flat pattern that regenerates with them. Bend data, blank size and the flat outline
feed drawings, cutting and nesting without manual re-measurement.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Sheet metal rule | Material, thickness or gauge, inner bend radius, unfold method (K-factor, bend allowance, bend deduction, bend table), relief types and sizes, corner rule, miter gap, minimum flange length | Stored in a Document or a rule library; assigned to a sheet metal Part or Body; can vary by Configuration |
| Gauge table | Gauge names mapped to thickness and default radius per material | Referenced by Sheet metal rules |
| Bend table | Rows of thickness, radius and angle mapped to allowance or deduction | Referenced by Sheet metal rules as an unfold method |
| Sheet metal feature | Kind (face, flange, contour flange, roll, lofted flange, hem, fold, bend, corner seam, relief, rip, cut, punch, tab and slot), parameters, rule overrides | Entry in the Feature history of a sheet metal Body |
| Bend | Angle, inner radius, direction (up or down), unfold method in effect, sequence number | Derived from features; listed in the Flat pattern and bend tables |
| Flat pattern | Base face, A-side, orientation, extents, bend lines, punch marks, flat-only features | Derived from one sheet metal Body; regenerates with it; source for drawing views and DXF output |
| Punch template | Profile and depth geometry, punch ID, placement point, orientation | Feature template in the standard parts library; placed by the punch feature |

## Rules and setup

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SHEET-001 | Sheet metal rule | Defines material, thickness, inner bend radius, unfold method, relief types and sizes, corner rule, miter gap and minimum flange length in one named rule. Changing the rule regenerates every feature and the flat pattern of the bodies using it. | T2 | 4 |
| SB-SHEET-002 | Unfold methods | Computes developed length by K-factor, by bend allowance, by bend deduction or by lookup in a bend table, interpolating between table rows and refusing values outside the table range with a clear message. | T2 | 4 |
| SB-SHEET-003 | Per-bend overrides | Overrides radius, unfold method or relief on individual bends while keeping the rule as default, with overridden bends marked in the feature panel and flat pattern. | T2 | 4 |
| SB-SHEET-004 | Gauge tables | Selects thickness and default radius by gauge name per material from a gauge table. | T2 | 4 |
| SB-SHEET-005 | Rule libraries | Stores sheet metal rules, gauge tables and bend tables in shared library documents, with a read-only set of starter rules and a command that updates documents from the library when a rule changes. | T2 | 4 |
| SB-SHEET-006 | Rule per configuration | Assigns a different sheet metal rule per Configuration so one part produces variants in several materials or thicknesses, each with its own flat pattern. | T2 | 4 |
| SB-SHEET-007 | Convert to sheet metal | Converts a solid or shelled body of constant thickness into a sheet metal body by picking a base face and rip edges; fails and highlights faces whose thickness differs from the rule. | T2 | 4 |

## Base and flange features

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SHEET-008 | Base face | Creates a flat sheet metal body from one or more closed sketch profiles with the rule thickness on a chosen side. | T2 | 4 |
| SB-SHEET-009 | Edge flange | Adds a flange to one or more edges with height measured from the outer, inner or tangent datum, angle, bend position (material inside, outside, bend from outer face), and full, offset, centered or to-geometry width. Reliefs are added from the rule. | T2 | 4 |
| SB-SHEET-010 | Contour flange | Creates flanges by sweeping an open sketch profile along an edge path or a distance, inserting bends at sketch corners with the rule radius. | T2 | 4 |
| SB-SHEET-011 | Contour roll | Creates a rolled body by revolving an open profile about an axis by an angle, with the unrolled length computed by the rule. | T2 | 4 |
| SB-SHEET-012 | Lofted flange | Creates a transition between two open or closed profiles, either as a formed loft or as press-brake facets with a chord tolerance, and develops it in the flat pattern. | T2 | 4 |
| SB-SHEET-013 | Hem | Adds a closed, open, teardrop, rolled or double hem to edges with gap, length and radius parameters. | T2 | 4 |
| SB-SHEET-014 | Fold | Bends a portion of a flat face along a sketch line with angle, direction and bend position. | T2 | 4 |
| SB-SHEET-015 | Bend between faces | Adds a bend that connects two separate faces of a sheet metal body, extending or trimming them to meet. | T2 | 4 |
| SB-SHEET-016 | Join by bend | Joins two sheet metal bodies of the same rule at selected edges with a bend, producing one body and one flat pattern. | T3 | 4 |

## Corners, reliefs and cuts

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SHEET-017 | Corner seam | Closes the corner between two flanges with overlap, butt or miter treatment and a gap. | T2 | 4 |
| SB-SHEET-018 | Corner relief | Applies a round, square, tear or trim-to-bend relief where two or three bends meet, using the rule default or a per-corner override. | T2 | 4 |
| SB-SHEET-019 | Bend relief | Applies straight, round or tear relief with width and depth where a bend does not run the full edge length. | T2 | 4 |
| SB-SHEET-020 | Corner round and chamfer | Rounds or chamfers the thickness-direction corner edges of a sheet metal body, keeping them valid in the flat pattern. | T2 | 4 |
| SB-SHEET-021 | Sheet metal fillet and chamfer | Applies fillets and chamfers that are aware of thickness and bends, so the operation stays consistent on both faces and unfolds correctly. | T2 | 4 |
| SB-SHEET-022 | Rip | Splits a sheet metal body along an edge, a face or between two points to allow unfolding, with a gap. | T2 | 4 |
| SB-SHEET-023 | Normal cut | Cuts profiles through the sheet perpendicular to the faces, including across bends by wrapping the profile around the bend. | T2 | 4 |
| SB-SHEET-024 | Unfold and refold | Temporarily unfolds selected bends so features can be added on the flat face, then refolds them; features added while unfolded follow the material on refold. | T2 | 4 |
| SB-SHEET-025 | Sheet metal hole | Places holes that are perpendicular to the sheet and stay associated with the face through bends and flat pattern. | T2 | 4 |
| SB-SHEET-026 | Punch | Places a punch template at sketch points with orientation and depth; the punch ID and its marks appear in the flat pattern and tables. | T2 | 4 |
| SB-SHEET-027 | Tab and slot | Creates tabs on one sheet metal body and matching slots in another at a shared edge, with tab length, spacing, count and clearance parameters. | T3 | 4 |
| SB-SHEET-028 | Forming features | Creates louvers, lances, beads and embossed forms from feature templates with formed geometry in the folded model and a marker or outline in the flat pattern. | T3 | 5 |

## Flat pattern

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SHEET-029 | Flat pattern | Creates the flat pattern of a sheet metal body with base face, A-side and orientation by edge or angle; the flat pattern regenerates automatically after every folded-model change. | T2 | 4 |
| SB-SHEET-030 | Bend lines and bend order | Shows bend center lines and bend extents with direction and angle, assigns a bend sequence automatically or manually, and exposes the sequence to drawings. | T2 | 4 |
| SB-SHEET-031 | Flat pattern edits | Adds cuts, corner treatments and marks that exist only in the flat state, and sets flat extents orientation; flat-only edits survive folded-model changes when their references resolve. | T2 | 4 |
| SB-SHEET-032 | Blank size properties | Exposes flat length, width, area, thickness, bend count and mass as properties usable in the bill of materials and drawings. | T2 | 4 |
| SB-SHEET-033 | Bend data | Exposes per-bend angle, radius, direction, developed length and unfold method as data used by bend tables in [Drawings](12-drawings.md). | T2 | 4 |
| SB-SHEET-034 | Flat pattern DXF and DWG export | Writes the flat pattern to DXF or DWG with a layer map for outer profile, inner profiles, bend lines up and down, bend extents, punch marks and text, optional conversion of splines to polylines or arcs with a tolerance, and origin at the extents corner. | T2 | 4 |
| SB-SHEET-035 | Flat pattern interference check | Reports overlaps of material in the developed flat pattern, such as flanges that unfold onto each other, with their locations. | T2 | 4 |
| SB-SHEET-036 | Conical and rolled development | Develops cones and rolled cylinders exactly, and approximates doubly curved transitions by facets with a stated tolerance and a warning. | T3 | 4 |
| SB-SHEET-037 | Flat pattern output to nesting | Publishes flat patterns with material and thickness to nesting as described in [Nesting and fabrication](25-nesting-and-fabrication.md), and updates them when the part changes. | T3 | 5 |

## Context and checks

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SHEET-038 | In-context sheet metal | Creates sheet metal parts inside an assembly using edges and faces of other occurrences as references, stored as external references that update the part and flat pattern. | T2 | 4 |
| SB-SHEET-039 | Derived sheet metal | Derives a sheet metal part from another sheet metal part or a mirrored copy, keeping rule and flat pattern behavior. | T2 | 4 |
| SB-SHEET-040 | Manufacturability checks | Checks flange length against the rule minimum, hole-to-bend and hole-to-edge distances, relief sizes and bend radius against thickness, listing violations with locations. | T2 | 4 |
| SB-SHEET-041 | Multi-body sheet metal | Holds several sheet metal bodies in one part, each with its own rule and flat pattern. | T2 | 4 |
