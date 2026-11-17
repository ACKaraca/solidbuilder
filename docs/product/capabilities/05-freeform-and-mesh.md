# Freeform and mesh modeling

Freeform subdivision bodies give an interactive way to shape organic forms that convert into exact
boundary-representation solids, and mesh bodies bring scanned, printed and polygonal data into the same
part. Both kinds of body live in the feature history next to solid and sheet bodies, so later features
can reference them and regenerate when they change.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Body (subdivision) | Control cage (vertices, edges, faces), creases, symmetry definitions, frozen elements, display mode | Owned by a Part; created and edited by freeform features; converted to a solid or sheet Body by a conversion feature |
| Freeform edit | Selected cage elements, transform, soft-modification falloff, operation kind | Feature in the Feature history acting on one subdivision Body; selections stored as Persistent references to cage elements |
| Symmetry definition | Kind (mirror, circular, repeating), plane or axis, count | Belongs to a subdivision Body; keeps paired cage elements in step |
| Body (mesh) | Vertices, triangles or quads, normals, face groups, units, shell count, source Import record | Owned by a Part; edited by mesh features; may be converted to a solid Body |
| Face group | Member faces, detected surface type (plane, cylinder, cone, sphere, free) | Belongs to a mesh Body; drives prismatic conversion and selection |
| Deviation map | Reference and test bodies, minimum, maximum, mean, color bands | Analysis display in the Document; recomputes when either body changes |
| Lattice region | Cell type, cell size, strut or sheet thickness, boundary skin thickness | Feature producing a mesh Body inside a solid or mesh Body |

## Freeform creation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FORM-001 | Freeform primitives | Creates a subdivision body from a box, plane, cylinder, sphere, torus, quadball or pipe with user-set dimensions and face counts per direction. Dimensions stay editable as feature parameters until the first cage edit. | T2 | 4 |
| SB-FORM-002 | Draw faces | Creates subdivision faces by placing vertices in space or on a plane, building a cage face by face with snapping to existing cage vertices. | T2 | 4 |
| SB-FORM-003 | Freeform extrude, revolve, sweep and loft | Creates subdivision bodies from sketch curves or edges by extrude, revolve, sweep or loft, with a face count along each direction. Edge flow follows the input curves so later edits stay predictable. | T2 | 4 |
| SB-FORM-004 | Reference image tracing | Uses reference images placed on planes as backdrops for cage editing, with calibration of image scale from two picked points and a known distance. | T2 | 4 |

## Freeform editing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FORM-005 | Cage transform | Moves, rotates and scales selected cage vertices, edges or faces with an on-screen gizmo or numeric input in world, local or view coordinates, with a movable pivot. Each edit session is one feature in the history. | T2 | 4 |
| SB-FORM-006 | Soft modification | Applies a transform to a selection with a falloff that moves surrounding cage elements by distance with a chosen profile. The falloff radius is shown before the edit is applied. | T2 | 4 |
| SB-FORM-007 | Extrude cage faces and edges | Pulls selected faces or boundary edges outward to create new cage faces, with distance and count. Symmetry definitions are honored. | T2 | 4 |
| SB-FORM-008 | Insert edge and point | Inserts edge loops across faces at a relative position or through picked points, and inserts vertices on edges, adding cage resolution without changing the limit surface shape where possible. | T2 | 4 |
| SB-FORM-009 | Subdivide faces | Splits selected faces into four or into a user-set grid, with an exact option that preserves the limit surface. | T2 | 4 |
| SB-FORM-010 | Merge, bridge and fill hole | Merges two boundary edge chains, bridges two face sets with a set number of faces, and fills a boundary hole with a star, collapsed or reduced-star fill. Mismatched edge counts are resolved by inserting edges and reported. | T2 | 4 |
| SB-FORM-011 | Weld and unweld vertices | Joins vertices within a tolerance or a picked pair into one, and separates a welded vertex to open a seam. | T2 | 4 |
| SB-FORM-012 | Crease and uncrease | Marks edges or vertices as sharp so the limit surface follows them exactly, and removes the marking. Creased edges become edges in the converted solid. | T2 | 4 |
| SB-FORM-013 | Bevel and slide edge | Bevels selected cage edges into a strip of faces with a width and segment count, and slides edges along neighboring faces without changing topology. | T2 | 4 |
| SB-FORM-014 | Delete cage elements | Deletes vertices, edges or faces from the cage and reconnects the remaining cage, opening a boundary where faces are removed. | T2 | 4 |
| SB-FORM-015 | Match to curves and surfaces | Moves a boundary edge chain of the cage so that the limit surface follows a target curve, edge or face with position or tangent continuity. The match is recomputed when the target changes. | T3 | 4 |
| SB-FORM-016 | Interpolate and pull to target | Moves cage vertices so the limit surface passes through them, or projects cage vertices onto a target body (solid, sheet or mesh) along normals or a direction. | T3 | 4 |
| SB-FORM-017 | Flatten, straighten and make uniform | Moves selected vertices onto a best-fit or chosen plane, straightens edge chains, and redistributes cage vertices to even spacing while keeping boundary vertices fixed. | T2 | 4 |
| SB-FORM-018 | Smooth cage | Relaxes selected cage regions toward even curvature with a strength and iteration count, keeping creased and frozen elements fixed. | T2 | 4 |
| SB-FORM-019 | Thicken freeform | Gives a subdivision sheet a thickness to produce a closed subdivision body, with sharp or soft rims and a direction. | T2 | 4 |
| SB-FORM-020 | Symmetry | Defines mirror, circular or repeating symmetry on a subdivision body so edits to one cage element update its counterparts, and allows symmetry to be cleared or re-detected. | T2 | 4 |
| SB-FORM-021 | Freeze elements | Locks selected cage elements so later edits, smoothing and soft modification do not move them. Frozen elements are shown in a distinct color. | T2 | 4 |
| SB-FORM-022 | Display modes | Shows a subdivision body as cage only, limit surface only, or both, and toggles the display per body. Zebra and curvature analysis apply to the limit surface. | T2 | 4 |

## Freeform conversion

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FORM-023 | Convert to exact body | Converts a subdivision body into an exact solid or sheet Body made of NURBS faces, with creases mapped to edges. The conversion is a feature, so later solid features regenerate when the cage is edited again. | T2 | 4 |
| SB-FORM-024 | Conversion diagnostics | Before conversion, reports cage conditions that prevent a valid result (non-manifold edges, self-intersection, extraordinary vertices adjacent to creases) with locations, and offers repair actions for the fixable ones. | T2 | 4 |
| SB-FORM-025 | Quad remesh to subdivision body | Converts a mesh Body into a subdivision body with a target face count, optional feature-edge preservation and symmetry, reporting deviation from the source mesh. | T3 | 5 |

## Mesh bodies

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-FORM-026 | Insert mesh | Places a mesh from STL, OBJ, 3MF or PLY into the part as a mesh Body with unit selection, placement transform and centering; file reading is described in [Interoperability](19-interoperability.md). The source Import record is kept for reload. | T2 | 3 |
| SB-FORM-027 | Mesh analysis | Reports shell count, triangle count, open boundaries, non-manifold edges, flipped normals, self-intersections, degenerate triangles, volume and area for a mesh Body, each finding selectable in the viewport. | T2 | 3 |
| SB-FORM-028 | Mesh repair | Closes holes, removes isolated shells below a size, stitches nearby boundaries, unifies normals and removes degenerate and duplicate triangles, reporting each change made. | T2 | 3 |
| SB-FORM-029 | Reduce mesh | Decimates a mesh to a target triangle count, ratio or maximum deviation while preserving boundaries and face group borders. The achieved deviation is reported. | T2 | 3 |
| SB-FORM-030 | Remesh | Rebuilds the triangulation uniformly, adaptively by curvature, or to a target edge length, keeping sharp edges above an angle threshold. | T2 | 4 |
| SB-FORM-031 | Plane cut and section | Cuts a mesh Body by a plane, keeping one or both sides with optional capping of the cut, and creates sketch curves from a mesh section plane for rebuilding exact features. | T2 | 3 |
| SB-FORM-032 | Mesh booleans | Combines mesh Bodies, or mesh and solid Bodies, by union, subtraction and intersection, producing a closed mesh when the inputs are closed. Inputs that are not closed are rejected with the open boundaries listed. | T2 | 4 |
| SB-FORM-033 | Separate mesh | Splits a mesh Body into separate Bodies by shell or by face group. | T2 | 3 |
| SB-FORM-034 | Reverse and smooth mesh | Reverses normals of selected triangles or shells, and smooths selected regions with a strength and iteration count while preserving boundaries. | T2 | 3 |
| SB-FORM-035 | Face groups | Detects face groups by curvature and angle thresholds, fits each to plane, cylinder, cone, sphere or free surface, and allows manual merge and split of groups. | T2 | 4 |
| SB-FORM-036 | Mesh to exact body | Converts a closed mesh Body into a solid by faceted conversion (one planar face per triangle or merged coplanar region), prismatic conversion (one analytic face per face group) or organic conversion (fitted freeform surfaces within a tolerance). The method and achieved deviation are stored with the feature. | T2 | 4 |
| SB-FORM-037 | Mesh compare | Computes the deviation between a mesh Body and a solid, sheet or other mesh Body, showing a color deviation map with user-set bands and reporting minimum, maximum and mean distances. | T2 | 4 |
| SB-FORM-038 | Mesh shell and offset | Hollows a closed mesh to a wall thickness with optional removed regions, and offsets a mesh along normals by a distance, resolving self-intersections. | T2 | 4 |
| SB-FORM-039 | Align mesh | Aligns a mesh Body to another body by picked point pairs followed by best-fit refinement, reporting the residual error. | T2 | 4 |
| SB-FORM-040 | Texture extrude | Displaces mesh faces along normals by the gray values of an image mapped onto selected faces, with depth and mapping controls. | T3 | 4 |
| SB-FORM-041 | Volumetric lattice | Fills a solid or closed mesh Body with a periodic lattice of a chosen cell type, cell size and strut or sheet thickness, with an optional outer skin, producing a mesh Body ready for additive manufacturing. | T3 | 5 |
| SB-FORM-042 | Mixed mesh and exact faces | Keeps mesh regions and exact faces in one body so that solid features such as holes, cuts and fillets can be applied to exact regions of a partly scanned part without converting the whole mesh. | T3 | 5 |
| SB-FORM-043 | Mesh display | Shows mesh Bodies with shaded, faceted, wireframe and face-group coloring modes, with triangle counts in the property panel. Large meshes use level-of-detail display without changing data. | T2 | 3 |
