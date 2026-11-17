# Nesting and fabrication

This area covers sheet nesting of flat patterns and profiles, material and sheet stock registers, remnant management, and two-dimensional cutting for laser, plasma, waterjet and router machines. Nests are derived from the design and update when parts change.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Nest job | Parts, quantities, sheets, machine, process, status | Owns nest sheets and cutting operations |
| Nest part | Source (flat pattern, face, sketch region, imported 2D), quantity, allowed rotations, grain rule, priority | Links to a design body or imported geometry |
| Sheet stock item | Material, thickness, size, grain, coating side, cost, quantity on hand | Stored in a stock register |
| Stock register | Sheets, coils, linear stock, remnants | Document, shared or hub scope |
| Remnant | Outline polygon, material, thickness, location label | Created from a nest; stored in the register |
| Exclusion zone | Region on a sheet where parts are not placed | Belongs to a sheet or remnant |
| Nest sheet | Sheet or remnant, placements, utilization | Belongs to a nest job |
| Placement | Part, position, rotation, mirror, pinned state | Belongs to a nest sheet |
| Cutting process | Process type, kerf table, speed, pierce and gas data | Library object referenced by machines |
| Cut path | Ordered contours, lead-ins, tabs, common lines | Derived from a nest sheet |
| Nest report | Per-sheet images, tables, costs | Derived from a nest job |

## Nest inputs

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-NEST-001 | Nest job | Creates a job that collects parts, quantities, sheet stock and a machine, and computes nest sheets and cut paths as derived data. | T1 | 5 |
| SB-NEST-002 | Part sources | Accepts sheet-metal flat patterns, planar faces of plate bodies, sketch regions and imported DXF or SVG outlines. Each part keeps its link to the source and updates when the source changes. | T1 | 5 |
| SB-NEST-003 | Quantities from assemblies | Takes part quantities from the assembly bill of materials times a build multiplier, with a per-part override. | T1 | 5 |
| SB-NEST-004 | Part attributes | Sets material, thickness, grain constraint, allowed rotations, mirror permission, priority and marking text per part. | T1 | 5 |
| SB-NEST-005 | Material grouping | Splits a job by material and thickness so that each group nests on matching stock. | T1 | 5 |
| SB-NEST-006 | Flat pattern preparation | Moves bend lines to a marking layer, removes features below a size threshold, and joins outline segments within tolerance before nesting. | T1 | 5 |

## Sheet stock and remnants

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-NEST-007 | Sheet stock register | Lists sheets by material, thickness, size, grain, cost and quantity on hand in a register stored in the document, locally or on an Arc Hub. | T1 | 5 |
| SB-NEST-008 | Remnant register | Stores leftover pieces with outline, material, thickness and location, and offers them as stock for later jobs. | T2 | 5 |
| SB-NEST-009 | Exclusion zones | Marks regions of a sheet or remnant, such as defects and clamp areas, where parts are not placed. | T2 | 5 |
| SB-NEST-010 | Sheet material properties | Stores density, grain direction and coating side so that mass and cost are computed and parts are placed right side up. | T1 | 5 |
| SB-NEST-011 | Coil and strip stock | Nests parts on strips of fixed width and open length and reports the length used. | T3 | 5 |
| SB-NEST-012 | Sheet selection | Chooses sheets from the register to cover the parts by lowest cost, first fit or preference for remnants, as the user selects. | T2 | 5 |
| SB-NEST-013 | Remnant creation | Finds the reusable region left on a sheet after nesting, subtracts a kerf margin and offers it as a new remnant. | T2 | 5 |
| SB-NEST-014 | Job commit | Updates the register when a job is committed by reducing sheet quantities and adding remnants, and keeps a ledger of the changes. | T2 | 5 |

## Nesting engine

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-NEST-015 | True-shape nesting | Places polygonal parts including holes, with arcs approximated within a tolerance, at stepped rotations, with part spacing. The result is deterministic for a given seed. | T1 | 5 |
| SB-NEST-016 | Guillotine nesting | Nests rectangles and blocks with full-width cuts for shear and panel saws, with a limit on cut stages. | T2 | 5 |
| SB-NEST-017 | Spacing and margins | Sets part-to-part spacing and sheet edge margins per material, thickness and cutting process. | T1 | 5 |
| SB-NEST-018 | Rotation and grain control | Honors allowed rotations, mirror permission and grain direction per part. | T1 | 5 |
| SB-NEST-019 | Objectives | Optimizes for utilization, sheet count, cut length, pierce count or a weighted mix under a time limit. | T2 | 5 |
| SB-NEST-020 | Multi-sheet nesting | Distributes quantities over several sheets and sheet sizes and lists parts that could not be placed with the reason. | T1 | 5 |
| SB-NEST-021 | Interactive nest editing | Moves, rotates, swaps and pins placements and re-nests the remaining parts. Every edit is an undoable command. | T2 | 5 |
| SB-NEST-022 | Inner nesting | Places small parts in holes and cutouts of larger parts. | T2 | 5 |
| SB-NEST-023 | Common-line nesting | Aligns straight edges of neighboring parts so that one cut separates both, and reports the cut length saved. | T2 | 5 |
| SB-NEST-024 | Associative update | Marks sheets stale when a part changes and re-nests the changed parts while keeping pinned placements that still fit. The changed parts are listed. | T2 | 5 |
| SB-NEST-025 | Background nesting | Runs nesting as a cancellable worker task that reports progress and the best result so far, tagged with the document revision. | T1 | 5 |
| SB-NEST-026 | Linear cut optimization | Assigns cut lengths of bar, tube and profile to stock lengths with saw kerf to minimize waste, and lists cuts per stock piece. | T2 | 5 |

## Cutting processes and paths

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-NEST-027 | Cutting process definition | Defines laser, plasma, waterjet, router and knife processes with kerf tables by material and thickness, cutting speeds, pierce parameters, power, gas or pressure, and nozzle data. | T1 | 5 |
| SB-NEST-028 | Contour cutting | Creates cut paths along nest contours with inside or outside kerf compensation, cutting inner contours before outer ones. | T1 | 5 |
| SB-NEST-029 | Kerf compensation | Applies kerf from the process table either as computed offsets or as controller compensation. | T1 | 5 |
| SB-NEST-030 | Lead-in and lead-out | Adds line, arc or no lead-in and lead-out with set lengths and places them on the scrap side away from corners and thin features. | T1 | 5 |
| SB-NEST-031 | Pierce control | Selects pierce type, such as standard, pulsed or edge start, and pierce position inside scrap, with pre-piercing for waterjet. | T2 | 5 |
| SB-NEST-032 | Tabs and micro-joints | Adds tabs of a set size and count to contours, automatically spaced or placed by hand, and skips tabs on short edges. | T1 | 5 |
| SB-NEST-033 | Common-line cutting | Cuts shared edges once and orders the cut so that both parts stay supported. | T2 | 5 |
| SB-NEST-034 | Cut order | Orders cutting by inner features first, small parts first, shortest travel or heat-aware sequences that move away from the previous cut. | T1 | 5 |
| SB-NEST-035 | Corner treatment | Adds loops, speed reduction or corner radii at sharp corners to prevent burn-through. | T3 | 5 |
| SB-NEST-036 | Router cutting | Produces router paths with tool offsets, ramps, onion skins and drilling using the milling strategies. | T2 | 5 |
| SB-NEST-037 | Bevel cutting | Produces tilted-head paths with a bevel angle for plasma and waterjet cutting and checks the tilt against machine limits. | T3 | 5 |
| SB-NEST-038 | Part marking | Marks part identifiers by laser, plasma or scribe on each part with text taken from the part properties. | T2 | 5 |
| SB-NEST-039 | Cut simulation | Previews the cut path with estimated time, pierces and consumables use. | T1 | 5 |
| SB-NEST-040 | Cutting machine posting | Posts cut paths through the post-processor system with dialects for pierce, torch height and gas control. | T1 | 5 |

## Reports and exports

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-NEST-041 | Nest report | Produces per-sheet images, utilization, parts and quantities, cut length, pierce count, time, cost and remnants as PDF or CSV. | T1 | 5 |
| SB-NEST-042 | Job cost estimate | Estimates material area and weight, machine time and cost per part and per job from rates in the register and the process. | T2 | 5 |
| SB-NEST-043 | Part identification | Labels placements with part identifiers in reports and exports, and ties labels to drawings. | T2 | 5 |
| SB-NEST-044 | Nest export | Exports nested sheets as DXF or SVG with separate layers for cutting and marking, and as NC files and PDF, headless from the command line. | T1 | 5 |
| SB-NEST-045 | Shared stock register | Synchronizes a stock register between users through an Arc Hub with change history. | T3 | 5 |
