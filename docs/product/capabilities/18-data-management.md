# Data management and collaboration

SolidBuilder documents are complete and usable on one machine with no network or account; Arc Hub adds
shared versions, lifecycles and multi-user work when a team connects. Version history, branches and
merges work on the operation journal, so changes are compared and combined at the level of features,
parameters and occurrences rather than whole files.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Document | Stable ID, schema version, components, drawings, journal | Stored as one SQLite package (`.solidb`) |
| External reference | Target document ID, path hints, pinned version or latest, status (current, out of date, missing) | Held by occurrences, derived components and parameter links |
| Workspace | Root folders, search paths, library locations | Resolves external references on one machine |
| Version | Number, author, time, journal range, message | Local or Arc Hub history of a document |
| Named version | Name, version, protected flag | Marks a version for reference and release |
| Branch | Name, base version, head version | Parallel line of versions of a document |
| Change set | Ordered transactions, base revision | Unit of exchange for merges and multi-user editing |
| Lifecycle state | Name, allowed transitions, edit permission | Defined by Arc Hub; applied to versions and items |
| Revision | Label, lifecycle state, released version | Arc Hub product data |

## Local documents and references

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DATA-001 | Local-first documents | Every capability that does not by definition involve other people works with documents on local storage, with no network, account or license server. | T1 | 3 |
| SB-DATA-002 | External references | Occurrences, derived components and parameter links may point to other documents; each reference stores the target document ID and path hints and shows its status. | T1 | 3 |
| SB-DATA-003 | Workspaces | A workspace file defines root folders, search paths and library locations used to resolve references, so a project opens identically on other machines. | T1 | 3 |
| SB-DATA-004 | Resolve missing references | When a reference cannot be found, a dialog searches the workspace by document ID, offers candidates, and lets the user skip, browse or open without resolving; choices can be applied to all similar references. | T1 | 3 |
| SB-DATA-005 | Rename and move with references | Renaming, moving or copying documents from within the application updates every referencing document in the workspace and reports those it could not update. | T1 | 3 |
| SB-DATA-006 | Reference update policy | Each reference follows latest or is pinned to a version; out-of-date references are marked and updated explicitly, or automatically when the policy says so. | T1 | 3 |
| SB-DATA-007 | Package with references | Gathers a document and everything it references into a folder or archive, with optional prefix or suffix renaming and inclusion of library documents. | T1 | 3 |
| SB-DATA-008 | Design copy | Copies an assembly tree with renaming rules, choosing per component whether to copy, reuse or replace, and copies drawings with their models. | T2 | 3 |
| SB-DATA-009 | Delete protection | A document referenced by others cannot be deleted from within the application without a list of referencing documents and explicit confirmation. | T1 | 3 |
| SB-DATA-010 | Format compatibility | Documents written by any released version open in every later version; an older application refuses a newer major format without modifying the file and names the version needed. | T1 | 3 |
| SB-DATA-011 | Batch migration | The CLI upgrades a folder of documents to the current schema, keeping originals and writing a report. | T2 | 3 |
| SB-DATA-012 | Readable archive | The document schema is published, so archived documents remain readable with standard SQLite tools without the application. | T1 | 3 |

## Versions, branches and comparison

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DATA-013 | Local version history | Each save records a version with author, time and message from the operation journal; versions can be listed, opened read-only and restored as a new version. | T1 | 3 |
| SB-DATA-014 | Named versions | A version can be given a protected name that cannot be removed by later saves. | T1 | 3 |
| SB-DATA-015 | Branches | Creates a branch from any version; work on a branch does not change the main line until merged. | T2 | 4 |
| SB-DATA-016 | Merge | Merges a branch by replaying its change sets onto the target; independent edits to different features, parameters and occurrences combine automatically. | T2 | 4 |
| SB-DATA-017 | Conflict preservation | When both sides changed the same object, both values are kept and shown side by side with geometry previews; the user picks one per object, and nothing is discarded until the merge is committed. | T2 | 4 |
| SB-DATA-018 | Model comparison | Compares two versions or branches and lists differences in features, parameters, sketches, occurrences, properties and BOM; changed geometry is highlighted in the viewport. | T2 | 4 |
| SB-DATA-019 | Audit trail | Shows which user changed which objects in each version, from the journal. | T2 | 4 |

## Arc Hub integration

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DATA-020 | Optional connection | Signing in to an Arc Hub through OpenID Connect is optional; disconnecting leaves every local document fully usable. | T1 | 3 |
| SB-DATA-021 | Check-out and check-in | Documents in a Hub project are checked out with a lock before editing and checked in as a new Hub version with a message; locks show who holds them. | T1 | 3 |
| SB-DATA-022 | Get latest | Downloads the latest versions of a document and its references, or a chosen named version, and reports what changed. | T1 | 3 |
| SB-DATA-023 | Lifecycle and revisions | Shows the lifecycle state and revision of each document and item; transitions are requested from the application and enforced by Hub. | T1 | 3 |
| SB-DATA-024 | Items and BOM transfer | Creates or updates Hub items and bills of materials from components, matching by part number. | T2 | 3 |
| SB-DATA-025 | Change orders | Lists change orders affecting open documents and attaches documents to new change orders with approvals handled in Hub. | T2 | 4 |
| SB-DATA-026 | Where-used | Lists every Hub document and item that uses a component, across projects the user can see. | T2 | 3 |
| SB-DATA-027 | Server-side copy | Runs design copy on the Hub without downloading the documents. | T3 | 4 |
| SB-DATA-028 | Hub libraries | Libraries of standard parts, materials, appearances, property definitions and templates can be hosted on Hub and cached locally for offline use. | T2 | 4 |
| SB-DATA-029 | Comments and markup | Comment threads pinned to 3D views and drawing locations are synchronized with Hub issues and can be resolved from the application. | T2 | 4 |
| SB-DATA-030 | Share view links | Publishes a version to the Hub web viewer and returns a view link with the access set in Hub. | T2 | 4 |
| SB-DATA-031 | Offline work | Hub documents can be pinned for offline use; offline edits are saved locally and checked in when the connection returns, with conflicts handled as merges. | T1 | 3 |
| SB-DATA-032 | Presence | Shows which users have a Hub document open or checked out. | T2 | 4 |

## Multi-user editing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-DATA-033 | Element-level borrowing | In a Hub editing session, users borrow components, features or drawings; borrowed objects are editable only by the borrower and others see them as read-only. | T3 | 5 |
| SB-DATA-034 | Change set exchange | Committed transactions travel as change sets from the operation journal and are applied by other session members; a change set based on a stale revision is rebased or turned into a conflict, never applied blindly. | T3 | 5 |
| SB-DATA-035 | Session conflicts | Conflicting change sets keep both versions and are resolved with the merge comparison; no user's work is discarded. | T3 | 5 |
| SB-DATA-036 | Supplier sharing | Exports a simplified representation with selected properties as STEP plus a package, without feature history, for sharing outside the team. | T2 | 3 |
