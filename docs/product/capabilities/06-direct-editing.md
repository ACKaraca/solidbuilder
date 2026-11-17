# Direct editing

Direct editing changes the faces of a body by selecting and moving them, without returning to the
sketches and features that created it. It works on bodies with no feature history, such as imported
solids, and inside a feature history, where every direct edit is recorded as a feature that regenerates
with the rest of the model.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Direct edit | Kind (push-pull, move, offset, replace, delete, resize), selected faces, transform or value, live rules applied | Feature in the Feature history; faces held as Persistent references |
| Live rule | Kind (tangent, concentric, coplanar, coaxial, symmetric, parallel, perpendicular, equal radius), tolerance, enabled state | Evaluated during a Direct edit to extend the selection with related faces |
| Recognized feature | Kind (hole, fillet, chamfer, extrusion, revolution, pattern), parameters, source faces | Created by feature recognition; replaces faces of a history-free Body with an editable Feature |
| History-free component | Capture setting (history recorded or not) | Property of a Part or Body; edits apply directly to the Body and are not replayed |
| Simplification set | Rules (feature size limits, hole fill, internal part removal), result Body | Produces a simplified copy used for sharing; linked to its source Body |

## Face edits

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DIRECT-001 | Push-pull face edit | Moves selected planar faces along their normal by a distance or to a target, extending or trimming adjacent faces so the body stays closed; selecting a fillet face changes its radius and selecting a cylindrical face changes its diameter. Inside a feature history, a face created by a feature edits that feature's driving value when it is unambiguous. | T1 | 3 |
| SB-DIRECT-002 | Move and rotate faces | Translates and rotates selected faces or edges with an on-screen gizmo, numeric values, point-to-point or along an axis, healing adjacent faces. Fails with the list of faces that cannot be extended when the move would open the body. | T1 | 3 |
| SB-DIRECT-003 | Offset face | Offsets selected faces by a distance along their normals, keeping tangent neighbors connected. | T1 | 3 |
| SB-DIRECT-004 | Replace face | Replaces one or more faces with a target face or surface, extending adjacent faces to it. | T1 | 3 |
| SB-DIRECT-005 | Delete face with healing | Removes selected faces and heals the gap by extending neighbors, or leaves an opening and converts the body to a sheet when the user chooses. Healing failures name the faces that could not be extended. | T1 | 3 |
| SB-DIRECT-006 | Resize holes, cylinders and fillets | Sets a new diameter, radius or depth on cylindrical, conical, toroidal and fillet faces of any solid, including imported ones, with all selected faces of equal size changed together on request. | T1 | 3 |
| SB-DIRECT-007 | Scale body | Scales bodies uniformly or by separate factors per axis about a point, recorded as a feature. | T1 | 3 |
| SB-DIRECT-008 | Copy and paste faces | Copies a connected face set to a new location, attaching it to a target body when it closes against it, or creates a sheet body. | T2 | 4 |
| SB-DIRECT-009 | Edge and vertex move | Moves an edge or vertex of a body, recomputing the adjacent faces when they remain planar or ruled; non-reconstructible results are refused with a reason. | T3 | 4 |

## Live rules

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DIRECT-010 | Live rule detection | During a face edit, finds faces related to the selection by tangency, concentricity, coaxiality, coplanarity, symmetry, parallelism and perpendicularity within tolerance and moves them together. The faces added by each rule are highlighted before the edit is applied. | T2 | 4 |
| SB-DIRECT-011 | Live rule control | Lets the user switch each rule kind on or off per edit and exclude individual faces from rule propagation; the chosen rule set is stored with the direct edit feature and replayed on regeneration. | T2 | 4 |
| SB-DIRECT-012 | Driving dimensions on faces | Places a dimension between faces of a history-free body and edits it to move the faces, with live rules applied. The dimension is kept as a parameter that can be referenced elsewhere. | T2 | 4 |

## History integration

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DIRECT-013 | History capture per component | Sets whether a Part or Body records a feature history; turning capture off collapses the history into a base body after confirmation, and turning it on starts a new history from the current body. | T1 | 3 |
| SB-DIRECT-014 | Direct edits recorded in history | Inside a history, records each direct edit as a feature placed at the rollback marker. On regeneration the edit reapplies to the faces its persistent references resolve to, and reports a repairable failure when a face no longer exists. | T1 | 3 |
| SB-DIRECT-015 | Base body from import | Places imported geometry as a base body feature at the start of a history, so later sketches, features and direct edits reference it and regenerate when the import is reloaded. | T1 | 3 |
| SB-DIRECT-016 | Direct edit diagnostics | Lists direct edit features whose target faces changed after an upstream edit, showing before and after face matches so the user can accept, reselect or remove the edit. | T2 | 3 |

## Feature recognition and simplification

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DIRECT-017 | Hole and fillet recognition | Detects holes (simple, counterbore, countersink, tapped by size) and constant-radius fillets and chamfers on a history-free body and replaces those faces with editable features. Each recognized feature can be accepted or skipped individually. | T2 | 4 |
| SB-DIRECT-018 | Extrusion and revolution recognition | Detects regions of a body that can be expressed as extrusions or revolutions of a sketch profile and rebuilds them as sketches and features with the original faces' persistent names mapped to the new ones. | T3 | 5 |
| SB-DIRECT-019 | Pattern recognition | Detects repeated features in linear, circular or mirrored arrangement and converts them into one feature with a pattern, so editing the seed updates all instances. | T3 | 4 |
| SB-DIRECT-020 | History reconstruction | Proposes a full ordered feature history for a history-free body, with a preview of each step and the deviation from the original geometry; the user applies it as a whole or up to a chosen step. | T3 | 5 |
| SB-DIRECT-021 | Remove small features | Removes fillets, chamfers, holes and bosses below user size limits from a body or a copy, reporting the features removed and any that could not be removed. | T2 | 4 |
| SB-DIRECT-022 | Simplify for sharing | Creates a simplified copy of a part or assembly by removing internal components and features below a size, filling holes and optionally merging to one body, linked to the source so it updates on request. | T2 | 4 |

## Context editing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DIRECT-023 | Direct edit in assembly context | Applies face edits to a part while it is shown in its assembly, allowing selection of target faces on other occurrences; edits are stored in the part, and cross-part targets are stored as external references. | T2 | 3 |
| SB-DIRECT-024 | Multi-body direct edits | Applies one face edit across faces belonging to several bodies of the same part, keeping each body closed. | T2 | 4 |
