# Standard parts library and component generators

The standard parts library supplies sized families of fasteners, bearings, profiles and other catalog
components from documented local or Arc Hub libraries. Component generators create calculated machine
elements, such as shafts, gears and springs, and keep their inputs so the result can be edited and
recalculated.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Library | Name, location (local folder or Arc Hub), read-only flag, families | Searched by placement commands |
| Library family | Category, standard, template component, size table columns, naming rule | Contains library items |
| Library item | Row values, generated member identity, part number | Placed as an occurrence; stored in-document or as a library document |
| Feature template | Saved features with input references, parameters, optional size table | Inserted into parts; used for punch tools and repeated features |
| Component generator | Kind, inputs, calculation results, report, generated components | Owns generated occurrences and can regenerate them |

## Standard parts library

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-LIB-001 | Library browser | Browses and searches families by category, standard, size and property, with previews; placing a size creates an occurrence. | T2 | 4 |
| SB-LIB-002 | Content | Ships families for screws, bolts, nuts, washers, pins, keys, bearings, retaining rings, O-rings, springs and structural profiles under open data licenses. | T2 | 4 |
| SB-LIB-003 | Library locations | Libraries are documented files in local folders or hosted by Arc Hub; several libraries can be active with an order of precedence. | T2 | 4 |
| SB-LIB-004 | Auto-sizing placement | Dropping a fastener on a hole edge selects the size that fits the hole, places it with a joint and offers stack members such as washers and nuts. | T2 | 4 |
| SB-LIB-005 | Change size or family | Changes the size or family of placed library items while keeping their joints. | T2 | 4 |
| SB-LIB-006 | Member storage | Generated members are stored in the document or written as library documents according to the library's setting; names follow the family naming rule. | T2 | 4 |
| SB-LIB-007 | Library editor | Adds families, categories and columns, edits size tables, and maps columns to properties; edits are validated before the library is saved. | T2 | 4 |
| SB-LIB-008 | Publish user content | Publishes a configured part or assembly as a library family with its configuration table as the size table. | T2 | 4 |
| SB-LIB-009 | Supplier catalog import | Imports supplier components from STEP files with a property sheet (CSV) as a library family, keeping source and license fields. | T3 | 4 |
| SB-LIB-010 | Thread and hole data | Thread and hole standards used by holes, threads and fasteners are library data that users can extend. | T1 | 3 |
| SB-LIB-011 | Similar component search | Finds components in open documents and libraries that are geometrically similar to a selected one, ranked by similarity. | T3 | 5 |

## Feature templates

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-LIB-012 | Save feature template | Saves selected features with their sketches and parameters as a feature template, declaring which references are inputs. | T2 | 4 |
| SB-LIB-013 | Insert feature template | Inserts a template by mapping its inputs to geometry and setting parameters, optionally from a size table; the inserted features appear as a group in the history. | T2 | 4 |
| SB-LIB-014 | Template catalog | Templates are browsable in the library with previews and categories, and serve as punch tools for sheet metal. | T2 | 4 |

## Component generators

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-LIB-015 | Bolted connection | Generates a bolted connection through selected holes with bolt, washers and nut sized to the clamped length and thread engagement, and checks preload and joint strength. | T2 | 4 |
| SB-LIB-016 | Fastener stack review | Re-evaluates existing fastener stacks for length, engagement and mismatched components and proposes corrected sizes. | T2 | 4 |
| SB-LIB-017 | Shaft | Builds a stepped shaft from cylinder, cone and polygon sections with chamfers, fillets, keyways, grooves and threads; sections are editable in a list and the shaft regenerates. | T2 | 4 |
| SB-LIB-018 | Shaft calculation | Computes deflection, slope, bending and torsional stress for supports and loads along the shaft and plots the results. | T3 | 4 |
| SB-LIB-019 | Gears | Generates spur, helical, bevel and worm gear pairs from module, teeth, face width and profile shift, and checks load capacity to ISO 6336 or AGMA methods. | T2 | 4 |
| SB-LIB-020 | Bearings | Selects rolling bearings by bore, load and speed and computes rating life to ISO 281. | T2 | 4 |
| SB-LIB-021 | Springs | Generates compression, extension, torsion and disc springs from loads and dimensions with stress and fatigue checks. | T2 | 4 |
| SB-LIB-022 | Belts and chains | Generates V-belt, timing belt and roller chain drives with pulleys and sprockets and computes length and center distance. | T3 | 4 |
| SB-LIB-023 | Cams | Generates disc and linear cams from follower motion laws and checks pressure angle and curvature. | T3 | 5 |
| SB-LIB-024 | Keys, pins and splines | Generates key, pin and spline connections with strength checks. | T2 | 4 |
| SB-LIB-025 | Interference fits | Computes pressures, stresses and torque capacity of press fits from the ISO 286 fit and materials. | T3 | 4 |
| SB-LIB-026 | Beam and plate calculators | Computes deflection and stress of standard beam and plate cases from section and material data. | T3 | 5 |
| SB-LIB-027 | Engineering data sets | Provides standard data tables (fits, threads, material values, screw dimensions) as versioned data sets that generators and expressions can read. | T2 | 4 |
| SB-LIB-028 | Generator reports | Every calculating generator writes a report in HTML and PDF with inputs, method, results and pass or fail per check. | T2 | 4 |
| SB-LIB-029 | Re-edit generated components | Reopening a generated component restores the generator inputs; applying changes regenerates the components and keeps their joints. | T2 | 4 |
| SB-LIB-030 | Custom generators | New generators can be added as scripts or plug-ins through [automation](16-automation-and-api.md) and appear in the generator catalog. | T3 | 4 |
