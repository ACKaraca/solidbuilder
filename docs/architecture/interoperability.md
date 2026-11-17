# Interoperability

This document states every format SolidBuilder reads or writes, by direction and fidelity level,
together with mapping principles, validation, loss reporting and what is deliberately unsupported. The
suite policy is ADR-0009 in [arc-suite](https://github.com/ACKaraca/arc-suite/tree/main/docs/adr);
user-facing behavior is in [interoperability capabilities](../product/capabilities/19-interoperability.md).

Fidelity levels: **view** (geometry and appearance), **data** (semantics, structure, properties,
relationships), **edit** (native parametric editing).

## Formats

| Format | Read | Write | Read fidelity | Write fidelity | Crate | Phase |
|---|---|---|---|---|---|---|
| `.solidb` (native) | Yes | Yes | edit | edit | `arc-doc` | 3 |
| Canonical text form | Yes | Yes | edit (per component) | edit | `solidbuilder-commands` | 3 |
| STEP AP242, AP214, AP203 | Yes | Yes | data (B-rep, assembly structure, names, colors, layers, properties; no history) | data | `arc-io-step` | 3 |
| STEP AP242 semantic PMI | Yes | Yes | data | data | `arc-io-step` | 4 |
| IGES | Yes | Yes | data (surfaces, curves) | data | `arc-io-step` family | 4 |
| Parasolid XT (text, binary) | Yes | No | data (B-rep) | n/a | bridge from published format reference | 5 |
| JT (ISO 14306) | Yes | No | view, data where exact geometry present | n/a | bridge | 5 |
| ACIS SAT | Yes, subject to open question | No | data | n/a | bridge | 5 |
| Rhino 3DM | Yes | No | data (NURBS) | n/a | `arc-io-3dm` over a permissively licensed reader | 5 |
| STL | Yes | Yes | view | view | `arc-io-mesh` | 3 |
| 3MF | Yes | Yes | view, data (materials, build items) | view, data | `arc-io-mesh` | 3 |
| OBJ, PLY | Yes | Yes | view | view | `arc-io-mesh` | 3 |
| glTF 2.0 | No | Yes | n/a | view (appearances, hierarchy) | `arc-io-mesh` | 3 |
| OpenUSD | No | Yes | n/a | view, data (hierarchy, materials) | `arc-io-usd` | 4 |
| Arc Scene | No | Yes (streamed) | n/a | view | `arc-scene` | 4 |
| DXF, DWG (geometry into sketches, underlays) | Yes | Yes | data (2D geometry, layers) | data | `arc-io-dxf`, `arc-io-dwg` | 3 |
| DXF, DWG (drawings, flat patterns) | n/a | Yes | n/a | data (layers, blocks, text, dimensions as geometry or entities) | `arc-draft`, `arc-io-dxf`, `arc-io-dwg` | 3 (drawings), 4 (flat patterns) |
| PDF | Underlay only | Yes (drawings) | view | view (vector) | `arc-io-pdf` | 3 (write), 4 (underlay) |
| SVG | Yes (into sketches) | Yes | data (paths) | view | `arc-io-pdf` | 3 |
| PNG, JPEG | Reference images, decals | Yes (viewport, renders) | view | view | `arc-app` | 3 |
| IFC 4.3 | Yes (reference bodies) | Yes (products as building elements) | view, data | data (types, property sets, ports) | `arc-io-ifc` | 4 |
| E57, LAS, LAZ | Yes | No | view | n/a | `arc-io-pointcloud` | 4 |
| URDF | No | Yes | n/a | data (links, joints, inertia) | `solidbuilder-io` | 5 |
| CSV, XLSX | Yes (parameters, tables) | Yes (BOM, tables, characteristic lists) | data | data | `solidbuilder-io` | 3 |
| 3D PDF | No | Yes | n/a | view, data (saved views, annotations) | to be decided | 5 |
| ECAD exchange (IDX, IDF) | Yes | Yes | data | data | `solidbuilder-io` | see [electronics co-design](../product/capabilities/28-electronics-codesign.md) |
| Arc exchange package (connectors) | Yes | Yes | data; edit where the host interface exposes it | data; edit where the host interface exposes it | `solidbuilder-io` over the exchange package | see [application connectors](../product/capabilities/33-application-connectors.md) |
| Gerber X2/X3, drill files | Yes | Yes | data | data | `solidbuilder-ecad` | 5 |
| IPC-2581 | Yes | Yes | data | data | `solidbuilder-ecad` | 5 |
| ODB++ (the content its published specification defines) | Yes | Yes | data | data | `solidbuilder-ecad` | 5 |
| IPC-D-356, pick-and-place CSV | No | Yes | n/a | data | `solidbuilder-ecad` | 5 |
| SPICE netlists, models and subcircuits | Yes (models) | Yes (netlists) | data | data | `solidbuilder-ecad` | 5 |
| Open ECAD native schematic, board and library files | Yes | No | data | n/a | `solidbuilder-ecad`, `solidbuilder-io` | 5 |
| MTConnect, OPC UA (machine status, program transfer) | Yes (status) | Yes (program transfer) | data | data | `solidbuilder-cam` | see [manufacturing setup](../product/capabilities/21-manufacturing-setup-and-tooling.md) |
| NC programs | No | Yes | n/a | data | `solidbuilder-cam` post-processors | see [manufacturing setup](../product/capabilities/21-manufacturing-setup-and-tooling.md) |
| Additive build files (3MF build, G-code for printers) | No | Yes | n/a | data | `solidbuilder-cam` | see [additive manufacturing](../product/capabilities/24-additive-manufacturing.md) |
| Solver decks and results | Yes (results) | Yes (decks) | data | data | `solidbuilder-sim` adapters | see [simulation](../product/capabilities/26-simulation.md) |

No import reaches edit fidelity for foreign data: imported geometry becomes base features that can be
extended with features and direct edits, and feature recognition can rebuild editable features.

## Mapping principles

- Format crates produce a neutral model of the format; `solidbuilder-io` owns the mapping into the
  domain model.
- STEP products map to component definitions, product occurrences to occurrences, shape
  representations to bodies, styled items to appearances, and property definitions to property values
  through the document's property mapping.
- Assembly structure is preserved; identical STEP products map to one definition with several
  occurrences.
- Units and up axis are converted at the import boundary into metres and the document frame.
- Exports write the active configuration and states unless the user chooses others; each configuration
  can be exported as a separate STEP product.
- Appearances export through the Arc Scene material model to glTF and OpenUSD and as colors to STEP.
- IFC export maps components to building element types chosen by the user, with property sets from
  property mappings and connection ports from connection frames marked as ports.

## Import records and loss reporting

Every import:

1. stores the source file as a content-addressed asset in the document;
2. creates an import record with format, reader version, options and an identifier map;
3. heals geometry and lists each repair;
4. keeps unmapped content as preserved foreign data attached to the nearest object;
5. produces a loss report with categories (unsupported entity, approximated geometry, dropped attribute,
   healed defect) and counts.

Every export produces a loss report of what the target format cannot carry (for example feature
history, configurations, joints in STEP).

Linked imports re-read their source when the file changes and re-resolve downstream persistent
references; the change and any broken references are reported.

## Validation

- Every writer is tested by reading its output with an independent reader in CI and comparing volume,
  surface area, topology counts, assembly structure and property values with the source within
  tolerance.
- Readers are tested against a corpus of files with known content and expected loss reports, held in
  a separate corpus repository with an owner, license and checksum per file.
- Importers are fuzzed and bound recursion, allocation and entity counts.

## Connectors, bridges and open readers

Native files of other applications that have no published specification are exchanged by connectors,
as decided in
[suite ADR-0017](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0017-exchange-connectors-and-licensed-bridges.md).
A connector is a small add-in that runs inside the user's own licensed copy of the other application,
uses only its public programming interface and reads or writes the Arc exchange package specified in
[the interchange architecture](https://github.com/ACKaraca/arc-core/blob/main/docs/architecture/interchange.md).
Connectors live in a separate repository and are not part of this application.

- **Content:** parts, assemblies, feature trees where the host interface exposes them, sketches,
  parameters, configurations, drawings and properties, at data fidelity and, where the host allows,
  edit fidelity.
- **Identifiers:** the round-trip map keeps host identifiers beside SolidBuilder IDs, so repeated
  exchanges match the same objects and downstream persistent references are re-resolved and reported
  when they break.
- **Licensed bridge:** a user-installed, user-licensed conversion library is called as a separate
  process; it is never distributed with SolidBuilder and never required for a core workflow.
- **Open readers:** formats with a published specification or a permissively licensed reader, such as
  3DM, are read natively behind a format crate.
- Every exchange produces an import record and a loss report like any other import.

## Electronics exchange

The electronics workspace writes fabrication data itself and exchanges board data with other tools
through the formats listed above. Writers are verified by re-reading their output with an independent
reader. ODB++ covers only the content that its published specification defines; the rest is listed in
the loss report.

## Deliberately unsupported

| Format family | Reason | Exchange instead |
|---|---|---|
| Undocumented formats and protected binaries or scripts of other applications | Reverse engineering and running protected content create a legal problem | A connector, a licensed bridge, an open reader or STEP from the source application |
| FBX | No published specification | glTF, OpenUSD |
| Proprietary rendering scene formats | Rendering goes through RenderBuilder | Arc Scene, OpenUSD |
| Building family formats of BIM tools | Owned by ArchBuilder, which has its own connector | IFC 4.3 product export |

Libraries under restrictive or uncertain licenses are used only through byte-level bridges in separate
processes and are never required for a core workflow.
