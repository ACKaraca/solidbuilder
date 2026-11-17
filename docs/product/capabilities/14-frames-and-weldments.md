# Frames and weldments

Frame design places structural profiles along skeleton geometry and trims their ends, and a weldment
records the preparation, welds and post-weld machining that turn members and plates into one
fabricated assembly. Cut lists and weld data come from the model and feed drawings and the bill of
materials.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Frame member | Profile section, path (sketch line, edge or 3D sketch segment), orientation, offset, rotation, end treatments, cut length | Occurrence of a generated part; references skeleton geometry |
| Profile section | Standard, designation, cross-section sketch, material, mass per length | Library item in the standard parts library |
| End treatment | Kind (miter, trim, extend, notch, butt, overlap), gap, target member or face | Modifies one or two frame members |
| Weldment | Stage contents (preparation, welds, machining), member occurrences | Kind of assembly definition |
| Weld | Kind (fillet, groove, plug, slot, cosmetic), faces, size, length, intermittent pattern, symbol data, bead body | Owned by a weldment; feeds drawings and BOM |
| Cut list | Grouped identical members, lengths, end angles, quantities | Derived from frame members |

## Frame members

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FRAME-001 | Place members | Places frame members along selected sketch lines, model edges or 3D sketch segments with a chosen standard, family and size; one command can place many members. | T2 | 4 |
| SB-FRAME-002 | Profile library | Provides profile sections for ISO, EN, ASTM, DIN and JIS shapes (beams, channels, angles, tubes, flats, bars) through the standard parts library. | T2 | 4 |
| SB-FRAME-003 | Custom profiles | A user sketch of a cross section can be published as a profile section with designation, material and mass per length. | T2 | 4 |
| SB-FRAME-004 | Orientation | Each member's profile is positioned relative to its path by anchor point, offset, rotation angle and mirror. | T2 | 4 |
| SB-FRAME-005 | Change size | Changes the standard, family or size of selected members; end treatments and connected members are recomputed. | T2 | 4 |
| SB-FRAME-006 | Skeleton driven | Members follow their skeleton geometry; editing the skeleton updates member lengths, positions and end treatments. | T2 | 4 |
| SB-FRAME-007 | Reuse members | Members on parallel, equal paths can share one definition so the cut list counts them as one item. | T2 | 4 |
| SB-FRAME-008 | Member properties | Each member exposes cut length, end angles, mass and profile designation as properties. | T2 | 4 |

## End treatments

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FRAME-009 | Miter | Miters two members at their shared corner with a gap applied to either or both sides. | T2 | 4 |
| SB-FRAME-010 | Trim and extend | Trims or extends a member to another member or to a planar face, with an optional gap. | T2 | 4 |
| SB-FRAME-011 | Notch | Notches a member to the shape of the member it meets, with clearance. | T2 | 4 |
| SB-FRAME-012 | Butt and overlap corners | Treats corners as butt or overlap joints with a chosen member on top. | T2 | 4 |
| SB-FRAME-013 | Lengthen and shorten | Changes member length at either end by a value. | T2 | 4 |
| SB-FRAME-014 | End caps, base plates and gussets | Adds end caps, base plates and gussets sized from the member profile, from the library or from a sketch. | T3 | 4 |

## Weldments

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FRAME-015 | Weldment assemblies | Converts an assembly into a weldment with preparation, welds and machining stages evaluated in that order. | T2 | 4 |
| SB-FRAME-016 | Preparations | Adds bevels, chamfers and grooves to members before welding; preparations appear in the weldment and not in the member definitions. | T2 | 4 |
| SB-FRAME-017 | Fillet welds | Creates fillet welds between face sets with leg or throat size, length, and continuous or intermittent pitch; the bead is a body with volume. | T2 | 4 |
| SB-FRAME-018 | Groove welds | Creates V, bevel, U and J groove welds with root gap and angle between prepared faces. | T2 | 4 |
| SB-FRAME-019 | Plug and slot welds | Creates plug and slot welds through holes or slots in one member. | T3 | 4 |
| SB-FRAME-020 | Cosmetic welds | Records weld data and a path without solid bead geometry. | T2 | 4 |
| SB-FRAME-021 | Weld symbol data | Every weld stores symbol data per ISO 2553 or AWS A2.4 (type, size, length, pitch, contour, finish, all-around, field weld) used by drawings and 3D callouts. | T2 | 4 |
| SB-FRAME-022 | Machining stage | Adds cuts and holes after welding that act on the welded result. | T2 | 4 |
| SB-FRAME-023 | Weld groups and report | Groups welds and reports total length, bead volume and mass per group and type as a table or CSV. | T2 | 4 |
| SB-FRAME-024 | Weld calculation | Checks weld strength for a chosen load and standard and writes a report. | T3 | 5 |
| SB-FRAME-025 | Extract machined part | Creates a part from the welded result for machining drawings and manufacturing preparation, linked to the weldment. | T3 | 4 |

## Cut lists and outputs

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FRAME-026 | Cut list | Groups identical members and lists profile, length, end angles and quantity; it updates when members change. | T2 | 4 |
| SB-FRAME-027 | Weldment BOM | The bill of materials of a weldment includes weld bead mass and lists members with cut-length quantities. | T2 | 4 |
| SB-FRAME-028 | Analysis hand-off | Exports members as beam idealizations with sections, connections and supports to [simulation](26-simulation.md). | T3 | 5 |
