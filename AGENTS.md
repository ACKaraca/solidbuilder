# AGENTS.md

## What this repository is

SolidBuilder: the Arc suite's parametric mechanical design application (parts, assemblies, drawings,
manufacturing preparation, simulation hooks). Rust end to end, built on `arc-core`.
Native document: `.solidb` (one SQLite package per document).

## Status and where to start

Design phase: no code and no builds exist. Read in this order: [docs/README.md](docs/README.md),
[docs/product/vision.md](docs/product/vision.md),
[docs/product/capabilities/README.md](docs/product/capabilities/README.md),
[docs/architecture/overview.md](docs/architecture/overview.md),
[docs/architecture/feature-history.md](docs/architecture/feature-history.md),
[docs/roadmap.md](docs/roadmap.md).

## Repository layout (target)

```
crates/
  solidbuilder-model/     domain objects, feature history, persistent references, regeneration (headless)
  solidbuilder-commands/  user-level commands, text form, rules runtime (headless)
  solidbuilder-io/        mapping to and from interchange formats (headless)
  solidbuilder-cam/       toolpaths and post-processing (headless; created at the manufacturing milestone)
  solidbuilder-sim/       studies and solver adapters (headless; created at the simulation milestone)
  solidbuilder-ui/        tools, panels, viewport interaction (egui)
apps/
  solidbuilder/           desktop application (wiring only)
  solidbuilder-cli/       headless CLI and automation protocol server
docs/
xtask/
```

## Commands

No build exists yet. Once code lands, these apply:

- `cargo fmt --check`
- `cargo clippy --workspace --all-targets -- -D warnings`
- `cargo nextest run --workspace`
- `cargo xtask layers`

## Suite rules

These rules apply in every Arc repository. The canonical text is
[`docs/engineering/agent-rules.md`](https://github.com/ACKaraca/arc-suite/blob/main/docs/engineering/agent-rules.md)
in `arc-suite`; change it there first.

1. **A person is accountable.** Whoever runs an agent answers for what it submits.
2. **Start from an issue.** Search issues and pull requests, check that nobody else is assigned,
   assign yourself and set the status to *In Progress* before changing files. When you finish,
   comment with what changed, the commit or pull request link and open items, then update the status.
   *Done* requires met and verified acceptance criteria.
3. **Verify, do not guess.** Open a definition before using it. Confirm a cause in the code.
4. **The document is the authority and everything is a command.** No change reaches a document
   except through a validated command inside a transaction. Programmatic paths never open dialogs.
5. **Respect the layers.** A crate depends only on lower layers. Nothing below the UI layer depends
   on windowing, GPU or UI crates. Products never depend on other products.
6. **Never crash, never lose work.** No `unwrap`, `expect`, `panic!`, `todo!` or `unreachable!` in
   production code. No `unsafe` outside the FFI crates named in an ADR. Treat every file and message
   as hostile: bound recursion, allocation and counts. A crash fix ships with a regression test.
7. **Clean room.** Implement from published specifications, observed behavior and original
   reasoning. Never copy or consult another product's code, decompiled output, file-format reverse
   engineering, documentation text, icons, fonts, templates or sample files. Do not name our
   features after another company's features.
8. **Licenses.** Add a dependency only if its license is on the allowlist in
   `docs/engineering/dependency-policy.md` in `arc-suite`. Record every asset's origin and license.
9. **Rust only in desktop applications.** No web view, no JavaScript or TypeScript.
10. **Small, tested changes.** One concern per pull request, with the test that fails without it.
    Update the documents that describe the behavior you change in the same pull request.
11. **Report only what ran.** Say what you tested and what you could not run.
12. **Ask a human before** adding a license outside the allowlist, publishing or releasing,
    changing a file format or public protocol, adding a new repository or remote, spending money,
    handling secrets, or any destructive git operation. Otherwise keep working.
13. **Keep secrets and personal data out** of repositories, logs and requests.

## Repository-specific invariants

- Geometry comes only from the `arc-brep` API; no kernel types, handles or `unsafe` in this repository.
- Every topological entity a feature creates gets a persistent name; a broken reference is reported,
  never silently reattached.
- Feature inputs refer only to earlier history entries, parameters, reference geometry or declared
  cross-component references; the dependency graph stays acyclic.
- Bodies of history-enabled parts, tessellation, drawing projections, BOM rows, toolpaths and results
  are derived and carry the revision they were computed from; stale results are discarded.
- Regeneration is deterministic: identical topology and names on one or many threads.
- Joints and assembly constraints are solved by one solver; results do not depend on creation order.
- Every feature type defines its canonical text form and round-trips through it.
- Third-party solvers and meshers run as external processes; nothing links a copyleft solver. A native
  kernel replaces a wrapped one only after passing the same validation suites.
- Imports keep the source file, an import record and a loss report.

## Documentation rules

- No trace of the planning process; never describe SolidBuilder as derived from, copying or replacing
  another product, and never use other products' feature names.
- Other companies' names only as formats, standards and protocols, in interoperability statements,
  once in the positioning paragraph of the vision, and in the README trademark notice.
- Own descriptive vocabulary (feature history, connection frame, standard parts library, component
  generator, lightweight loading).
- Each fact once; link suite rules instead of copying them. Write behavior in original words.
- Capability identifiers are permanent; status lives in issues, not in capability tables.

## Issue workflow

Search issues and pull requests first; take an unassigned issue, assign yourself, set *In Progress*.
Statuses: Backlog, Ready, In Progress, In Review, Blocked, Done. Labels: `type:`, `tier:`, `phase:`,
`area:` (area token in lower case), `status: blocked`, `needs: verification`. On finishing, comment with
what changed, the link and open items; open follow-up issues after a duplicate search. Conventional
Commits with DCO sign-off.

## Verification

Run the narrowest check that covers the change (the affected crate's tests, the naming corpus for
naming changes, the drawing golden tests for view changes). Report only what ran.

## Ask a human about

File format or text form grammar changes, automation API breaking changes, new crates, kernel or
solver choices, dependency licenses outside the allowlist, and anything listed in suite rule 12.
