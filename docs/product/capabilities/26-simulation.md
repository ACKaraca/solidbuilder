# Simulation

Simulation covers structural, thermal, dynamic and process studies on parts, assemblies and frames. Studies are objects in the document that reference model geometry by persistent names, prepare a study model, mesh it, serialize it to a documented solver input and run it locally or on a remote worker through open solvers. Results, reports and stale-result handling follow the document revision.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Study | Type, model reference, configuration or model state, status | Owns a study model, materials, constraints, loads, contacts, mesh settings and result sets |
| Study model | Included bodies, defeaturing rules, symmetry, idealizations | Derived from the design; independent of design edits |
| Study material | Physical properties, nonlinear data, failure criterion | Assigned to bodies; sourced from the material library |
| Constraint | Type, target geometry, free directions | Belongs to a study; references persistent geometry |
| Load | Type, target, magnitude or function, direction | Belongs to a load case |
| Load case | Name, loads, combination factors | Belongs to a study |
| Contact | Type, pair, friction, tolerance | Belongs to a study |
| Connector | Bolt, rigid link, spring or damper definition | Joins geometry or bodies |
| Mesh | Element type, order, global size, local controls, quality metrics | Derived from the study model |
| Solver job | Adapter, input hash, resources, status, log | Created per run; local or remote |
| Result set | Fields, solver and version, mesh hash, document revision | Belongs to a study; marked stale when inputs change |
| Frame model | Members, cross sections, joints, releases | Derived from frame or weldment members |
| Dynamic model | Rigid bodies, joints, forces, drivers, contacts | Derived from an assembly |
| Fill setup | Gates, polymer, process parameters | Belongs to an injection-molding study |
| Report | Template, sections, images | Derived from a study and its results |

## Study management and preparation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SIM-001 | Study | Creates a study of a chosen type that references a part, assembly, configuration or model state and stores its definitions and results in the document. A document holds any number of studies. | T1 | 5 |
| SB-SIM-002 | Study duplication | Copies a study with its definitions to create variants such as another material or load case. | T2 | 5 |
| SB-SIM-003 | Load cases | Holds several load sets in one study and combines results of linear studies with factors. | T2 | 5 |
| SB-SIM-004 | Defeaturing | Suppresses holes, fillets and small features below a size threshold and removes selected bodies from the study model without changing the design. | T2 | 5 |
| SB-SIM-005 | Symmetry reduction | Cuts the study model at a symmetry plane and applies the matching symmetry constraints. | T2 | 5 |
| SB-SIM-006 | Shell and beam idealization | Replaces thin solids with mid-surface shells and slender members with beams, with the thickness and section taken from the geometry. | T2 | 5 |
| SB-SIM-007 | Design-change associativity | Remaps constraints, loads and contacts through persistent references when the design changes. Unmapped items are flagged and results are marked stale and never deleted. | T1 | 5 |
| SB-SIM-008 | Parametric study | Sweeps selected parameters, such as dimensions, loads and thickness, runs each case as a separate job and presents a results table and chart. | T2 | 5 |
| SB-SIM-009 | Study comparison | Compares result sets of two studies or runs side by side and as a difference field. | T2 | 5 |
| SB-SIM-010 | Pre-solve validation | Checks for missing materials, unconstrained rigid-body motion, overlapping loads and unconnected bodies, and lists each problem with the object concerned. | T1 | 5 |

## Materials and failure criteria

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SIM-011 | Study materials | Assigns materials with modulus, Poisson ratio, density, yield and ultimate strength, thermal expansion, conductivity and specific heat to bodies or regions, from the material library or by entry. | T1 | 5 |
| SB-SIM-012 | Orthotropic and temperature-dependent data | Accepts orthotropic constants and property tables that vary with temperature. | T2 | 5 |
| SB-SIM-013 | Plasticity | Accepts bilinear and multilinear stress-strain data with hardening for nonlinear studies. | T2 | 5 |
| SB-SIM-014 | Hyperelastic and creep models | Accepts hyperelastic and creep material models for nonlinear studies, with fitting from entered test data. | T3 | 5 |
| SB-SIM-015 | Failure criteria | Evaluates von Mises, Tresca, maximum principal and Mohr-Coulomb criteria and reports a safety factor per element. | T1 | 5 |

## Constraints, loads and contacts

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SIM-016 | Constraints | Provides fixed, pin, frictionless, prescribed displacement, symmetry, elastic support and remote constraints on faces, edges and vertices, with free directions set per constraint. | T1 | 5 |
| SB-SIM-017 | Structural loads | Provides force, pressure, moment, bearing, remote force with offset, gravity, acceleration, rotational speed, bolt preload and hydrostatic loads with direction by vector, normal or axis. | T1 | 5 |
| SB-SIM-018 | Thermal loads and conditions | Provides temperature, heat flux, convection, radiation, internal heat generation and initial temperature. | T1 | 5 |
| SB-SIM-019 | Function-driven values | Accepts tables and expressions in time or in parameters for loads, constraints and thermal conditions. | T2 | 5 |
| SB-SIM-020 | Loads from motion | Imports forces and reactions from a time step of a rigid-body dynamic study as loads of a structural study and links them to the source. | T2 | 5 |
| SB-SIM-021 | Contacts | Detects contacts in assemblies within a tolerance and provides bonded, separation, sliding with friction, shrink-fit and rigid types, plus manual contact pairs. | T1 | 5 |
| SB-SIM-022 | Bolt connectors | Joins hole pairs with a bolt model of set stiffness and preload and reports tension and shear per bolt. | T2 | 5 |
| SB-SIM-023 | Rigid, spring and weld connectors | Provides rigid links, springs, dampers, spot welds and seam welds as connectors between points, edges or faces. | T3 | 5 |
| SB-SIM-024 | Contact results | Reports contact status, gap and pressure per contact pair. | T2 | 5 |

## Meshing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SIM-025 | Mesh generation | Generates tetrahedral meshes with linear or quadratic elements, shell meshes for thin bodies and beam meshes for frames through open mesher adapters. Meshing is deterministic for the same inputs. | T1 | 5 |
| SB-SIM-026 | Global mesh settings | Sets element size, curvature refinement, minimum size, growth rate and element order. | T1 | 5 |
| SB-SIM-027 | Local mesh controls | Sets element size on faces, edges, vertices or bodies and defines refinement regions as boxes or spheres. | T1 | 5 |
| SB-SIM-028 | Adaptive refinement | Repeats solves and refines the mesh where an error indicator exceeds a target until the change in a chosen result is below a limit or a maximum number of iterations is reached, then reports convergence. | T2 | 5 |
| SB-SIM-029 | Mesh quality | Reports aspect ratio, Jacobian and skewness and lists elements outside the limits. | T2 | 5 |
| SB-SIM-030 | Mesh display and export | Displays the mesh with sections and exports it in open mesh formats. | T2 | 5 |
| SB-SIM-031 | Mesh caching | Meshes in the background and caches the mesh by geometry revision and settings, so that unchanged studies skip meshing. | T1 | 5 |
| SB-SIM-032 | Hexahedral-dominant meshing | Generates meshes dominated by hexahedral elements for sweepable regions. | T3 | 5 |

## Structural and thermal study types

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SIM-033 | Linear static | Solves small-displacement linear elastic problems and reports displacement, strain, stress, reaction forces and safety factor. | T1 | 5 |
| SB-SIM-034 | Modal | Computes the lowest natural frequencies and mode shapes with an optional preload, and reports mass participation and rigid-body modes. | T1 | 5 |
| SB-SIM-035 | Thermal | Solves steady-state and transient heat transfer and reports temperature and heat flux. | T2 | 5 |
| SB-SIM-036 | Thermal stress | Applies the temperature field of a thermal study as load in a structural study and reports thermal stress and distortion. | T2 | 5 |
| SB-SIM-037 | Linear buckling | Computes critical load factors and buckling modes for the applied loads. | T2 | 5 |
| SB-SIM-038 | Nonlinear static | Solves large-displacement, material-nonlinear and contact-nonlinear problems with load stepping, automatic increments and a convergence log. | T2 | 5 |
| SB-SIM-039 | Explicit dynamics | Solves transient impact and drop problems with time-dependent loads, initial velocities and contact by explicit integration, with output at set intervals. | T3 | 5 |
| SB-SIM-040 | Harmonic response | Computes steady-state response over a frequency sweep by modal superposition with damping. | T3 | 5 |
| SB-SIM-041 | Fatigue | Estimates life from linear results with load cycles, stress-life data and mean-stress correction. | T3 | 5 |

## Frame analysis

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SIM-042 | Frame model derivation | Derives beam members, nodes, sections and materials from frame and weldment members, with node merging at intersections. The model updates when the frame changes. | T2 | 5 |
| SB-SIM-043 | Frame supports and releases | Applies fixed, pinned and roller supports to nodes and end releases to members. | T2 | 5 |
| SB-SIM-044 | Frame loads | Applies point, distributed and moment loads and self weight to members and nodes. | T2 | 5 |
| SB-SIM-045 | Frame results | Reports reactions and plots axial force, shear, bending moment, deflection and stress along members. | T2 | 5 |
| SB-SIM-046 | Member checks | Reports utilization against yield and Euler buckling for each member and highlights members above a limit. | T3 | 5 |

## Rigid-body dynamic simulation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SIM-047 | Dynamic model | Derives rigid bodies with mass properties and joints from the assembly, and converts assembly constraints to joints where possible. Constraints that cannot convert are listed. | T2 | 5 |
| SB-SIM-048 | Forces and drivers | Applies gravity, forces and torques as functions of time, springs, dampers and imposed position or velocity profiles on joints. | T2 | 5 |
| SB-SIM-049 | Dynamic contacts | Defines contacts between bodies with friction, restitution and stiffness, and rolling, gear and cam pairs. | T3 | 5 |
| SB-SIM-050 | Dynamic solver | Integrates the equations of motion with step control and initial conditions, and reports over-constrained mechanisms and singularities. | T2 | 5 |
| SB-SIM-051 | Output signals | Reports position, velocity, acceleration, forces, reactions and energy per body or joint as plots and CSV. | T2 | 5 |
| SB-SIM-052 | Clearance over motion | Reports the minimum distance between selected bodies across the simulated motion and flags interference. | T2 | 5 |

## Solving

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SIM-053 | Solver adapter interface | Serializes a study to a documented, solver-neutral input of mesh, materials, conditions and steps, and reads results from open result formats. Each open solver is an adapter that runs as an external process. | T1 | 5 |
| SB-SIM-054 | Local solve | Runs a solver adapter in a worker process with limits on threads and memory, with progress, cancellation and the solver log. | T1 | 5 |
| SB-SIM-055 | Remote worker | Runs a job on an Arc Hub runner or another machine through a documented job protocol that uploads the packaged job, streams progress and returns results. No hosted vendor service is required. | T2 | 5 |
| SB-SIM-056 | Job queue | Shows queued and running jobs with priority, estimated time, logs and error diagnostics. | T1 | 5 |
| SB-SIM-057 | Solver verification suite | Ships verification cases with analytic or published reference results that run against each solver adapter. | T2 | 5 |
| SB-SIM-058 | Result provenance | Stores with each result set the solver and version, mesh hash, input hash and document revision, so that a run can be repeated and audited. | T1 | 5 |

## Results and reports

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SIM-059 | Result fields | Provides displacement, stress components, von Mises and principal stress, strain, safety factor, reactions, contact pressure and status, temperature, heat flux and mode shapes, with nodal and averaged display. | T1 | 5 |
| SB-SIM-060 | Result display | Shows contours with legend and extremes, deformed shape with scale, transparency, section planes, iso-surfaces, vector glyphs and animation. | T1 | 5 |
| SB-SIM-061 | Probes and queries | Reports values at picked points, locations of extremes, plots along a path and integrals over a face or volume. | T1 | 5 |
| SB-SIM-062 | Allowables | Highlights regions above an allowable value and clamps the legend range to chosen limits. | T1 | 5 |
| SB-SIM-063 | Section forces | Computes resultant forces and moments on a section plane and linearized stress through a wall. | T3 | 5 |
| SB-SIM-064 | Result export | Exports images, CSV tables, VTK fields and colored glTF for review in other tools. | T2 | 5 |
| SB-SIM-065 | Report | Generates a report with model, materials, mesh, loads, constraints, results, convergence, assumptions and images, from a template and headless from the command line. | T1 | 5 |
| SB-SIM-066 | Results to parameters | Writes selected result values, such as maximum stress, to document parameters so that rules and optimization can use them. | T3 | 5 |
| SB-SIM-067 | Stale result handling | Marks a result set stale when the geometry, materials or conditions it depends on change, and keeps it available for comparison. | T1 | 5 |

## Injection molding and electronics cooling

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-SIM-068 | Mold fill analysis | Simulates the filling of a thin-walled part from gate locations with a polymer from the library and process settings for melt temperature, mold temperature and injection time. Results include fill time, pressure, weld lines, air traps, shear and short shots. | T3 | 5 |
| SB-SIM-069 | Gate advisor | Proposes gate locations that balance the flow path and lists the fill-time difference between the candidates. | T3 | 5 |
| SB-SIM-070 | Warpage estimate | Estimates warpage from fill results and the shrinkage data of the polymer. | T3 | 5 |
| SB-SIM-071 | Electronics cooling | Solves heat transfer in an enclosure with a populated board, component power, fans with curves, vents and ambient conditions through an open flow solver adapter, and reports component temperatures. | T3 | 5 |
| SB-SIM-072 | Component power import | Reads component power dissipation from the board data of the electronics co-design area or from CSV and applies it as heat sources. | T3 | 5 |
