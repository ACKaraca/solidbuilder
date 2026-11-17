# Routed systems

Routed systems cover tube, pipe and hose runs, and the cable and wire harnesses that follow three-dimensional paths through an assembly. The area defines styles, standards tables, fittings, route creation and editing, bend and cut data, harness electrical parts, segments, flattened formboard layouts and the reports that feed manufacturing. Routes and harnesses are ordinary typed objects in the assembly: they regenerate when the parts they connect to move.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Route style | Kind (rigid tube, rigid pipe, bent tube, hose), standard, nominal sizes, material, bend rules, allowed end types, auto-route rules | Referenced by routes; stored in a document or a shared library |
| Size table | Nominal size, outside diameter, wall thickness, schedule or series, unit | Owned by a route style; selects the section of every segment |
| Port | Position, axis, nominal size, end type, flow direction | Hosted by a part, fitting or equipment body; ends of routes attach to ports |
| Route | Ordered vertices and segments, style, tag, parameters, constraints | Attaches to ports; owns fittings, runs and spools; contributes to a routed-system bill of materials |
| Route segment | Straight, bend or branch type, length, radius, plane | Belongs to a route; drives bend table rows |
| Fitting | Standard, type, nominal size, end types, take-out distances, ports | Placed at route corners, branches and size changes; sourced from the standard parts library |
| Run | Sequence of cut pieces and fittings, cut length, joint allowances | Derived from a route; feeds the cut list and spools |
| Spool | Identifier, member pieces, field-joint boundaries | Subdivision of a route; owns a spool drawing |
| Bend table | Per-bend straight length, angle, rotation, radius, coordinates | Derived from a route; used in drawings and bender output |
| Electrical part | Connector, terminal, splice or accessory with pin table and cavity positions | Placed in the assembly; referenced by wires and cables |
| Wire type | Gauge, color and stripe, insulation diameter, minimum bend radius, mass per length | Stored in a wire library; assigned to wires |
| Cable type | Conductor list, jacket diameter, shield, minimum bend radius | Stored in a cable library; owns conductors that map to pins |
| Harness | Name, part number, properties, segments, wires, cables, accessories | Belongs to an assembly; owns a formboard and wire list |
| Harness segment | Centerline path, computed diameter, covering, fill ratio | Connects branch points; carries wires and cables |
| Formboard | Flattened planar layout of segments, node positions, scale | Derived from a harness; source for a formboard drawing |
| Wire list | Rows of from, to, wire or cable, color, length, signal | Derived from a harness; exported as a report |

## Styles, standards and libraries

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ROUTE-001 | Route style | Defines a named style for rigid tube, rigid pipe, bent tube or hose with standard, material, size table, bend rules, allowed fittings and auto-route rules. Changing a style regenerates every route that references it. | T1 | 4 |
| SB-ROUTE-002 | Size and schedule tables | Stores nominal size, outside diameter, wall thickness and schedule or series per standard in unit-aware data files. Choosing a size updates the section of all segments and the clearance envelope of the route. | T1 | 4 |
| SB-ROUTE-003 | Standard dimensional tables | Ships tables for common metric and imperial tube and pipe series and accepts additional tables as documented data files. Each table records its source standard and edition. | T2 | 4 |
| SB-ROUTE-004 | Bend rules | Stores minimum bend radius as a multiple of outside diameter, the list of standard radii, bend angle limits, minimum straight length between bends and at ends, and a springback allowance. Rule violations are flagged on the route with the offending segment named. | T1 | 4 |
| SB-ROUTE-005 | Hose rules | Stores minimum bend radius, end-fitting allowances, maximum twist and a length model for flexible hose. The cut length is computed from the centerline plus fitting allowances. | T2 | 4 |
| SB-ROUTE-006 | End-type compatibility | Defines which end types may connect (butt weld, flange, thread, flare, compression, push-fit) and flags incompatible connections between a route and a port or fitting. | T2 | 4 |
| SB-ROUTE-007 | Material and contents attributes | Assigns material, density, working fluid and working pressure to a style or route. Mass properties of the assembly can include the contents of a route. | T2 | 4 |
| SB-ROUTE-008 | Style libraries | Stores styles, size tables and wire and cable types in library files that live locally or on an Arc Hub. The document records the library revision it used and reports differences when the library changes. | T2 | 4 |

## Ports and route creation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ROUTE-009 | Ports | Defines a port on a part as a position, axis, nominal size, end type and flow direction. A route end attached to a port follows the part when the part moves. | T1 | 4 |
| SB-ROUTE-010 | Route from a path | Creates a route from a selected 3D sketch path made of lines and arcs. Bends are inserted at corners according to the style and the route stays associative to the sketch. | T1 | 4 |
| SB-ROUTE-011 | Route between ports | Creates a route from one port to another through user-placed waypoints. Straight segments align to the port axes at both ends. | T1 | 4 |
| SB-ROUTE-012 | Auto-route | Computes candidate paths between two ports that avoid obstacle bodies and respect the style rules for bend count, minimum straight length and clearance. The command returns a ranked list of candidates, runs as a cancellable background task and applies only the candidate the user selects. | T2 | 4 |
| SB-ROUTE-013 | Routing guides | Accepts keep-out regions, preferred corridors, required waypoints and preferred directions as inputs to auto-route. Each guide is a named object that can be toggled without deleting it. | T2 | 4 |
| SB-ROUTE-014 | Orthogonal routing mode | Restricts a route to segments parallel to the coordinate axes or to selected reference directions. The mode is stored on the route and applied when vertices are edited. | T2 | 4 |
| SB-ROUTE-015 | Branches | Starts a branch from a point on an existing route and inserts a tee or branch fitting there. The branch regenerates when the parent route changes. | T1 | 4 |
| SB-ROUTE-016 | Route editing | Moves, adds and removes vertices and segments by direct manipulation or numeric entry, with a live preview of bends and rule violations. Each edit is one undoable command. | T1 | 4 |
| SB-ROUTE-017 | Route dimensions and constraints | Applies length, angle, offset, parallel, perpendicular and planar constraints to route segments, solved by the same solver as sketches. The route reports the degrees of freedom that remain. | T2 | 4 |
| SB-ROUTE-018 | Route associativity | Re-solves a route when the parts holding its ports move or change. When a route cannot be satisfied, it keeps its last valid shape, marks the failing constraint and never deletes geometry silently. | T1 | 4 |
| SB-ROUTE-019 | Persistent references | Stores route attachments, guides and constraints as persistent references to faces, edges and ports. References that cannot be resolved after a model edit are listed with a repair action. | T1 | 4 |
| SB-ROUTE-020 | Parametric routes | Drives route dimensions, tube size and style from named parameters and configurations. Changing a parameter regenerates the route, its fittings and its bill of materials. | T2 | 4 |
| SB-ROUTE-021 | Route copy, mirror and pattern | Copies, mirrors and patterns a route with its fittings and supports across an assembly, remapping ports to the destination geometry. Routes whose ports cannot be remapped are created detached and flagged. | T2 | 4 |
| SB-ROUTE-022 | Route templates | Saves a configured route with its style, fittings and supports as a reusable template that reattaches to matching ports in another assembly. | T3 | 4 |

## Fittings, runs and supports

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ROUTE-023 | Fitting library | Provides parametric elbows, tees, crosses, reducers, flanges, couplings, caps, unions, adapters and valves, each with ports, take-out distances and end types. Fittings are part of the standard parts library and are sized by nominal size and standard. | T1 | 4 |
| SB-ROUTE-024 | Automatic fitting placement | Places fittings at route corners, branches and size changes according to the style rules and replaces them when the style changes. Where several fittings qualify, the command lists the choices and applies the first by rule priority. | T1 | 4 |
| SB-ROUTE-025 | Fitting substitution | Swaps a placed fitting for another that matches size and end types without changing the route geometry. The swap updates take-out values, cut lengths and the bill of materials. | T2 | 4 |
| SB-ROUTE-026 | Run cut lengths | Computes the cut length of each straight piece from the centerline length minus the take-out distances of the fittings at both ends. The value updates when fittings or geometry change. | T1 | 4 |
| SB-ROUTE-027 | Joint allowances | Adds weld gaps, thread engagement or insertion depth to cut lengths per end type, defined in the style. Each allowance is shown in the cut list. | T2 | 4 |
| SB-ROUTE-028 | In-line components | Places valves, filters and sensors on a route with flow direction and rotational orientation, such as handle angle. Orientation is a parameter the user can constrain. | T2 | 4 |
| SB-ROUTE-029 | Hose assemblies | Models a hose as a centerline curve with end fittings and a bend-radius check, and reports the cut length and installed length. The hose follows its ports when parts move within the limits of the length model. | T2 | 4 |
| SB-ROUTE-030 | Supports and clamps | Places supports along a route at spacing set by rule for size and material, hosted on a selected structure. Spacing rules are stored in the style and violations are listed. | T2 | 4 |
| SB-ROUTE-031 | Insulation and coverings | Adds an insulation or covering thickness to a route that enlarges its clearance envelope and mass without changing the centerline. | T3 | 4 |
| SB-ROUTE-032 | Miter and cope joints | Generates miter angles and cope profiles for welded tube joints and produces the cut geometry on each piece. Results update when the joint angle changes. | T2 | 4 |

## Bend data, spools and checks

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ROUTE-033 | Bend data | Computes for each bend the straight length, bend angle, plane rotation and radius, and the end-point coordinates of the bent tube. Output is available as length-rotation-angle and coordinate lists. | T1 | 4 |
| SB-ROUTE-034 | Bend table object | Provides a bend table with configurable columns that appears in drawings and exports to CSV. Values follow the route and the document units. | T1 | 4 |
| SB-ROUTE-035 | Developed length | Computes the developed length of a bent tube from the bend radius and a bend deduction defined by the material. The value is shown with the cut length and checked against stock length. | T1 | 4 |
| SB-ROUTE-036 | Bender definition | Stores the tooling set of a tube bender: die radii, clamp length, springback table and travel limits. A bend that the selected tooling cannot form is flagged. | T3 | 4 |
| SB-ROUTE-037 | Bender program output | Writes a bender program in documented text formats from the bend data and the selected bender definition. Posting uses the same post-processor system as machining. | T3 | 5 |
| SB-ROUTE-038 | End treatments | Records flares, beads, swages and end cuts as route features with allowances that change cut length. | T3 | 4 |
| SB-ROUTE-039 | Spools | Divides a route into spools at flanges or marked field joints and assigns each an identifier. Each spool owns its cut list and weld list. | T2 | 4 |
| SB-ROUTE-040 | Weld and joint list | Lists every joint of a route with a number, type, size and spool membership. The list updates with the route and is available as a report. | T2 | 4 |
| SB-ROUTE-041 | Isometric route view | Creates a derived isometric drawing view of a route with symbols, flow arrows, lengths and a parts list. The view regenerates when the route changes. | T2 | 4 |
| SB-ROUTE-042 | Clearance and interference check | Measures minimum distance between a route, other bodies and other routes against a clearance defined per style. Violations list the pair and the location, and the check re-runs after relevant changes. | T1 | 4 |
| SB-ROUTE-043 | Route validity panel | Lists disconnected ports, size mismatches, incompatible end types, bend-rule violations and unresolved references for one route or for the whole assembly. Each entry selects the failing object. | T1 | 4 |

## Wires, cables and harness parts

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ROUTE-044 | Electrical part library | Stores connectors, terminals, splices, grommets and clips with pin tables, cavity positions, wire exit directions and mating references. Parts are placed in the assembly like any component. | T1 | 4 |
| SB-ROUTE-045 | Connector pin definition | Defines each pin with identifier, name, signal, cavity position and orientation, and optional terminal type. Wires reference the pin, not the geometry near it. | T1 | 4 |
| SB-ROUTE-046 | Wire library | Stores wire types with gauge, color and stripe, insulation diameter, minimum bend radius, mass per length and part number. Libraries are shared through local files or an Arc Hub. | T1 | 4 |
| SB-ROUTE-047 | Cable library | Stores multi-conductor cable types with conductor colors, jacket and shield definitions, outside diameter and minimum bend radius. Conductors of a placed cable map to pins one to one. | T2 | 4 |
| SB-ROUTE-048 | Ribbon cables | Defines a ribbon cable by conductor count and pitch with fold lines and connectors at the ends. Pin 1 orientation is tracked, and folds update the flattened length. | T2 | 4 |
| SB-ROUTE-049 | Wire creation | Creates a wire between two pins and assigns a wire type. The wire stays connected when connectors move and is routed through segments. | T1 | 4 |
| SB-ROUTE-050 | Cable creation | Creates a cable between connectors and assigns each conductor to a pin pair. Unassigned conductors are reported. | T2 | 4 |
| SB-ROUTE-051 | Connection import | Reads a wire list in CSV or XML and creates or updates wires by pin identifier. Rows that match no pin are listed and nothing is created for them. | T2 | 4 |
| SB-ROUTE-052 | Splices and junctions | Creates splice objects that join wires at a point on a segment, with the wire set and an identifier. Splices appear in the wire list and bill of materials. | T2 | 4 |

## Harness segments and routing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ROUTE-053 | Harness object | Groups wires, cables, segments and accessories into a named harness with part number and properties. An assembly can hold several harnesses. | T1 | 4 |
| SB-ROUTE-054 | Segments | Creates segments as three-dimensional centerline paths joined at branch points, with a diameter and a covering. Segments are created manually or from a sketch path. | T1 | 4 |
| SB-ROUTE-055 | Wire auto-routing | Routes each wire through the shortest path of the segment network, limited by segment fill. The user can override the path of any wire and the override persists. | T2 | 4 |
| SB-ROUTE-056 | Segment diameter | Computes the segment diameter from the bundled wires and cables using a packing factor and flags a segment whose fill exceeds its limit. The value updates when wires are added or removed. | T2 | 4 |
| SB-ROUTE-057 | Segment bend radius | Checks the segment centerline against the largest minimum bend radius of the wires it carries. Violations are shown on the segment. | T2 | 4 |
| SB-ROUTE-058 | Wire length | Computes each wire length along its path plus end allowances, service loops and a tolerance. Lengths update when segments or connectors move. | T1 | 4 |
| SB-ROUTE-059 | Coverings and bundles | Applies tape, sleeve, conduit, corrugated tube, twist and shield coverings over a range of a segment. Coverings change the diameter and enter the bill of materials by length. | T2 | 4 |
| SB-ROUTE-060 | Harness accessories | Places clips, ties and grommets along a segment at a rule-based spacing and attaches them to host geometry. | T2 | 4 |
| SB-ROUTE-061 | Moving harness check | Evaluates a harness at the limit positions of an assembly joint and reports the change in wire length and the minimum bend radius reached. | T3 | 5 |

## Formboard and reports

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ROUTE-062 | Formboard layout | Flattens a harness into a planar layout that keeps segment lengths and branch order. The user can reposition branches and the layout stays linked to the harness. | T1 | 4 |
| SB-ROUTE-063 | Formboard drawing | Produces a drawing from the layout with connector views, pin tables, tape and clip callouts and a title block. Full-scale output tiles across sheets and exports to PDF and DXF. | T1 | 4 |
| SB-ROUTE-064 | Wire list report | Produces a table of from, to, wire or cable, color, gauge, length and signal, sortable and exportable to CSV. The report regenerates when the harness changes. | T1 | 4 |
| SB-ROUTE-065 | Harness data exchange | Exports and imports harness data in standard harness exchange formats. Unmapped data is preserved and listed in a loss report. | T3 | 5 |

## Bills of materials and automation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ROUTE-066 | Routed-system bill of materials | Rolls fittings, tube by cut length, hose, supports, wires, cables, connectors and accessories into the assembly bill of materials, grouped by route, spool or harness. Quantities update with the model. | T1 | 4 |
| SB-ROUTE-067 | Linear stock cut list | Assigns cut pieces to stock lengths of tube, pipe or wire so that waste is minimized, and lists the cuts per stock length. Stock lengths and saw kerf are inputs. | T2 | 4 |
| SB-ROUTE-068 | Route annotations | Adds length, bend and tag annotations and parts lists to drawings. Annotations read from the route and update with it. | T1 | 4 |
| SB-ROUTE-069 | Service tags | Stores a line number, service and tag as properties on a route or harness for drawings and reports. | T3 | 4 |
| SB-ROUTE-070 | Routed-system commands | Exposes route creation, auto-route, style assignment and report generation as commands with typed parameters. The headless command-line runner can regenerate and export routed-system reports without opening a dialog. | T2 | 4 |
