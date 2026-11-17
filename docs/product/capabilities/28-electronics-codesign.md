# Electronics co-design

Electronics co-design connects mechanical assemblies with boards designed in open ECAD tools. It exchanges board outlines, keep-outs, holes and component models, places three-dimensional boards in assemblies, synchronizes changes in both directions through reviewed change sets, checks enclosure clearance and links harness connectors to schematics. Schematics and boards designed inside SolidBuilder are specified in [schematic capture](30-schematic-capture.md), [PCB layout](31-pcb-layout.md) and [electronics simulation and outputs](32-electronics-simulation-and-outputs.md). This area keeps the exchange with open ECAD tools for teams whose boards are designed elsewhere, and the harness links to schematics.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| ECAD link | Source file or Hub item, format, last synchronized revision, identity map | Ties a board in the assembly to its ECAD source |
| Board | Outline, thickness, cutouts, holes, layer stack, rigid and flex regions | Lives in an assembly; linked to an ECAD source |
| Layer stack | Layers, materials, thicknesses, total thickness | Belongs to a board |
| Board hole | Position, diameter, plated flag, role (mounting, via, tooling) | Belongs to a board; may map to a mechanical feature |
| Keep-out region | Footprint, minimum and maximum height, scope | Belongs to a board; exchanged both ways |
| Component placement | Reference designator, value, footprint, side, position, rotation, model | Belongs to a board; carries properties for the bill of materials |
| Component model | Package body, origin and alignment, footprint mapping | Stored in a component library |
| Change set | Items (added, moved, removed, modified), origin side, status | Created by pull or push; accepted or rejected item by item |
| Ownership rule | Attribute group, owning side | Resolves which side may change an attribute |
| Clearance rule | Object categories, minimum distance | Evaluated between boards, components and enclosure bodies |
| Connector mapping | Board connector footprint, harness connector, pin map | Links boards and harnesses |
| Schematic link | Source schematic or netlist, reference designators | Supplies connections for harness checks |

## Exchange formats and libraries

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ECAD-001 | ECAD link | Records the source file or Hub item, format, last synchronized revision and identity map of a board in an assembly. All pull and push commands start from this link. | T1 | 4 |
| SB-ECAD-002 | Board import from STEP | Reads a board assembly from STEP and recognizes the board body, component bodies and reference designators from names and properties. Unrecognized bodies are kept as plain bodies. | T1 | 4 |
| SB-ECAD-003 | Board import from IDF and incremental exchange | Reads IDF board and library files and the incremental board exchange schema, creating outline, holes, keep-outs and placements. | T1 | 4 |
| SB-ECAD-004 | Open ECAD native board import | Reads KiCad board files directly for outline, holes, layer stack and footprint placement, with model references resolved from the component library. | T2 | 4 |
| SB-ECAD-005 | IPC-2581 import | Reads outline, stack-up, drill data and component placement from IPC-2581. | T2 | 4 |
| SB-ECAD-006 | Fabrication data import | Reads Gerber and drill files to display copper, mask and silkscreen on the board model and to extract the outline. | T3 | 4 |
| SB-ECAD-007 | Outline from 2D geometry | Creates a board outline from DXF, SVG or a sketch profile. | T2 | 4 |
| SB-ECAD-008 | Mechanical constraints export | Exports outline, cutouts, mounting holes, keep-outs with height limits and connector positions as an exchange file for the ECAD tool. | T1 | 4 |
| SB-ECAD-009 | Board model export | Exports the populated board as STEP and as a mesh for use in other tools. | T1 | 4 |
| SB-ECAD-010 | Component model library | Stores three-dimensional component models as STEP or VRML with origin, alignment and footprint mapping, locally or on an Arc Hub. | T2 | 4 |
| SB-ECAD-011 | Fidelity and loss report | States the fidelity level of each imported format and lists data that was kept as foreign data or not mapped. | T1 | 4 |

## Board and component objects

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ECAD-012 | Board object | Creates a board solid from the outline and layer stack thickness, with cutouts, holes and surface appearance for mask and silkscreen. | T1 | 4 |
| SB-ECAD-013 | Layer stack | Stores layers with material and thickness and derives the total board thickness, with an optional thin copper layer display. | T2 | 4 |
| SB-ECAD-014 | Rigid-flex boards | Describes rigid and flex regions with bend zones and bend radius, and folds the board in three dimensions along the bend zones. | T3 | 4 |
| SB-ECAD-015 | Component placements | Places components with reference designator, value, footprint, side, position and rotation as instances in the assembly, grouped by board. | T1 | 4 |
| SB-ECAD-016 | Generated component bodies | Generates simple package bodies from footprint dimensions when no model exists, and marks them as generated. | T2 | 4 |
| SB-ECAD-017 | Component properties | Carries reference designator, value, manufacturer part number and do-not-fit state as properties for the bill of materials. | T1 | 4 |
| SB-ECAD-018 | Assembly variants | Shows the board with fitted or not-fitted components for a chosen variant read from the ECAD source. | T3 | 4 |
| SB-ECAD-019 | Mounting holes and standoffs | Maps mounting holes to mechanical features and places standoffs and fasteners that mate to them. | T1 | 4 |
| SB-ECAD-020 | Connector and port placement | Identifies connectors and their mating direction and positions panel cutouts and openings against them. | T2 | 4 |
| SB-ECAD-021 | Mechanical parts on boards | Tracks heat sinks, brackets and shields that attach to the board as board components with their own identity. | T2 | 4 |
| SB-ECAD-022 | Board sketch outline | Defines the board outline in a constrained sketch so that the exported outline follows mechanical dimensions. | T1 | 4 |

## Change synchronization

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ECAD-023 | Pull from ECAD | Reads the current ECAD source and lists changes in outline, holes, keep-outs and component placement against the last synchronized state. Accepted items apply in one transaction. | T1 | 4 |
| SB-ECAD-024 | Push to ECAD | Writes mechanical changes to outline, mounting holes, keep-outs and connector positions as a proposal file that the ECAD tool can review and accept. | T2 | 4 |
| SB-ECAD-025 | Change set review | Records each proposal with its origin and status, applies accepted items and keeps rejected items on record. No change from either side overwrites the other silently. | T1 | 4 |
| SB-ECAD-026 | Identity mapping | Matches components and holes between the assembly and the ECAD source by identifier, then by reference designator, and tracks renames. | T1 | 4 |
| SB-ECAD-027 | Ownership rules | Assigns each attribute group to one side, such as outline to mechanical and placement to electrical, and reports a conflict when the other side changes it. | T1 | 4 |
| SB-ECAD-028 | Source change detection | Detects a changed ECAD file or Hub version and flags the board as out of date. | T2 | 4 |
| SB-ECAD-029 | Synchronization history | Stores each synchronized state, allows rolling back an import and compares any two board revisions in three dimensions. | T2 | 4 |
| SB-ECAD-030 | Hub linking | Links the board to a Hub item and a version so that the assembly follows released versions. | T3 | 4 |

## Fit and clearance checks

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ECAD-031 | Enclosure clearance check | Measures the minimum distance between board, components and enclosure bodies against a clearance set per object category, and lists violations with locations. The check updates after each synchronization. | T1 | 4 |
| SB-ECAD-032 | Keep-out violation check | Lists components that enter a mechanical keep-out or exceed a height limit. | T1 | 4 |
| SB-ECAD-033 | Connector alignment check | Compares connector axis and position with the matching opening or mating part and reports offsets. | T2 | 4 |
| SB-ECAD-034 | Mating envelope check | Checks that the volume needed to insert or remove a connector or cable is free of other bodies. | T2 | 4 |
| SB-ECAD-035 | Board height map | Displays component heights over the board and cuts sections through the populated board. | T2 | 4 |
| SB-ECAD-036 | Thermal hand-off | Passes component power dissipation and board data to the electronics cooling study of the simulation area. | T3 | 5 |

## Harness links to schematics

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ECAD-037 | Schematic link | Reads connections from a netlist or schematic export in CSV or XML, or from a schematic of the same document ([schematic capture](30-schematic-capture.md)), and ties harness connectors to reference designators and pin names. | T2 | 4 |
| SB-ECAD-038 | Connector pin mapping | Maps board connector pins to harness connector pins and checks pin counts and keying for compatibility. | T2 | 4 |
| SB-ECAD-039 | Connection check | Reports connections present in the schematic but not in the harness and the reverse, and unconnected pins. | T2 | 4 |
| SB-ECAD-040 | Electronic bill of materials merge | Merges board component lines with the mechanical bill of materials by part number. | T2 | 4 |
| SB-ECAD-041 | Headless board commands | Provides import, pull, compare and check commands in the command-line runner without dialogs. | T2 | 4 |
| SB-ECAD-042 | Board appearance from fabrication data | Applies copper, mask and silkscreen textures from fabrication data to the board for review and rendering. | T3 | 4 |
