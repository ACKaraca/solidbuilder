# Assemblies

An assembly places component definitions as occurrences and relates them through joints and assembly
constraints that one solver evaluates, so a mechanism moves the way it will be built. Top-down
techniques let layout components drive part geometry, and large-assembly techniques keep models with
tens of thousands of occurrences responsive.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Occurrence | Stable ID, component definition, configuration, transform, grounded, suppressed, visible, flexible, adaptive flags, load state | Child of an assembly definition; target of joints and constraints |
| Joint | Kind (rigid, revolute, slider, cylindrical, pin-slot, planar, ball), two connection frames, offsets, limits, rest value, current value, suppressed | Relates two occurrences; solved by the assembly solver |
| Connection frame | Origin and axes defined on geometry through persistent references, offset, flip | Owned by a component definition or an assembly; referenced by joints |
| Connection definition | Named set of connection frames and joint kinds stored on a component, matching key | Used for automatic placement and by rules |
| Assembly constraint | Kind (mate, flush, angle, tangent, insert, symmetry, center), references, offset expression | Relates two occurrences; solved with joints in one system |
| Rigid group | Member occurrences | Removes relative motion between members |
| Motion coupling | Kind (rotation ratio, rotation-translation, cam follower), two joints, ratio expression, direction | Couples joint values |
| Contact set | Member occurrences, enabled flag | Stops motion on contact during drives |
| Motion study | Driven joints, ranges, steps, recorded frames | Produces position states and animation input |
| Interference result | Occurrence pair, volume, centroid, optional interference body | Output of an interference check; not stored unless saved |
| Simplified representation | Method (envelope, wrapped), source occurrences, removal options, generated body | Substitutes for a subassembly in lightweight contexts |

## Placement and structure

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ASM-001 | Place component | Places a component definition from the current document, another document, the standard parts library or a configured family member as an occurrence at the cursor or at the origin. The first occurrence of a new assembly is grounded at the origin. | T1 | 3 |
| SB-ASM-002 | Place multiple | Placing continues until the user cancels, creating one occurrence per click with the same definition and configuration. | T1 | 3 |
| SB-ASM-003 | Create component in place | Creates a new empty component definition inside the active assembly and activates it for editing; sketches may reference geometry of other occurrences, recorded as cross-component references. | T1 | 3 |
| SB-ASM-004 | Ground and unground | A grounded occurrence is fixed in the assembly frame; several occurrences may be grounded. Ungrounding releases it to joints and constraints. | T1 | 3 |
| SB-ASM-005 | Restructure | Occurrences can be moved between subassemblies by dragging in the model tree; joints whose references remain valid are kept, others are reported for repair. | T1 | 3 |
| SB-ASM-006 | Suppress and visibility | Suppressed occurrences are excluded from solving, mass properties and the bill of materials; hidden occurrences are only excluded from display. Both states can be stored in display states and configurations. | T1 | 3 |
| SB-ASM-007 | Isolate and search | Isolates a selection in the viewport and filters the model tree by name, property, definition or state; the filtered set can be saved as a selection set. | T1 | 3 |
| SB-ASM-008 | Assembly coordinate systems | Datum planes, axes, points and coordinate systems can be created at assembly level between occurrences and used as joint and constraint references. | T1 | 3 |
| SB-ASM-009 | Origin preview while relating | While a joint or constraint command is active, hovering an occurrence with a modifier key shows its origin planes, axes and point for selection. | T2 | 3 |

## Joints and constraints

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ASM-010 | Joints | Creates a rigid, revolute, slider, cylindrical, pin-slot, planar or ball joint between connection frames on two occurrences and moves the second occurrence into place with an animated preview. Offsets, angle and flip are editable afterwards. | T1 | 3 |
| SB-ASM-011 | Connection frames from geometry | Connection frames snap to vertices, edge midpoints, circle centers, face centroids or the point between two faces; they are stored through persistent references and follow geometry edits. | T1 | 3 |
| SB-ASM-012 | As-built joints | Creates a joint between occurrences at their current positions without moving either, using a selected point or axis as the frame. | T1 | 3 |
| SB-ASM-013 | Joint limits and rest | Each joint degree of freedom has optional minimum, maximum and rest values; dragging and drives stop at limits, and the rest value is restored by a reset command. | T1 | 3 |
| SB-ASM-014 | Assembly constraints | Mate, flush, angle (directed, undirected, with reference), tangent (inside, outside), insert, symmetry and center constraints relate faces, edges, axes and points with expression-driven offsets. | T1 | 3 |
| SB-ASM-015 | One solver for joints and constraints | Joints and constraints in one assembly are solved together as one geometric system; any mix is allowed and the result does not depend on creation order. | T1 | 3 |
| SB-ASM-016 | Rigid groups | Selected occurrences are locked relative to each other without individual joints; dissolving the group restores their individual freedom. | T1 | 3 |
| SB-ASM-017 | Motion couplings | Couples two joint values with a ratio (gears), a rotation-to-translation lead (rack, screw) or a cam profile follower; couplings work inside nested subassemblies. | T2 | 3 |
| SB-ASM-018 | Degrees of freedom display | Shows the remaining translational and rotational freedom of each occurrence as glyphs and lists fully constrained, underconstrained and overconstrained occurrences. | T1 | 3 |
| SB-ASM-019 | Free drag within freedom | Dragging an occurrence moves it within its remaining freedom while the solver keeps every joint and constraint satisfied; dragging past a limit is stopped. | T1 | 3 |
| SB-ASM-020 | Conflict diagnostics | When the system cannot be solved, the solver reports the smallest set of conflicting joints and constraints, highlights them and offers suppress, edit or delete for each. | T1 | 3 |
| SB-ASM-021 | Broken reference repair | A joint or constraint whose geometry reference no longer resolves is marked, keeps its last position and can be remapped by picking new geometry in one step. | T1 | 3 |
| SB-ASM-022 | Connection definitions | A component can store named connection frames with joint kinds and matching keys; placing it near a component with a matching definition proposes the joint automatically. | T2 | 4 |
| SB-ASM-023 | Constraint lock and suppression | Individual joints and constraints can be suppressed or locked at their current value without deleting them. | T2 | 3 |

## Motion and analysis

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ASM-024 | Drive joint | Animates a joint value through a range with a step count, once, looped or back and forth; coupled joints follow. The run can be recorded to a video file or to position states. | T1 | 3 |
| SB-ASM-025 | Contact sets | Occurrences in an enabled contact set are treated as solid during drags and drives, so motion stops at first contact. | T2 | 3 |
| SB-ASM-026 | Collision check during motion | While driving or dragging, interference between selected occurrences is detected and the frame where it first occurs is reported. | T2 | 3 |
| SB-ASM-027 | Interference analysis | Checks one set of occurrences against another or all against all and lists each interfering pair with volume and centroid; an interference body can be created as a separate body for inspection. | T1 | 3 |
| SB-ASM-028 | Clearance check | Reports the minimum distance between selected occurrences or sets and flags pairs below a given clearance. | T2 | 3 |
| SB-ASM-029 | Fastener stack check | For each bolt in a selection, reports the clamped length, thread engagement and protrusion against the fastener's specification and flags mismatches. | T2 | 4 |
| SB-ASM-030 | Motion studies | Saves a set of driven joints with ranges and steps as a named motion study that can be replayed, used by animation sequences and handed to simulation. | T2 | 4 |

## Patterns, mirror and replace

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ASM-031 | Component patterns | Patterns occurrences in rectangular, circular or along-path layouts with counts and spacing driven by parameters; suppressing an instance keeps the pattern. | T1 | 3 |
| SB-ASM-032 | Feature-driven pattern | Patterns occurrences to follow a hole or feature pattern in another component; when the source pattern count changes, the occurrence pattern updates. | T1 | 3 |
| SB-ASM-033 | Sketch-driven pattern | Places occurrences at the points of a sketch; adding or removing points adds or removes occurrences. | T2 | 3 |
| SB-ASM-034 | Mirror components | Mirrors occurrences about a plane, either reusing symmetric definitions or creating mirrored definitions; selected occurrences can be excluded, and associative placement keeps mirrored occurrences positioned when the source moves. | T2 | 3 |
| SB-ASM-035 | Copy components | Copies occurrences with their internal joints, either reusing definitions or creating renamed copies. | T2 | 3 |
| SB-ASM-036 | Replace component | Replaces one or all occurrences of a definition with another definition or configuration; joints and constraints are re-attached through matching persistent references and names, and unmatched ones are listed for repair. | T1 | 3 |

## Assembly features and top-down design

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ASM-037 | Assembly features | Cut, hole, fillet, chamfer and sweep cut features defined at assembly level act on a chosen list of participating occurrences without changing their definitions. | T2 | 3 |
| SB-ASM-038 | Layout components | A layout component holds master sketches, datums and parameters; parts reference its geometry so editing the layout updates every dependent part. | T1 | 3 |
| SB-ASM-039 | Cross-component references | A sketch or feature may reference geometry of another occurrence; the reference is listed with its source and can be frozen, re-pointed or broken. | T1 | 3 |
| SB-ASM-040 | Adaptive geometry | A feature marked adaptive resizes to satisfy assembly relationships, such as a length matching the distance between two occurrences; adaptivity is enabled per occurrence and can be locked. | T2 | 4 |
| SB-ASM-041 | Flexible subassemblies | A subassembly occurrence marked flexible lets its internal joints move independently in the parent, so two occurrences of one cylinder subassembly can sit at different strokes. | T2 | 3 |
| SB-ASM-042 | Freeze references | Converts all cross-component references and adaptive features of a component into fixed values in one command, with a report of what was frozen. | T2 | 3 |
| SB-ASM-043 | Edit in context | Activates an occurrence's definition for editing inside the assembly with other occurrences dimmed and selectable as references; the edit is recorded in the definition's own feature history. | T1 | 3 |

## Large assemblies

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ASM-044 | Lightweight loading | Opens an assembly with display tessellation and structure only; exact geometry of an occurrence is loaded on demand when it is selected for measurement, editing or referencing. | T1 | 3 |
| SB-ASM-045 | Partial loading | Opens only chosen subtrees or a region defined by a box or section; unloaded occurrences keep their place in the structure and bill of materials. | T2 | 3 |
| SB-ASM-046 | Simplified representations | Generates an envelope or wrapped body for a subassembly, with options to remove internal parts, voids, small features and parts below a size; the substitute updates on request when the source changes. | T2 | 3 |
| SB-ASM-047 | Use substitutes | An occurrence can display, solve and export through a simplified representation instead of full geometry, chosen per occurrence or per display state. | T2 | 3 |
| SB-ASM-048 | Size ranking | Lists occurrences ranked by triangle count, exact face count, memory and load time so heavy components can be simplified first. | T2 | 3 |
| SB-ASM-049 | Deferred update | Suspends solving and regeneration of the assembly while edits are made; a pending indicator shows until the user updates. | T1 | 3 |
| SB-ASM-050 | Out-of-date indicators | Occurrences whose definitions changed in other documents are marked out of date until updated, and the update is applied according to the reference policy in data management. | T1 | 3 |
| SB-ASM-051 | Parallel assembly solve | Independent subassemblies and rigid clusters are solved on worker threads; a stale result from an earlier revision is never applied. | T2 | 3 |
