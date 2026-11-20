# Roadmap

SolidBuilder milestones mapped onto the suite phases defined in
[arc-suite](https://github.com/ACKaraca/arc-suite). Milestones have exit criteria, not dates. Status
of individual capabilities is tracked in GitHub issues, not here.

| Milestone | Suite phase | Theme |
|---|---|---|
| M0 Kernel readiness | 0–2 (prerequisites) | Foundation services SolidBuilder needs, proven by a corpus |
| M1 Part modeling | 3 | Sketches, features, feature history, persistent naming, parameters, STEP, CLI |
| M2 Assemblies and drawings | 3 | Assemblies, configurations, BOM, drawings, Hub product data |
| M3 Product breadth | 4 | Surfacing, freeform, sheet metal, plastic, frames, libraries, MBD, rendering, routing, ECAD exchange, rules |
| M4 Manufacturing preparation | 5 | Milling, turning, additive, nesting, post-processing |
| M5 Analysis, optimization and multi-user | 5 | Simulation, generative design, tolerance analysis, element-level multi-user editing, assistant |
| M6 Electronics design workspace | 5 | Schematic capture, PCB layout, circuit simulation, manufacturing outputs, electronics libraries |
| M7 Application connectors | 5 | Exchange with native files of other applications, licensed bridges, open readers |

## M0 Kernel readiness

- **Goal:** the `arc-core` services that SolidBuilder depends on exist and pass SolidBuilder's corpora.
- **Scope:** no product capabilities; SolidBuilder contributes the kernel regression corpus and the
  naming regression corpus described in [geometry kernel](architecture/geometry-kernel.md) and
  [feature history](architecture/feature-history.md).
- **Dependencies on arc-core:** `arc-base`, `arc-geom`, `arc-brep` (primitives, sweeps, booleans,
  blends, shell, draft, naming, sectioning, tessellation, STEP), `arc-solve` (2D sketches, expressions),
  `arc-doc`, `arc-command`, `arc-gpu`, `arc-ui`, `arc-app` minimum from suite phase 0.
- **Dependencies on other products:** none.
- **Exit criteria:**
  - The kernel corpus runs headless in CI against `arc-brep` and its results are published.
  - A script creates a block with a hole and a fillet through `arc-brep`, edits the block length,
    and the fillet and hole keep their persistent names.

## M1 Part modeling

- **Goal:** a user can model, edit and exchange production parts.
- **Scope:** SB-DOC-001–057 (T1 and phase-3 T2 rows), SB-SKETCH-001–066 (phase 3 rows),
  SB-FEAT-001–069 (phase 3 rows), SB-SURF-001–044 phase-3 rows, SB-DIRECT-001–024 phase-3 rows,
  SB-PARAM-001–015, SB-BOM-001–013, SB-AUTO-001–018 phase-3 rows, SB-IO-001–011, SB-IO-019–022,
  SB-IO-025–030, SB-IO-035–036, SB-DATA-001–014, SB-VIS-001–006, SB-VIS-011, SB-VIS-015, SB-LIB-010.
- **Dependencies on arc-core:** M0 plus `arc-io-step`, `arc-io-mesh`, `arc-io-dxf`, `arc-io-dwg` read,
  `arc-io-pdf`.
- **Dependencies on other products:** none (Arc Review can view exported STEP and glTF).
- **Exit criteria:** [workflows](product/workflows.md) W1 (sketch to part, without the drawing steps),
  W2 (upstream edit) and W11 steps 1–4 (code-first part) pass their acceptance bullets; the performance
  targets for sketch drag and part regeneration in the [architecture overview](architecture/overview.md)
  are met.

## M2 Assemblies and drawings

- **Goal:** complete the suite phase 3 deliverable: assemblies, drawings and STEP for mechanical
  products, with Hub product data.
- **Scope:** SB-ASM-001–051 (phase-3 rows), SB-PARAM-016–034 (phase-3 rows), SB-BOM-014–030
  (phase-3 rows), SB-DRAW-001–056 (phase-3 rows), SB-DATA-020–026, SB-DATA-031, SB-DATA-036,
  SB-IO-013, SB-VIS-017–020.
- **Dependencies on arc-core:** `arc-solve` 3D assembly solving with conflict extraction, `arc-view`,
  `arc-draft`, `arc-hub-api`.
- **Dependencies on other products:** Arc Hub Documents and product-data modules (suite phases 2–3)
  for SB-DATA-020 onward.
- **Exit criteria:** workflows W1 (complete), W3 (top-down assembly), W4 (large assembly), W5
  (configurable product, phase-3 parts), W10 (supplier STEP), and W13 steps 1–5 (Hub check-out,
  revision and offline work) pass; headless drawing publishing (SB-DRAW-054) runs in CI on the
  reference models.

## M3 Product breadth

- **Goal:** professional depth for fabrication, industrial design and model-based definition.
- **Scope:** phase-4 rows of SB-SURF, SB-FORM, SB-DIRECT, SB-SHEET-001–041, SB-MOLD-001–025,
  SB-FRAME-001–028, SB-LIB-001–030, SB-MBD-001–019, SB-VIS-007–026, SB-PARAM (rules, families,
  generated members), SB-BOM (cost, comparison), SB-DRAW (overlay, flat pattern, weld tables,
  automatic layout), SB-AUTO-010, SB-AUTO-017, SB-AUTO-019–035, SB-ASSIST-001–016, SB-ASSIST-024
  (assistance rows never gate the milestone: each is offered when its task suite passes),
  SB-DATA-015–019, SB-DATA-025–030,
  SB-DATA-032, SB-IO-012, SB-IO-014, SB-IO-023–024, SB-IO-029, SB-IO-031–033; phase-4 rows of
  [routed systems](product/capabilities/20-routed-systems.md),
  [electronics co-design](product/capabilities/28-electronics-codesign.md),
  [tolerance and inspection](product/capabilities/29-tolerance-and-inspection.md) and
  [additive manufacturing](product/capabilities/24-additive-manufacturing.md).
- **Dependencies on arc-core:** the suite assistant host in `arc-app` for the assistance rows;
  `arc-mesh` subdivision surfaces, `arc-brep` sheet metal unbend and
  surface continuity, `arc-nodes`, `arc-scene`, `arc-io-usd`, `arc-io-ifc`, `arc-io-pointcloud`.
- **Dependencies on other products:** RenderBuilder (suite phase 4) for SB-VIS-013–014; Arc Hub
  automation jobs for SB-AUTO-017.
- **Exit criteria:** workflows W5 (complete, with rules and forms), W6 (sheet metal), W7 (welded
  frame), W8 (plastic housing), W9 (freeform to solid), W11 (complete, with branch and merge), W12
  (model-based definition) and W14 (exploded instructions and render) pass.

## M4 Manufacturing preparation

- **Goal:** validated NC programs and print and cut files from the model.
- **Scope:** all rows of [manufacturing setup and tooling](product/capabilities/21-manufacturing-setup-and-tooling.md),
  [milling](product/capabilities/22-milling.md),
  [turning and mill-turn](product/capabilities/23-turning-and-mill-turn.md),
  [additive manufacturing](product/capabilities/24-additive-manufacturing.md) (phase-5 rows) and
  [nesting and fabrication](product/capabilities/25-nesting-and-fabrication.md); SB-SHEET-037. The
  shop-floor connection rows SB-MFG-073–080 (program transfer and machine status through MTConnect and
  OPC UA) are part of this milestone.
- **Dependencies on arc-core:** `arc-mesh` stock models and booleans at toolpath scale; `arc-geom`
  offsetting robustness.
- **Dependencies on other products:** Arc Hub automation jobs for headless toolpath and post runs.
- **Exit criteria:** the reserved manufacturing files' acceptance scenarios pass; a 3-axis part and a
  flat-pattern nest regenerate their toolpaths after a model edit and post identical NC text on two
  runs; the post-processor interface ([ADR 0006](adr/0006-external-solvers-and-owned-toolpath-kernels.md))
  is published.

## M5 Analysis, optimization and multi-user

- **Goal:** analysis-driven design and real-time teamwork.
- **Scope:** all rows of [simulation](product/capabilities/26-simulation.md),
  [generative design and optimization](product/capabilities/27-generative-design-and-optimization.md),
  phase-5 rows of [tolerance and inspection](product/capabilities/29-tolerance-and-inspection.md);
  SB-DATA-033–035, SB-AUTO-036, SB-ASSIST-017–023, SB-FRAME-024, SB-FRAME-028, SB-MBD-018, SB-IO-015–018, SB-IO-034,
  remaining phase-5 rows of every area file.
- **Dependencies on arc-core:** `arc-doc` change-set exchange; `arc-hub-api` editing sessions.
- **Dependencies on other products:** Arc Hub element-level editing sessions (suite phase 5); open
  solvers installed locally or on Hub workers.
- **Exit criteria:** workflow W13 (complete, with element-level editing) passes; two users edit
  different components of one assembly in a Hub session and both see each other's committed changes
  without losing work on conflict; a static study of a W1 part reruns after a parameter change and
  reports updated results; the assistant applies a previewed parameter change as one undoable
  transaction.

## M6 Electronics design workspace

- **Goal:** a board is designed, simulated and released inside the document that holds its enclosure.
- **Scope:** all rows of [schematic capture](product/capabilities/30-schematic-capture.md),
  [PCB layout](product/capabilities/31-pcb-layout.md) and
  [electronics simulation and outputs](product/capabilities/32-electronics-simulation-and-outputs.md);
  [electronics co-design](product/capabilities/28-electronics-codesign.md) remains in M3 and SB-ECAD-036
  in M5. The crate `solidbuilder-ecad` is created at this milestone
  ([ADR 0007](adr/0007-electronics-workspace-on-the-suite-document-model.md)).
- **Dependencies on arc-core:** `arc-draft` for sheets and drawings, `arc-geom` polygon operations at
  copper scale, `arc-brep` for the board solid, `arc-doc` merge support for the new object kinds.
- **Dependencies on other products:** Arc Hub for hosted libraries and release jobs.
- **Exit criteria:** workflow W15 passes; the written Gerber, drill and IPC-2581 files re-read with an
  independent reader and match the board; zone fill gives identical results on one and many threads.

## M7 Application connectors

- **Goal:** native mechanical CAD files of other applications are exchanged with data fidelity and,
  where the host interface allows, edit fidelity, with identifiers that survive repeated exchange.
- **Scope:** all rows of [application connectors](product/capabilities/33-application-connectors.md);
  the connectors themselves live in a separate repository
  ([suite ADR-0017](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0017-exchange-connectors-and-licensed-bridges.md)).
- **Dependencies on arc-core:** the Arc exchange package specification and `arc-io-*` format crates.
- **Dependencies on other products:** none; connectors run in the user's own copy of the other
  application.
- **Exit criteria:** an assembly exchanged through a connector, edited in SolidBuilder and pushed back,
  then edited in the host and pulled again, keeps the same identifiers for unchanged objects, lists
  changed and broken references, and reports losses; a 3DM file opens through the open reader with a
  loss report.

## Native kernels

Native finite-element, meshing, slicing and multi-axis toolpath kernels replace wrapped external
engines once each passes the same validation suites as the engine it replaces
([ADR 0006](adr/0006-external-solvers-and-owned-toolpath-kernels.md)). The wrapped engines are the
default in M4 and M5; a native kernel is released per function, with the adapter interface and the
user-visible capabilities unchanged. This adds no capability rows.
