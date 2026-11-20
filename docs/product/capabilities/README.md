# Capability index

The capability specification of SolidBuilder: one file per functional area, each row a testable
behavior with a permanent identifier.

## Legend

| Item | Meaning |
|---|---|
| Identifier | `SB-<AREA>-<NNN>`: product prefix, area token, number from 001 within the area. Identifiers are permanent; a withdrawn capability keeps its number and is marked withdrawn. |
| T1 Core | Required for first production use |
| T2 Professional | Required for parity with established professional tools |
| T3 Advanced | Specialized or differentiating |
| Phase | The suite phase (0–5) in which the capability is planned; see the [roadmap](../../roadmap.md) |

Each file starts with a purpose paragraph and a domain objects table, then capability tables with the
columns `ID | Capability | Behavior | Tier | Phase`.

## Areas

| File | Area token | Scope | Rows |
|---|---|---|---|
| [01-documents-and-components.md](01-documents-and-components.md) | DOC | Document structure, components, occurrences, multi-body parts, workspace, navigation, selection, measurement, recovery | 57 |
| [02-sketching-and-reference-geometry.md](02-sketching-and-reference-geometry.md) | SKETCH | 2D and 3D sketches, constraints, solver, projection, datums and coordinate systems | 66 |
| [03-solid-features.md](03-solid-features.md) | FEAT | Feature history, health and repair, sketched and placed features, patterns, multi-body operations | 69 |
| [04-surfacing.md](04-surfacing.md) | SURF | Surface creation and editing, surfacing curves, quality analysis | 44 |
| [05-freeform-and-mesh.md](05-freeform-and-mesh.md) | FORM | Freeform subdivision bodies, mesh bodies, conversion | 43 |
| [06-direct-editing.md](06-direct-editing.md) | DIRECT | Face edits, live rules, direct edits in history, feature recognition, simplification | 24 |
| [07-sheet-metal.md](07-sheet-metal.md) | SHEET | Rules, flanges, corners, flat patterns, checks | 41 |
| [08-plastic-parts-and-molds.md](08-plastic-parts-and-molds.md) | MOLD | Plastic part features, moldability analysis, mold tooling | 25 |
| [09-assemblies.md](09-assemblies.md) | ASM | Placement, joints and constraints, motion, interference, top-down design, large assemblies | 51 |
| [10-parameters-and-configurations.md](10-parameters-and-configurations.md) | PARAM | Parameters and expressions, configurations, model, display and position states | 34 |
| [11-properties-and-bom.md](11-properties-and-bom.md) | BOM | Properties, materials, mass properties, bills of materials | 30 |
| [12-drawings.md](12-drawings.md) | DRAW | Sheets, views, dimensions, annotations, tables, output | 56 |
| [13-model-based-definition.md](13-model-based-definition.md) | MBD | Semantic 3D annotation, validation, AP242 exchange | 19 |
| [14-frames-and-weldments.md](14-frames-and-weldments.md) | FRAME | Frame members, end treatments, weldments, cut lists | 28 |
| [15-standard-parts-and-generators.md](15-standard-parts-and-generators.md) | LIB | Standard parts library, feature templates, component generators | 30 |
| [16-automation-and-api.md](16-automation-and-api.md) | AUTO | Commands, automation protocol, code-first modeling, headless operation, rules, forms, plug-ins, assistant | 36 |
| [17-visualization-and-animation.md](17-visualization-and-animation.md) | VIS | Display styles, appearances, rendering through RenderBuilder, exploded states, animation | 26 |
| [18-data-management.md](18-data-management.md) | DATA | Local documents and references, versions, branches and merges, Arc Hub, multi-user editing | 36 |
| [19-interoperability.md](19-interoperability.md) | IO | Import and export behavior and formats | 36 |
| [20-routed-systems.md](20-routed-systems.md) | ROUTE | Tube, pipe, hose and cable harness routing | 70 |
| [21-manufacturing-setup-and-tooling.md](21-manufacturing-setup-and-tooling.md) | MFG | Setups, stock, machines, tools, simulation, post-processing, shop-floor connection | 80 |
| [22-milling.md](22-milling.md) | MILL | 2.5-axis, 3-axis and multi-axis milling and drilling | 66 |
| [23-turning-and-mill-turn.md](23-turning-and-mill-turn.md) | TURN | Turning and mill-turn | 45 |
| [24-additive-manufacturing.md](24-additive-manufacturing.md) | ADD | Additive build preparation | 44 |
| [25-nesting-and-fabrication.md](25-nesting-and-fabrication.md) | NEST | Sheet nesting and profile cutting | 45 |
| [26-simulation.md](26-simulation.md) | SIM | Structural, thermal and other studies through open solvers | 72 |
| [27-generative-design-and-optimization.md](27-generative-design-and-optimization.md) | GEN | Generative design and topology optimization | 46 |
| [28-electronics-codesign.md](28-electronics-codesign.md) | ECAD | Board exchange with open ECAD tools, board and component objects, synchronization, fit checks, harness links | 42 |
| [29-tolerance-and-inspection.md](29-tolerance-and-inspection.md) | TOL | Tolerance stack analysis and inspection | 45 |
| [30-schematic-capture.md](30-schematic-capture.md) | SCH | Sheets and hierarchy, symbols, nets and buses, annotation, electrical rule checking, variants, netlist, design blocks, cross-probing | 54 |
| [31-pcb-layout.md](31-pcb-layout.md) | PCB | Board and stack-up, footprints, placement, routing, copper pours, design rules, autorouting, 3D board and enclosure clearance, panelization | 64 |
| [32-electronics-simulation-and-outputs.md](32-electronics-simulation-and-outputs.md) | EOUT | SPICE simulation, basic signal-integrity checks, manufacturing outputs, library management and supplier data | 51 |
| [33-application-connectors.md](33-application-connectors.md) | CONNECT | Exchange with native files of other applications through connectors, licensed bridges and open readers | 39 |
| [34-assistance.md](34-assistance.md) | ASSIST | Optional assistance: model questions, sketch and reference repair, parts and variants from descriptions, feature recognition, manufacturability, fits and stack-ups, drawings and BOM, toolpaths, simulation and board checks | 24 |

## Capabilities and issues

- Work on a capability starts from a GitHub issue that names its identifiers in the title or body.
- Epics group capability ranges of a roadmap milestone and carry `type: epic`, `phase: N` and
  `area: <token>` labels, with the area token in lower case.
- Status lives in issues and the project board, never in these tables.
- A pull request that changes specified behavior updates the capability row in the same pull request.
- New behavior gets a new identifier at the end of its area; identifiers are never renumbered.
