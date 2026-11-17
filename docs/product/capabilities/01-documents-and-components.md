# Documents, components and workspace

This area defines how SolidBuilder organizes design data: documents, component definitions, occurrences
and bodies, and the choice between keeping parts, assemblies and drawings in one document or in
separate files. It also covers the shared workspace every other area relies on: the model tree,
command access, navigation, display, selection, measurement and the safety net of undo, autosave and
recovery.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Document | Stable ID, format version, document settings (length, angle and mass display units, precision, up axis, local origin), structure mode, embedded component definitions, drawings, thumbnails | Contains component definitions, drawings and saved views; stores external references to other documents; persisted as one `.solidb` package |
| Component definition (Part, Assembly) | Stable ID, name, kind (part or assembly), part number, origin and default reference geometry, history enabled flag, properties | A part owns bodies, sketches and a feature history; an assembly owns occurrences, joints and assembly features; may be internal to a document or referenced from another document |
| Occurrence | Stable ID, referenced component definition, transform, grounded flag, visibility, suppression, selected configuration and display state | Places one component definition inside an assembly; many occurrences may share one definition |
| Body | Stable ID, kind (solid, sheet, wire, mesh, subdivision), material, appearance, owning feature | Belongs to one part; produced and modified by features; may be promoted to its own component definition |
| External reference | Target document ID and component ID, pinned version or latest, resolution state (resolved, out of date, missing) | Links an occurrence or derived component to a definition stored in another document |
| Derived component | Source component and version, selected bodies, sketches, parameters and reference geometry, scale, mirror plane, link state | A part feature that copies selected source content into a new part and updates when the source changes |
| Document template | Units, standards, properties, default reference geometry, default materials, drawing sheet formats | Source from which a new document or component definition is created |
| Saved view | Camera, projection, visibility set, section planes, display style | Stored in a document; referenced by drawings, 3D annotations and render scenes |
| Selection set | Name, ordered list of persistent references to bodies, faces, edges, occurrences or features | Stored in a component definition; used as input to commands and scripts |
| Measurement | Inputs (persistent references), result values with units, display placement | Optional saved object that recomputes after regeneration |

## Document structure

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DOC-001 | Single-document project | A document holds any number of part definitions, assembly definitions and drawings. Creating a part, assembly or drawing inside an open document adds it to the model tree without creating a file, and the whole document saves atomically as one `.solidb` package. | T1 | 3 |
| SB-DOC-002 | Separate-file structure | A component definition can live in its own document and be placed through an external reference. The user chooses per definition whether it is internal or external, and the choice is recorded in the document so the same design can be organized either way. | T1 | 3 |
| SB-DOC-003 | Move definition between documents | A command moves an internal component definition to a new document (or an external one back into the current document), preserving its stable ID, history, configurations and every reference that pointed at it. References in other open documents are rewritten in the same transaction and listed in a report. | T2 | 3 |
| SB-DOC-004 | Structure mode choice | When a design starts, the user selects part, assembly or mixed structure. Part mode allows bodies and features in one definition, assembly mode allows only occurrences, joints and assembly features at the top level, and mixed mode allows both; the mode can be changed later with a validation report of objects that do not fit the new mode. | T2 | 3 |
| SB-DOC-005 | Document settings | Each document stores display units for length, angle, mass and time, display precision, up axis and default material. Changing a display unit changes only presentation; stored values stay in SI units and no geometry changes. | T1 | 3 |
| SB-DOC-006 | Document templates | New documents and new component definitions start from a template that supplies units, properties, drawing standard, default materials and sheet formats. Templates are ordinary documents marked as templates and can be stored locally or in a shared library. | T1 | 3 |
| SB-DOC-007 | Application preferences | Global preferences cover input mapping, display, default units for new documents, file locations, autosave interval and graphics options. Preferences export to and import from a documented text file so a team can share them. | T1 | 3 |
| SB-DOC-008 | Document thumbnails | Saving writes a thumbnail rendered from the current or a chosen saved view, with a selectable plain, gradient or image background. The operating-system file browser shows the thumbnail through a documented preview handler where the platform supports one. | T2 | 3 |
| SB-DOC-009 | Display name and file name | Document tabs and the model tree show either the file name or the root component's display name, as set in preferences. Renaming the display name never renames the file. | T2 | 3 |
| SB-DOC-010 | Package size control | Saving compacts unused storage and stores derived data (tessellation caches, drawing view caches) separately from authored data, so the authored part of a document stays small and caches can be discarded and rebuilt. | T2 | 3 |

## Components and occurrences

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DOC-011 | Create component | A command creates an empty part or assembly definition, internal or in a new document, and places one occurrence of it in the active assembly at the origin or at a picked position. | T1 | 3 |
| SB-DOC-012 | Active component | Exactly one component definition is active for editing; new sketches, features and bodies are created in it. The model tree marks the active component and the viewport dims geometry outside it. | T1 | 3 |
| SB-DOC-013 | Occurrences share a definition | Editing a component definition updates every occurrence of it in every open assembly after regeneration. An occurrence can be made independent, which copies the definition and repoints that occurrence only. | T1 | 3 |
| SB-DOC-014 | Insert existing component | The user inserts a component from the current document, another document or a library into the active assembly as an internal copy or as an external reference, choosing version (latest or pinned) and configuration. | T1 | 3 |
| SB-DOC-015 | Restructure by drag | Dragging occurrences in the model tree moves them between subassemblies, creates a subassembly from a selection, or dissolves a subassembly into its parent. Joints and constraints are kept, and those that cannot be kept are listed before the change is applied. | T2 | 3 |
| SB-DOC-016 | Break external link | Breaking an external reference converts the referenced definition into an internal copy at its current version. The source document is not modified. | T2 | 3 |
| SB-DOC-017 | Reference resolution | On open, every external reference resolves by stable document ID first, then by recorded path and then by search folders. Unresolved references are listed with actions to locate, replace or skip, and the document opens with placeholders for missing content without data loss. | T1 | 3 |
| SB-DOC-018 | Out-of-date references | When a referenced document has a newer version than the one loaded, the occurrence shows an out-of-date state. The user updates selected references or all of them; references pinned to a version update only when the pin is changed. | T1 | 3 |

## Multi-body parts and derivation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DOC-019 | Multi-body parts | A part can contain any number of solid, sheet, wire, mesh and subdivision bodies, each a model tree node with its own name, material and appearance. Each feature lists the bodies it participates in. | T1 | 3 |
| SB-DOC-020 | Body visibility and grouping | Bodies can be shown, hidden, isolated and placed in folders. Visibility is a display state setting and never changes geometry or mass properties. | T1 | 3 |
| SB-DOC-021 | Bodies to components | A command creates one component definition per selected body (internal or external) and an assembly that places them in their original positions. The new parts stay linked to the source bodies and update when the multi-body part changes, until the link is broken. | T2 | 3 |
| SB-DOC-022 | Extract part from bodies | Selected bodies are copied into a new part definition through a derived component, with an option to remove them from the source. The extracted part keeps its own history from that point. | T2 | 3 |
| SB-DOC-023 | Derived component | A derived component copies selected bodies, sketches, reference geometry and parameters from a source part or assembly into a part. Options include uniform scale, mirror about a plane, and per-item inclusion; the result updates when the source changes and records the source version. | T2 | 3 |
| SB-DOC-024 | Derived assembly simplification | Deriving from an assembly can merge all bodies into one solid, keep one body per occurrence, or remove occurrences below a size threshold. The derived part updates when the assembly changes. | T2 | 4 |
| SB-DOC-025 | Derived link control | Each derived component shows its link state (current, out of date, broken). The user updates, pins to a version, replaces the source with another compatible component, or breaks the link to freeze the copied geometry. | T2 | 3 |
| SB-DOC-026 | Cross-document geometry link | A sketch or feature can reference geometry from another component through a persistent cross-component reference. When the source changes, dependents update on regeneration; a broken cross-component reference is reported with a repair action. | T2 | 3 |

## Workspace and command access

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DOC-027 | Model tree | A dockable tree shows document settings, saved views, origin, components, occurrences, bodies, sketches, reference geometry, joints and the feature history. It supports rename, visibility, isolate, activate, search by name or type, and full keyboard navigation with screen-reader names. | T1 | 3 |
| SB-DOC-028 | Command search | A palette opened by one key finds any registered command by label, alias or description and runs it. Results show the command's shortcut and are filtered to commands enabled in the current context. | T1 | 3 |
| SB-DOC-029 | Radial context menu | Right-click opens a radial menu with context-specific commands, repeat-last-command, undo and delete, plus a linear overflow menu. Menu contents are configurable per selection type. | T2 | 3 |
| SB-DOC-030 | Keyboard shortcuts and aliases | Every command can be bound to a key chord and to typed aliases. Bindings are stored in a documented file, conflicts are reported, and preset maps reproduce common navigation and shortcut habits under neutral names. | T1 | 3 |
| SB-DOC-031 | Command panels | Commands open a non-blocking panel with selection fields, numeric fields that accept expressions with units, on-canvas manipulators and live preview. Enter applies, Escape cancels, and selection advances to the next required field automatically. | T1 | 3 |
| SB-DOC-032 | Workspace layouts | Panels dock, float and tab; named layouts save and restore panel arrangement per task (modeling, drawing, review). Layouts are per user and never stored in documents. | T2 | 3 |

## Navigation and display

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DOC-033 | View navigation | Orbit, pan, zoom, zoom to fit, zoom to selection and look-at-face are available by mouse, keyboard and touchpad gestures. Orbit pivots around the point under the cursor, and navigation never alters the document. | T1 | 3 |
| SB-DOC-034 | Orientation cube | An on-screen cube shows the current orientation and snaps the camera to faces, edges and corners when clicked. The user can redefine the front and top directions per document. | T1 | 3 |
| SB-DOC-035 | Six-degree-of-freedom input devices | Supported 3D navigation devices move the camera in all six axes with configurable sensitivity and dominant-axis filtering. Device buttons can be bound to commands. | T2 | 3 |
| SB-DOC-036 | Projection and camera | The viewport switches between perspective and orthographic projection and exposes field of view. The current camera can be saved as a saved view. | T1 | 3 |
| SB-DOC-037 | Display styles | Styles include shaded, shaded with visible edges, shaded with hidden edges, wireframe, hidden-line removed and realistic. Optional effects are ambient occlusion, shadows, reflections, silhouette edges and ground plane; tangent edges can be shown, dimmed or hidden. | T1 | 3 |
| SB-DOC-038 | Saved views | A saved view stores camera, projection, visibility, section planes and display style. Restoring a saved view reapplies all of them, and saved views are available to drawings, annotations and render scenes. | T1 | 3 |
| SB-DOC-039 | Component color cycling and opacity | A display toggle colors each occurrence distinctly for inspection, and per-occurrence opacity can be set. Both are display state settings that do not change appearances. | T2 | 3 |
| SB-DOC-040 | Graphics diagnostics | A diagnostics panel reports the graphics adapter, driver, frame time, triangle count and memory use, and can export the report as a text file for support. | T2 | 3 |

## Selection

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DOC-041 | Selection filters | Filters restrict picking to occurrences, bodies, faces, edges, vertices, sketch entities, reference geometry or features, with a priority mode that prefers one type. The active filter is shown in the status bar. | T1 | 3 |
| SB-DOC-042 | Window and crossing selection | Dragging left to right selects objects fully inside the rectangle; right to left selects objects touching it. Freeform lasso selection and paint selection across faces are also available. | T1 | 3 |
| SB-DOC-043 | Select other | Holding the cursor over stacked geometry opens a list of every candidate under the cursor; choosing an entry selects it and highlights it in the viewport. | T1 | 3 |
| SB-DOC-044 | Chain and tangent selection | Selecting an edge or face can extend to the tangent chain, the loop, the face boundary or all faces of a feature. The extension mode is chosen in the selection panel. | T1 | 3 |
| SB-DOC-045 | Named selection sets | A selection can be saved as a named selection set of persistent references. Sets survive regeneration and are usable as inputs to commands, scripts and rules. | T2 | 3 |

## Measurement and inspection

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DOC-046 | Measure | Measures distance (minimum, center to center, along axis), angle, length, radius, diameter, area, perimeter and loop length between any two selectable entities, including cones and between components. Results are shown in display units with X, Y and Z deltas and can be copied as values or expressions. | T1 | 3 |
| SB-DOC-047 | Persistent measurement | A measurement can be saved as an object that recomputes after regeneration and can drive a reference parameter. Saved measurements whose inputs no longer resolve are flagged in the health list. | T2 | 3 |
| SB-DOC-048 | Mass properties display | For selected bodies, components or the whole assembly, the panel shows mass, volume, surface area, center of mass and inertia tensor about the center of mass and about a chosen coordinate system. A center-of-mass glyph can be shown in the viewport and follows regeneration. | T1 | 3 |
| SB-DOC-049 | Section analysis | One or more section planes cut the displayed model with capped, hatched faces per body and optional offset and box sections. Sections are display-only, can be saved in saved views, and can create a sketch from the section curves. | T1 | 3 |
| SB-DOC-050 | Curvature and edge inspection | Selecting an edge or curve shows its curvature comb with adjustable scale and density; selecting a face shows its type, normal and principal curvatures at the picked point. | T2 | 3 |

## Undo, recovery and file safety

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DOC-051 | Unlimited undo and redo | Every command is one undoable transaction. Undo and redo are unlimited within the session, survive saving, and are scoped per document; an undo history list allows stepping back several transactions at once. | T1 | 3 |
| SB-DOC-052 | Autosave and crash recovery | Transactions are written to a crash-safe recovery journal as they commit. After an abnormal exit, reopening the document offers to restore every committed transaction, independent of explicit saves. | T1 | 3 |
| SB-DOC-053 | Atomic save | Saving writes the document package atomically; an interrupted save leaves the previous saved state intact. Save, save as and save a copy are available, and save never discards undo history. | T1 | 3 |
| SB-DOC-054 | Open options | Opening a document can select the configuration, model state and display state to load, defer regeneration until requested, and load occurrences lightweight. Deferred documents show a pending-regeneration indicator. | T1 | 3 |
| SB-DOC-055 | Older documents open | A document written by any released version opens in every later version and is migrated in memory; the file is rewritten in the new format only on save. A newer major format is refused by an older application without modifying the file. | T1 | 3 |
| SB-DOC-056 | Batch migration | A headless command migrates a folder of documents to the current format, keeping originals unless told otherwise and writing a report of migrated, skipped and failed files. | T2 | 3 |
| SB-DOC-057 | Read-only open | A document can be opened read-only, for example when another user holds the check-out lock. Edits are possible as a scratch session but cannot be saved to the original file, only saved as a copy. | T1 | 3 |
