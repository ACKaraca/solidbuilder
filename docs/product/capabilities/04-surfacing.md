# Surfacing

Surface modeling builds and edits sheet bodies with controlled continuity, then closes them into solids
for downstream solid features, drawings and manufacturing. Every surface operation is a feature in the
feature history, references its inputs through persistent references and regenerates when they change.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Body (sheet) | Faces, boundary edges, open or closed state, orientation, stitch tolerance | Owned by a Part; produced and consumed by surface features; becomes a solid Body when stitched closed |
| Surface feature | Kind (extrude, revolve, sweep, loft, patch, ruled, offset, trim, extend, stitch, match, merge), inputs, parameters | Entry in the Feature history; inputs held as Persistent references to sketch entities, edges and faces |
| Continuity condition | Edge or boundary, level (G0 position, G1 tangent, G2 curvature, G3 curvature rate), weight, reference face | Attached to a boundary of a patch, loft, match or fill feature |
| Surfacing curve | Kind (isoparametric, intersection, silhouette, projected, control-point), degree, control polygon | Wire Body or Sketch entity used as an input by surface features |
| Analysis display | Kind (zebra, curvature comb, curvature map, isocurve, draft, reflection, continuity, gap), settings, target bodies | Display-only object stored in the Document; never changes geometry; recomputes on regeneration |

## Surface creation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SURF-001 | Extruded surface | Creates a sheet body by extruding open or closed sketch curves or edges along a direction with distance, to-object, symmetric or two-sided extents and an optional taper angle. The surface regenerates when the profile or extent references change. | T1 | 3 |
| SB-SURF-002 | Revolved surface | Creates a sheet body by revolving curves about an axis by an angle, to an object or symmetrically. Revolving a curve that touches the axis produces a degenerate pole without a gap. | T1 | 3 |
| SB-SURF-003 | Swept surface | Creates a sheet body by sweeping a profile along a path with optional guide rail or guide surface, orientation mode (normal to path, fixed, guided), twist and taper. The feature reports self-intersection along the path as an error instead of producing invalid geometry. | T1 | 3 |
| SB-SURF-004 | Lofted surface | Creates a sheet body through ordered sections with optional rails or a center line, closed loft, point ends and per-end conditions (free, tangent, curvature) to adjacent faces. Section alignment is controlled by matching vertices, and the result regenerates when any section changes. | T1 | 3 |
| SB-SURF-005 | Boundary patch | Fills a closed loop of edges or curves with a single face, with a continuity condition up to G2 per boundary edge and optional interior guide curves or points. The patch reports the maximum deviation from each boundary after creation. | T1 | 3 |
| SB-SURF-006 | Curvature-rate continuity | Extends patch, loft, match and fill features to G3 continuity at boundaries where the adjacent face degree allows it, and reports per boundary the continuity achieved. When G3 cannot be met within tolerance the feature falls back to G2 and flags the boundary. | T3 | 4 |
| SB-SURF-007 | Network surface | Creates one surface from two crossing families of curves, with continuity conditions on the outer boundary curves. Curves that do not intersect within tolerance are listed and the feature fails without a partial result. | T2 | 4 |
| SB-SURF-008 | Ruled surface | Creates a ruled sheet from an edge chain by normal, tangent or fixed-direction rule with a distance and angle. Tangent chains are followed automatically, with an option to stop at sharp corners. | T2 | 3 |
| SB-SURF-009 | Offset surface | Creates sheet bodies offset from selected faces by a distance in either direction, following tangent chains. Regions where the offset exceeds local curvature radius are reported and excluded instead of producing self-intersecting faces. | T1 | 3 |
| SB-SURF-010 | Fill between bodies | Creates bounding faces from a selection of surfaces, planes and solids and keeps the enclosed cells the user picks as new solid bodies or as material joined to or cut from an existing body. | T2 | 3 |
| SB-SURF-011 | Solid from surfaces | Builds a solid from a set of sheet bodies that together enclose a volume, trimming them against one another, with join, cut, intersect or new-body result. Openings larger than the stitch tolerance are listed by location. | T2 | 3 |
| SB-SURF-012 | Blend surface | Creates a connecting surface between two edges or edge chains of separate faces with chosen continuity at each side and adjustable bulge. The blend updates when either source face changes. | T2 | 4 |

## Surface editing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SURF-013 | Trim surface | Removes regions of sheet bodies using curves, surfaces, planes or bodies as cutting tools; the user picks the regions to keep or remove. Picked regions are stored as persistent references and survive upstream changes that keep the region. | T1 | 3 |
| SB-SURF-014 | Untrim surface | Removes selected trim boundaries or holes from a face, restoring the face to its underlying surface extent or to a chosen loop. Natural edges of the underlying surface are kept. | T2 | 3 |
| SB-SURF-015 | Extend surface | Lengthens sheet edges by distance or to a target with natural, tangent or perpendicular extension and an option to merge the extension into the face. Corner extensions between adjacent extended edges are filled without gaps. | T1 | 3 |
| SB-SURF-016 | Stitch | Joins sheet bodies along coincident edges within a stated tolerance into one sheet, and into a solid when the result is closed. Edges left open are highlighted with their gap size, and the tolerance used is stored with the feature. | T1 | 3 |
| SB-SURF-017 | Unstitch | Separates selected faces of a sheet or solid body into independent sheet bodies, converting a solid to sheets when needed. The unaffected faces keep their persistent names. | T1 | 3 |
| SB-SURF-018 | Reverse normal | Flips the orientation of selected sheet bodies or faces. Orientation is used by offset, thicken and analysis, and those features regenerate after the flip. | T1 | 3 |
| SB-SURF-019 | Merge faces | Merges adjacent faces that lie on the same underlying surface, or that meet tangentially within tolerance, into a single face. Faces that cannot be represented as one surface are left unchanged and reported. | T2 | 4 |
| SB-SURF-020 | Match surface | Deforms the edge region of a face so that it meets a target edge, curve or face with G0, G1 or G2 continuity, with options to preserve the opposite boundary and limit the affected region. The deviation from the original face is reported. | T2 | 4 |
| SB-SURF-021 | Replace face with surface | Replaces faces of a solid or sheet with a target surface, extending or trimming adjacent faces so the body stays closed. Fails with a message naming the faces that cannot reach the target. | T1 | 3 |
| SB-SURF-022 | Split face and body by surface | Divides faces or bodies using sheet bodies, planes or projected curves as tools, keeping all pieces. Resulting faces receive persistent names derived from the tool and the split face. | T1 | 3 |
| SB-SURF-023 | Surface delete with heal | Deletes faces from a sheet body, optionally healing the hole with a patch that matches neighboring tangency. Without healing, the body becomes or stays an open sheet. | T2 | 3 |
| SB-SURF-024 | Silhouette split | Splits faces along the silhouette seen from a pull direction, creating edges usable for draft, parting and drawing outlines. The split updates when the direction or faces change. | T2 | 4 |
| SB-SURF-025 | Thicken reference | Sheet bodies are thickened into solids by the thicken feature described in [Solid features](03-solid-features.md); surface features expose orientation and offset limits that thicken uses. | T1 | 3 |
| SB-SURF-026 | Control-point surface edit | Edits the control points of an untrimmed or trimmed NURBS face with a gizmo, numeric moves and symmetry, keeping chosen boundary continuity fixed. The edit is recorded as a feature and replayed on regeneration as offsets relative to the input surface. | T3 | 4 |
| SB-SURF-027 | Surface rebuild | Approximates a selected face with a new surface of a requested degree and span count within a stated tolerance, reporting the maximum deviation. Used to simplify imported or dense surfaces before matching. | T3 | 4 |

## Curves for surfacing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SURF-028 | Isoparametric curve | Creates a curve along the U or V direction of a face at a parameter value or through a picked point. The curve is associative to the face and updates with it. | T2 | 4 |
| SB-SURF-029 | Intersection curve | Creates wire geometry where two sets of faces, planes or sketches intersect. Multiple disjoint intersection branches are kept as separate curves. | T1 | 3 |
| SB-SURF-030 | Silhouette curve | Creates a curve along the silhouette of faces seen from a direction. The curve updates when the faces or direction change. | T2 | 4 |
| SB-SURF-031 | Projected and wrapped curve | Projects sketch curves onto faces along a direction or along face normals, or wraps them onto cylindrical and conical faces preserving length. Portions that miss the faces are reported. | T1 | 3 |
| SB-SURF-032 | Control-point curve with curvature control | Creates and edits curves by control polygon with degree selection, end tangency and curvature handles, and a live curvature comb. End conditions can be bound to adjacent geometry with G1, G2 or G3 continuity. | T2 | 4 |
| SB-SURF-033 | Curve fit and simplify | Fits a smooth curve through selected edges, points or polylines within a tolerance, or reduces the control points of an existing curve, reporting maximum deviation. | T2 | 4 |
| SB-SURF-034 | Composite curve | Joins connected edges and curves into one wire that surface features can use as a single profile, rail or boundary. Gaps above tolerance are reported by location. | T1 | 3 |

## Quality analysis

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SURF-035 | Zebra analysis | Displays parallel stripes reflected on selected faces with adjustable direction, density and thickness, so that G0, G1 and G2 transitions show as stripe breaks, kinks or smooth flow. The display updates live during edits. | T1 | 3 |
| SB-SURF-036 | Curvature comb | Displays curvature combs on curves and edges with adjustable density and scale, and reports minimum and maximum curvature values. The comb updates while the curve is being edited. | T1 | 3 |
| SB-SURF-037 | Curvature map | Colors faces by Gaussian, mean, minimum or maximum curvature with a user-set range and a legend. Values outside the range are shown in clamp colors. | T2 | 3 |
| SB-SURF-038 | Isocurve display | Draws the U and V isoparametric lines of selected faces with a chosen density. It reveals parameterization quality and degenerate regions. | T2 | 4 |
| SB-SURF-039 | Draft analysis | Colors faces by their angle to a pull direction against positive and negative threshold angles, separating positive, negative, vertical and insufficient draft. Face-level and point-level angle readouts are available. | T1 | 3 |
| SB-SURF-040 | Reflection analysis | Displays an environment map reflected on selected faces with adjustable map and rotation, showing highlight flow across boundaries. Rendering uses the viewport, not a separate renderer. | T2 | 4 |
| SB-SURF-041 | Continuity check | Measures position gap, tangent angle and curvature difference along shared edges between selected faces and reports maximum values per edge against user thresholds. Edges that fail a threshold are highlighted and listed. | T2 | 4 |
| SB-SURF-042 | Gap and tolerance report | Lists open edges, sliver faces, short edges and edges whose tolerance exceeds the document modeling tolerance for selected bodies, with location and value. The report can be saved with the Document and rerun after regeneration. | T1 | 3 |
| SB-SURF-043 | Persistent analysis displays | Saves analysis settings and target bodies as analysis displays in the Document that can be shown or hidden. They recompute after every regeneration and are excluded from exports and drawings. | T2 | 3 |
| SB-SURF-044 | Deviation between surfaces | Computes the distance between two sets of faces or between faces and a mesh, reports minimum, maximum and mean values and colors the result by deviation band. | T2 | 4 |
