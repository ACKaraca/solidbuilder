# Assistance

This area specifies what the optional assistant does in SolidBuilder: answering questions about
parts and assemblies, completing and repairing sketches, repairing references, recognizing features
on imported bodies, building parametric parts and variants from a description, reviewing designs
for manufacture, proposing fits and tolerances, drawings and bills of materials, and preparing
toolpaths, simulations and electronics checks, always as previewed commands checked by the
product's own solver, rebuild, analysis and simulation engines. The suite rules for levels,
grounding, data handling, provenance and evaluation are in
[assistance](https://github.com/ACKaraca/arc-suite/blob/main/docs/architecture/assistance.md); the
panel and its general tasks are [SB-AUTO-034 to SB-AUTO-036](16-automation-and-api.md) and the
protocol endpoint is [SB-AUTO-010](16-automation-and-api.md). Every capability here also has a
manual and scripted path.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Assistance request | Text, level, scope (sketch, features, components, configurations, documents), protected items, base revision | Produces answers or proposals |
| Proposal | Items, command invocations, rebuild result, checker findings, citations | Bound to a base revision; applied as one undo step |
| Feature source draft | Text form of a feature history (SB-AUTO-011) with an explanation | Built by the product before any proposal is shown |
| Manufacturing proposal | Setups, operations, tools, feeds and speeds, simulation results | Applied to the manufacturing model only after review |
| Provenance mark | Origin, proposal, revision | On features, components and annotations created or changed by an applied proposal until reviewed |

## Questions and repair

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ASSIST-001 | Model questions | The assistant answers questions about parts and assemblies ("mass of the aluminum parts in this configuration", "where is this bracket used", "which holes are M5 tapped") from mass properties, BOM and feature queries (SB-BOM-011, SB-DATA-026), citing features and components that select on request. | T2 | 4 |
| SB-ASSIST-002 | Sketch constraint completion | For an under-constrained sketch the assistant proposes geometric constraints and dimensions that express the evident design intent (symmetry about a construction line, equal holes, dimensions from datum edges), beyond the deterministic rule set of SB-SKETCH-029; the solver's degree-of-freedom display (SB-SKETCH-034) confirms the sketch is fully constrained and the shape unchanged. | T3 | 4 |
| SB-ASSIST-003 | Over-constrained sketch repair | When the solver reports conflicting or redundant constraints (SB-SKETCH-035), the assistant explains which constraints conflict and why, and proposes the smallest set to remove or convert to driven dimensions; the solver confirms the repaired state. | T3 | 4 |
| SB-ASSIST-004 | Reference repair after an edit | After an upstream edit leaves features with unresolved references, the assistant proposes new references for each, chosen by the geometric role of the lost reference, with before and after highlights; a full rebuild confirms the history regenerates and reports any changed volume. | T3 | 4 |

## Building and changing models

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ASSIST-005 | Parametric part from a description | From a description with dimensions ("a 60 by 40 mounting plate, 5 thick, four M4 clearance holes on a 50 by 30 pattern, corners filleted R3"), the assistant writes the feature history in its text form (SB-AUTO-011) with named parameters; the product builds it (SB-AUTO-013), checks body validity and compares the stated dimensions with measured ones before showing it. | T3 | 4 |
| SB-ASSIST-006 | Variants and configurations | From a description of a product family, the assistant proposes a configuration table (SB-PARAM-016) with driving parameters, suppression states and part numbers (SB-PARAM-021), and configuration rules (SB-PARAM-022); every configuration is rebuilt and checked for validity and interference before apply. | T3 | 4 |
| SB-ASSIST-007 | Feature recognition on imported bodies | On an imported body, the recognition engines (SB-DIRECT-017 to SB-DIRECT-019) find holes, fillets, extrusions and patterns; the assistant orders them into an editable feature history with names and driving dimensions, and the rebuilt body is compared with the import for volume and surface deviation within the stated tolerance. | T3 | 4 |
| SB-ASSIST-008 | Assembly constraints | For placed components, the assistant proposes joints and constraints from mating geometry (coaxial holes, coincident faces, fastener stacks); the assembly solver (SB-ASM-015) and the interference analysis (SB-ASM-027) evaluate the result, and motion limits are shown for review. | T3 | 4 |
| SB-ASSIST-009 | Standard part selection | For a selected hole or hole pattern the assistant proposes standard fasteners and washers from the library with auto-sized lengths (SB-LIB-004), checked by the fastener stack check (SB-ASM-029) and the clearance check. | T3 | 4 |

## Design review

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ASSIST-010 | Design for manufacture review | For a chosen process the assistant runs the applicable checks (sheet-metal manufacturability SB-SHEET-040, wall thickness, draft and undercut, boss proportions SB-MOLD-013 to SB-MOLD-015, draft analysis SB-SURF-039, printability SB-ADD-012), explains each finding with the rule and its value, and proposes changes such as draft angles, fillet radii or rib thickness that the same checks evaluate. | T3 | 4 |
| SB-ASSIST-011 | Interference and clearance triage | For interference and clearance results (SB-ASM-027, SB-ASM-028), the assistant groups findings by cause (intended press fits, thread overlaps, real collisions), proposes which to mark as intended through the product's fit settings, and proposes fixes for the rest that a new analysis evaluates. | T3 | 4 |
| SB-ASSIST-012 | Fit and tolerance proposals | For mating features and a described function (sliding, location, press), the assistant proposes a limits-and-fits class and tolerances (SB-TOL-002, SB-DRAW-028), and sets up a stack-up study (SB-TOL-004) whose worst-case and statistical results (SB-TOL-008, SB-TOL-009) are shown with the proposal. | T3 | 4 |
| SB-ASSIST-013 | Stack-up explanation | After a stack-up study, the assistant explains the contribution report (SB-TOL-012), names the dominant contributors, and proposes allocations (SB-TOL-013) whose results come from re-running the study. | T3 | 4 |

## Documentation and data

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ASSIST-014 | Drawing proposals | For a part or assembly the assistant proposes a drawing with views, sections and details from automatic view sets (SB-DRAW-024), dimensions and tolerances following the project's drafting standard, and annotation layout (SB-DRAW-031); a dimensioning completeness check lists features that are not fully located and sized. | T3 | 4 |
| SB-ASSIST-015 | Bill of materials and sourcing notes | The assistant proposes BOM descriptions, material and standard designations, and cost fields (SB-BOM-024) from the model and from supplier catalogs the user attaches, citing the catalog entry of each value; values without a source stay empty and flagged. | T3 | 4 |
| SB-ASSIST-016 | Rule and script drafting | The assistant drafts expression and script rules (SB-AUTO-019, SB-AUTO-020) or Python scripts for a described task; the static check (SB-AUTO-023) and a dry run on a copy of the document validate them before the user saves them. | T3 | 4 |

## Manufacturing and analysis

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ASSIST-017 | Toolpath strategy | From manufacturing feature recognition (SB-MFG-040), stock and the machine, the assistant proposes setups, operation sequences and tools from the user's tool library; stock and machine simulation (SB-MFG-048, SB-MFG-055), axis-limit checks (SB-MFG-056) and collision checks run on the proposal, and remaining stock and cycle time are shown. | T3 | 5 |
| SB-ASSIST-018 | Feeds and speeds proposals | For each operation the assistant proposes cutting data from the feeds and speeds calculation (SB-MFG-027) with the tool and material data in the library, explaining each adjustment (engagement, stick-out, machine limits); values outside the library's ranges are refused, and simulation results accompany the proposal. | T3 | 5 |
| SB-ASSIST-019 | Simulation setup and results | From a description of loads and supports the assistant proposes a study with load cases, restraints and defeaturing (SB-SIM-001 to SB-SIM-005); mesh quality and solver convergence checks run, and results are explained with their locations. The assistant never states that a part is safe; safety factors are shown as computed. | T3 | 5 |
| SB-ASSIST-020 | Generative study setup | From a description the assistant proposes a generative study's design space, preserved and obstacle geometry, load cases, goals and manufacturing constraints (SB-GEN-001 to SB-GEN-009) for the user to review before the study runs. | T3 | 5 |
| SB-ASSIST-021 | Board rule findings explained | After a board rule check (SB-PCB-044 to SB-PCB-046), the assistant groups violations by rule and net, explains them, and proposes routing or rule-area changes that a new check evaluates. | T3 | 5 |
| SB-ASSIST-022 | Schematic from a description | From a description of a circuit function, the assistant proposes a schematic from the library's parts with connections, checked by the electrical rule check, as a starting point that the engineer completes. | T3 | 5 |
| SB-ASSIST-023 | Sketch from an image | From a photograph or scan of a hand sketch with written dimensions, the assistant proposes a constrained sketch whose driving dimensions match the written values, leaving unreadable values for the user. | T3 | 5 |

## Review

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-ASSIST-024 | Assisted content review | Features, components and annotations carrying a provenance mark are shown with a distinct overlay and filter; the user reviews and clears marks, and releasing an item through Arc Hub lists unreviewed content first. | T3 | 4 |

## Readiness

Readiness classes are defined in
[assistance](https://github.com/ACKaraca/arc-suite/blob/main/docs/architecture/assistance.md#readiness-classes).

| Class | ID | Checked by | Depends on |
|---|---|---|---|
| R1 | SB-ASSIST-001 | Mass properties, BOM and feature queries | SB-AUTO-010, SB-AUTO-034, SB-BOM-011 |
| R1 | SB-ASSIST-002 | Constraint solver, degree-of-freedom display | SB-SKETCH-024, SB-SKETCH-034 |
| R1 | SB-ASSIST-003 | Constraint solver | SB-SKETCH-035 |
| R1 | SB-ASSIST-004 | Full rebuild, volume comparison | Persistent naming in `arc-brep` |
| R1 | SB-ASSIST-005 | Build from source, body validity, measured dimensions | SB-AUTO-011, SB-AUTO-013 |
| R1 | SB-ASSIST-006 | Rebuild of every configuration, interference | SB-PARAM-016, SB-PARAM-021, SB-PARAM-022 |
| R2 | SB-ASSIST-007 | Volume and surface deviation | SB-DIRECT-017 to SB-DIRECT-019 |
| R1 | SB-ASSIST-008 | Assembly solver, interference analysis | SB-ASM-015, SB-ASM-027 |
| R1 | SB-ASSIST-009 | Fastener stack and clearance checks | SB-LIB-004, SB-ASM-029 |
| R2 | SB-ASSIST-010 | Manufacturability, thickness, draft and printability checks | SB-SHEET-040, SB-MOLD-002, SB-MOLD-013 to SB-MOLD-015, SB-SURF-039 |
| R1 | SB-ASSIST-011 | Interference and clearance analysis | SB-ASM-027, SB-ASM-028 |
| R1 | SB-ASSIST-012 | Stack-up study | SB-TOL-002, SB-TOL-004, SB-TOL-008, SB-TOL-009 |
| R1 | SB-ASSIST-013 | Stack-up study | SB-TOL-012, SB-TOL-013 |
| R2 | SB-ASSIST-014 | Dimensioning completeness check | SB-DRAW-024, SB-DRAW-031, a completeness check for drawings |
| R1 | SB-ASSIST-015 | Catalog citations | SB-BOM-024, supplier catalogs supplied by the user |
| R1 | SB-ASSIST-016 | Static check, dry run | SB-AUTO-019, SB-AUTO-020, SB-AUTO-023 |
| R2 | SB-ASSIST-017 | Stock and machine simulation, axis limits, collision | SB-MFG-040, SB-MFG-048, SB-MFG-055, SB-MFG-056 |
| R2 | SB-ASSIST-018 | Feeds and speeds calculation, simulation | SB-MFG-027, tool and material data |
| R2 | SB-ASSIST-019 | Mesh quality, solver convergence | SB-SIM-001 to SB-SIM-005 |
| R2 | SB-ASSIST-020 | Study input validation | SB-GEN-001 to SB-GEN-009 |
| R1 | SB-ASSIST-021 | Board rule check | SB-PCB-044 to SB-PCB-046 |
| R3 | SB-ASSIST-022 | Electrical rule check | Schematic capture and its part library |
| R3 | SB-ASSIST-023 | Constraint solver, dimension consistency | Image input in the provider |
| R1 | SB-ASSIST-024 | Provenance query | Provenance marks in `arc-core` |

## Evaluation

The assistance task suite is built from the [reference workflows](../workflows.md): W1
(SB-ASSIST-002, SB-ASSIST-005, SB-ASSIST-014), W2 (SB-ASSIST-004), W3 and W4 (SB-ASSIST-008,
SB-ASSIST-011), W5 (SB-ASSIST-006), W6 and W8 (SB-ASSIST-010), W10 (SB-ASSIST-007), W11
(SB-ASSIST-005, SB-ASSIST-016) and W15 (SB-ASSIST-021). Thresholds are settled under
[OQ-26 and OQ-32](../open-questions.md).
