# Properties, materials and bills of materials

Properties attach identity and engineering data to components; materials provide the physical data
behind mass properties and analysis; and the bill of materials derives product structure and
quantities from the assembly so drawings, Arc Hub items and exports stay consistent with the model.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Property definition | Key, label, type (text, number with unit, date, boolean, picklist, expression), scope (document, library, Hub), default | Defines property values; Hub-scoped definitions come from Arc Hub |
| Property value | Definition, value or expression, source (user, parameter, computed) | Owned by a component definition, configuration or occurrence |
| Material | Name, density, elastic constants, thermal properties, strength values, appearance link, standard designation | Assigned to bodies and components; read by mass properties, simulation and manufacturing |
| Bill of materials | View kind (structured, parts-only, model data), configuration, item numbering, column set | Derived from an assembly; feeds drawing tables and Hub items |
| BOM item | Item number, component definition and configuration, quantity, unit, structure kind, overrides | Row in a bill of materials |
| Virtual component | Name, properties, quantity, unit, optional mass | BOM-only item with no geometry |

## Properties

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-BOM-001 | Standard property sets | Every component definition has part number, description, revision, designer, project, status and creation fields, plus computed physical fields; values are editable in a property panel. | T1 | 3 |
| SB-BOM-002 | Custom properties | Users add properties of type text, number with unit, date, boolean or picklist; a value may be an expression over parameters and other properties. | T1 | 3 |
| SB-BOM-003 | Property definitions | Property definitions with type, allowed values and defaults can be shared through a library file or provided by Arc Hub, so every document uses the same keys. | T2 | 3 |
| SB-BOM-004 | Per-configuration values | Property values can differ per configuration and per occurrence where the definition allows it. | T1 | 3 |
| SB-BOM-005 | Bulk edit | Selected components in the model tree or bill of materials can have a property set in one transaction, with a preview of old and new values. | T2 | 3 |
| SB-BOM-006 | Property search | Components in the open documents and in the local library are searchable by property value, part number and description. | T2 | 3 |
| SB-BOM-007 | Property mapping | Property keys can be mapped to Arc Hub item fields and to export fields (STEP, IFC, CSV), with the mapping stored per document or library. | T2 | 3 |

## Materials and mass properties

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-BOM-008 | Material library | Provides a library of engineering materials with density, elastic modulus, Poisson ratio, yield and ultimate strength, thermal conductivity, expansion and specific heat; users add libraries as documented files. | T1 | 3 |
| SB-BOM-009 | Assign material | Assigns a material to a component or to individual bodies, overriding the component default; the appearance follows the material unless overridden. | T1 | 3 |
| SB-BOM-010 | Library update | Detects when a material in a document differs from its library source and offers to update one or all uses. | T2 | 3 |
| SB-BOM-011 | Mass properties | Computes mass, volume, surface area, center of gravity and the inertia tensor about the center and a chosen coordinate system at a selected accuracy, updating after regeneration. | T1 | 3 |
| SB-BOM-012 | Mass overrides | Mass, center of gravity and inertia can be overridden per component or per virtual component, and the override is marked wherever the values are shown. | T2 | 3 |
| SB-BOM-013 | Assembly roll-up | Assembly mass properties roll up from occurrences, honoring overrides, suppression and BOM exclusion. | T1 | 3 |

## Bill of materials

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-BOM-014 | Structured view | Shows the assembly as a multi-level bill of materials with item numbers, quantities and chosen columns; it updates when occurrences are added, removed or reconfigured. | T1 | 3 |
| SB-BOM-015 | Parts-only view | Shows one row per unique component definition and configuration with total quantity across all levels. | T1 | 3 |
| SB-BOM-016 | Model data view | Shows every occurrence including suppressed and excluded ones, for checking why an item is or is not in the other views. | T2 | 3 |
| SB-BOM-017 | Structure kinds | Each component has a structure kind: normal, phantom (children promoted to the parent level), reference (excluded), purchased (children hidden), or inseparable (treated as one item). | T1 | 3 |
| SB-BOM-018 | Virtual components | Adds BOM items without geometry, such as paint, grease or labels, with quantity, unit and optional mass. | T1 | 3 |
| SB-BOM-019 | Quantity units | Items can be counted in each, length, area, volume or mass, with a base quantity expression such as the cut length of a frame member. | T2 | 3 |
| SB-BOM-020 | Merge and unmerge rows | Rows with the same part number from different definitions can be merged into one item and split again. | T2 | 3 |
| SB-BOM-021 | Item numbering | Item numbers are assigned in structure order, can be locked, and are renumbered on request with a chosen start and step; balloons follow item numbers. | T1 | 3 |
| SB-BOM-022 | Quantity overrides | An item quantity can be overridden manually; overrides are flagged and survive regeneration until cleared. | T2 | 3 |
| SB-BOM-023 | Exclusions | Occurrences or definitions can be excluded from the bill of materials or from mass roll-up, for example fixtures and reference geometry. | T1 | 3 |
| SB-BOM-024 | Cost fields | Items carry estimated unit cost and currency; costs roll up through the structure, and overrides are flagged. | T2 | 4 |
| SB-BOM-025 | Per-configuration BOM | The bill of materials is evaluated for any configuration and model state of the assembly. | T1 | 3 |
| SB-BOM-026 | BOM comparison | Compares two bills of materials (versions, branches or configurations) and lists added, removed and changed items with quantity and property differences. | T2 | 4 |
| SB-BOM-027 | Composition state | Each item shows a lifecycle state (working, pending, released) synchronized from Arc Hub; released items are read-only in the document. | T2 | 3 |
| SB-BOM-028 | Export | Exports any view of the bill of materials to CSV, XLSX or JSON with chosen columns, grouping and units. | T1 | 3 |
| SB-BOM-029 | Transfer to Hub items | Sends the bill of materials to Arc Hub product data as items and structure, matching existing items by part number and reporting new and changed items. | T2 | 3 |
| SB-BOM-030 | Saved column sets | Column sets, sort orders and filters can be saved as named BOM views and reused by drawing tables. | T2 | 3 |
