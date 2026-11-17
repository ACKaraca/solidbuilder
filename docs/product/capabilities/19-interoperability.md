# Interoperability

SolidBuilder exchanges models with other tools through open, published formats and states the fidelity
of each exchange. Every import keeps its source and reports what it could not represent; every export is
checked by an independent reader. The format policy and fidelity table are in
[the interoperability architecture](../../architecture/interoperability.md).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Import record | Source file asset, format, options, reader version, time, source identifier map | Owned by the component or document created by the import |
| Loss report | List of unrepresented or approximated items with counts and severity | Produced by every import and export |
| Preserved foreign data | Original format, raw content, source identifiers | Kept with imported objects; re-emitted where the format allows |
| Linked import | Source path, source checksum, import options, refresh policy | Base feature that re-reads its source file |

## General import and export behavior

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-IO-001 | Keep the source | Every import stores the original file as a content-addressed asset in the document and an import record with format, options and reader version. | T1 | 3 |
| SB-IO-002 | Identifier mapping | Source identifiers (STEP entity names, product IDs, layer names) are mapped to SolidBuilder IDs and kept, so re-imports and exports can match objects. | T1 | 3 |
| SB-IO-003 | Loss reports | Every import and export produces a loss report listing unsupported entities, approximations, healed defects and dropped attributes with counts; the report is stored and viewable. | T1 | 3 |
| SB-IO-004 | Preserved foreign data | Content that has no SolidBuilder equivalent is kept as preserved foreign data attached to the nearest object and is written back on export to the same format where possible. | T2 | 3 |
| SB-IO-005 | Import healing | Imported B-rep data is checked and repaired (gap sewing, small edge removal, tolerance adjustment); every repair is listed and the unrepaired original remains available. | T1 | 3 |
| SB-IO-006 | Units and orientation | Imports read source units and up axis, convert to the document frame, and let the user override both. | T1 | 3 |
| SB-IO-007 | Export validation | Exported files are re-read by an independent reader in the test suite and compared for volume, area, topology counts and assembly structure. | T1 | 3 |
| SB-IO-008 | Background exchange | Imports and exports run on worker threads with progress and cancel; cancel leaves the document unchanged. | T1 | 3 |
| SB-IO-009 | Batch conversion | The CLI converts files between supported formats in bulk with options files and writes loss reports for each. | T1 | 3 |

## Exact geometry formats

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-IO-010 | STEP import | Imports STEP AP242, AP214 and AP203 solids, sheets, wires, assembly structure, names, colors, layers, validation properties and user properties; imported parts start as base features without history. | T1 | 3 |
| SB-IO-011 | STEP export | Exports parts and assemblies as STEP AP242 (or AP214 on request) with assembly structure, names, colors, layers, validation properties and selected properties. | T1 | 3 |
| SB-IO-012 | STEP semantic PMI | Imports and exports AP242 semantic PMI and saved views, as specified in [model-based definition](13-model-based-definition.md). | T2 | 4 |
| SB-IO-013 | Linked import | A STEP file can be inserted as a linked import that re-reads the source when it changes; downstream features and joints re-attach through persistent references, and the change is reported. | T2 | 3 |
| SB-IO-014 | IGES | Imports and exports IGES surfaces, curves and trimmed surfaces; imported surfaces can be stitched into solids. | T2 | 4 |
| SB-IO-015 | Parasolid XT import | Imports B-rep geometry from Parasolid XT text and binary files using the published format reference. | T3 | 5 |
| SB-IO-016 | JT import | Imports JT (ISO 14306) assembly structure, tessellated geometry and exact geometry where present, at view or data level. | T3 | 5 |
| SB-IO-017 | ACIS SAT import | Imports B-rep geometry from SAT files where the version is covered by a published description. | T3 | 5 |
| SB-IO-018 | Rhino 3DM import | Imports NURBS curves, surfaces and polysurfaces natively through a permissively licensed reader behind its own format crate, as specified in [application connectors](33-application-connectors.md). | T3 | 5 |

## Mesh and scene formats

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-IO-019 | STL | Imports and exports binary and ASCII STL; export sets units and chordal and angular tessellation tolerances per body or document. | T1 | 3 |
| SB-IO-020 | 3MF | Imports and exports 3MF with meshes, colors, materials, units and multiple build items. | T2 | 3 |
| SB-IO-021 | OBJ and PLY | Imports and exports OBJ (with materials) and PLY meshes. | T2 | 3 |
| SB-IO-022 | glTF export | Exports glTF 2.0 with appearances and assembly hierarchy for viewers and Arc Review. | T2 | 3 |
| SB-IO-023 | OpenUSD export | Exports assemblies and scenes as OpenUSD with appearances on the Arc Scene material model. | T2 | 4 |
| SB-IO-024 | Arc product exchange | Sends models to RenderBuilder through the Arc Scene contract and to Arc Review as glTF, STEP or IFC. | T2 | 4 |

## 2D and drawing formats

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-IO-025 | DXF and DWG into sketches | Imports DXF and DWG geometry into a sketch with layer selection, units, block explosion and optional conversion of layers to construction geometry. | T1 | 3 |
| SB-IO-026 | DXF and DWG export of sketches and faces | Exports sketches and planar faces as DXF or DWG geometry with layer mapping and spline-to-polyline option. Flat pattern and drawing exports are specified in [sheet metal](07-sheet-metal.md) and [drawings](12-drawings.md). | T1 | 3 |
| SB-IO-027 | DWG and DXF underlays | Attaches a DWG or DXF as an underlay in a part or assembly, positioned on a plane, for reference and snapping. | T2 | 3 |
| SB-IO-028 | SVG | Imports SVG paths into sketches and exports sketches as SVG. | T2 | 3 |
| SB-IO-029 | PDF and raster underlays | Attaches PDF pages or raster images as scaled underlays on planes. | T3 | 4 |
| SB-IO-030 | Viewport images | Exports the viewport as PNG or JPEG. | T1 | 3 |

## Other domains

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-IO-031 | IFC product export | Exports components as IFC 4.3 building elements with type, property sets, simplified geometry and connection ports, for building projects. | T2 | 4 |
| SB-IO-032 | IFC reference import | Imports IFC elements as reference bodies with their properties to design products in a building context. | T3 | 4 |
| SB-IO-033 | Point clouds | Attaches E57, LAS and LAZ point clouds as references with crop, alignment to the model and sectioning. | T3 | 4 |
| SB-IO-034 | URDF export | Exports an assembly as URDF with links from components, joints from joints and inertia from mass properties. | T3 | 5 |

## Proprietary and unsupported formats

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-IO-035 | Proprietary native CAD | Native files of other applications are exchanged through connectors that run inside the user's own licensed copy, through user-licensed bridges and through open readers, as specified in [application connectors](33-application-connectors.md). Reverse engineering of undocumented formats is not provided; STEP and linked import remain the format-level path. | T1 | 3 |
| SB-IO-036 | FBX | FBX is not read or written; animated and scene exchange uses glTF and OpenUSD. | T1 | 3 |
