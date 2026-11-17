# Schematic capture

Schematic capture defines the electrical design of a board inside the same document as the enclosure that houses it. A schematic is a set of sheets drawn with the suite's shared drafting engine, built from library symbols that carry pins, nets, buses and properties. Connectivity, reference designators, variants, the bill of materials and the netlist are derived from the sheets, checked by electrical rules and linked to the board layout and the three-dimensional model. Every edit is a command inside a transaction, so schematics undo, branch, merge and script like any other SolidBuilder object. Exchange with other ECAD tools stays available through [electronics co-design](28-electronics-codesign.md).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Schematic | Name, sheet set, root sheet, variants, design rules, net classes | Lives in a document beside the assemblies; linked to one or more boards |
| Sheet | Size, format, title block fields, sheet number, annotation IDs | Belongs to a schematic; a hierarchical sheet may instantiate another sheet |
| Sheet symbol | Referenced sheet, sheet pins, instance count | Placed on a parent sheet; defines a hierarchical instance |
| Symbol definition | Name, graphics, pins, units, properties, default footprint | Stored in a symbol library; placed as symbol instances |
| Symbol instance | Reference designator, value, unit, position, rotation, mirror, properties | Belongs to a sheet; resolves to a symbol definition |
| Pin | Number, name, electrical type, position, hidden flag | Belongs to a symbol definition; connects to nets |
| Net | Name, class, nodes, scope (local, global, hierarchical) | Derived from wires, labels and pins; owns net properties |
| Bus | Name, member nets, alias | Groups nets for drawing and naming |
| Wire, junction, label, port | Geometry, name, direction | Primary connectivity elements on a sheet |
| Power symbol | Net name, style | Declares a global net such as a supply or ground |
| Net class | Name, member nets, rule values | Carries routing and clearance intent to the board |
| Variant | Name, fitted and not-fitted overrides, alternate values | Belongs to a schematic; selects the content of the bill of materials and outputs |
| Design block | Name, sheet content, ports, parameters | Reusable circuit stored in a library and placed as a hierarchical sheet or copied |
| Electrical rule | Check, severity, scope, waivers | Evaluated on the derived connectivity |
| Netlist | Nodes, nets, component attributes, source revision | Derived; consumed by layout, simulation and outputs |

## Sheets and hierarchy

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SCH-001 | Schematic object | Creates a schematic as an object of the document with its own sheet set, net classes, variants and rules. A document holds any number of schematics, and a schematic links to the boards it defines. | T3 | 5 |
| SB-SCH-002 | Sheets | Adds, orders, renames and removes sheets with a size, a format and title block fields that read document and schematic properties. | T3 | 5 |
| SB-SCH-003 | Shared drafting engine | Draws sheets with the suite's drafting engine, so lines, text, grips, snaps, layers, units and plotting behave as in drawings. Selection, copy, move, rotate and mirror work on any mix of elements. | T3 | 5 |
| SB-SCH-004 | Hierarchical sheets | Places a sheet symbol that stands for another sheet, with sheet pins matching the ports of the child sheet. Pin and port mismatches are reported on both sides. | T3 | 5 |
| SB-SCH-005 | Multiple instances | Instantiates one child sheet several times. Each instance has its own reference designators, while the circuit is edited once. | T3 | 5 |
| SB-SCH-006 | Flat and hierarchical views | Shows the design as one sheet list or as a tree, and opens a child sheet from its symbol and returns to the parent. | T3 | 5 |
| SB-SCH-007 | Sheet navigator | Lists sheets, nets, symbols and ports with search and filter, and selects the matching elements on the sheet. | T3 | 5 |
| SB-SCH-008 | Title blocks and revision fields | Fills title blocks from properties and tracks the revision of each sheet from the document version. | T3 | 5 |
| SB-SCH-009 | Sheet output | Plots sheets to PDF and SVG at a chosen scale, with layers, hierarchy bookmarks and net-name hyperlinks. | T3 | 5 |

## Symbols and libraries

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SCH-010 | Symbol placement | Places symbols from a library browser with preview, search by name, property and keyword, and repeated placement. The next reference designator is assigned on placement. | T3 | 5 |
| SB-SCH-011 | Multi-unit symbols | Supports symbols made of several units, such as the gates of a logic package or the sections of a connector, with a power unit, alternate graphical forms and per-unit placement. | T3 | 5 |
| SB-SCH-012 | Symbol editor | Draws symbol graphics, places pins with number, name, electrical type and length, and defines properties and a default footprint. Pin tables edit many pins at once. | T3 | 5 |
| SB-SCH-013 | Symbol library | Stores symbols in libraries that are local files, embedded in the document or hosted on an Arc Hub. Each placed symbol keeps a snapshot, so a missing library never breaks a schematic. | T3 | 5 |
| SB-SCH-014 | Library update check | Compares placed symbols with their library definitions and lists differences in graphics, pins and properties. The user applies updates one symbol or all at once. | T3 | 5 |
| SB-SCH-015 | Derived symbols | Defines a symbol as an extension of another symbol, sharing graphics and pins while overriding properties, so that families of parts stay consistent. | T3 | 5 |
| SB-SCH-016 | Symbol import | Reads symbol libraries of open ECAD tools with a report of graphics, pins and properties that were not mapped. | T3 | 5 |
| SB-SCH-017 | Symbol generation | Creates a symbol from a pin table in CSV, with grouping, side assignment and unit splitting rules. | T3 | 5 |

## Nets, buses, power and ground

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SCH-018 | Wires and junctions | Draws wires orthogonally or at any angle, creates junctions at connections and splits or merges wires when elements move. Crossing wires connect only at a junction. | T3 | 5 |
| SB-SCH-019 | Net labels | Names nets with local, global and hierarchical labels. Two labels with the same name in one scope join their nets. | T3 | 5 |
| SB-SCH-020 | Net naming | Names unlabeled nets from a stable rule based on a pin, so that names remain the same after unrelated edits. | T3 | 5 |
| SB-SCH-021 | Buses | Defines buses with indexed and grouped members, bus aliases and bus entries that tap a member net. Members expand into individual nets in the netlist. | T3 | 5 |
| SB-SCH-022 | Power and ground symbols | Declares global nets through power symbols, with style variants for supplies and grounds. Pins marked as power inputs join their supply net implicitly where the symbol allows it. | T3 | 5 |
| SB-SCH-023 | No-connect markers | Marks pins that are intentionally unconnected. A marked pin that is connected is reported by the rule check. | T3 | 5 |
| SB-SCH-024 | Net classes and properties | Assigns nets to classes with clearance, track width, via style and differential pair intent, and attaches properties such as voltage, current and impedance target to a net. | T3 | 5 |
| SB-SCH-025 | Differential pairs | Declares differential pairs by name suffix or explicit assignment and carries the pairing to layout and checks. | T3 | 5 |
| SB-SCH-026 | Net ties and net merge | Joins two nets deliberately, such as analog and digital ground, through a net tie that stays visible on the sheet and in layout. | T3 | 5 |
| SB-SCH-027 | Directives and notes | Places text notes, graphical annotations, simulation directives and layout directives on a sheet as objects that tools can read. | T3 | 5 |

## Annotation and reference designators

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SCH-028 | Automatic annotation | Assigns reference designators by prefix from component type, in a chosen order across sheets, with a starting number per sheet or per hierarchy level. The result is deterministic for the same input. | T3 | 5 |
| SB-SCH-029 | Annotation lock | Locks designators so that annotation never changes them, and reports duplicates and unannotated symbols. | T3 | 5 |
| SB-SCH-030 | Back annotation | Applies designator changes made in layout to the schematic as a reviewed change set. | T3 | 5 |
| SB-SCH-031 | Designator rules | Defines prefixes per component class and a format for multi-unit and hierarchical designators, and exports the rules for reuse. | T3 | 5 |

## Electrical rule checking

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SCH-032 | Rule check run | Checks connectivity and pin types and lists findings with severity, sheet and location. Selecting a finding selects the elements on the sheet. | T3 | 5 |
| SB-SCH-033 | Pin type matrix | Defines which pin type pairs are errors, warnings or allowed, such as two outputs on one net or an unpowered input, as an editable table stored with the schematic. | T3 | 5 |
| SB-SCH-034 | Connectivity checks | Reports dangling wires, unconnected pins, single-pin nets, duplicate designators, labels without a partner and ports without a match across hierarchy. | T3 | 5 |
| SB-SCH-035 | Power checks | Reports power inputs without a driving power output or power flag and supply nets that connect to unexpected pins. | T3 | 5 |
| SB-SCH-036 | Waivers | Records a waiver for a finding with a reason and an author, keeps it in the journal and shows waived findings on request. | T3 | 5 |
| SB-SCH-037 | Library and footprint checks | Reports symbols without a footprint, footprints whose pad numbers do not match the symbol pins, and symbols that differ from their library. | T3 | 5 |

## Variants and bill of materials

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SCH-038 | Assembly variants | Defines variants that mark components as fitted or not fitted and substitute values or part numbers, and previews a variant on the sheets with not-fitted symbols marked. | T3 | 5 |
| SB-SCH-039 | Component properties | Carries manufacturer, part number, value, tolerance, package, supplier fields and notes as properties of each symbol, with a property table for bulk editing. | T3 | 5 |
| SB-SCH-040 | Electronics bill of materials | Derives a grouped bill of materials from the symbols for a chosen variant, with quantity, designators and any property column, and exports it as CSV or XLSX. | T3 | 5 |
| SB-SCH-041 | Bill of materials merge | Merges the electronics bill of materials into the mechanical bill of materials by part number, as specified in [properties and bills of materials](11-properties-and-bom.md) and [electronics co-design](28-electronics-codesign.md). | T3 | 5 |
| SB-SCH-042 | Variant comparison | Lists the differences between two variants or two revisions of the bill of materials. | T3 | 5 |

## Netlist and design blocks

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SCH-043 | Netlist | Derives a netlist from the sheets with nets, nodes and component attributes, tagged with the document revision. A stale netlist is never used by layout or simulation. | T3 | 5 |
| SB-SCH-044 | Netlist export | Exports the netlist as XML or CSV for other tools and as IPC-D-356 test data, as specified in [outputs](32-electronics-simulation-and-outputs.md). | T3 | 5 |
| SB-SCH-045 | Netlist import | Reads a netlist from an open format to create or compare a schematic, and reports components and nets that exist on only one side. | T3 | 5 |
| SB-SCH-046 | Design blocks | Saves a selection of symbols, wires and sheets as a design block with ports and parameters, and places it again as a copy or as a linked block. | T3 | 5 |
| SB-SCH-047 | Block library | Stores design blocks in a library with preview and properties, locally or on an Arc Hub, and lists where each block is used. | T3 | 5 |
| SB-SCH-048 | Linked block update | Compares a placed linked block with its library version and applies updates as a reviewed change set that keeps designators. | T3 | 5 |

## Cross-probing and links

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SCH-049 | Cross-probe to board | Selecting a symbol, net or pin on a sheet selects and frames the matching footprint, track or pad in the board layout, and the reverse. | T3 | 5 |
| SB-SCH-050 | Cross-probe to 3D model | Selecting a symbol or net highlights the placed component or connector in the three-dimensional board and in the assembly, and selecting a component in the model selects its symbol. | T3 | 5 |
| SB-SCH-051 | Forward annotation | Sends added, removed and changed components and nets to the board as a reviewed change set. Accepted items apply in one transaction; rejected items stay on record. | T3 | 5 |
| SB-SCH-052 | Harness connection | Supplies connector pins and nets to the harness area so that wiring checks read the schematic directly instead of through a netlist file. See [electronics co-design](28-electronics-codesign.md). | T3 | 5 |
| SB-SCH-053 | Schematic comparison | Compares two revisions of a schematic by components, nets and properties and shows the changes on the sheets. | T3 | 5 |
| SB-SCH-054 | Headless schematic commands | Provides annotation, rule check, netlist, bill of materials and output commands in the command-line runner and as Arc Hub jobs, without dialogs. | T3 | 5 |
