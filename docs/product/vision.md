# Product vision

This document states why SolidBuilder exists, who it serves, the principles that decide trade-offs,
and what it deliberately does not do.

## Problem and purpose

Mechanical products are defined by parametric 3D models: parts built from sketches and features,
assemblies that must move and fit, drawings and annotated models that tell a shop what to make, and
the manufacturing and analysis data derived from them. The tools that hold this data today tie it to
a single vendor's file format, licensing service or cloud store, rebuild feature histories on one
processor core, and lose references when upstream geometry changes.

SolidBuilder is a native, open-source parametric modeling application for parts, assemblies,
drawings and manufacturing preparation. Its documents belong to the user, open without a network or
account, follow a published schema, and regenerate on all cores. It is one product of the
[Arc suite](https://github.com/ACKaraca/arc-suite) and shares its foundation, command model and
collaboration server with the other Arc applications.

## Primary users and their deliverables

| User | Deliverables |
|---|---|
| Machine and equipment designers | Parts, assemblies, mechanisms, bills of materials, manufacturing drawings |
| Product and industrial designers | Surface and freeform shapes, plastic housings, renderings, appearance models |
| Sheet metal and fabrication shops | Sheet metal parts, flat patterns, welded frames, cut lists, nested sheets |
| Machinists and CNC programmers | Toolpaths, NC programs, setup sheets |
| Additive manufacturing users | Print-ready parts, build layouts, supports |
| Analysis engineers | Simulation studies, reports, optimized shapes |
| Electronics hardware teams | Enclosures designed around boards, board outlines exchanged with ECAD tools |
| Design automation engineers | Configurable products, rules, scripts, batch pipelines, code-first part libraries |
| Suppliers and contract manufacturers | Imported customer models edited directly, simplified exports |

## Product principles

1. **The model is the authority.** Drawings, bills of materials, renderings, toolpaths and studies are
   derived from the model and rebuilt from it; nothing downstream holds an independent copy of
   geometry.
2. **References survive edits.** Every reference to geometry is a persistent reference designed in
   from the first feature; when a reference cannot be resolved the product says so and offers a repair,
   never a silent reattachment.
3. **History and direct editing coexist.** Users may model with a feature history, without one, or
   with direct edits recorded inside a history; each choice is per component.
4. **Every action is a command.** Interactive tools, scripts, rules, the command line, the automation
   protocol and assistants use the same validated commands with the same undo.
5. **Local first.** A document on disk is complete. Arc Hub adds sharing, lifecycles and multi-user
   editing; it is never required.
6. **Structure by choice.** Parts, assemblies and drawings live in one document or in separate
   documents, decided per project and changeable later.
7. **Fast by construction.** Regeneration, assembly solving, view projection and toolpath generation
   run in parallel and in the background; a stale result is never shown as current.
8. **Open exchange.** STEP is the primary exchange format; every import keeps its source and reports
   losses.

## Scope

SolidBuilder covers the capability areas listed in the
[capability index](capabilities/README.md): documents and components; sketching and reference
geometry; solid features and feature history; surfacing; freeform subdivision and mesh modeling;
direct editing; sheet metal; plastic parts and molds; assemblies; parameters, configurations and
states; properties and bills of materials; drawings; model-based definition; frames and weldments;
standard parts and component generators; automation; visualization and animation; data management;
interoperability; routed systems; manufacturing setup, milling, turning, additive manufacturing,
nesting and fabrication; simulation; generative design and optimization; electronics co-design; and
tolerance analysis and inspection.

## Non-goals

| Non-goal | Reason |
|---|---|
| Photoreal path tracing inside SolidBuilder | Rendering is provided by RenderBuilder through the Arc Scene contract; one renderer is maintained for the suite. |
| Schematic capture and PCB layout | Electronics design stays in dedicated open ECAD tools; SolidBuilder exchanges board outlines, components and keep-outs with them. |
| Reading or writing proprietary native CAD files without published specifications | They are exchanged through STEP; formats are never reverse engineered. |
| Building authoring (walls, floors, storeys) and factory floor layout | Building and plant layout belong to ArchBuilder and DraftBuilder; SolidBuilder exports products as IFC elements for them. |
| Architectural and 2D-only drafting outside model drawings | DraftBuilder is the suite's general drafting product; SolidBuilder drawings use the same drafting engine. |
| Polygon modeling for film and games, sculpting, rigging and character animation | These belong to MeshBuilder; SolidBuilder keeps mesh tools needed for engineering (repair, conversion, comparison). |
| Hosted cloud solving, rendering or assistant services | The open-source product runs solvers and renderers locally or on a self-hosted Arc Hub; assistant providers are configured by the user. |
| Supplier ordering and vendor catalog services | The standard parts library imports supplier data from STEP and property sheets; it does not integrate commercial ordering services. |
| Shop-floor machine monitoring | Manufacturing preparation ends at validated NC programs and setup sheets. |

## Positioning

SolidBuilder covers the professional scope of parametric mechanical design and manufacturing
preparation addressed by tools such as Autodesk Inventor, Dassault Systèmes SolidWorks and
CATIA, Siemens NX and Solid Edge, PTC Creo and Onshape, and open-source projects such as FreeCAD:
part and assembly modeling, drawings, sheet metal and fabrication, manufacturing preparation and
simulation, delivered as an open-source, local-first application.

## Differentiators

| Capability | Where it is specified |
|---|---|
| Persistent naming designed in from the first feature, with a reference audit, one-pick remap and a regression corpus | [Solid features](capabilities/03-solid-features.md), [feature history architecture](../architecture/feature-history.md) |
| Local-first documents with optional Arc Hub collaboration and offline check-in | [Data management](capabilities/18-data-management.md) |
| Branching and merging of design history at feature, parameter and occurrence level, with conflicts kept side by side | [Data management](capabilities/18-data-management.md) |
| Multi-threaded rebuild, assembly solving and drawing view projection with cancel | [Solid features](capabilities/03-solid-features.md), [architecture overview](../architecture/overview.md) |
| Code-first modeling: a stable, versioned automation API and a canonical, diffable text form of the feature history | [Automation](capabilities/16-automation-and-api.md) |
| Parts, assemblies and drawings in one document or separate documents by choice | [Documents and components](capabilities/01-documents-and-components.md) |
| One solver for joints and assembly constraints, with minimal conflict sets | [Assemblies](capabilities/09-assemblies.md) |
| Direct edits recorded inside a feature history, with live rules | [Direct editing](capabilities/06-direct-editing.md) |
| Semantic model-based definition with validation and STEP AP242 exchange | [Model-based definition](capabilities/13-model-based-definition.md) |
| Open solver integration and toolpath kernels with a documented post-processor format | [Simulation](capabilities/26-simulation.md), [manufacturing setup](capabilities/21-manufacturing-setup-and-tooling.md) |
| Headless model checking and drawing publishing in CI | [Automation](capabilities/16-automation-and-api.md), [drawings](capabilities/12-drawings.md) |
| One executable with identical features on Windows, macOS and Linux | [Architecture overview](../architecture/overview.md) |
| Loss-reported imports that keep the source file | [Interoperability](capabilities/19-interoperability.md) |
| Calculating component generators whose inputs stay editable | [Standard parts and generators](capabilities/15-standard-parts-and-generators.md) |
| Provider-neutral, optional assistant that applies changes only through previewed commands | [Automation](capabilities/16-automation-and-api.md) |
