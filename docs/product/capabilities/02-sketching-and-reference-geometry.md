# Sketching and reference geometry

Sketches capture design intent as 2D and 3D geometry held by geometric and dimensional constraints,
and reference geometry supplies the stable planes, axes, points and coordinate systems that sketches
and features attach to. Both are solved by the shared constraint and expression engine, so a value
typed in a sketch dimension, a parameter table or a script behaves the same way.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Sketch | Stable ID, placement (plane or face persistent reference, or 3D), local coordinate system, solve state (under, fully, over constrained, failed), visibility | Owned by a part or assembly definition; a feature in the feature history; consumed by features through profiles and curves |
| Sketch entity | Stable ID, kind (point, line, arc, circle, ellipse, conic, spline, text, image), construction flag, centerline flag, fixed flag, projected source | Belongs to one sketch; referenced by constraints, dimensions and profiles |
| Sketch constraint | Kind, ordered entity references, driving or suppressed state | Removes degrees of freedom from entities in one sketch |
| Dimension | Kind (linear, aligned, angular, radial, diameter, arc length, ordinate), expression, driving or driven, display mode | Binds an entity measurement to a parameter; driven dimensions publish reference parameters |
| Sketch profile | Closed loop set, region, inner loops | Derived from a sketch on every solve; selected by features through persistent references |
| Projected entity | Source persistent reference, projection mode, associative flag | Copies model or other-sketch geometry into a sketch and updates when the source changes |
| Sketch block | Definition (entities, constraints), instances with transform, nesting | Reusable constrained group inside a sketch; definitions can be saved to a library |
| Reference image | Image asset, plane, calibration points and distance, opacity | Placed on a plane or face for tracing; never part of profiles |
| Reference geometry | Kind (datum plane, datum axis, datum point, coordinate system), construction method, inputs as persistent references | Feature in the feature history; used as placement and direction for sketches, features, joints and manufacturing setups |

## Sketch placement and environment

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SKETCH-001 | Sketch on plane or face | A sketch is placed on an origin plane, a datum plane or a planar face, with its local X axis taken from a chosen edge or axis. When the supporting face moves or rotates after an upstream edit, the sketch follows it through the persistent reference. | T1 | 3 |
| SB-SKETCH-002 | Redefine sketch placement | The user changes a sketch's supporting plane or orientation after creation. Entities keep their local coordinates, and features consuming the sketch regenerate in the new position. | T1 | 3 |
| SB-SKETCH-003 | Look at and slice | Entering a sketch can orient the camera normal to the sketch plane and temporarily clip the model on the sketch plane so hidden geometry becomes visible. Both are view settings and do not change the model. | T1 | 3 |
| SB-SKETCH-004 | Sketch grid and snaps | An optional grid with configurable spacing and snapping, plus object snaps to endpoints, midpoints, centers, quadrants, intersections and tangents. Snaps create matching constraints when constraint inference is on. | T1 | 3 |
| SB-SKETCH-005 | Sketch display controls | Toggles show or hide profiles, points, dimensions, constraint glyphs and projected geometry. Dimension display can show the value, the parameter name or the full expression. | T1 | 3 |
| SB-SKETCH-006 | Shared sketch | A sketch already consumed by one feature can be consumed by further features without copying. Each consumer selects its own profiles and curves through persistent references. | T1 | 3 |

## Sketch creation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SKETCH-007 | Lines and polylines | Creates line segments and chained polylines by points or typed length and angle, with tangent or perpendicular arc continuation inside the same command. | T1 | 3 |
| SB-SKETCH-008 | Rectangles and polygons | Creates rectangles by two corners, three points or center and corner, and regular polygons inscribed, circumscribed or by edge, each with the constraints that keep the shape valid when edited. | T1 | 3 |
| SB-SKETCH-009 | Circles and arcs | Creates circles by center and diameter, two points, three points, tangent to two or three entities, and arcs by three points, center and endpoints, or tangent to an entity end. | T1 | 3 |
| SB-SKETCH-010 | Ellipses and conics | Creates full and partial ellipses by center and axes, and conic curves by endpoints, apex and a rho value that is itself dimensionable. | T1 | 3 |
| SB-SKETCH-011 | Slots | Creates straight slots by center-to-center, overall length or center point, and arc slots by three points or center point, with width as a dimension. | T1 | 3 |
| SB-SKETCH-012 | Splines | Creates fit-point splines and control-point splines with adjustable degree. Fit points expose tangent and curvature handles that can be dimensioned and constrained. | T1 | 3 |
| SB-SKETCH-013 | Points | Creates sketch points, used as hole centers, pattern seeds and constraint anchors. | T1 | 3 |
| SB-SKETCH-014 | Sketch text | Creates single-line and multi-line text with font, height, alignment and rotation, along a path or in a box. Text content can be a text parameter or a property field, and the outlines are usable as profiles for embossing and engraving. | T1 | 3 |
| SB-SKETCH-015 | Equation curves | Creates curves from explicit, parametric or polar equations over a parameter range, in Cartesian or cylindrical form. Equations may use parameters, and the curve regenerates when they change. | T2 | 3 |
| SB-SKETCH-016 | Construction and centerline entities | Any entity can be toggled to construction or centerline. Construction entities never form profiles; centerlines produce diameter dimensions for revolved profiles. | T1 | 3 |
| SB-SKETCH-017 | Isoparametric and face curves | Creates curves on a selected face at a constant U or V parameter, or at a picked point, as projected entities that update with the face. | T2 | 4 |

## Sketch editing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SKETCH-018 | Trim, extend and break | Trims an entity to the nearest intersections, extends it to the next boundary, or breaks it at a point. Constraints on surviving pieces are preserved where they still apply and others are removed with a notice. | T1 | 3 |
| SB-SKETCH-019 | Sketch fillet and chamfer | Rounds a corner with a dimensioned radius or chamfers it by equal distances, two distances or distance and angle, keeping tangency and coincidence constraints. | T1 | 3 |
| SB-SKETCH-020 | Offset | Offsets a chain or loop by a distance with sharp or rounded corners. The result keeps an offset relation so it updates when the source changes or the offset distance is edited. | T1 | 3 |
| SB-SKETCH-021 | Mirror, patterns and transforms | Mirrors entities about a line with symmetry constraints, creates rectangular and circular sketch patterns with driven counts and spacing, and moves, copies, rotates, scales or stretches selected entities. | T1 | 3 |
| SB-SKETCH-022 | Spline editing | Adds or removes fit points, switches control frame display, simplifies a spline to fewer points within a tolerance, and fits a spline through selected curves. | T2 | 3 |
| SB-SKETCH-023 | Direct manipulation | Dragging an under-constrained entity moves it while all existing constraints hold. Fully constrained entities do not move, and dragging a dimension value handle changes the value. | T1 | 3 |

## Dimensions and constraints

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SKETCH-024 | Geometric constraints | Provides coincident, collinear, concentric, midpoint, fix, parallel, perpendicular, horizontal, vertical, tangent, smooth (curvature continuous), symmetric and equal constraints. Constraint glyphs are shown next to entities and can be selected and deleted individually. | T1 | 3 |
| SB-SKETCH-025 | Constraint inference | While drawing, coincident, tangent, horizontal, vertical, parallel and perpendicular relations are inferred from cursor position and applied on placement. A modifier key suppresses inference for one placement, and inference can be turned off per type. | T1 | 3 |
| SB-SKETCH-026 | Dimensions | Adds linear, aligned, angular, radial, diameter, arc-length, ordinate and baseline dimensions by selecting entities. A single dimension tool infers the kind from the selection. | T1 | 3 |
| SB-SKETCH-027 | Expressions in dimensions | A dimension value is an expression with units that may reference parameters, functions and other dimensions. The dimension becomes a named parameter visible in the parameter table. | T1 | 3 |
| SB-SKETCH-028 | Driven dimensions | A dimension can be switched to driven: it shows the measured value in parentheses, publishes a reference parameter and never changes geometry. | T1 | 3 |
| SB-SKETCH-029 | Automatic dimensioning | A command proposes dimensions and constraints that fully constrain the selected entities using a deterministic rule set relative to chosen origin references. Proposals are previewed and applied as one transaction; an optional assistant can submit proposals through the same command. | T2 | 3 |
| SB-SKETCH-030 | Near-miss correction | Before applying proposed constraints, entities that are almost parallel, almost tangent or almost coincident within a tolerance are listed and can be snapped exactly. | T2 | 4 |
| SB-SKETCH-031 | Constrain on finish | An option applies automatic dimensioning when the sketch is closed, so sketches leave the editor fully constrained unless the user declines. | T3 | 4 |
| SB-SKETCH-032 | Dimension retrieval | Driven sketch dimensions can be created from existing geometry in one command, and sketch dimensions are available to drawings as model dimensions. | T2 | 3 |

## Solver behavior and diagnostics

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SKETCH-033 | Solve state display | Entities are colored by state: under-constrained, fully constrained, over-constrained and failed. The sketch reports its remaining degrees of freedom in the status bar and in the model tree. | T1 | 3 |
| SB-SKETCH-034 | Degree-of-freedom display | An overlay shows, for each entity, the directions in which it can still move. Selecting an entity lists the constraints acting on it. | T2 | 3 |
| SB-SKETCH-035 | Over-constraint handling | Adding a constraint or dimension that conflicts with existing ones is not applied; the conflicting set is highlighted and the user chooses to cancel, make the new dimension driven, or remove one of the listed constraints. | T1 | 3 |
| SB-SKETCH-036 | Stable solutions | When a dimension changes, the solver keeps the solution closest to the previous configuration and does not flip arcs, mirror orientations or tangency sides. If no nearby solution exists, the sketch reports failure and keeps the last valid geometry. | T1 | 3 |
| SB-SKETCH-037 | Sketch diagnostics | A diagnostics command finds open loops, gaps below tolerance, overlapping duplicates, zero-length entities, self-intersecting profiles and failed constraints, and offers a fix for each finding. | T1 | 3 |
| SB-SKETCH-038 | Interactive solve budget | Dragging and dimension edits re-solve at interactive rate for sketches of several hundred entities; larger solves continue in the background and the last valid state stays visible until the new one is ready. | T1 | 3 |

## Profiles

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SKETCH-039 | Profile detection | Closed regions formed by non-construction entities are detected automatically, including nested islands and regions split by intersecting entities. Hovering highlights a region and clicking selects it. | T1 | 3 |
| SB-SKETCH-040 | Select all profiles | A feature input can take every closed region of a sketch in one action. If the sketch later gains or loses regions, the feature tracks the selection rule rather than the original region list. | T2 | 3 |
| SB-SKETCH-041 | Stable profile references | Features reference profile regions through persistent references based on bounding entities, so adding unrelated entities or splitting other regions does not change which region a feature uses. | T1 | 3 |

## Projection and derived curves

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SKETCH-042 | Project geometry | Projects model edges, faces, vertices, reference geometry and other sketches onto the sketch plane. Projected entities are associative and update when the source changes. | T1 | 3 |
| SB-SKETCH-043 | Associativity control | Each projected entity can be made non-associative, which freezes its current shape. Automatic projection of referenced edges while dimensioning can be turned on or off. | T1 | 3 |
| SB-SKETCH-044 | Intersection curves | Creates sketch curves where the sketch plane intersects selected bodies or faces, updating with the model. | T1 | 3 |
| SB-SKETCH-045 | Project to surface | Projects curves onto a surface along the surface normal, along a vector or to the closest point, and wraps curves onto cylindrical and conical faces preserving length. | T2 | 3 |
| SB-SKETCH-046 | Silhouette and cut-edge projection | Projects the silhouette of bodies as seen along the sketch normal, and the edges where a section at the sketch plane cuts the model. | T2 | 3 |
| SB-SKETCH-047 | Include from flat pattern | Projects geometry from a sheet metal flat pattern into a sketch on the flat pattern so cuts can be drawn in the unfolded state. | T2 | 4 |

## 3D sketches

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SKETCH-048 | 3D sketch entities | Creates lines, polylines, arcs, splines and points in 3D, using a triad to constrain motion to an axis or plane and snapping to model geometry. | T1 | 3 |
| SB-SKETCH-049 | 3D constraints and dimensions | Applies coincident, parallel, perpendicular, collinear, tangent, parallel-to-axis and on-face constraints and 3D distance and angle dimensions. | T2 | 3 |
| SB-SKETCH-050 | Helix and spiral curves | Creates helical and spiral curves by axis, pitch, revolutions, height and taper, usable as sweep paths. | T2 | 3 |
| SB-SKETCH-051 | Automatic bends | Polylines can carry an automatic bend radius at every vertex, producing tangent arcs for frame, tube and wire paths. | T2 | 4 |
| SB-SKETCH-052 | Curves from faces | Creates 3D curves at face intersections, by projecting a 2D curve onto faces, and by combining two planar curves into one 3D curve. | T2 | 3 |

## Reuse and import

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SKETCH-053 | Sketch blocks | Groups entities and constraints into a block definition placed as instances. Editing the definition updates every instance; blocks can nest, carry flexible degrees of freedom and be saved to a library. | T2 | 3 |
| SB-SKETCH-054 | Insert DXF and DWG | Inserts 2D geometry from a DXF or DWG file into a sketch with unit and scale selection, layer filtering and layer-to-construction mapping. Imported entities can be constrained like any others. | T1 | 3 |
| SB-SKETCH-055 | Insert SVG | Inserts paths from an SVG file as lines, arcs and splines at a chosen scale. | T2 | 3 |
| SB-SKETCH-056 | Reference images | Places a raster image on a plane or face, calibrated by two points and a known distance, with opacity and visibility controls. A reference image never contributes to profiles and can be hidden in drawings. | T2 | 3 |
| SB-SKETCH-057 | Copy and paste between sketches | Copies entities with their internal constraints to another sketch; constraints to entities outside the selection are dropped and reported. | T1 | 3 |

## Reference geometry

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SKETCH-058 | Origin references | Every component has origin planes, axes and a point that never move relative to the component and can be shown or hidden. | T1 | 3 |
| SB-SKETCH-059 | Datum planes | Creates planes offset from a plane, at an angle about an edge, midway between two planes, through two edges, through three points, tangent to a face, tangent to a face at a point, normal to a curve at a point, and normal to an axis through a point. | T1 | 3 |
| SB-SKETCH-060 | Datum axes | Creates axes through a cylinder, cone or torus, along an edge, through two points, at the intersection of two planes, normal to a face at a point, and perpendicular to a plane through a point. | T1 | 3 |
| SB-SKETCH-061 | Datum points | Creates points at a vertex, at the center of a circle, sphere or torus, at a curve midpoint, at an edge-plane or two-edge intersection, at three planes, along a path by distance or ratio, and at fixed coordinates. | T1 | 3 |
| SB-SKETCH-062 | Coordinate systems | Creates named coordinate systems from an origin and two directions or from a planar face and an edge. Coordinate systems serve as placement for imports, measurements, mass properties, joints, manufacturing setups and scripts. | T2 | 3 |
| SB-SKETCH-063 | Reference geometry updates | Reference geometry is a feature with persistent references to its inputs and regenerates when they change. If an input disappears, the item is flagged with a repair action rather than silently moved. | T1 | 3 |
| SB-SKETCH-064 | Fixed and grounded references | A datum plane, axis or point can be fixed in component space, detaching it from its inputs, so it serves as a stable skeleton reference independent of model topology. | T1 | 3 |
| SB-SKETCH-065 | Reference geometry in assemblies | Reference geometry can be created in assembly context between occurrences and follows them when they move. | T2 | 3 |
| SB-SKETCH-066 | Reference geometry visibility in drawings | Datum planes, axes and points can be included in drawing views as centerlines or reference lines, controlled per view. | T2 | 3 |
