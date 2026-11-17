# Electronics simulation and outputs

This area covers circuit simulation, basic signal-integrity checks, manufacturing outputs and library management for the electronics workspace. Simulation runs from the schematic through an open circuit engine in a separate process. Outputs are derived from the board and carry the document revision they were computed from. Libraries tie symbols, footprints, models and supplier data together. Related areas are [schematic capture](30-schematic-capture.md), [PCB layout](31-pcb-layout.md) and [electronics co-design](28-electronics-codesign.md).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Circuit study | Schematic, variant, analyses, engine, status | Owns analyses and result sets; derived from the netlist |
| Analysis | Kind (operating point, DC sweep, transient, AC, noise), parameters, sweeps | Belongs to a study |
| Device model | Name, kind, source (SPICE model or subcircuit), pin map | Linked to symbols through the library |
| Probe | Net, pin or differential pair, quantity | Placed on the schematic; reads results |
| Waveform set | Signals, axes, run, source revision | Derived from a study; stored as a result asset |
| Signal-integrity check | Net or pair, kind, inputs, limits, findings | Derived from the board and stack-up |
| Output job | Output set, board, variant, revision, target folder, naming rule | Produces fabrication and assembly data |
| Output set | Layers and files, format and options | Part of a job |
| Fabrication note | Text, stack-up table, drill table, tolerances | Placed on a fabrication drawing |
| Assembly drawing | Views, designators, polarity marks, notes | Derived from the board and variant |
| Component library | Parts, symbols, footprints, models, supplier data | Local, embedded or hosted on an Arc Hub |
| Library part | Part number, parametric fields, symbol, footprint, model, lifecycle state, supplier records | Stored in a component library |
| Supplier record | Supplier, supplier part number, package data, price break and stock fields as imported | Belongs to a library part; informational only |

## SPICE circuit simulation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-EOUT-001 | Circuit study | Creates a study from a schematic and variant, writes a SPICE netlist from the current netlist and records the engine and its version. Results carry the document revision. | T3 | 5 |
| SB-EOUT-002 | Open circuit engine | Runs an open SPICE engine as a separate process through an adapter, with a documented input and result format. No modeling workflow depends on an engine being installed, and a missing engine is reported. | T3 | 5 |
| SB-EOUT-003 | Device models | Assigns SPICE models and subcircuits to symbols through the library, with pin mapping checked against the symbol and a report of unmapped pins. | T3 | 5 |
| SB-EOUT-004 | Model import | Reads SPICE model and subcircuit files, lists the devices they define and stores the source in the library with its license field. | T3 | 5 |
| SB-EOUT-005 | Operating point | Computes DC node voltages and branch currents and displays them on the schematic. | T3 | 5 |
| SB-EOUT-006 | DC sweep | Sweeps a source, a parameter or temperature and plots the response. | T3 | 5 |
| SB-EOUT-007 | Transient analysis | Simulates time-domain behavior with stop time, step limits, initial conditions and sources defined by waveform expressions. | T3 | 5 |
| SB-EOUT-008 | AC analysis | Computes the small-signal frequency response with magnitude and phase, and derived quantities such as gain margin and bandwidth. | T3 | 5 |
| SB-EOUT-009 | Noise analysis | Computes input and output noise spectra and integrated noise over a band. | T3 | 5 |
| SB-EOUT-010 | Parameter sweeps and corners | Runs one or more sweeps over component values, supply levels, temperature or model corners and overlays the results. | T3 | 5 |
| SB-EOUT-011 | Statistical runs | Varies component tolerances at random over many runs and reports the spread of chosen measurements. | T3 | 5 |
| SB-EOUT-012 | Probes and sources | Places voltage, current and differential probes and stimulus sources on the schematic, stored as objects with the schematic. | T3 | 5 |
| SB-EOUT-013 | Waveform viewer | Plots signals with multiple axes, cursors, zoom, math expressions, FFT and measurements such as rise time, overshoot and RMS. | T3 | 5 |
| SB-EOUT-014 | Result cross-probing | Selects a signal by clicking a net on the schematic and the reverse, and shows the value at the cursor time on the sheet. | T3 | 5 |
| SB-EOUT-015 | Result export | Exports waveforms and measurements as CSV and plots as SVG or PNG. | T3 | 5 |
| SB-EOUT-016 | Background and headless runs | Runs studies on worker threads with progress and cancel, from the command line and as Arc Hub jobs. | T3 | 5 |
| SB-EOUT-017 | Thermal power hand-off | Writes simulated component power dissipation to the component properties used by the electronics cooling study in [simulation](26-simulation.md). | T3 | 5 |

## Signal-integrity checks

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-EOUT-018 | Impedance calculator | Computes characteristic impedance of microstrip, stripline, coplanar and differential structures from the stack-up and track geometry, and proposes a width or gap for a target. | T3 | 5 |
| SB-EOUT-019 | Impedance check | Reports tracks whose computed impedance deviates from the target of their net class beyond a tolerance, by layer and segment. | T3 | 5 |
| SB-EOUT-020 | Propagation delay and skew | Computes delay per net from length and stack-up and reports skew within groups and pairs against limits. | T3 | 5 |
| SB-EOUT-021 | Crosstalk estimate | Estimates coupling between parallel segments from spacing, length and height above the reference plane, and lists segments above a limit. | T3 | 5 |
| SB-EOUT-022 | Return path check | Reports high-speed tracks that cross gaps in their reference plane or change reference layer without a nearby stitching via. | T3 | 5 |
| SB-EOUT-023 | Via and stub check | Reports via stubs above a length limit and long unterminated branches. | T3 | 5 |
| SB-EOUT-024 | Termination review | Lists nets that are marked as high speed and have no termination component on the schematic, as an advisory. | T3 | 5 |
| SB-EOUT-025 | Checks in layout | Shows signal-integrity findings with the other rule violations, with waivers and a machine-readable report. | T3 | 5 |

## Manufacturing outputs

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-EOUT-026 | Output jobs | Defines output sets for a board and variant, runs them in one command and writes a job file that records revision, options and checksums. | T3 | 5 |
| SB-EOUT-027 | Gerber output | Writes one Gerber X2 or X3 file per layer with file function, polarity and attributes for nets, pads and components, and with aperture and zone handling that follows the published specification. | T3 | 5 |
| SB-EOUT-028 | Drill files | Writes drill files for plated and non-plated holes, slots and via types, with a drill table and optional per-layer-pair files. | T3 | 5 |
| SB-EOUT-029 | IPC-2581 output | Writes a single IPC-2581 file with stack-up, layers, drills, components, bill of materials and netlist for the chosen variant. | T3 | 5 |
| SB-EOUT-030 | ODB++ output | Writes ODB++ for the content that its published specification defines, and lists layers and attributes that cannot be expressed, in the loss report. | T3 | 5 |
| SB-EOUT-031 | Fabrication data import | Reads Gerber, drill, IPC-2581 and the ODB++ content covered by its specification to verify outputs and to show fabrication data, as described in [electronics co-design](28-electronics-codesign.md). | T3 | 5 |
| SB-EOUT-032 | Output verification | Re-reads each written file with an independent reader in the test suite and compares layers, apertures, drills, outline and net attributes with the board. | T3 | 5 |
| SB-EOUT-033 | Pick-and-place file | Writes component position, rotation, side and designator for the chosen variant as CSV or text, with an option for origin and rotation conventions, and excludes not-fitted parts. | T3 | 5 |
| SB-EOUT-034 | Assembly drawing | Produces assembly views per side with designators, polarity and pin-one marks, courtyards and notes, as a drawing in the drafting engine and as PDF. | T3 | 5 |
| SB-EOUT-035 | Fabrication drawing | Produces a fabrication drawing with outline dimensions, hole table, stack-up table, layer legend, tolerances and fabrication notes. | T3 | 5 |
| SB-EOUT-036 | Fabrication notes | Stores note templates with fields for finish, mask color, controlled impedance and class, and places them on the fabrication drawing. | T3 | 5 |
| SB-EOUT-037 | Bill of materials output | Writes the bill of materials of a variant as CSV, XLSX, PDF or HTML with grouping, designators and supplier fields from the library. | T3 | 5 |
| SB-EOUT-038 | Test data | Writes IPC-D-356 netlist test data and a list of test points with coordinates. | T3 | 5 |
| SB-EOUT-039 | 3D and drawing exports | Exports the populated board as STEP and as a mesh, and layers as DXF and PDF, with the loss report of each. | T3 | 5 |
| SB-EOUT-040 | Release package | Collects the output job files, the schematic and board documents, the bill of materials and a checksum manifest into one folder or archive and records it as a named version. | T3 | 5 |
| SB-EOUT-041 | Stale output guard | Marks outputs out of date when the board, variant or rules change after the run, and refuses to package a release from stale outputs. | T3 | 5 |

## Library management and supplier data

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-EOUT-042 | Component libraries | Groups library parts with their symbol, footprint, device model, 3D model and properties in libraries that are local, embedded or hosted on an Arc Hub. | T3 | 5 |
| SB-EOUT-043 | Part fields | Defines typed parametric fields per part category, such as value, tolerance, rating and package, with units and validation. | T3 | 5 |
| SB-EOUT-044 | Library search | Searches parts by text and by parametric filters and previews symbol, footprint and 3D model before placement. | T3 | 5 |
| SB-EOUT-045 | Library validation | Checks that symbol pins, footprint pads and device model pins agree, that required fields are filled and that 3D models align with the footprint, and lists failing parts. | T3 | 5 |
| SB-EOUT-046 | Part lifecycle | Stores a lifecycle state per part, such as proposed, approved, not recommended or obsolete, and warns when a design uses parts in a state that the project forbids. | T3 | 5 |
| SB-EOUT-047 | Supplier data import | Imports supplier part data from CSV, JSON and STEP files in published formats into supplier records with source and license fields, extending [standard parts and generators](15-standard-parts-and-generators.md). | T3 | 5 |
| SB-EOUT-048 | Supplier data refresh | Re-reads a supplier data file when it changes and lists changed fields, new parts and parts that disappeared, as a reviewed change set. | T3 | 5 |
| SB-EOUT-049 | Alternate parts | Records approved alternates with their differences and applies them through variants without changing the schematic. | T3 | 5 |
| SB-EOUT-050 | Library migration | Imports parts, symbols and footprints from the libraries of open ECAD tools with a report of unmapped fields. | T3 | 5 |
| SB-EOUT-051 | Library sharing | Publishes a library as a versioned package on an Arc Hub with release notes, and pins projects to a version. | T3 | 5 |
