# Drawings

Drawings present components as associative 2D documentation: views are projected from the model in the
background, annotations attach through persistent references, and tables are derived from the bill of
materials and features. Output goes through the shared Arc drafting and sheet pipeline, so line
weights, plot styles and file exports match the other Arc products.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Drawing | Drawing standard, sheets, referenced components | Stored in the same document as the model or in a separate document |
| Sheet | Size, orientation, sheet format, scale, views, annotations | Owned by a drawing |
| Sheet format | Border, title block fields bound to properties, zones | Applied to sheets; stored in templates |
| Drawing view | Kind, source component, configuration, model state, display state, position state, orientation, scale, style, parent view, revision computed from | Placed on a sheet; projected from the model through the view engine |
| Annotation | Kind, attachment through persistent references or view geometry, style, value source | Placed in a view or on the sheet |
| Drawing standard | Dimension, text, leader, symbol, hatch, table, layer and line styles | Applied to a drawing |
| Table | Kind (parts list, revision, hole, bend, weld, general), data source, columns, filters | Placed on a sheet; derived from the model where applicable |
| Balloon | Item number source, shape, attachment | Linked to a parts list row |

## Sheets and templates

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DRAW-001 | Create drawing | Creates a drawing for a component, configuration or state from a template that sets the drawing standard, units, sheet size and sheet format. | T1 | 3 |
| SB-DRAW-002 | Drawing standards | Provides style sets for ISO, ASME, DIN, JIS, BSI, GB and AS conventions, editable and savable as library files; changing a drawing's standard restyles all annotations. | T1 | 3 |
| SB-DRAW-003 | Sheet formats | Borders and title blocks with fields bound to component, drawing and sheet properties update when the properties change. | T1 | 3 |
| SB-DRAW-004 | Multiple sheets | Adds, copies, reorders and deletes sheets with independent sizes and formats; sheet numbers and counts are available as fields. | T1 | 3 |
| SB-DRAW-005 | Templates | Drawing templates are documents with standards, sheet formats and default views; a project can define its default template. | T1 | 3 |
| SB-DRAW-006 | Layers | Annotations and view lines are assigned to layers with color, line type and weight, set by the drawing standard and overridable. | T1 | 3 |
| SB-DRAW-007 | Transfer resources | Copies standards, sheet formats, symbols and table styles from one drawing or template to another. | T2 | 3 |

## Views

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DRAW-008 | Base view | Places a view of a component from a standard or saved orientation with scale, style and the configuration and states to show. | T1 | 3 |
| SB-DRAW-009 | Projected views | Projects orthographic and isometric views from a parent view in first-angle or third-angle convention; projected views stay aligned with their parent. | T1 | 3 |
| SB-DRAW-010 | Auxiliary view | Projects a view perpendicular to a selected edge or face of a parent view. | T1 | 3 |
| SB-DRAW-011 | Section views | Creates full, half, offset and aligned sections from a sketched cut line with depth or slice options; hatching follows material, and each component can be excluded from cutting. | T1 | 3 |
| SB-DRAW-012 | Detail view | Enlarges a circular or rectangular region at a chosen scale with a smooth or jagged boundary and a label. | T1 | 3 |
| SB-DRAW-013 | Break view | Removes one or more middle portions of long components with break lines in a chosen style; dimensions across the break show true values. | T1 | 3 |
| SB-DRAW-014 | Break-out view | Removes material inside a closed sketch to a depth to reveal internal features. | T2 | 3 |
| SB-DRAW-015 | Crop view | Clips a view to a closed boundary. | T2 | 3 |
| SB-DRAW-016 | Overlay view | Shows several position states or model states in one view with the alternates drawn in a phantom line style. | T2 | 4 |
| SB-DRAW-017 | Exploded view | Shows an assembly in a saved exploded state with trail lines. | T1 | 3 |
| SB-DRAW-018 | Flat pattern view | Shows a sheet metal flat pattern with bend lines and bend annotations. | T2 | 4 |
| SB-DRAW-019 | Draft view and view sketches | A draft view holds 2D geometry only; any view can carry sketches that move with the view and may reference view geometry. | T1 | 3 |
| SB-DRAW-020 | View styles | Each view shows visible lines, hidden lines, or shaded rendering, with tangent edges, thread representation and silhouette options; child views inherit unless overridden. | T1 | 3 |
| SB-DRAW-021 | Arrange views | Views can be moved with their children, aligned, rotated by angle or edge, locked, and relabeled with automatic labels per the standard. | T1 | 3 |
| SB-DRAW-022 | Background view update | Views are recomputed on worker threads after model changes; a view shows an out-of-date mark until its result for the current revision arrives, and a stale result is never shown as current. | T1 | 3 |
| SB-DRAW-023 | Multi-model drawings | One drawing can show views of different components and documents, each with its own source list. | T2 | 3 |
| SB-DRAW-024 | Automatic view sets | Generates a standard set of views (front, top, side, isometric) for a component with spacing and scale chosen to fit the sheet. | T2 | 3 |

## Dimensions

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DRAW-025 | Retrieve model dimensions | Places the driving dimensions of selected features in a view; a dimension already shown in another view is skipped. Editing a retrieved dimension's value changes the model. | T1 | 3 |
| SB-DRAW-026 | Drawing dimensions | Adds linear, aligned, angular, radial, diameter, arc length and chamfer dimensions on view geometry as reference dimensions that follow the model. | T1 | 3 |
| SB-DRAW-027 | Ordinate, baseline and chain | Creates ordinate, baseline and chain dimension sets from an origin; adding a member keeps spacing. | T1 | 3 |
| SB-DRAW-028 | Tolerances | Dimensions show symmetric, deviation, limit, basic and fit tolerances; fit designations use ISO 286 tables and show computed limits on request. | T1 | 3 |
| SB-DRAW-029 | Alternate units | Dimensions can show a second unit system with its own precision and format. | T2 | 3 |
| SB-DRAW-030 | Arrange dimensions | Spaces overlapping dimensions per the standard and aligns selected dimensions. | T2 | 3 |
| SB-DRAW-031 | Automatic annotation layout | Places retrieved dimensions to avoid overlaps with geometry and each other, keeping dimension lines outside the part outline where possible. | T3 | 4 |

## Annotations and symbols

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DRAW-032 | Text and leaders | Places notes and leader notes with fields bound to properties and parameters that update with the model. | T1 | 3 |
| SB-DRAW-033 | Hole and thread notes | Generates notes from hole and thread specifications (size, depth, counterbore, countersink, thread designation, class) and updates them when the feature changes. | T1 | 3 |
| SB-DRAW-034 | Chamfer notes | Generates chamfer notes from chamfer features or geometry in the format of the standard. | T1 | 3 |
| SB-DRAW-035 | Center marks and centerlines | Adds center marks, centerlines, bisectors and centered patterns associated with circles and edges; automatic placement can be set per view. | T1 | 3 |
| SB-DRAW-036 | Geometric tolerancing symbols | Places feature control frames, datum identifiers and datum targets attached to geometry or dimensions. | T1 | 3 |
| SB-DRAW-037 | Surface texture symbols | Places surface texture symbols per ISO 21920 and ASME Y14.36. | T1 | 3 |
| SB-DRAW-038 | Weld symbols | Places weld symbols per ISO 2553 or AWS A2.4, either sketched or generated from weld data in the model. | T2 | 4 |
| SB-DRAW-039 | Edge and taper symbols | Places edge condition, taper and slope symbols. | T2 | 3 |
| SB-DRAW-040 | Sketched symbols | Users define reusable symbols with geometry and prompted text fields, stored in templates or libraries. | T1 | 3 |
| SB-DRAW-041 | Annotation scale | Annotations keep their paper size when a view scale changes. | T1 | 3 |
| SB-DRAW-042 | Physical property fields | Mass and center of gravity of the viewed component can be shown as fields and as a center of gravity mark. | T2 | 3 |
| SB-DRAW-043 | Annotation survival | Annotations attached to model geometry follow it through regeneration via persistent references; an annotation whose target disappears is marked orphaned and listed for reattachment, never silently moved. | T1 | 3 |

## Tables

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DRAW-044 | Parts list | Places a parts list from a saved BOM view with filters, grouping, sorting and column choice; the table splits across columns or sheets when it overflows. | T1 | 3 |
| SB-DRAW-045 | Balloons | Places balloons manually or automatically around a view; balloon numbers follow item numbers and update when the bill of materials is renumbered. | T1 | 3 |
| SB-DRAW-046 | Revision table | Maintains a revision table whose rows can be filled from Arc Hub revisions; revision tags can be placed on the sheet. | T1 | 3 |
| SB-DRAW-047 | Hole table | Tags holes in a view and lists positions from an origin with size and thread data; added holes are tagged on update. | T2 | 3 |
| SB-DRAW-048 | Bend table | Lists bends of a flat pattern with tag, direction, angle, radius and K-factor. | T2 | 4 |
| SB-DRAW-049 | Weld and cut list tables | Lists welds and frame cut lists from the model data. | T2 | 4 |
| SB-DRAW-050 | General tables | Creates tables by hand or from CSV and XLSX data, optionally linked for refresh. | T2 | 3 |

## Output and automation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DRAW-051 | PDF export | Exports sheets to vector PDF with layers, embedded fonts and searchable text. | T1 | 3 |
| SB-DRAW-052 | DWG and DXF export | Exports sheets to DWG or DXF with layer mapping and model or paper space choice. | T1 | 3 |
| SB-DRAW-053 | SVG export and printing | Exports sheets to SVG and prints through the system print service with plot styles. | T2 | 3 |
| SB-DRAW-054 | Headless publishing | The CLI regenerates drawings and exports a set of drawings to PDF, DWG or DXF without a display, failing with a report if any view is out of date or orphaned annotations exist when requested. | T1 | 3 |
| SB-DRAW-055 | Drawing compare | Compares two versions of a drawing and highlights added, removed and changed geometry and annotations. | T2 | 4 |
| SB-DRAW-056 | Retrieve 3D annotations | Places 3D annotations from the model into views where their annotation plane is parallel to the view. | T2 | 4 |
