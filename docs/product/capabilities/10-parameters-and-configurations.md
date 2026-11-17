# Parameters, configurations and states

Parameters carry design intent as named, unit-aware values and expressions that drive sketches,
features, assemblies and properties. Configurations derive product variants from one definition, and
model, display and position states describe stages and presentations of a single variant.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Parameter | Stable ID, name, kind (model, user, reference, linked, derived), value type (length, angle, mass, unitless, text, boolean, list), expression, current value, tolerance, favorite, comment, group, export flag | Owned by a component definition; read by features, sketches, joints, properties, rules |
| Expression | Text, parsed form, unit, referenced parameters | Evaluated by the expression engine; part of the dependency graph |
| Parameter link | Source (document, spreadsheet range), key, refresh policy | Feeds linked parameters |
| Configuration table | Columns (aspects), rows (configurations), default row | Owned by a component definition |
| Configuration | Name, row values, part number, generated member flag | Variant of a component definition; selected per occurrence, drawing view and bill of materials |
| Configuration rule | Condition, constraint or computed value over table aspects | Restricts and derives configuration values |
| Model state | Name, suppression overrides, parameter overrides, substitute flag | Stage of one configuration (for example as-cast, as-machined) |
| Display state | Visibility, appearance overrides, camera | Presentation of an assembly or part |
| Position state | Joint values, grounded overrides | Saved mechanism pose |

## Parameters

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-PARAM-001 | Parameter table | Lists model parameters created by dimensions and features alongside user, reference, linked and derived parameters with name, unit, expression, value, tolerance, comment and favorite; values edited in the table regenerate the model in one transaction. | T1 | 3 |
| SB-PARAM-002 | User parameters | Creates named parameters with a unit type and expression; names are unique within a component definition and renaming updates every expression that uses them. | T1 | 3 |
| SB-PARAM-003 | Unit-aware expressions | Expressions mix units, check dimensional consistency and convert automatically; an inconsistent expression is rejected with the position of the error. | T1 | 3 |
| SB-PARAM-004 | Functions | Expressions support arithmetic, trigonometric, exponential, logarithmic, rounding, minimum, maximum, absolute value, sign and modulo functions. | T1 | 3 |
| SB-PARAM-005 | Conditional expressions | Expressions support comparisons, boolean operators and if-then-else, so a value can switch on another parameter. | T2 | 3 |
| SB-PARAM-006 | Text, boolean and list parameters | Text parameters drive sketch text, names and properties; boolean parameters drive suppression; list parameters restrict a value to a defined set shown as a drop-down. | T2 | 3 |
| SB-PARAM-007 | Reference parameters | Driven dimensions and measurements appear as read-only reference parameters that other expressions can use. | T1 | 3 |
| SB-PARAM-008 | Organization | Parameters can be grouped, marked favorite, commented and filtered by used, unused, favorite or kind; unused parameters can be purged after a preview. | T2 | 3 |
| SB-PARAM-009 | Export as property | A parameter marked for export appears as a property value of the component and in the bill of materials, updating with the parameter. | T2 | 3 |
| SB-PARAM-010 | Spreadsheet links | Parameters can be linked to a named range in a CSV or XLSX file; changes in the file are detected on open or on refresh and applied in one transaction. | T2 | 4 |
| SB-PARAM-011 | Cross-document links | Parameters can be linked from another component definition or document; the link shows its source and refreshes according to the reference policy. | T2 | 3 |
| SB-PARAM-012 | Derived parameters | When a component is derived, selected parameters of the source are carried as derived parameters that follow the source. | T2 | 3 |
| SB-PARAM-013 | Dependency graph | Shows for any parameter what it drives and what drives it, across components and documents, and flags cycles before they are committed. | T2 | 3 |
| SB-PARAM-014 | Parameter tolerances | Each length or angle parameter can carry a tolerance or fit designation; the model can be evaluated at nominal, upper, lower or mid tolerance. | T2 | 4 |
| SB-PARAM-015 | Display of dimension values | Dimensions can display value, name, expression or name with value, set per document. | T1 | 3 |

## Configurations

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-PARAM-016 | Configuration table | Creates a table whose rows are configurations and whose columns are configurable aspects; one row is the default. Cells accept values and expressions. | T1 | 3 |
| SB-PARAM-017 | Configurable aspects | Columns can be parameters, feature or body suppression, visibility, material, appearance, sheet metal rule, plastic rule, property values including part number, choice of configured subcomponent, and joint, hole or thread options. | T1 | 3 |
| SB-PARAM-018 | Edit scope | Every edit applies to the active configuration, to all configurations or to a selected list, chosen in the command panel; the scope is shown while editing. | T1 | 3 |
| SB-PARAM-019 | Quick configuration panel | A compact panel shows the key aspects of the active configuration and lets the user change them in the modeling view without opening the table. | T2 | 3 |
| SB-PARAM-020 | Per-occurrence configuration | Each occurrence selects a configuration of its definition; changing it updates geometry, joints referencing persistent names and the bill of materials. | T1 | 3 |
| SB-PARAM-021 | Part numbers per configuration | Each configuration has its own part number and description, or shares those of a named group of configurations. | T1 | 3 |
| SB-PARAM-022 | Configuration rules | Rules declare allowed combinations, value ranges and computed values between aspects; invalid combinations are rejected with the violated rule named. | T2 | 4 |
| SB-PARAM-023 | On-demand configurations | A configuration can be requested by aspect values through the interface or automation without adding a stored row; it is computed, validated against rules and optionally saved. | T3 | 4 |
| SB-PARAM-024 | Table-driven families | A configured component can be published as a library family whose rows become selectable sizes in the standard parts library. | T2 | 4 |
| SB-PARAM-025 | Generated member documents | Configurations can be written out as separate member documents on demand, each linked back to its row for update. | T2 | 4 |
| SB-PARAM-026 | Spreadsheet-backed tables | A configuration table can be imported from or exported to CSV or XLSX, and optionally linked for refresh. | T2 | 4 |
| SB-PARAM-027 | Configuration comparison | Shows the differences between two configurations by aspect, and highlights differing geometry in the viewport. | T2 | 4 |

## States

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-PARAM-028 | Model states | Creates named model states of a configuration that override suppression and parameter values to represent manufacturing stages; one state is primary. | T2 | 3 |
| SB-PARAM-029 | Substitute states | A model state can be marked substitute, holding a simplified representation that is used in place of the full model where chosen. | T2 | 3 |
| SB-PARAM-030 | Display states | Saves visibility, appearance overrides and camera as named display states selectable in the viewport, drawings and exports. | T1 | 3 |
| SB-PARAM-031 | Position states | Saves joint values and grounded overrides as named position states; drawings can show several position states in one view. | T2 | 3 |
| SB-PARAM-032 | Nested state selection | An assembly state can select configurations and states of its subcomponents, so a left-hand and right-hand variant propagate through levels. | T2 | 4 |
| SB-PARAM-033 | State per consumer | Drawing views, bill of materials instances, exports and occurrences each select configuration, model state and display state independently. | T1 | 3 |
| SB-PARAM-034 | Open with state | A document can be opened in a chosen configuration, model state and display state, loading only what that state needs. | T2 | 3 |
