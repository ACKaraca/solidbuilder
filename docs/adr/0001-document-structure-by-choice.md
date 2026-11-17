# 0001 Parts, assemblies and drawings in one document or separate documents

- Status: Accepted
- Deciders: maintainers of this repository

## Context

Some teams want one file per part, assembly and drawing so that ownership, check-out and release work
per item. Others, especially small teams and code-first users, want a whole product in one file so
that references never break and a single version describes a consistent design. The Arc document
package (one SQLite database per document, stable 128-bit object IDs) can hold any number of
component definitions and drawings.

## Decision

A `.solidb` document may contain any number of part definitions, assembly definitions and drawings.
An occurrence refers to a component definition by its stable ID, either in the same document or in
another document through an external reference. Moving a definition between documents keeps its ID,
and the product rewrites references in the workspace in one command. Arc Hub versions, locks and
lifecycles apply to whole documents; a team that wants per-item release keeps one item per document.

## Consequences

- References inside a document never break on rename or move.
- The external reference resolver is needed from the start and must handle documents that contain
  several definitions.
- Bills of materials, drawings and Hub items identify components by definition ID, not by file.
- Large single documents rely on partial loading of object tables.

## Alternatives considered

- One file per part, assembly and drawing only: simple release model, but frequent broken links and
  no single consistent version for small products.
- Single document only: no per-item check-out or release for larger teams.

## Trigger to reopen

Hub check-out granularity per document causes measurable contention in teams that keep products in one
document, or definition moves between documents fail to preserve references in the regression corpus.
