# Solid features and feature history

Solid features turn sketches and reference geometry into bodies, and the feature history records them
in order so any earlier decision can be edited and the model regenerated. Features attach to model
topology through persistent references that are designed to survive upstream edits, and every failure
is reported with a concrete repair path. Branching, merging and comparing history versions are covered
in [Data management and collaboration](18-data-management.md).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Feature history | Ordered list of features per component definition, rollback marker position, history enabled flag | Owned by one part or assembly definition; its evaluation produces the component's bodies |
| Feature | Stable ID, kind, inputs (persistent references, parameters), operation mode (new body, join, cut, intersect, new component), participating bodies, suppressed flag, health state (ok, warning, error), comment | One entry in a feature history; depends on earlier features and reference geometry; produces or modifies bodies |
| Rollback marker | Position in the feature history | Features after the marker are not evaluated; new features are inserted at the marker |
| Feature group | Name, contiguous feature range, collapsed flag | Groups features for display, suppression and reuse |
| Persistent reference | Feature-derived name of a face, edge or vertex, geometric signature, resolution result | Held by features, sketches, joints, annotations and drawings; re-resolved after every regeneration |
| Base feature | Imported or history-free body snapshot, source import record | Starts a feature history from geometry that has no recorded history |
| Hole specification | Type, standard, size, fit or thread designation, depth mode, tip angle | Owned by a hole feature; reported to drawings and manufacturing |
| Thread specification | Standard, designation, class, handedness, length, modeled or cosmetic | Owned by a thread or hole feature; read by drawings, annotations and manufacturing |
| Pattern | Seed features or bodies, layout (rectangular, circular, path, point set, table), instance list with suppression | Produces instances that track the seed |

## Feature history

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FEAT-001 | Ordered history | Every sketch, reference geometry item and feature is recorded in creation order in the component's feature history, shown in the model tree and in a compact history bar. Editing any item regenerates that item and every dependent item after it. | T1 | 3 |
| SB-FEAT-002 | Edit feature | Double-clicking a feature reopens its command panel with the original inputs; applying the edit regenerates dependents in one transaction that undoes as a single step. | T1 | 3 |
| SB-FEAT-003 | Rollback marker | Dragging the rollback marker to any position evaluates the model up to that point only. New features are inserted at the marker, and moving the marker to the end restores the full model. | T1 | 3 |
| SB-FEAT-004 | Reorder | Features can be dragged to another position when every input is still defined before the new position. A move that would break a dependency is refused and the blocking dependency is named. | T1 | 3 |
| SB-FEAT-005 | Suppress and unsuppress | A suppressed feature is skipped during evaluation; dependents that need it are reported as errors until it is restored. Suppression state can be stored per configuration. | T1 | 3 |
| SB-FEAT-006 | Delete with dependents | Deleting a feature lists its dependents and offers to delete them too, keep them with broken references for later repair, or cancel. | T1 | 3 |
| SB-FEAT-007 | Feature groups | Contiguous features can be grouped, named, collapsed, suppressed together and saved as a feature template. | T1 | 3 |
| SB-FEAT-008 | Feature comments | Each feature, group and sketch carries an optional design-intent note shown on hover and in the model tree. Notes are searchable and exported with the feature history text form. | T2 | 3 |
| SB-FEAT-009 | History per component | Each component definition has history enabled or disabled. Disabling history converts the current state into a base feature after confirmation; enabling it on a history-free component starts a history from a base feature of the current bodies. | T1 | 3 |
| SB-FEAT-010 | Base features from imports | Imported geometry enters the history as a base feature that keeps its import record. Later features can be added on top, and replacing the import with a newer file re-resolves downstream references. | T1 | 3 |
| SB-FEAT-011 | Regenerate on demand | The user can force regeneration of the active component or of every component in the document. Deferred regeneration mode queues edits and regenerates only when requested, showing a pending indicator. | T1 | 3 |
| SB-FEAT-012 | Parallel rebuild with progress | Regeneration runs independent branches of the dependency graph on worker threads. A progress indicator shows the current feature, the viewport stays responsive, and the user can cancel, which keeps the last valid model. | T1 | 3 |
| SB-FEAT-013 | Evaluation timing | A history view lists the evaluation time of each feature from the last regeneration and highlights the slowest items. | T2 | 3 |
| SB-FEAT-014 | Dependency viewer | For a selected feature, parameter or sketch, a graph view shows what it depends on and what depends on it, across sketches, features, parameters and other components. Selecting a node selects the item in the model tree. | T2 | 3 |
| SB-FEAT-015 | Textual history form | The feature history of a component can be shown and exported as a canonical text form that lists features, inputs and parameters in order. The same form is accepted by the automation interface to rebuild the component. | T2 | 3 |

## Health, failure and repair

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FEAT-016 | Health states | Each feature reports ok, warning (result produced with a recoverable issue) or error (no result). The state is shown by color in the model tree and history bar, with the reason on hover. | T1 | 3 |
| SB-FEAT-017 | Health list | A panel lists every warning and error in the document with feature, component, message and suggested repairs. Selecting an entry highlights the feature and its missing or ambiguous inputs. | T1 | 3 |
| SB-FEAT-018 | Continue past failures | A failed feature does not stop regeneration; later independent features still evaluate, and dependents of the failed feature report a dependency error instead of failing silently. | T1 | 3 |
| SB-FEAT-019 | Reference repair | When a persistent reference cannot be resolved, the repair panel shows the last known geometry as a ghost and lets the user pick the replacement entity. One pick can remap every reference to the same lost entity across the component. | T1 | 3 |
| SB-FEAT-020 | Ambiguous reference report | When a lost entity split into several candidates or matched only by geometric similarity, the feature evaluates with the best candidate, reports a warning naming the chosen match, and lets the user confirm or choose another. | T1 | 3 |
| SB-FEAT-021 | Persistent references across edits | References to faces, edges and vertices keep pointing at the same design entity when upstream sketches gain or lose segments, features are reordered, patterns change count or fillets are added. The regression corpus defines the edits that must keep references stable. | T1 | 3 |
| SB-FEAT-022 | Reference audit | A command lists the persistent references held by a feature or sketch, the entity each resolves to and how it was matched, so fragile references (for example to model faces where a datum would be stable) can be replaced. | T2 | 3 |
| SB-FEAT-023 | Roll to failure | A repair action moves the rollback marker to just before the first failed feature so it can be fixed in the model state it was created in. | T1 | 3 |

## Sketched features

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FEAT-024 | Extrude | Extrudes profiles or planar faces by distance, symmetric, two-sided, to next, through all, to a face or body, or between two faces, with optional taper per side and start offset. Produces a solid or, optionally, a sheet body; the operation mode is new body, join, cut, intersect or new component. | T1 | 3 |
| SB-FEAT-025 | Thin extrude | Extrudes an open or closed profile as a wall of given thickness placed inside, outside or centered, with the same extent options as a full extrude. | T1 | 3 |
| SB-FEAT-026 | Revolve | Revolves profiles about a sketch line, edge, datum axis or origin axis by angle, full turn, symmetric, two-sided, to next or to a face, with the standard operation modes. | T1 | 3 |
| SB-FEAT-027 | Sweep | Sweeps a profile along a path, with optional guide rail or guide surface, orientation fixed or following the path, taper and twist angles, and partial sweep by distance along the path. | T1 | 3 |
| SB-FEAT-028 | Loft | Creates a body through two or more profiles or points, with optional rails or a centerline, tangent or curvature end conditions, closed loop, and explicit section alignment to control twisting. | T1 | 3 |
| SB-FEAT-029 | Rib | Creates a rib from an open sketch curve with thickness, direction parallel or normal to the sketch, and extent to the next face or a fixed depth; the rib extends automatically to meet surrounding faces. | T1 | 3 |
| SB-FEAT-030 | Web | Creates a set of ribs from several open curves in one feature, joining them where they intersect, with a single thickness and extent. | T2 | 3 |
| SB-FEAT-031 | Emboss and engrave | Raises or recesses a profile or text on a planar or curved face by a depth, either projected or wrapped onto the face so the shape keeps its developed size. | T2 | 3 |
| SB-FEAT-032 | Coil | Creates a helical or spiral body by pitch, revolutions, height and taper, with section shape (circle, square, triangle or a profile), flat or natural ends, and cut or join operation. | T2 | 3 |
| SB-FEAT-033 | Primitives | Creates box, cylinder, sphere, torus and pipe bodies from typed dimensions and a placement, without a sketch; each stays fully editable as a feature. | T1 | 3 |
| SB-FEAT-034 | Pipe along path | Creates a hollow or solid round section swept along a path by outer diameter and wall thickness. | T2 | 3 |
| SB-FEAT-035 | Slot feature | Cuts a straight or arc slot sized by width and length, by centerlines or by fastener clearance for a chosen fastener size, with through, depth or to-face extent. Slot data is available to drawing annotations. | T2 | 3 |
| SB-FEAT-036 | Cut options | Cut operations can follow a face (cut normal to a curved wall) and can extend the profile to fully clear the material it crosses. | T2 | 3 |

## Holes and threads

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FEAT-037 | Hole feature | Creates simple, counterbore, spotface and countersink holes as drilled, clearance, tapped or taper-tapped. Placement is from sketch points, a point on a face with two edge offsets, concentric to an edge, or several preselected points in one feature. | T1 | 3 |
| SB-FEAT-038 | Hole extents and tip | Hole depth is by distance, through all, to next or to a face, with a configurable drill tip angle or flat bottom. | T1 | 3 |
| SB-FEAT-039 | Standard hole data | Clearance and tapped sizes come from standards tables (metric and inch) with fit classes such as close, normal and loose. Tables are documented data files that can be extended. | T1 | 3 |
| SB-FEAT-040 | Hole presets | A combination of hole type, standard, size and depth can be saved as a named preset and reused. | T2 | 3 |
| SB-FEAT-041 | Thread feature | Adds a thread to a cylindrical or conical face by standard, designation, class and handedness, over full length or a set length with offset. The thread is cosmetic (data and display only) or modeled as geometry. | T1 | 3 |
| SB-FEAT-042 | Thread library | Thread standards are stored in a library of documented tables, shareable across a team, with user-added standards. Changing a library entry is reported for every thread that uses it. | T2 | 4 |
| SB-FEAT-043 | Hole and thread data publication | Hole and thread specifications are exposed to drawings for hole notes and hole tables, to 3D annotations and to manufacturing preparation. | T1 | 3 |

## Placed modifying features

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FEAT-044 | Constant fillet | Rounds selected edges with a constant radius, with tangent chain propagation and multiple edge sets of different radii in one feature. | T1 | 3 |
| SB-FEAT-045 | Variable and chord fillet | Rounds edges with radii defined at points along the edge, or by a constant chord width. | T2 | 3 |
| SB-FEAT-046 | Fillet continuity and corners | Fillets can be circular or curvature continuous (G2) and can be asymmetric with two distances. Corner setbacks control the blend where three or more filleted edges meet. | T2 | 3 |
| SB-FEAT-047 | Face and full-round fillets | Blends between two face sets regardless of shared edges, and creates a full round that replaces a middle face tangent to two side faces. | T2 | 3 |
| SB-FEAT-048 | Rule-based fillet | Selects edges by rule (all edges of a feature, edges between two features, all concave or convex edges, edges against a face) and re-evaluates the rule on regeneration so new edges are included. | T2 | 3 |
| SB-FEAT-049 | Chamfer | Bevels edges by equal distance, two distances or distance and angle, with tangent chain propagation and corner treatment options. | T1 | 3 |
| SB-FEAT-050 | Shell | Hollows a body to a wall thickness inside, outside or both, removing selected faces, with per-face thickness overrides. | T1 | 3 |
| SB-FEAT-051 | Draft | Tapers faces by an angle relative to a pull direction from a fixed plane, fixed edges or a parting line, in one or two directions, with tangent face propagation. | T1 | 3 |
| SB-FEAT-052 | Scale | Scales bodies, sketches or components uniformly or non-uniformly about a point. | T2 | 3 |
| SB-FEAT-053 | Thicken | Converts sheet bodies to solids by thickness on one side, both sides or symmetric, with optional rounded edges at open boundaries. | T1 | 3 |
| SB-FEAT-054 | Bend part | Bends a portion of a body around a line on a sketch by radius, angle or arc length, for conceptual bending of non-sheet-metal parts. | T3 | 4 |

## Patterns and mirror

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FEAT-055 | Rectangular pattern | Repeats features, faces or bodies in one or two directions by count and spacing or count and extent, with symmetric option. A distribution table allows irregular spacing per instance. | T1 | 3 |
| SB-FEAT-056 | Circular pattern | Repeats items about an axis by count and angle, full or partial, with symmetric option and irregular angular spacing from a table. | T1 | 3 |
| SB-FEAT-057 | Pattern along path | Repeats items along a curve by count and spacing or count and length, with orientation following the path or kept fixed. | T2 | 3 |
| SB-FEAT-058 | Point-driven pattern | Places instances at the points of a sketch, updating when points are added or removed. | T1 | 3 |
| SB-FEAT-059 | Pattern compute modes | A pattern evaluates either by repeating the seed's feature operation per instance (adapts to local geometry) or by copying the seed's resulting geometry (faster, identical instances). | T2 | 3 |
| SB-FEAT-060 | Instance control | Individual instances can be suppressed or skipped, and the suppression set is stored by instance index and can be configured. | T1 | 3 |
| SB-FEAT-061 | Parameter-driven pattern count | Pattern counts and spacing are parameters, so expressions such as a count derived from length and pitch keep the pattern consistent when the part changes size. | T1 | 3 |
| SB-FEAT-062 | Mirror | Mirrors features, faces or bodies about a plane or planar face, either as a new body or joined to the source. | T1 | 3 |

## Multi-body operations

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FEAT-063 | Combine | Joins, subtracts or intersects selected tool bodies with a target body, with an option to keep tool bodies and to place the result in a new component. | T1 | 3 |
| SB-FEAT-064 | Split body | Splits a body by a plane, face, sheet body or sketch curve into separate bodies, keeping all pieces or selected ones. | T1 | 3 |
| SB-FEAT-065 | Split face | Divides faces by curves, planes or sheet bodies without changing the solid, so the new faces can be used for drafts, appearances or loads. Silhouette split divides faces along the silhouette seen from a direction. | T2 | 3 |
| SB-FEAT-066 | Move and copy bodies | Moves or copies bodies by free drag, along an axis, by rotation, point to point or to a coordinate system, recorded as a feature with editable values. | T1 | 3 |
| SB-FEAT-067 | Align bodies | Moves a body so a selected face, edge or point aligns with a target, recorded as a feature. | T2 | 3 |
| SB-FEAT-068 | Operation mode per feature | Every body-producing feature selects new body, join, cut, intersect or new component, and the participating bodies can be edited after creation without recreating the feature. | T1 | 3 |
| SB-FEAT-069 | Boolean diagnostics | When a join, cut or intersect fails, the feature reports the cause (no intersection, coincident faces, tolerance gap, invalid tool) and highlights the offending regions. | T1 | 3 |
