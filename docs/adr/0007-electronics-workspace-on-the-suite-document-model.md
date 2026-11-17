# 0007 Electronics workspace on the suite document and command model with the shared drafting engine

- Status: Accepted
- Deciders: maintainers of this repository

## Context

Enclosures and boards constrain each other: the outline, mounting holes, connector positions and
component heights come from the mechanical model, and board changes move parts in the enclosure.
Keeping the electronics design in a separate application forces file exchange and reviewed change
sets at every iteration. The suite already has a typed object store with transactions, a command
model with one automation protocol, a drafting engine for sheets, and exchange paths to open ECAD
tools ([electronics co-design](../product/capabilities/28-electronics-codesign.md)).

## Decision

1. **Same document.** Schematics, boards, libraries, circuit studies and output jobs are typed
   objects in the `.solidb` document, created and changed only by commands inside transactions. They
   undo, journal, branch, merge and script like mechanical objects, and the board links to
   assemblies by object identity, not by file exchange.
2. **Shared drafting engine.** Schematic sheets, assembly drawings and fabrication drawings use
   `arc-draft`. Copper geometry uses `arc-geom` polygons; the board solid uses `arc-brep`.
3. **One headless crate.** `solidbuilder-ecad` holds the electronics domain logic, rule checks,
   routing, zone fill and output writers, with no dependency on UI, windowing or GPU crates.
4. **Derived data carries its revision.** Netlist, ratsnest, filled zones, rule findings, studies and
   output files are derived, tagged with the revision they came from and discarded or marked out of
   date when stale.
5. **Open engines run as separate processes.** SPICE engines and autorouters run through adapters
   with documented input and result formats, as for solvers in
   [ADR 0006](0006-external-solvers-and-owned-toolpath-kernels.md). No electronics workflow depends
   on one being installed.
6. **Exchange stays.** Boards designed in other open ECAD tools remain exchangeable through the
   existing co-design area; the electronics workspace adds no format that replaces it.

## Consequences

- Board and enclosure checks run on live objects, so clearance feedback needs no export.
- The object store, journal and merge rules gain electronics object kinds; merge granularity for
  tracks and zones needs its own tests.
- Output writers carry the verification burden of fabrication formats, tested with independent
  readers.
- Library management is shared infrastructure between mechanical and electronics libraries.

## Alternatives considered

- A separate electronics application in the suite: duplicates the document, command and drafting
  layers and keeps the iteration loop file-based.
- Exchange only: no live clearance, no shared change history, no single bill of materials.
- A second document type for electronics inside the same application: splits undo and branching.

## Trigger to reopen

Board layouts of realistic size cannot meet interactive routing and rule-check budgets inside the
shared transaction model, or a second Arc product needs the electronics objects and a lower layer
should own them.
