# PCB layout

PCB layout turns a schematic into a manufacturable board that is also a mechanical object. The board outline, stack-up thickness, holes and keep-outs come from the mechanical model, footprints are placed and routed with interactive and automatic tools, copper pours fill the free area, and design rules are checked continuously. The board is shown in three dimensions with component models and is checked against the enclosure in real time. Layout uses the same document, command and drafting foundations as the rest of SolidBuilder. Exchange with other ECAD tools remains available through [electronics co-design](28-electronics-codesign.md).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Board | Outline, cutouts, thickness, layer stack, rigid and flex regions, origin | Linked to a schematic; solid derived for the assembly |
| Stack-up | Copper and dielectric layers, thickness, material, dielectric constant, finish | Belongs to a board; drives impedance and thickness |
| Footprint definition | Pads, silkscreen, courtyard, fabrication layer, 3D model reference, properties | Stored in a footprint library; placed as footprint instances |
| Footprint instance | Reference designator, side, position, rotation, locked flag, pad net assignment | Belongs to a board; matches a symbol instance |
| Pad | Number, shape, size, drill, layers, mask and paste settings | Belongs to a footprint; carries a net |
| Track | Net, layer, width, path | Belongs to a board |
| Via | Net, position, drill, diameter, layer span, type (through, blind, buried, micro), tenting | Belongs to a board |
| Zone | Net, layers, outline, fill style, clearance, thermal relief, priority | Belongs to a board; filled copper is derived |
| Rule set | Constraints by scope, priority, conditions | Applies to nets, classes, layers and areas |
| Differential pair | Nets, gap, width, target impedance, skew limit | Derived from the schematic; used by routing and checks |
| Length group | Nets, target length, tolerance | Used by tuning and checks |
| Ratsnest | Unrouted connections | Derived from the netlist and the routed copper |
| Panel | Board instances, rails, tabs, fiducials, tooling holes | Created from a board for fabrication |
| Violation | Rule, objects, location, severity | Derived by the rule check; waivable |

## Board and stack-up

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-PCB-001 | Board from mechanical model | Creates a board from a sketch profile, a face of a part or the interior of an enclosure, and keeps the outline linked, so that a mechanical edit moves the outline and lists affected footprints and tracks. | T3 | 5 |
| SB-PCB-002 | Stack-up editor | Defines copper and dielectric layers with thickness, material and dielectric constant, from presets or imported tables, and derives total thickness and the board solid. | T3 | 5 |
| SB-PCB-003 | Mechanical objects on the board | Holds mounting holes, cutouts, slots, keep-outs with height limits, connector positions and text as objects that stay linked to mechanical features and are exchanged through the board link. | T3 | 5 |
| SB-PCB-004 | Board setup rules | Sets default track widths, clearances, via styles, mask expansion, paste settings and minimum feature sizes for the board and for a fabrication class. | T3 | 5 |
| SB-PCB-005 | Rigid-flex regions | Defines rigid and flex regions on the stack-up with bend zones and radius limits, and checks that tracks and vias respect the zone. | T3 | 5 |
| SB-PCB-006 | Board coordinate and grid | Sets the origin, grids for placement and routing, and units, and aligns the board frame with the assembly. | T3 | 5 |
| SB-PCB-007 | Board layers | Manages copper, mask, paste, silkscreen, fabrication, courtyard and mechanical layers with color, visibility and order. | T3 | 5 |

## Footprints and libraries

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-PCB-008 | Footprint library | Stores footprints in libraries that are local files, embedded in the document or hosted on an Arc Hub, with a snapshot in each board so that a missing library never breaks a board. | T3 | 5 |
| SB-PCB-009 | Footprint editor | Draws pads, silkscreen, courtyards and fabrication outlines with arrays, alignment and dimension tools, and checks pad numbering against the symbol. | T3 | 5 |
| SB-PCB-010 | Footprint wizards | Generates footprints for regular packages from parameters, such as dual in-line, quad, grid array and chip packages, and keeps the parameters editable. | T3 | 5 |
| SB-PCB-011 | Component 3D models | Attaches a STEP or VRML model to a footprint with offset, rotation and scale, and uses the model for display, clearance checks and the assembly. | T3 | 5 |
| SB-PCB-012 | Generated bodies | Generates a package body from the courtyard and a height property when no model exists, and marks it as generated. | T3 | 5 |
| SB-PCB-013 | Footprint import | Reads footprint libraries of open ECAD tools and IPC-7351 style dimension tables with a report of unmapped content. | T3 | 5 |
| SB-PCB-014 | Footprint update | Compares placed footprints with their library versions and applies updates as a reviewed change set that keeps position and net assignment. | T3 | 5 |
| SB-PCB-015 | Pad stacks | Defines pad shapes per layer, custom polygon pads, thermal spokes, drilled and slotted holes, and mask and paste overrides. | T3 | 5 |

## Placement

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-PCB-016 | Forward annotation to board | Adds, removes and updates footprints and nets from the schematic as a reviewed change set, with each item accepted or rejected. | T3 | 5 |
| SB-PCB-017 | Interactive placement | Moves, rotates, flips and aligns footprints with snap to grid, to pads and to the outline, shows the ratsnest while moving and keeps locked footprints in place. | T3 | 5 |
| SB-PCB-018 | Placement by group | Moves and arranges footprints by schematic sheet, by hierarchy instance or by selection, and replicates a routed layout from one sheet instance to others. | T3 | 5 |
| SB-PCB-019 | Alignment and distribution | Aligns and distributes selected footprints along edges, centers and pad rows. | T3 | 5 |
| SB-PCB-020 | Placement from the model | Positions connectors, switches, indicators and mounting-sensitive parts from coordinates in the mechanical model and holds them there through ownership rules from [electronics co-design](28-electronics-codesign.md). | T3 | 5 |
| SB-PCB-021 | Placement rule areas | Defines areas where only some sides, heights or component classes may be placed, and reports violations. | T3 | 5 |
| SB-PCB-022 | Courtyard checks | Reports overlapping courtyards and parts outside the board outline or in a keep-out. | T3 | 5 |
| SB-PCB-023 | Placement optimization | Proposes an arrangement that reduces ratsnest length within the rules and locks, shown as a preview that is applied only on request. | T3 | 5 |

## Routing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-PCB-024 | Interactive routing | Draws tracks from a pad or track with live clearance, modes that walk around, push or stop at obstacles, and angle styles of 45 degrees, 90 degrees or free. | T3 | 5 |
| SB-PCB-025 | Layer change | Inserts a via of the net class style when the user changes layer during routing, with fanout of multiple tracks together. | T3 | 5 |
| SB-PCB-026 | Differential pair routing | Routes both nets of a pair together with the gap and width of the rule, with coupled turns, layer changes and uncoupled fanout at pads. | T3 | 5 |
| SB-PCB-027 | Multi-track routing | Routes a bundle of nets in parallel with a set pitch, including around corners and across layer changes. | T3 | 5 |
| SB-PCB-028 | Length tuning | Adds serpentine, trombone or accordion meanders to single nets and pairs to reach a target length or skew, within the clearance rules, and shows the current length against the target. | T3 | 5 |
| SB-PCB-029 | Length groups | Groups nets to match their lengths, including the length inside packages, and reports each member against the group target. | T3 | 5 |
| SB-PCB-030 | Track editing | Drags, shoves, merges, splits and re-routes tracks, changes width along a track, and removes loops and redundant corners. | T3 | 5 |
| SB-PCB-031 | Via types | Supports through, blind, buried and micro vias with layer spans, drill and annular ring limits and tenting, and checks span against the stack-up. | T3 | 5 |
| SB-PCB-032 | Via stitching and fencing | Places arrays of vias along a net or a track for shielding and return paths, with spacing and keep-away rules. | T3 | 5 |
| SB-PCB-033 | Teardrops and fillets | Adds teardrops at pad and via junctions and rounds track corners within the rules. | T3 | 5 |
| SB-PCB-034 | Curved and arc tracks | Routes arc tracks and converts polylines to arcs without violating clearance. | T3 | 5 |
| SB-PCB-035 | Net highlighting | Highlights a net, class or pair, dims all others and shows the ratsnest and connectivity state of the selection. | T3 | 5 |
| SB-PCB-036 | Routing from the model | Routes a net along a path drawn in the mechanical model, such as a flex fold line, and keeps the track linked to that path. | T3 | 5 |

## Copper pours

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-PCB-037 | Zones | Creates copper zones on one or several layers with a net, an outline, a priority and a fill style of solid or hatched. | T3 | 5 |
| SB-PCB-038 | Zone fill | Computes fills on worker threads from the document revision and tags the result with that revision. A stale fill is never used for outputs. | T3 | 5 |
| SB-PCB-039 | Thermal relief and connection | Connects pads to a zone through thermal spokes, solid connection or no connection, per pad and per net class, with spoke width and gap from the rules. | T3 | 5 |
| SB-PCB-040 | Keep-out areas | Defines areas that forbid copper, tracks, vias or footprints, separately per object kind and layer. | T3 | 5 |
| SB-PCB-041 | Island removal and minimum area | Removes zone islands and slivers below a set area and reports disconnected pours. | T3 | 5 |
| SB-PCB-042 | Zone rule areas | Applies different clearance and connection rules inside a drawn area of a zone, for example under a connector. | T3 | 5 |

## Design rules and checking

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-PCB-043 | Rule system | Defines rules by constraint, scope and priority, such as clearance, width, via size, annular ring, hole size, silk clearance and differential pair gap, with conditions on net, class, layer and area. | T3 | 5 |
| SB-PCB-044 | Rule check run | Checks the board against all rules and lists violations with rule, objects, location and severity. Selecting a violation selects and frames the objects in the board and the model. | T3 | 5 |
| SB-PCB-045 | Connectivity check | Reports unrouted nets, shorts between nets, dangling tracks and vias, and differences against the schematic netlist. | T3 | 5 |
| SB-PCB-046 | Manufacturing checks | Reports copper slivers, acid traps, mask slivers, silkscreen over pads, minimum drill and hole-to-hole distance, and edge clearance against a fabrication class. | T3 | 5 |
| SB-PCB-047 | Continuous checking | Evaluates rules while editing for the objects that change, and shows violations at once without a full run. | T3 | 5 |
| SB-PCB-048 | Violation waivers | Records a waiver with a reason and an author, keeps it in the journal and lists waived violations on request. | T3 | 5 |
| SB-PCB-049 | Rule presets | Stores and shares rule sets and fabrication classes as documented files, locally or on an Arc Hub. | T3 | 5 |
| SB-PCB-050 | Headless rule check | Runs the rule check from the command-line runner and as an Arc Hub job, with a machine-readable report. | T3 | 5 |

## Autorouting

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-PCB-051 | Autorouter exchange | Exports the routing problem, with outline, stack-up, rules, pads, locked tracks and net classes, in a documented format and imports the routed result as a reviewed change set. The autorouter runs as a separate process. | T3 | 5 |
| SB-PCB-052 | Routing scope and strategy | Limits the run to selected nets, classes or areas, keeps locked and existing tracks, and sets layer preferences, via cost and pass count. | T3 | 5 |
| SB-PCB-053 | Route cleanup | Optimizes a routed result by removing loops and corners, shortening tracks and aligning pairs, and then runs the rule check before the change set is offered. | T3 | 5 |

## Three-dimensional board and enclosure

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-PCB-054 | 3D board view | Shows the board in three dimensions with copper, mask, silkscreen, component models and holes, with layer visibility and section planes. | T3 | 5 |
| SB-PCB-055 | Board in assembly | Places the board solid and its component bodies in an assembly as instances grouped by board, derived from the layout and updated when the layout changes. | T3 | 5 |
| SB-PCB-056 | Real-time enclosure clearance | Measures the distance between components, copper and the enclosure bodies against a clearance set per category while parts move in either domain, and marks violations in the layout and the model. | T3 | 5 |
| SB-PCB-057 | Height and keep-out feedback | Shows component heights against the height limits of keep-outs and enclosure features and lists components that exceed them. | T3 | 5 |
| SB-PCB-058 | Mechanical fit actions | Moves a footprint or the outline within allowed bounds to resolve a clearance violation, as a previewed command. | T3 | 5 |
| SB-PCB-059 | Board export to the model | Exports the board as STEP and as a mesh from the layout directly, with the fidelity and loss report of [electronics co-design](28-electronics-codesign.md). | T3 | 5 |
| SB-PCB-060 | Fold and flex preview | Shows rigid-flex boards folded in the assembly and checks the bend radius and tracks in bend zones. | T3 | 5 |

## Panelization

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-PCB-061 | Panel layout | Arranges board instances in rows and columns with spacing, rotation and mixed boards, with rails, fiducials and tooling holes. | T3 | 5 |
| SB-PCB-062 | Board separation | Adds breakaway tabs with perforations, V-score lines or routed outlines, with the keep-out zones those need. | T3 | 5 |
| SB-PCB-063 | Panel rules and outputs | Checks the panel against fabricator limits and produces fabrication data for the panel as specified in [outputs](32-electronics-simulation-and-outputs.md). | T3 | 5 |
| SB-PCB-064 | Panel in the model | Shows the panel in three dimensions and as a drawing with dimensions, for fixtures and fabrication notes. | T3 | 5 |
