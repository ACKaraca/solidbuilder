# Manufacturing setup and tooling

This area defines how a design becomes a machining program: manufacturing models, setups, work coordinate systems, stock, fixtures, tool and holder libraries, machine definitions, operation templates, verification, the post-processor system and NC output. Toolpaths are derived data. They regenerate from the design on worker threads, keep their links to model geometry through persistent names, and are never applied from a stale revision.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Manufacturing model | Selected bodies, local modifications, source revision | Derived from the design; referenced by setups; never writes back to the design |
| Setup | Name, type (milling, turning, mill-turn, cutting, additive, probing), machine, status | Owns a work coordinate system, stock, fixtures and an ordered list of operations |
| Work coordinate system | Origin, axes, offset number, rotary orientation | Belongs to a setup; may reference a user coordinate system object |
| Stock | Kind (box, cylinder, tube, solid, mesh, previous setup), offsets, material | Belongs to a setup; may take the simulated result of an earlier setup |
| Fixture | Bodies, clamp regions, no-cut zones, parametric dimensions | Placed in a setup; acts as a collision object |
| Tool | Type, geometry, flutes, material, tool number, offsets, cutting data presets | Stored in a tool library; referenced by operations |
| Holder | Revolved profile or body, gauge length, collet or extension | Combined with a tool into a tool assembly |
| Tool library | Ordered tools and holders, schema version, scope | Local, shared or embedded in a document |
| Machine definition | Type, axes, travel limits, spindle limits, kinematic chain, controller profile, optional connection | Referenced by setups and by post processors |
| Machine connection | Protocol, endpoint, security mode, credential reference, node map | Belongs to a machine definition; carries program transfer and read-only status |
| Operation | Strategy, tool, geometry selection, parameters, state | Owned by a setup; produces a toolpath |
| Operation template | Operation list with parameters and tool rules | Applied to setups or recognized features |
| Toolpath | Moves, feeds, tool orientation, source revision | Derived from an operation; consumed by simulation and posting |
| NC program | Ordered operations, post processor, output rules | Produces NC files and a setup sheet |
| Post-processor definition | Format, blocks, words, hooks, properties | Referenced by NC programs; bound to a controller profile |
| Setup sheet | Template, rendered pages, operation and tool lists | Derived from setups and NC programs |

## Manufacturing models and setups

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MFG-001 | Manufacturing model | Creates a derived working copy of selected bodies for machining that may add stock features, fill holes or split bodies without changing the design. The copy regenerates when the design changes. | T1 | 5 |
| SB-MFG-002 | Setup | Creates a setup that holds a manufacturing model, work coordinate system, stock, fixtures, machine and an ordered list of operations. Setup types are milling, turning, mill-turn, cutting, additive and probing. | T1 | 5 |
| SB-MFG-003 | Setup from orientation | Creates a setup whose Z axis follows a selected face normal, axis or plane and whose X axis follows a selected edge or direction. The setup updates when the referenced geometry moves. | T2 | 5 |
| SB-MFG-004 | Setup duplication | Copies a setup with its operations to another orientation, mirrored position or fixture position. Selections are remapped and failures are flagged. | T2 | 5 |
| SB-MFG-005 | Setup order and status | Orders setups, marks them as draft or released, and blocks posting of draft setups when the program is set to require release. | T3 | 5 |

## Work coordinate systems

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MFG-006 | Work coordinate system | Defines origin and axes from a model point, face, edge, stock box point, user coordinate system or numeric entry. Each system receives an offset number that the post processor outputs. | T1 | 5 |
| SB-MFG-007 | Multiple coordinate systems | Holds several work coordinate systems in one setup for fixture plates and pallets, with a numbering scheme for the offsets. Operations select the system they use. | T2 | 5 |
| SB-MFG-008 | Rotary orientation | Describes a tilted or indexed orientation by plane angles and lets the post processor output it as a rotary position or a tilted-plane command. Orientations that exceed machine limits are flagged. | T2 | 5 |
| SB-MFG-009 | Origin travel check | Warns when the model, stock or fixtures in a setup extend beyond the travel limits of the selected machine from the chosen origin. | T2 | 5 |

## Stock and fixtures

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MFG-010 | Stock definition | Defines stock as a relative-size box, cylinder or tube with offsets, a fixed size, a solid body or a mesh. The stock material links to the material library. | T1 | 5 |
| SB-MFG-011 | Stock from previous setup | Takes the simulated stock result of an earlier setup, including the reorientation between setups. The setup is marked stale when the earlier setup changes. | T1 | 5 |
| SB-MFG-012 | Near-net stock | Defines stock as an offset of a casting, forging or additive model with a uniform or per-face allowance. | T2 | 5 |
| SB-MFG-013 | Bar stock selection | Selects standard bar, plate and tube sizes from a stock list and applies cut-off allowances. The smallest listed size that contains the part is suggested. | T3 | 5 |
| SB-MFG-014 | Fixture bodies | Marks bodies as fixtures so that they take part in collision checks and simulation but stay out of the bill of materials and stock. Fixtures can be shown or hidden per setup. | T1 | 5 |
| SB-MFG-015 | Workholding library | Provides parametric vises, chucks, jaws, clamps and plates with adjustable dimensions and clamp zones. A clamp zone becomes a no-cut zone for operations. | T2 | 5 |
| SB-MFG-016 | Soft jaw and pocket generation | Subtracts the part with clearance from a blank fixture body to produce a manufacturable jaw or nest. The result is a normal solid with a link to the part. | T3 | 5 |
| SB-MFG-017 | Fixture plate grid | Places fixtures on a hole grid with snap positions and reports the grid holes used. | T3 | 5 |

## Tool and holder libraries

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MFG-018 | Tool definition | Defines tools of every supported type, including end mills, drills, taps, thread mills, face mills, chamfer and engraving tools, probes, turning inserts, and cutting nozzles, with geometry, flutes, material, tool number and offset numbers. | T1 | 5 |
| SB-MFG-019 | Tool geometry validation | Checks tool parameters for consistency, such as shank diameter against cutting diameter and length against stick-out, and generates the revolved profile used by simulation. | T1 | 5 |
| SB-MFG-020 | Holder library | Stores holders as revolved profiles or imported bodies with gauge length, collet and extension data. A tool assembly combines a tool, a holder and a stick-out. | T1 | 5 |
| SB-MFG-021 | Tool library files | Stores tool libraries in a documented, versioned text schema that is readable without the product. Libraries are local, shared through an Arc Hub, or embedded in the document. | T1 | 5 |
| SB-MFG-022 | Library snapshot in document | Keeps a snapshot of every tool used by a document so that programs regenerate identically when the shared library changes. Differences against the library are reported. | T1 | 5 |
| SB-MFG-023 | Tool library exchange | Imports and exports tool data as CSV and in the ISO 13399 data model. Unmapped fields are preserved and listed. | T3 | 5 |
| SB-MFG-024 | Tool search | Filters tools by type, diameter, length, material and tags. | T2 | 5 |
| SB-MFG-025 | Tool usage tracking | Totals the cutting time of each tool across a program and warns when a configured tool life is exceeded. | T3 | 5 |

## Feeds and speeds

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MFG-026 | Cutting data presets | Stores per tool and workpiece material the spindle speed, feed per tooth or per revolution, plunge and ramp feeds, lead feeds, stepover, stepdown and coolant mode. Operations pick the preset that matches their material. | T1 | 5 |
| SB-MFG-027 | Feeds and speeds calculation | Computes spindle speed and feed from a surface speed and a chip load, limited by the spindle and feed limits of the machine. The calculation shows the intermediate values. | T1 | 5 |
| SB-MFG-028 | Power and torque estimate | Estimates cutting power and torque from specific cutting force data of the material and flags operations that exceed the spindle limits of the machine. | T3 | 5 |
| SB-MFG-029 | Chip thinning compensation | Adjusts the feed for radial engagement below half the tool diameter so that chip thickness stays at the preset value. | T2 | 5 |
| SB-MFG-030 | Workpiece material library | Stores machinability data per material and links it to the document material so that presets and estimates follow the part. | T2 | 5 |

## Machine definitions

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MFG-031 | Machine definition | Defines a machine by type, axes, travel limits, spindle speed, power and torque limits, feed limits, tool changer capacity, coolant options and controller profile. The type may be a mill, lathe, mill-turn, router, laser, plasma, waterjet or printer. | T1 | 5 |
| SB-MFG-032 | Kinematic chain | Describes the machine as a chain of linear and rotary joints with offsets and pivots, each carrying a body for simulation. The definition is a documented text schema. | T2 | 5 |
| SB-MFG-033 | Machine library | Stores machines locally or on an Arc Hub. Swapping the machine of a setup regenerates affected operations and lists those that no longer apply. | T2 | 5 |
| SB-MFG-034 | Machine options | Describes optional equipment such as rotary tables, tilting heads and tool presetters, and enables only the operations the chosen options support. | T3 | 5 |
| SB-MFG-035 | Controller profile | Describes the capabilities of a controller dialect, including canned cycles, arc planes, cutter compensation, tool-center-point control and tilted-plane commands. Operations and posts use the profile to choose between native cycles and expanded moves. | T2 | 5 |

## Operations and templates

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MFG-036 | Operation | Creates an operation as a typed object with strategy, tool, geometry selection and parameters, ordered within a setup. Operations can be enabled, disabled, copied and reordered. | T1 | 5 |
| SB-MFG-037 | Operation parameters as expressions | Accepts expressions in operation parameters that use document parameters, tool properties and stock dimensions. The toolpath regenerates when a referenced value changes. | T2 | 5 |
| SB-MFG-038 | Operation templates | Stores sets of operations with tools, cutting data and parameters, and applies a template to a setup or to selected features. Imported template files report fields they cannot map. | T2 | 5 |
| SB-MFG-039 | Folders and patterns | Groups operations in folders and repeats them in linear or circular patterns of work coordinate systems or positions. A pattern regenerates when its definition changes. | T2 | 5 |
| SB-MFG-040 | Manufacturing feature recognition | Detects pockets, slots, bosses, steps and planar faces in the manufacturing model and groups them by size and access direction. Results update when the design changes. | T2 | 5 |
| SB-MFG-041 | Template-based programming | Applies templates to recognized features and shows the proposed operations as a preview. Operations are created only when the user applies the preview. | T3 | 5 |
| SB-MFG-042 | Tool order optimization | Reorders operations across setups to reduce tool changes while keeping dependencies such as roughing before finishing. The proposed order is shown before it is applied. | T2 | 5 |

## Toolpath regeneration and associativity

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MFG-043 | Toolpath regeneration | Computes toolpaths on worker threads from the document revision and tags each result with that revision. Stale results are marked and never applied, and computation is cancellable with visible progress. | T1 | 5 |
| SB-MFG-044 | Associativity with design changes | Stores operation geometry as persistent references and remaps them after design edits. Only operations whose geometry changed become stale, and unresolved references are flagged for repair, not removed. | T1 | 5 |
| SB-MFG-045 | Parameter-driven manufacturing | Lets stock offsets, heights and tool choice reference document parameters so that a parameter change updates the program. | T2 | 5 |
| SB-MFG-046 | Configuration programs | Stores operation sets per configuration and activates the set that matches the active configuration. | T2 | 5 |
| SB-MFG-047 | Toolpath storage | Stores computed toolpaths with their revision tag in the document package or omits them on request. A headless run regenerates any omitted toolpath from the operation definitions. | T2 | 5 |

## Verification

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MFG-048 | Stock simulation | Simulates material removal for all operations in order using the tool and holder, and displays the result colored by operation, tool or remaining thickness. The user can step, play, section and measure. | T1 | 5 |
| SB-MFG-049 | Collision and gouge detection | Detects contact of the tool, shank and holder with the model, stock and fixtures, including rapid moves, and reports the first occurrence with operation, move and location. | T1 | 5 |
| SB-MFG-050 | Deviation compare | Compares the simulated stock with the target model and colors over-cut and remaining material against thresholds. A report lists the maximum gouge and the largest remaining volume. | T1 | 5 |
| SB-MFG-051 | Rest stock from simulation | Provides the simulated stock after earlier operations as input to later operations so that they cut only the remaining material. | T1 | 5 |
| SB-MFG-052 | Cycle time estimate | Estimates time per operation and setup from feeds, rapids, acceleration limits and tool change time, with a configurable correction factor. | T2 | 5 |
| SB-MFG-053 | GPU-assisted simulation | Runs stock simulation on GPU compute with a CPU path that produces results within the stated tolerance. | T2 | 5 |
| SB-MFG-054 | Toolpath analysis display | Colors toolpaths by feed, engagement angle or cutting load to show where the program is slow or heavily loaded. | T3 | 5 |

## Machine simulation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MFG-055 | Machine simulation | Runs the posted program on the kinematic model and reports collisions among machine components, tool, holder, fixtures and stock. | T2 | 5 |
| SB-MFG-056 | Axis limit checks | Reports axis travel violations, rotary axis flips and positions near a kinematic singularity, with the operation and move that cause them. | T2 | 5 |
| SB-MFG-057 | NC code verification | Interprets an NC file in the dialect of its controller profile and simulates it independently of the toolpaths. This verifies edited or imported programs. | T2 | 5 |

## Post-processor system

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MFG-058 | Post-processor definition format | Describes output through a documented, versioned text format of word formats, block templates, event handlers and properties. The format is readable and editable without the product. | T1 | 5 |
| SB-MFG-059 | Post scripting hooks | Allows event handlers to run logic written in a restricted expression language or a sandboxed WebAssembly module with no file or network access. Resource limits apply to every handler. | T2 | 5 |
| SB-MFG-060 | Bundled post library | Ships posts for ISO 6983 G-code, open controller dialects and common industrial dialects, each paired with a controller profile. | T1 | 5 |
| SB-MFG-061 | Post properties | Exposes typed properties such as program number, sequence numbers, safe retract, coolant mode, tool change style, arc output and output tolerance. Property values are stored with the NC program. | T1 | 5 |
| SB-MFG-062 | Post editing and testing | Edits a post with validation against the schema, runs it on a sample toolpath and shows a difference against the previous output. | T3 | 5 |
| SB-MFG-063 | Post regression corpus | Ships each bundled post with a regression toolpath and expected output that tests compare on every change. | T3 | 5 |

## NC programs and setup documents

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MFG-064 | NC program | Collects operations or setups in order and assigns a post, an output folder, file naming, program number and sub-program rules. A program can produce one file per setup, tool or operation. | T1 | 5 |
| SB-MFG-065 | Post processing run | Produces NC files deterministically from the toolpaths and the post and reports unsupported items. The same run is available through the command line without a dialog. | T1 | 5 |
| SB-MFG-066 | NC code viewer | Shows the generated code with syntax highlighting and links each line to its toolpath move in both directions. | T2 | 5 |
| SB-MFG-067 | Program transfer | Sends output to a machine through documented transfer plug-ins; open-protocol transfer is specified in the shop-floor connection rows below. No transfer happens without an explicit command. | T3 | 5 |
| SB-MFG-068 | Setup sheet | Renders a setup sheet from a template with setup views, stock, work offsets, operation list, tools, holders, stick-out, estimated times and notes, as HTML or PDF. | T1 | 5 |
| SB-MFG-069 | Setup sheet templates | Provides sheet templates that read document properties and fields, in a documented template format. | T2 | 5 |
| SB-MFG-070 | Tool and operation lists | Exports tool lists with assembly lengths for presetting and operation lists as CSV. | T2 | 5 |
| SB-MFG-071 | Manufacturing notes | Attaches text and images to setups and operations and includes them in setup sheets. | T3 | 5 |
| SB-MFG-072 | Headless manufacturing run | Regenerates all toolpaths and produces NC files and setup sheets for a document from the command line or an Arc Hub job. | T2 | 5 |

## Shop-floor connection

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MFG-073 | Machine connection | Records the network endpoint, protocol (MTConnect or OPC UA), security mode and credential reference of a machine on its machine definition. Credentials are held in the operating system's secret store, never in the document. | T3 | 5 |
| SB-MFG-074 | Program transfer over OPC UA | Sends an NC program to a machine that exposes file transfer through OPC UA, after an explicit command that names the program, the machine and the target. Transfers use an encrypted, authenticated session. | T3 | 5 |
| SB-MFG-075 | Transfer verification | Compares a checksum of the transferred program with the posted file and records the result, the time and the user in the NC program history. A failed transfer leaves the program marked as not delivered. | T3 | 5 |
| SB-MFG-076 | Machine status through MTConnect | Reads execution state, controller mode, active program, part count and alarms from an MTConnect agent and shows them in a machine panel. | T3 | 5 |
| SB-MFG-077 | Machine status through OPC UA | Subscribes to the same status items through OPC UA nodes mapped by a documented node map, with reconnect handling. | T3 | 5 |
| SB-MFG-078 | Read-only status | Status connections never start, stop or change a machine. Only the explicit transfer command writes to a machine. | T3 | 5 |
| SB-MFG-079 | Cycle data feedback | Stores observed cycle times of a program from machine status and offers them as input to the correction factor of the cycle time estimate (SB-MFG-052). | T3 | 5 |
| SB-MFG-080 | Headless transfer and status | Runs transfers and status queries from the command-line runner and as Arc Hub jobs with the same explicit-command rule and a machine-readable result. | T3 | 5 |
