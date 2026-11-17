# Generative design and optimization

This area covers design spaces with preserve and obstacle geometry, loads and objectives, manufacturing-method constraints, exploration of multiple outcomes, shape and topology optimization, conversion of an outcome to editable geometry, automated feature creation from bounding geometry, and lattice and implicit modeling. Studies run through the solver adapter and worker model of the simulation area.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Generative study | Design space, load cases, goals, constraints, methods, materials | Owns outcomes; reuses loads and constraints of simulation studies |
| Design space | Starting body, resolution, symmetry | Bounded by preserve and obstacle geometry |
| Preserve region | Bodies or regions, clearance offset | Remains in every outcome |
| Obstacle region | Bodies or regions, clearance offset | Remains empty in every outcome |
| Goal | Objective type and weight | Belongs to a study |
| Design constraint | Quantity, limit, load case | Belongs to a study |
| Manufacturing constraint | Method, direction, angle or tool limits | Belongs to a study or an outcome run |
| Outcome | Density field or shape, method, material, metrics, provenance | Belongs to a study; may be converted to geometry |
| Outcome geometry | Smoothed solid, freeform or mesh body | Created from an outcome; linked to it |
| Optimization run | Variables, objectives, history | Drives studies or parameters |
| Implicit body | Field expression, bounds, resolution | Converts to mesh or boundary-representation body |
| Field | Scalar function over space or a body | Used by implicit bodies and lattices |
| Lattice | Cell type, cell size, strut or wall thickness, grading field | Fills a region; belongs to an implicit body |
| Suggested feature | Feature type, fitted parameters, source geometry | Proposed from bounding geometry; applied on approval |

## Study definition

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-GEN-001 | Generative study | Creates a study object holding the design space, load cases, goals, constraints, manufacturing methods and materials. The study is reproducible from its stored definition. | T1 | 5 |
| SB-GEN-002 | Design space | Defines the region material may occupy from a starting body or selected regions, with a resolution that controls outcome detail. | T1 | 5 |
| SB-GEN-003 | Preserve geometry | Marks bodies and regions that remain in every outcome, with an optional clearance offset. Faces that carry loads or constraints are preserved automatically. | T1 | 5 |
| SB-GEN-004 | Obstacle geometry | Marks bodies and regions that stay empty, with an optional clearance offset. | T1 | 5 |
| SB-GEN-005 | Load cases and constraints | Defines forces, pressures, torques, bearing loads, gravity and remote loads with fixed, pin and frictionless supports, or imports them from a simulation study and keeps the link. | T1 | 5 |
| SB-GEN-006 | Goals | Minimizes mass, maximizes stiffness, minimizes displacement or maximizes the first natural frequency, singly or as a weighted combination. | T2 | 5 |
| SB-GEN-007 | Design constraints | Limits stress through a safety factor, displacement and mass fraction, and optionally buckling load factor and minimum frequency. | T1 | 5 |
| SB-GEN-008 | Symmetry and pattern constraints | Applies mirror, cyclic and extrusion symmetry to the outcome. | T2 | 5 |
| SB-GEN-009 | Minimum member size | Sets a length scale below which the result has no members and smooths the field accordingly. | T1 | 5 |
| SB-GEN-010 | Materials | Assigns one or several materials from the material library to the study, and creates one outcome set per material on request. | T1 | 5 |

## Manufacturing-method constraints

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-GEN-011 | Unrestricted method | Produces outcomes without a manufacturing restriction. | T1 | 5 |
| SB-GEN-012 | Additive constraint | Limits overhang to a given angle for a build direction so that the outcome prints without supports where possible. | T2 | 5 |
| SB-GEN-013 | Cutting constraint | Restricts the outcome to a profile extruded along one axis for two-axis cutting. | T2 | 5 |
| SB-GEN-014 | Milling constraints | Restricts the outcome to shapes reachable from one, several or all directions without undercuts, with a minimum internal radius from the tool and a depth limit for pocketed shapes. | T2 | 5 |
| SB-GEN-015 | Casting constraint | Restricts the outcome to shapes that release from a die along a parting plane or line with a draft angle and no undercuts. | T2 | 5 |
| SB-GEN-016 | Multiple methods | Runs one study for several methods and tags each outcome with its method. | T2 | 5 |

## Solving and optimization

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-GEN-017 | Topology optimization | Solves a density-based or level-set topology problem with filtering through the solver adapter and reports the density field and its iso-surface. Runs are deterministic for the same inputs and seed. | T2 | 5 |
| SB-GEN-018 | Optimization mesh | Discretizes the design space on a regular grid or a tetrahedral mesh with resolution controls. | T2 | 5 |
| SB-GEN-019 | Multiple outcomes | Creates several outcomes by varying method, material, goal or target and runs them as independent jobs, in parallel across workers. | T1 | 5 |
| SB-GEN-020 | Local and remote execution | Runs jobs locally or on a remote worker with the same job model as simulation, with progress and early stop. | T1 | 5 |
| SB-GEN-021 | Shape optimization | Adjusts the boundary of an existing body within allowed regions to lower stress or mass while it keeps preserved geometry and the stated constraints. | T2 | 5 |
| SB-GEN-022 | Parameter optimization | Varies selected document parameters to meet objectives by running studies in a loop, with single and multi-objective modes and a Pareto set. | T2 | 5 |
| SB-GEN-023 | Convergence history | Plots objective, constraint and volume fraction against iteration and applies stop criteria. | T2 | 5 |

## Exploration and comparison

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-GEN-024 | Outcome table | Lists outcomes with mass, volume, maximum stress, safety factor, maximum displacement, method, material and estimated cost, sortable and filterable. | T1 | 5 |
| SB-GEN-025 | Outcome preview and comparison | Shows an outcome in three dimensions with a stress overlay and compares two outcomes side by side or as an overlay. | T1 | 5 |
| SB-GEN-026 | Ranking and Pareto view | Ranks outcomes by constraint satisfaction and a weighted score and plots trade-offs between two metrics. | T2 | 5 |
| SB-GEN-027 | Outcome validation | Creates a linear static study of the converted geometry with the study loads and reports the difference from the optimization estimate. | T1 | 5 |
| SB-GEN-028 | Outcome cost estimate | Estimates cost per outcome from material volume, additive supports and estimated machining time in the manufacturing areas. | T3 | 5 |
| SB-GEN-029 | Provenance | Stores solver and version, input hash, settings and document revision with each outcome. | T1 | 5 |
| SB-GEN-030 | Study commands | Exposes study definition, generation and export as commands in the automation protocol, including batch runs over parameters. | T2 | 5 |

## Export to editable geometry

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-GEN-031 | Outcome to boundary representation | Converts an outcome to a smooth solid or freeform body by surface fitting with a deviation tolerance, and links it to the outcome. | T2 | 5 |
| SB-GEN-032 | Outcome to mesh | Exports an outcome as a mesh in 3MF, STL or OBJ at a set tolerance. | T1 | 5 |
| SB-GEN-033 | Smoothing and cleanup | Smooths the surface, closes small holes and reports minimum wall thickness after conversion. | T2 | 5 |
| SB-GEN-034 | Merge with preserved geometry | Joins the converted outcome with preserve bodies by Boolean union, keeping the link to both. | T1 | 5 |
| SB-GEN-035 | Feature suggestions from bounding geometry | Fits ribs, webs, bosses, holes and sweeps inside an outcome or bounding volume and proposes them as parametric features. Features are created only when the user approves the suggestions. | T3 | 5 |
| SB-GEN-036 | Rib generation between regions | Creates rib and web layouts between loaded regions of a bounding volume from rules for thickness, spacing and draft, as editable features. | T3 | 5 |

## Lattice and implicit modeling

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-GEN-037 | Implicit body | Defines a body by a scalar field with union, subtraction, blend, offset and shell operations, and converts it to a mesh or a boundary-representation body. | T3 | 5 |
| SB-GEN-038 | Fields | Builds fields from distance, position, curvature or simulation results, combines them with expressions and remaps their values. | T3 | 5 |
| SB-GEN-039 | Lattice generation | Fills a region with strut lattices such as body-centered, octet and cubic cells, or with surface lattices such as gyroid and diamond, with cell size and thickness set by a constant or a field. | T3 | 5 |
| SB-GEN-040 | Unit cell library | Stores unit cells and accepts user-defined cells as small parametric models. | T3 | 5 |
| SB-GEN-041 | Stress-graded lattice | Grades lattice thickness by the stress field of a simulation result and updates it when the result changes. | T3 | 5 |
| SB-GEN-042 | Conformal lattice | Aligns lattice cells to the surface or a direction field of the region. | T3 | 5 |
| SB-GEN-043 | Lattice export | Exports lattices as meshes or as a 3MF beam lattice and checks printability limits for minimum strut size. | T3 | 5 |
| SB-GEN-044 | Lattice analysis | Builds a beam model of a strut lattice and runs it through the frame analysis of the simulation area. | T3 | 5 |
| SB-GEN-045 | Field to mesh conversion | Converts fields to meshes by adaptive surface extraction with a feature-preserving option and a size tolerance. | T3 | 5 |
| SB-GEN-046 | Implicit node graphs | Exposes implicit operations as nodes in a visual graph that evaluates on the node-graph engine and bakes to a body. | T3 | 5 |
