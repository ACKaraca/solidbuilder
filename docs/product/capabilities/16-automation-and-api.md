# Automation and programming interface

Every SolidBuilder operation is a registered command, so the user interface, scripts, rules, the
command-line tool, the automation protocol and assistants all drive the model through one validated
path. This file specifies that surface, the rule and form layer for configurable products, and code-first
modeling with a stable API and a diffable text form.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Command | Stable ID, label, parameter schema, enabled predicate, non-interactive run, optional prompt machine | Registered in the command registry; produces transactions |
| Script | Language (Python client or other protocol clients), source, entry points | Runs out of process and calls commands through the protocol |
| Rule | Kind (expression, script, node graph), scope (document, library), inputs, actions, triggers | Owned by a document or shared library; executes commands |
| Trigger | Event kind, filter | Starts rules |
| Form | Controls bound to parameters and properties, layout, rules to run | Owned by a document or library |
| Node graph | Nodes, typed links, evaluation cache | Compiles to rules or drives features |
| Plug-in feature | WebAssembly module, manifest, input schema, trust level | Lives in a feature history like a built-in feature |
| Plug-in package | Manifest, modules, permissions requested | Installed per user or per machine |

## Commands and protocol

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-AUTO-001 | Command registry | Every modeling, assembly, drawing and data operation is a command with a stable ID and a machine-readable parameter schema; running it non-interactively never opens a dialog. | T1 | 3 |
| SB-AUTO-002 | Command line and palette | Commands are runnable by typing their name or alias in a command line or searchable palette, with parameters entered as text. | T1 | 3 |
| SB-AUTO-003 | Macro recording | Records the commands a user runs, with resolved parameters and persistent references, as a replayable Python script. | T2 | 3 |
| SB-AUTO-004 | Automation protocol | Exposes commands and typed queries over the Arc Automation Protocol (JSON-RPC over stdio or a local socket) for documents, components, sketches, features, parameters, joints, BOM and drawings. | T1 | 3 |
| SB-AUTO-005 | Typed queries | Clients query objects by ID, type, name, property and topology (faces of a feature, edges of a face) and receive persistent references usable in later commands. | T1 | 3 |
| SB-AUTO-006 | Stable API versioning | The protocol carries a semantic version; additions are backward compatible within a major version, deprecated calls keep working for at least one major version and report a deprecation warning. | T1 | 3 |
| SB-AUTO-007 | Python client | An official Python package wraps the protocol with typed objects and documentation; scripts run in their own process against a running application or the headless runner. | T1 | 3 |
| SB-AUTO-008 | Transactions from clients | A client can group several commands into one transaction that commits or rolls back as a whole and undoes as one step. | T1 | 3 |
| SB-AUTO-009 | Events | Clients subscribe to open, save, close, parameter change, property and material change, suppression change, configuration change, regeneration completed or failed, and drawing update events. | T2 | 3 |
| SB-AUTO-010 | Model Context Protocol endpoint | Serves the command registry and queries as a Model Context Protocol endpoint so external assistants can inspect and edit documents with the same validation and undo as users. | T2 | 4 |

## Code-first modeling

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-AUTO-011 | Text form of the feature history | A component definition can be written as a canonical text form listing parameters, sketches, constraints and features in order; reading it back produces the same model with the same persistent names. | T2 | 3 |
| SB-AUTO-012 | Deterministic text output | The text form is stable: unchanged models produce identical text, and a single edit produces a minimal textual difference suitable for code review. | T2 | 3 |
| SB-AUTO-013 | Build from source | The CLI builds documents from text form files and parameter files, so parts can be kept in version control and regenerated in CI. | T2 | 3 |
| SB-AUTO-014 | Parameter sweeps | The CLI regenerates a component over a set of parameter values and writes geometry, mass properties and failures for each set. | T2 | 4 |

## Headless operation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-AUTO-015 | Headless CLI | The command-line application opens, regenerates, validates, converts and exports documents, runs scripts and serves the protocol without a display. | T1 | 3 |
| SB-AUTO-016 | Batch jobs | A job file lists documents and operations (update, migrate, export, publish drawings, export BOM); the runner processes them in parallel and writes a report. | T2 | 3 |
| SB-AUTO-017 | Hub automation jobs | The same job format runs as an Arc Hub automation job on a headless runner when a version is checked in or a lifecycle state changes. | T2 | 4 |
| SB-AUTO-018 | Model checking in CI | Validation rules (naming, required properties, failed features, orphaned annotations, standards) run headless and return a non-zero exit code with a machine-readable report when they fail. | T2 | 3 |

## Rules and forms

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-AUTO-019 | Expression rules | Declarative rules set parameters, suppression, properties and materials from conditions over parameters and properties, evaluated on change. | T2 | 4 |
| SB-AUTO-020 | Script rules | Script rules stored in a document or shared library run through the protocol with access limited to the document they belong to, unless the user grants more. | T2 | 4 |
| SB-AUTO-021 | Rule triggers | Rules run on open, before and after save, close, parameter change, property or material change, suppression change, configuration change and drawing update, or on demand. | T2 | 4 |
| SB-AUTO-022 | Assembly rules | Rules add, replace and remove occurrences, choose configurations and connect occurrences through connection definitions, so an assembly can be configured from inputs. | T2 | 4 |
| SB-AUTO-023 | Static check | Before a script rule or generated script runs, a static check reports calls outside the allowed set (file system, network, other documents) and refuses to run until the user approves or the code is changed. | T2 | 4 |
| SB-AUTO-024 | Rule management | A rule panel lists rules with folders, enable state, last result and errors; rules can be run, edited, debugged with breakpoints in the client tooling, and exported. | T2 | 4 |
| SB-AUTO-025 | Forms | Forms with text, number with unit, checkbox, drop-down, image, group and tab controls are bound to parameters and properties and can run rules; forms dock or float and exist per document or globally. | T2 | 4 |
| SB-AUTO-026 | Visual rules | Rules can be built as node graphs whose nodes are inputs, conditions and actions; a graph compiles to the same rule representation as text rules. | T2 | 4 |
| SB-AUTO-027 | Product configurators | A configured assembly with forms and rules can be published so users choose options in a form and receive the configured assembly, drawings and BOM. | T3 | 4 |
| SB-AUTO-028 | Drawing automation | Rules and scripts create drawings, place views, retrieve dimensions, place tables and export PDF through drawing commands. | T2 | 4 |

## Plug-ins and customization

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-AUTO-029 | Plug-in features | A WebAssembly plug-in can define a feature type with an input schema; its instances live in the feature history, use persistent references and regenerate like built-in features. If the plug-in is missing, the last result is kept and the feature is marked. | T3 | 4 |
| SB-AUTO-030 | Plug-in sandbox | Plug-ins run without ambient authority, with memory and time limits; a plug-in that faults or exceeds limits is stopped and its transaction rolled back. | T3 | 4 |
| SB-AUTO-031 | Plug-in packages | Plug-ins are installed from packages with a manifest declaring commands, features and requested permissions; the host assigns trust and the user can disable a package. | T3 | 4 |
| SB-AUTO-032 | Interface customization | Toolbars, panels, keyboard shortcuts and command aliases can be customized and exported as a settings file. | T2 | 3 |
| SB-AUTO-033 | Custom commands | Scripts and plug-ins can register commands with icons and parameter schemas that appear in menus, the palette and the protocol. | T2 | 4 |

## Assistant

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-AUTO-034 | Provider-neutral assistant | An optional assistant panel connects to a user-configured model endpoint, including local models; no document content leaves the machine without an explicit user action. | T3 | 5 |
| SB-AUTO-035 | Preview and apply | Assistant proposals are shown as a list of commands with a model preview and are applied only on explicit confirmation, as one undoable transaction. | T3 | 5 |
| SB-AUTO-036 | Assistant tasks | The assistant can create and run scripts and rules after the static check, manage configurations and states, bulk-edit properties, and assemble drawing packages. | T3 | 5 |
