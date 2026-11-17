# Geometry kernel

This document states how SolidBuilder uses the `arc-brep` API of `arc-core`: the operations it requires,
the tolerance model, tessellation for display, sectioning and hidden-line data for drawings, and what
the product never does itself. The suite decision on geometry tiers is ADR-0003 in
[arc-suite](https://github.com/ACKaraca/arc-suite/tree/main/docs/adr).

## Boundary

SolidBuilder depends on `arc-brep`, never on the kernel behind it. `arc-brep` exposes Arc-owned types
(bodies, faces, edges, vertices, curves, surfaces, naming tables) and operations with Arc-owned error
types. Its first implementation delegates robust operations to an established open-source kernel
through an isolated FFI crate in `arc-core`; individual operations are replaced by native Rust
implementations when they pass the regression corpus below. No SolidBuilder crate contains `unsafe`
code or kernel-specific types.

## Operations required

| Group | Operations | Used by |
|---|---|---|
| Primitives and sweeps | Box, cylinder, cone, sphere, torus; extrude with draft, revolve, sweep along path with orientation, twist and scale laws, loft with guides and end conditions, helix sweep | Solid features, sheet metal, frames, freeform conversion |
| Booleans | Union, subtract, intersect, split by body or sheet, imprint, with multi-body inputs and keep-tool option | Feature operation modes, assembly features, mold splits, weldments |
| Blends | Constant, variable, chordal and rule-selected fillets with setbacks; curvature-continuous blends; chamfers | Fillet and chamfer features |
| Offsets | Face offset, shell with per-face thickness, thicken sheets, offset surfaces | Shell, thicken, sheet metal, plastic walls |
| Draft and replace | Draft faces about a neutral plane or parting line, replace face, delete face with healing, move face with neighbors | Draft, direct editing |
| Surfaces | Ruled, filled patch with G0, G1 and G2 boundaries, trim, untrim, extend, sew into sheets and solids, unsew, reverse | Surfacing |
| Sheet metal | Bend and unbend of developable faces about a neutral fiber with given K-factor; flange construction; corner relief geometry | Sheet metal and flat patterns |
| Conversion | Subdivision surface to B-spline faces; mesh to faceted or fitted B-rep; B-rep to mesh | Freeform and mesh modeling |
| Queries | Point classification, closest point, minimum distance between bodies, ray cast, curvature, area, volume, mass properties with inertia, bounding boxes, face and edge adjacency | Measurement, interference, clearance, mass properties |
| Interference | Intersection volume between bodies, contact detection between bodies in positions | Assemblies, motion |
| Validation and healing | Topology and geometry checks, gap sewing, small feature removal, tolerance adjustment, with a report | Imports, linked imports |
| Naming | Generated names and split and merge history for every operation (see [feature history](feature-history.md)) | Persistent references |
| Exchange | STEP read and write of B-rep with assembly structure, colors, layers and PMI; IGES read and write | Interoperability |

Each operation returns either a result with a naming table and warnings or a typed error with a cause
code and the input entities involved. No operation panics or aborts the process; kernel faults are
caught at the FFI boundary and reported as errors.

## Tolerance model

- Model lengths are f64 metres; angles are radians (suite ADR-0008).
- The document has a modeling tolerance from `arc-base` (default 1e-7 m linear, 1e-9 rad angular
  equivalent) used for coincidence and sewing decisions. It is a document setting, not a literal in
  code.
- Imported bodies may carry larger local tolerances on edges and vertices; `arc-brep` keeps them as
  entity tolerances and reports them. Healing may tighten them.
- Sketch solving uses `arc-solve` tolerances scaled to the sketch extent.
- Comparisons in product code use `arc-base` tolerance helpers only.
- Very small and very large models are supported within the f64 range; a document whose model extent
  exceeds the range where the modeling tolerance is meaningful is warned about.

## Tessellation for display

- Each body is tessellated per face with chordal deviation, angular deviation and maximum edge length
  limits derived from the body's size and the document's display quality setting.
- Tessellation runs on worker threads per face and is cached with the body's revision; a dimension
  change re-tessellates only changed faces where naming shows faces unchanged.
- Each triangle carries its face's persistent name for picking; edges are tessellated as polylines with
  edge names.
- Lightweight loading stores tessellation in the document package so assemblies display without exact
  geometry.
- Export tessellation (STL, 3MF, glTF) uses separate, user-controlled tolerances.

## Sectioning and hidden-line data for drawings

Drawings use `arc-view` for projection and hidden-line removal and `arc-draft` for the sheet model.
SolidBuilder provides `arc-view` with:

| Input | Source |
|---|---|
| Exact edges and silhouettes | `arc-brep` exact silhouette curves for the view direction, plus model edges with names |
| Section cuts | Exact planar and stepped sections of bodies from `arc-brep`, producing cut faces with their body and material for hatching |
| Visibility | Hidden-line removal on exact curves against exact faces, with tessellation used only for coarse culling |
| Thread and cosmetic data | Thread specifications rendered as standard drawing representations, not as geometry |
| Attachment names | Every projected curve carries the persistent name of its source entity, so dimensions and annotations attach to names and survive regeneration |

View projection runs in the background per view and per revision; results are vector geometry in
`arc-draft` units with the view scale applied at the sheet boundary.

## What the product never does itself

- It does not implement booleans, blends, offsets, surface-surface intersection or healing; these
  belong to `arc-brep`.
- It does not hold kernel types, kernel handles or kernel error codes outside `arc-brep`.
- It does not modify topology by editing kernel data structures directly; every change is an
  `arc-brep` operation that returns naming.
- It does not compute hidden lines on meshes for drawings.
- It does not parse STEP or IGES itself; it maps the neutral models from `arc-io-step`.
- It does not choose tolerances by literal values in code.

## Kernel regression corpus

SolidBuilder maintains a corpus of models and scripted operations that any `arc-brep` implementation
must pass: feature regeneration with exact topology and naming, booleans and blends on known
difficult cases, sheet metal unfold results, sections and silhouettes against reference drawings, and
STEP round trips. Suite ADR-0003 names passing this corpus as the condition for replacing delegated
operations with native ones.
