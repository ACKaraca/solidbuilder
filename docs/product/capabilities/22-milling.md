# Milling

Milling covers hole recognition and drilling, two-dimensional and three-dimensional strategies, multi-axis machining, linking and lead rules, and toolpath editing and trimming. Every strategy is an operation in a setup (see the manufacturing setup area) and produces a toolpath that regenerates when the design, the stock or a parameter changes.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Milling operation | Strategy, tool, geometry selection, heights, passes, tolerances | Belongs to a setup; produces a toolpath |
| Geometry selection | Faces, edge chains, sketch curves, bodies, regions, containment mode | Persistent references into the manufacturing model |
| Height set | Clearance, retract, feed, top, bottom, each with a reference and offset | Owned by an operation |
| Pass definition | Stepover, stepdown, finishing passes, spring passes, stock to leave | Owned by an operation |
| Lead and link rule | Entry type, exit type, ramp, transition mode, retract policy | Owned by an operation |
| Hole group | Hole type, diameter, depth, axis, thread data, access direction | Derived from the manufacturing model; mapped to drilling operations |
| Hole template | Mapping from hole type and size to an operation sequence and tools | Applied to hole groups |
| Tool orientation | Mode, lead and tilt angles, target geometry, limits | Reusable object referenced by multi-axis operations |
| Trim region | Boundary, mode (inside, outside), limits | Owned by an operation; applied to the toolpath |
| Toolpath edit | Selected moves, action, matching key | Overlay stored on an operation |

## Common operation parameters

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MILL-001 | Geometry selection | Selects faces, edge chains, sketch curves, bodies or regions for an operation, with a chain tolerance. Selections are stored as persistent references and report when they can no longer be resolved. | T1 | 5 |
| SB-MILL-002 | Height set | Defines clearance, retract, feed, top and bottom heights from stock, model, a selected feature or an offset. Heights update when the referenced geometry changes. | T1 | 5 |
| SB-MILL-003 | Tolerance and smoothing | Sets machining tolerance, smoothing and the arc-fit tolerance of the output. Lower tolerances increase computing time and the program length, and both are reported. | T1 | 5 |
| SB-MILL-004 | Stock to leave | Sets radial and axial stock to leave per operation, with optional values per selected face. | T2 | 5 |
| SB-MILL-005 | Cutter compensation | Computes tool-center paths or outputs controller compensation with entry and exit moves that suit the chosen mode. Wear compensation uses the offset registers of the tool. | T1 | 5 |
| SB-MILL-006 | Multiple depths | Cuts in stepdowns of a fixed or maximum size with an optional finishing pass and spring passes. The final depth can be machined in one pass. | T1 | 5 |
| SB-MILL-007 | Feeds by move type | Sets separate feeds for lead-in, lead-out, ramp, plunge and transition moves, and the coolant and spindle mode per operation. | T1 | 5 |
| SB-MILL-008 | Containment and avoid regions | Limits cutting by a boundary, a containment mode of tool inside, on or outside the boundary, and avoid regions that the tool does not enter. | T2 | 5 |

## Hole recognition and drilling

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MILL-009 | Hole recognition | Detects cylindrical and conical holes, including blind, through, stepped, counterbored, countersunk, tapered and threaded holes, and groups them by diameter, depth, axis and access direction. Groups update when the design changes. | T1 | 5 |
| SB-MILL-010 | Hole templates | Maps hole type and size ranges to a sequence of operations and tools, such as spot, drill and tap, in an editable table. Applying a template creates the operations for the selected groups. | T2 | 5 |
| SB-MILL-011 | Drilling cycles | Provides spot, drill, peck, partial peck, deep-hole, tap, bore, ream, counterbore, countersink and dwell cycles. Output uses the canned cycle of the controller profile or expanded moves where the controller lacks it. | T1 | 5 |
| SB-MILL-012 | Drill depth control | Sets depth to the tool tip or to full diameter, adds a break-through allowance and takes the depth from the recognized hole bottom, including cone angle. | T1 | 5 |
| SB-MILL-013 | Thread data link | Reads thread designation and class from the hole feature and selects the tap drill and tap by lookup tables. A mismatch between thread and tool is flagged. | T2 | 5 |
| SB-MILL-014 | Hole ordering | Orders holes by coordinate, nearest neighbor or shortest path within groups of the same tool. | T2 | 5 |
| SB-MILL-015 | Multi-axis drilling | Orients the tool along the axis of each hole in four-axis and five-axis setups, chooses tilt solutions that avoid collisions and reports holes that cannot be reached. | T2 | 5 |

## Two-dimensional strategies

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MILL-016 | Face milling | Machines a planar surface across the stock in zigzag, one-way or spiral patterns with a stepover given as a percentage of the tool diameter. A finishing pass and a cut-to-boundary mode are available. | T1 | 5 |
| SB-MILL-017 | Contour | Cuts along open or closed chains on, inside or outside the line with multiple stepdowns, finishing stepovers, tabs and leads. | T1 | 5 |
| SB-MILL-018 | Pocket | Clears closed regions with islands using offset or parallel patterns, in climb or conventional direction, then finishes walls and floor. | T1 | 5 |
| SB-MILL-019 | Constant-engagement clearing | Clears regions with a limit on tool engagement angle using arcs that keep chip load steady. The tool stays down between neighboring passes within a distance limit and the path reports its maximum engagement. | T2 | 5 |
| SB-MILL-020 | Slot | Cuts slots along a center path of lines and arcs with stepdown and ramp, with width taken from the tool or from the geometry. | T2 | 5 |
| SB-MILL-021 | Trace | Moves the tool center along a selected curve at a set depth, in two or three dimensions, for scribing and light cutting. | T2 | 5 |
| SB-MILL-022 | Thread milling | Cuts internal and external threads by helical interpolation with single-point or multi-form tools, with pitch, hand and pass settings taken from the recognized hole or entered. | T2 | 5 |
| SB-MILL-023 | Bore milling | Finishes circular holes with helical entry and orbiting passes and a finishing allowance. | T2 | 5 |
| SB-MILL-024 | Circular pocket and boss | Machines circular pockets and bosses by spiral or helical entry from a selected circular edge or point. | T2 | 5 |
| SB-MILL-025 | Engraving | Engraves text and graphics at constant depth, or at a depth set by the width of the shape for V-shaped tools. | T2 | 5 |
| SB-MILL-026 | Chamfer | Cuts chamfers along selected chains with a chamfer tool, set by width and tip offset. | T1 | 5 |
| SB-MILL-027 | Corner rounding | Rounds sharp corners of two-dimensional paths with a radius to keep feed constant, within the tolerance. | T2 | 5 |
| SB-MILL-028 | Two-dimensional rest machining | Cuts the material left by a previous tool or operation, taken from the simulated stock or from a reference tool diameter. | T2 | 5 |

## Three-dimensional strategies

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MILL-029 | Constant-engagement roughing | Roughs a three-dimensional shape from a stock model with a limit on tool engagement, optimal load and minimal retracts. Air cutting is reported as a percentage of the path. | T2 | 5 |
| SB-MILL-030 | Pocket clearing | Clears material in Z-level layers with offset passes and islands, aware of rest stock from earlier operations. | T1 | 5 |
| SB-MILL-031 | Parallel finishing | Finishes surfaces with raster passes at a chosen angle in zigzag or one-way order, limited by slope and contact area. | T1 | 5 |
| SB-MILL-032 | Contour finishing | Finishes steep surfaces with constant-Z contours with an option to join them in a continuous spiral. | T1 | 5 |
| SB-MILL-033 | Constant-scallop finishing | Finishes surfaces with offset passes that hold a constant scallop height across steep and shallow regions. | T2 | 5 |
| SB-MILL-034 | Spiral and radial finishing | Finishes round shapes with passes that spiral outward from a center or run radially from it. | T2 | 5 |
| SB-MILL-035 | Pencil finishing | Traces concave corners and fillets that the preceding tool cannot reach, with optional offset passes on each side. | T1 | 5 |
| SB-MILL-036 | Steep-and-shallow finishing | Finishes steep regions with contours and shallow regions with scallop or parallel passes, split at a threshold angle that blends the two. | T2 | 5 |
| SB-MILL-037 | Flowline finishing | Finishes selected surfaces along the flow of their surface parameters with a constant step between passes. | T2 | 5 |
| SB-MILL-038 | Blend between curves | Generates passes that interpolate between two boundary curves for surfaces without clean parameter flow. | T3 | 5 |
| SB-MILL-039 | Flat-area finishing | Detects flat areas and finishes each at constant Z with a stepover and a leave-at-edge option. | T2 | 5 |
| SB-MILL-040 | Projected finishing | Projects planar curves or patterns onto selected surfaces to cut engraving and decoration paths. | T2 | 5 |
| SB-MILL-041 | Three-dimensional rest machining | Cuts material left after larger tools, using a simulated stock or a reference tool, with limits on the minimum region size. | T1 | 5 |
| SB-MILL-042 | Slope and contact limits | Restricts finishing to a range of surface slopes and tool contact regions, and includes or excludes selected surfaces. | T2 | 5 |
| SB-MILL-043 | Mesh model machining | Accepts mesh bodies as model or stock for roughing and finishing strategies and reports the tolerance of the mesh. | T3 | 5 |

## Multi-axis machining

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MILL-044 | Tool orientation control | Controls the tool axis by a fixed vector, lead and tilt angles, a point, a curve, an axis or interpolation between orientations, with angle limits. Orientation definitions are stored as reusable objects. | T2 | 5 |
| SB-MILL-045 | Flank milling | Cuts ruled or nearly ruled walls with the side of the tool, fitting the tool axis between two guide curves and reporting overcut. | T2 | 5 |
| SB-MILL-046 | Multi-axis contour | Follows curves or edges with controlled tool axis for trimming and chamfering in five axes. | T2 | 5 |
| SB-MILL-047 | Multi-axis clearing | Roughs with the tool tilted to reach undercut regions from a stock model, with collision checks against the model and holder. | T3 | 5 |
| SB-MILL-048 | Multi-axis finishing | Finishes surfaces with parallel or flow passes and controlled contact point and axis. | T2 | 5 |
| SB-MILL-049 | Rotary machining | Machines wrapped geometry with a rotary axis, outputting either rotary axis moves or cylindrical interpolation as the controller profile allows. | T2 | 5 |
| SB-MILL-050 | Indexed positioning | Machines at fixed tilted orientations and outputs each plane definition. An automatic mode proposes the orientations that reach selected features. | T2 | 5 |
| SB-MILL-051 | Automatic collision avoidance | Changes tool tilt, shortens stick-out or lifts the tool to avoid the model, fixtures and stock as the user policy selects, and reports every change. | T2 | 5 |
| SB-MILL-052 | Multi-axis linking | Controls rotary repositioning and retraction between passes, including retract to a box around the part and avoiding axis flips. | T2 | 5 |
| SB-MILL-053 | Axis limit handling | Selects between equivalent tilt solutions to stay within machine limits and keep away from singular positions. | T3 | 5 |
| SB-MILL-054 | Accessibility analysis | Shows which tool and orientation combinations reach each region of the model with a color map. | T3 | 5 |

## Linking, leads and ordering

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MILL-055 | Lead-in and lead-out | Provides perpendicular, tangent arc, line, helical, ramp, plunge and vertical arc entries and exits with sizes set per end. | T1 | 5 |
| SB-MILL-056 | Entry moves | Sets ramp and helix angle, diameter and maximum length, a preferred entry position and use of a predrilled hole. | T1 | 5 |
| SB-MILL-057 | Transition moves | Links passes by direct move, move at a clearance height, move following the surface or a stay-down link up to a distance limit. Each transition is checked for gouges. | T1 | 5 |
| SB-MILL-058 | Retraction policy | Sets retraction to safe, minimal or operation-defined heights and applies the policy to every pass. | T1 | 5 |
| SB-MILL-059 | Cut order and direction | Orders regions to shorten rapid moves, and applies climb or conventional milling per operation. | T2 | 5 |
| SB-MILL-060 | Tabs and holding material | Adds tabs of a set size and count to contours, either automatically spaced or placed by the user. | T2 | 5 |

## Toolpath editing and trimming

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MILL-061 | Toolpath trimming | Trims a toolpath by a boundary, tool containment, stock limits, height limits or regions the user selects. The trim is stored as a parametric definition on the operation. | T2 | 5 |
| SB-MILL-062 | Toolpath editing | Deletes, splits, relinks and reorders selected moves as commands recorded on the operation. Edits replay after regeneration where moves match, and unmatched edits are flagged. | T3 | 5 |
| SB-MILL-063 | Toolpath transform | Translates, rotates, mirrors and patterns the toolpath of an operation about a work coordinate system. | T2 | 5 |
| SB-MILL-064 | Toolpath preview | Shows the toolpath with the tool and holder, colored by move type, with stepping and a link to the matching line of generated code. | T1 | 5 |
| SB-MILL-065 | Cut quality display | Shows scallop height and cusp analysis after finishing operations as a color map on the model. | T3 | 5 |
| SB-MILL-066 | Output optimization | Fits arcs and smooth curves to dense point sequences within the output tolerance to shorten programs for controllers that accept them. | T3 | 5 |
