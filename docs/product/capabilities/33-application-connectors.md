# Application connectors

Application connectors exchange mechanical designs with the native files of other applications whose formats have no published specification. A connector is a small add-in that runs inside the user's own licensed copy of the other application, uses only that application's public programming interface and reads or writes the Arc exchange package, an open container specified in [the interchange architecture](https://github.com/ACKaraca/arc-core/blob/main/docs/architecture/interchange.md). The decision is recorded in [suite ADR-0017](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0017-exchange-connectors-and-licensed-bridges.md). Format-level exchange is specified in [interoperability](19-interoperability.md) and in the [interoperability architecture](../../architecture/interoperability.md).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Connector | Host application kind, host version range, connector version, capabilities offered | Runs in the host; speaks the exchange package |
| Exchange package | Schema version, content manifest, geometry, structure, parameters, properties, identifiers, connector log | Container read and written by connectors and by SolidBuilder |
| Round-trip map | Host object identifiers, SolidBuilder IDs, content hashes, last exchanged revision | Stored in the import record; used for repeated exchange |
| Exchange session | Direction, scope, fidelity level, status, loss report | Created per pull or push; produces a change set |
| Licensed bridge | Library name and version, user-supplied location, license status | Separate process called by a reader; optional |
| Open reader | Format, reader crate, version | Reads a format that has a published specification or a permissive reader |

## Connector model

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-CONNECT-001 | Connector contract | Defines what a connector must provide: the host kinds and versions it supports, the content classes it can read and write, and the fidelity it reaches for each. SolidBuilder lists these before an exchange starts. | T3 | 5 |
| SB-CONNECT-002 | Public interface only | A connector uses only the public programming interface of the host and the host's documented file operations. It does not read the host's files directly and does not bypass the host's licensing. | T3 | 5 |
| SB-CONNECT-003 | Exchange package | Reads and writes the exchange package, which holds geometry, structure, parameters, properties, identifiers and a manifest of content classes with a per-item status. | T3 | 5 |
| SB-CONNECT-004 | Connector discovery | Lists installed connectors with their host, version and state, and reports a host version that is outside the supported range with the capabilities that remain. | T3 | 5 |
| SB-CONNECT-005 | Transfer by file or session | Exchanges packages as files that the user moves, or through a local session between the host and SolidBuilder on one machine. No data leaves the machine unless the user sends the package. | T3 | 5 |
| SB-CONNECT-006 | Exchange session record | Records each exchange with direction, scope, host and connector versions, fidelity reached and the loss report, in the document journal. | T3 | 5 |
| SB-CONNECT-007 | Headless exchange | Imports and compares packages in the command-line runner and as Arc Hub jobs. Running the host side stays with the user's own installation. | T3 | 5 |

## Content exchanged

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-CONNECT-008 | Parts | Reads parts as exact solids, sheets and surfaces with names, appearances and material assignments, and writes SolidBuilder parts back as bodies to the host. | T3 | 5 |
| SB-CONNECT-009 | Assemblies | Reads assembly structure with occurrences, transforms, suppression and visibility, and writes assemblies back with the same structure. | T3 | 5 |
| SB-CONNECT-010 | Mates and joints | Reads the assembly relations that the host exposes and maps them to joints and constraints, reporting relations it cannot map. | T3 | 5 |
| SB-CONNECT-011 | Feature trees | Where the host interface exposes the feature tree, reads each feature with its type, inputs and order and maps it to a SolidBuilder feature when an equivalent exists. Other features become base features that carry the host geometry. | T3 | 5 |
| SB-CONNECT-012 | Feature tree writing | Where the host interface allows creating features, writes mapped features back in order. Features without a host equivalent are written as bodies and reported. | T3 | 5 |
| SB-CONNECT-013 | Sketches | Reads sketch geometry and constraints and dimensions where the host exposes them, and writes sketches back with constraints that the host can represent. | T3 | 5 |
| SB-CONNECT-014 | Parameters | Exchanges named parameters, equations and linked values with units, and updates either side from a reviewed change set. | T3 | 5 |
| SB-CONNECT-015 | Configurations | Reads configurations or variants with their parameter values and suppression states and maps them to configurations and model states. | T3 | 5 |
| SB-CONNECT-016 | Drawings | Reads drawing sheets with views, dimensions, notes, tables and title block fields as drawing objects, with a fallback to geometry where an entity has no equivalent. Writes drawings back as geometry and text. | T3 | 5 |
| SB-CONNECT-017 | Properties | Exchanges custom and standard properties, mass properties and material data and maps them through the property mapping of the document. | T3 | 5 |
| SB-CONNECT-018 | Reference geometry | Exchanges datum planes, axes, points and coordinate systems with their names. | T3 | 5 |
| SB-CONNECT-019 | Bill of materials | Reads the bill of materials of a host assembly and compares it with the derived bill of materials of the mapped assembly. | T3 | 5 |
| SB-CONNECT-020 | External file references | Preserves references between host files, such as part files inside an assembly, as external references and resolves them through the workspace. | T3 | 5 |
| SB-CONNECT-021 | Appearances and decals | Exchanges colors, materials and textures as appearances on the Arc Scene material model. | T3 | 5 |

## Identifiers and repeated round trips

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-CONNECT-022 | Persistent host identifiers | Stores the host's object identifiers in the round-trip map beside each SolidBuilder ID, so that every later exchange matches the same objects. | T3 | 5 |
| SB-CONNECT-023 | Face and edge matching | Matches faces and edges between exchanges by host identifiers when the host provides them, and otherwise by geometric signature with a confidence value. Ambiguous matches are listed, never applied silently. | T3 | 5 |
| SB-CONNECT-024 | Incremental exchange | Reads only objects whose content hash changed since the last exchange and applies the differences as a change set. | T3 | 5 |
| SB-CONNECT-025 | Change set review | Lists additions, removals and modifications from either side for review and applies accepted items in one transaction. Rejected items stay on record. | T3 | 5 |
| SB-CONNECT-026 | Downstream references | After an incoming change, re-resolves persistent references of features, drawings, joints and manufacturing operations and reports those that broke. | T3 | 5 |
| SB-CONNECT-027 | Ownership rules | Assigns attribute groups to one side, such as geometry to the host and manufacturing data to SolidBuilder, and reports a conflict when the other side changes it. | T3 | 5 |
| SB-CONNECT-028 | Exchange history | Stores each exchanged state, allows rolling back an import and compares two exchanged revisions in three dimensions. | T3 | 5 |

## Fidelity and loss

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-CONNECT-029 | Fidelity levels | States for each content class whether the exchange reaches view, data or edit fidelity for the connected host and version. | T3 | 5 |
| SB-CONNECT-030 | Loss report | Lists features, relations, entities and attributes that were approximated, kept as foreign data or dropped, with counts and severity. | T3 | 5 |
| SB-CONNECT-031 | Preserved foreign data | Keeps content without an equivalent as preserved foreign data and returns it to the host on the next push where the host interface allows. | T3 | 5 |
| SB-CONNECT-032 | Geometry verification | Compares volume, area and topology counts of every exchanged body with the source value reported by the connector and flags differences beyond tolerance. | T3 | 5 |
| SB-CONNECT-033 | Host version changes | Detects that the host file or interface version changed since the last exchange and reports changed behavior before applying results. | T3 | 5 |

## Licensed bridges and open readers

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-CONNECT-034 | Licensed bridge | Calls a user-installed, user-licensed conversion library as a separate process for a format whose vendor or consortium licenses one. The library is never distributed with SolidBuilder and no core workflow requires it. | T3 | 5 |
| SB-CONNECT-035 | Bridge setup and status | Lets the user point to an installed library, checks version and license state, and shows the formats that become available. | T3 | 5 |
| SB-CONNECT-036 | Bridge isolation | Runs the bridge with no network access and bounded memory and time, and treats its output as untrusted input with import healing and a loss report. | T3 | 5 |
| SB-CONNECT-037 | Open reader for 3DM | Reads NURBS curves, surfaces, polysurfaces, layers, names, colors and user attributes from 3DM natively through a permissively licensed reader, with a loss report. | T3 | 5 |
| SB-CONNECT-038 | Open readers by specification | Reads other formats natively when a published specification or a permissively licensed reader exists, each behind its own format crate, with a fuzzed reader and a corpus. | T3 | 5 |
| SB-CONNECT-039 | Excluded content | Reverse engineering of undocumented formats, execution of compiled or protected script files of other applications and codecs whose license forbids open distribution are not provided. The user is directed to a connector, a bridge or an open format. | T3 | 5 |
