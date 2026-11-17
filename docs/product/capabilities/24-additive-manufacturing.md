# Additive manufacturing

Additive manufacturing covers printer and print-setting libraries, orientation and arrangement, support generation, slicing and toolpath preview for extrusion, resin and powder-bed processes, process simulation, directed-energy and hybrid processes, and export of print files. A printer is a machine definition of additive type, so builds share setups, posting and verification with the rest of manufacturing.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Printer definition | Process, build volume and shape, bed, extruders, nozzles, kinematics, firmware dialect | A machine definition of additive type; referenced by builds |
| Material profile | Process, density, temperatures, shrinkage, cost, color | Stored in a library; assigned to parts or regions |
| Print profile | Layer height, walls, infill, speeds, cooling, support settings, parent profile | Inherited by builds; overridden per part or region |
| Build | Printer, plate, material, profile, parts, report | Owns part placements, supports and slices |
| Part placement | Source body, orientation, position, scale factors, overrides | Belongs to a build; references a design body |
| Modifier volume | Region, setting overrides, layer range | Belongs to a part placement |
| Support structure | Type, anchor points, interface settings, blockers and enforcers | Belongs to a part placement |
| Slice set | Layers, perimeters, fill regions or bitmaps, source revision | Derived from a build |
| Scan strategy | Pattern, rotation per layer, laser parameters | Used by powder-bed slice sets |
| Deposition path | Bead width, height, rate, orientation | Derived for directed-energy builds |
| Process simulation | Model, thermal and mechanical inputs, displacement field | Derived from a build; feeds compensation |
| Print file | Format, units, metadata | Output of a build |

## Printer, material and profile libraries

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ADD-001 | Printer definition | Defines a printer by process, build volume and shape, bed, extruders, nozzle sizes, kinematics and firmware dialect. The definition is a documented, versioned schema. | T1 | 5 |
| SB-ADD-002 | Printer library | Stores printers locally or on an Arc Hub and imports open printer profiles. Fields that cannot be mapped are listed. | T2 | 5 |
| SB-ADD-003 | Material profile | Stores process-specific material data including density, temperatures, shrinkage, cost and color, linked to the physical material so that mass and cost follow the part. | T1 | 5 |
| SB-ADD-004 | Print profiles | Stores layer height, wall count, infill, speeds, retraction, cooling, adhesion and support settings in profiles that inherit from a parent. Parts and regions override single values. | T1 | 5 |
| SB-ADD-005 | Region settings | Applies setting overrides to a modifier volume, a layer range or a body, and shows the affected region in the preview. | T2 | 5 |
| SB-ADD-006 | Printer as machine | Treats the printer as a machine definition so that additive setups reuse tool, post-processor and verification features. | T1 | 5 |

## Orientation, arrangement and printability

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ADD-007 | Manual orientation | Orients a part by rotation values, by placing a selected face on the plate, or by laying it flat. | T1 | 5 |
| SB-ADD-008 | Automatic orientation | Scores candidate orientations by overhang area, support volume, build height, load direction relative to layers and surface quality, and applies the one the user selects from a ranked list. | T2 | 5 |
| SB-ADD-009 | Arrangement | Packs several parts onto the plate in two or three dimensions with spacing and rotation constraints, using true shapes. For powder-bed builds the packing stacks parts in height. Results are deterministic for a given seed. | T1 | 5 |
| SB-ADD-010 | Multiple plates | Distributes parts across several plates and keeps the assignment as an editable list. | T2 | 5 |
| SB-ADD-011 | Shrinkage compensation | Scales parts by anisotropic factors taken from the material profile and keeps the unscaled design unchanged. | T2 | 5 |
| SB-ADD-012 | Printability analysis | Colors the part by wall thickness, feature size, overhang angle and bridge span, and flags trapped volumes and non-manifold geometry against limits from the printer and material. | T1 | 5 |
| SB-ADD-013 | Print mesh preparation | Tessellates bodies to a set tolerance for slicing, checks the mesh for watertightness and lists defects with their locations. | T1 | 5 |
| SB-ADD-014 | Build volume check | Checks parts, supports and gantry height against the build volume and warns when any exceeds it. | T2 | 5 |

## Supports and internal structure

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ADD-015 | Support generation | Generates supports under overhangs beyond a threshold angle as pillars, trees, bridges, blocks or lattices, with interface layers, gap and density. Supports may touch the plate only or the part as well. | T1 | 5 |
| SB-ADD-016 | Support editing | Adds support blockers and enforcers by region, and adds, removes or edits single supports. Edits stay when the supports regenerate. | T2 | 5 |
| SB-ADD-017 | Resin supports | Generates point-contact supports and checks for islands and suction cups in resin builds, with lift and peel settings. | T2 | 5 |
| SB-ADD-018 | Powder-bed supports | Generates solid, lattice and line supports and plate anchors that carry heat away and resist distortion, with thickness settings. | T2 | 5 |
| SB-ADD-019 | Support removal access | Checks that tools can reach the supports to be removed and flags supports with no access path. | T3 | 5 |
| SB-ADD-020 | Hollowing | Hollows a part to a wall thickness and adds drain holes for resin builds, then reports the removed volume. | T2 | 5 |
| SB-ADD-021 | Infill structures | Fills the interior with regular patterns or with a lattice from the lattice and implicit modeling area. | T2 | 5 |

## Slicing and preview

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ADD-022 | Extrusion slicing | Slices parts into layers with perimeters, top and bottom skins, infill patterns, seam placement, retraction, travel and bridging. The result is derived data tied to the build revision. | T1 | 5 |
| SB-ADD-023 | Adaptive layer height | Varies layer height by surface slope and curvature within limits from the printer and profile. | T2 | 5 |
| SB-ADD-024 | Extrusion preview | Shows layers and moves colored by feature type, with a layer scrubber, estimated time and material use. | T1 | 5 |
| SB-ADD-025 | Multi-material printing | Assigns materials or colors to regions and generates tool changes and purge volumes. | T2 | 5 |
| SB-ADD-026 | Non-planar slicing | Slices with conical or curved layers for multi-axis printers and checks nozzle clearance against the part. | T3 | 5 |
| SB-ADD-027 | Resin slicing | Produces layer images or vector slices with exposure parameters, anti-aliasing and bottom layer settings. | T1 | 5 |
| SB-ADD-028 | Powder-bed scan strategies | Generates stripe, island and contour scan patterns with layer rotation, hatch spacing, speed, power and separate parameters for up-skin and down-skin zones. | T2 | 5 |
| SB-ADD-029 | Binder-jetting slices | Produces layer bitmaps with compensation for binder spread and shrinkage. | T3 | 5 |
| SB-ADD-030 | Directed-energy deposition paths | Generates bead paths for contours and fills with multi-axis orientation, with bead width and layer height taken from a deposition model. | T3 | 5 |
| SB-ADD-031 | Hybrid additive and subtractive | Alternates deposition and milling in one setup, taking stock for milling from the simulated deposited shape and using the milling strategies. | T3 | 5 |

## Process simulation and estimates

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ADD-032 | Powder-bed distortion simulation | Predicts distortion and residual stress of a metal build from layer-by-layer inherent strain, using an open solver through the solver adapter. Results include displacement, stress and recoater collision risk. | T3 | 5 |
| SB-ADD-033 | Distortion compensation | Pre-deforms the part with the inverse of the predicted displacement field and re-slices the result. The original design stays unchanged. | T3 | 5 |
| SB-ADD-034 | Layer cooling estimate | Estimates cooling time per layer and flags layers whose time falls below the material minimum. | T3 | 5 |
| SB-ADD-035 | Time, material and cost estimate | Estimates print time, material mass, support mass, energy and cost per build and per part from rates in the profile. | T1 | 5 |
| SB-ADD-036 | Feature compensation table | Applies dimension corrections for holes and slender features by a table tied to the material and process. | T3 | 5 |

## Export and hand-off

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ADD-037 | 3MF export | Writes 3MF with units, build items, materials, colors and per-object settings. The same writer is available for a single part without a build. | T1 | 4 |
| SB-ADD-038 | STL and OBJ export | Writes binary STL and OBJ at a set tessellation tolerance and unit and reports the triangle count. | T1 | 4 |
| SB-ADD-039 | G-code output | Produces printer G-code from start and end templates with variables, for the firmware dialect of the printer definition. | T1 | 5 |
| SB-ADD-040 | Layer file output | Writes resin and powder-bed layer data as ZIP archives of bitmaps or SVG slices, with writers for other formats added through plug-ins. | T2 | 5 |
| SB-ADD-041 | Hand-off to external slicer | Exports a 3MF build for an external slicer and reads back the printer and profile names. | T2 | 5 |
| SB-ADD-042 | Network print | Sends a print file to a printer through documented network protocols implemented as plug-ins. Nothing is sent without an explicit command. | T3 | 5 |
| SB-ADD-043 | Headless slicing | Slices a build and writes print files from the command line with the same results as the application. | T2 | 5 |
| SB-ADD-044 | Build report | Produces a PDF report with the part list, orientation, supports, estimates, settings and preview images. | T2 | 5 |
