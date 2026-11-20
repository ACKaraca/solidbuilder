# Open questions

Behaviors that need verification or a design decision before implementation. Each entry names the
affected capabilities and what closes it.

| ID | Question | Capabilities affected | Closed by |
|---|---|---|---|
| OQ-01 | How strictly does a document's structure mode (part-only, assembly-only, mixed) restrict which objects may be created, and how are existing documents migrated when the mode changes? | SB-DOC-004 | A decision record after prototyping mode changes on the regression models |
| OQ-02 | Which automatic dimensioning and near-miss correction rules give stable, predictable results without an assistant? | SB-SKETCH-029–031 | Usability tests on a sketch corpus measuring accepted versus rejected proposals |
| OQ-03 | What signature-matching thresholds give at least 99 percent correct resolution with no wrong resolutions on the naming corpus? | SB-FEAT-019–022 | Naming corpus results across threshold settings |
| OQ-04 | Which options do web, emboss and slot features need to cover common use without duplicating other features? | SB-FEAT-030, SB-FEAT-031, SB-FEAT-035 | Review of user workflows and a decision recorded in the feature specification |
| OQ-05 | What is the defined behavior of fillets with different radii on the two adjacent faces at corners and setbacks? | SB-FEAT-046 | Kernel prototype on the blend corpus |
| OQ-06 | Can curvature-rate (third-order) continuity be offered on general patch boundaries with the delegated kernel, or only on four-sided patches? | SB-SURF-006 | Kernel evaluation on a surfacing corpus |
| OQ-07 | Which fitting and matching behaviors are needed for freeform subdivision bodies (match to curves, pull to surfaces, fit to scans)? | SB-FORM-015, SB-FORM-016, SB-FORM-025 | User tests with industrial design reference tasks |
| OQ-08 | Which mesh-to-exact conversion methods and parameters are needed, and how is deviation reported? | SB-FORM-036, SB-FORM-037 | Conversion tests on a scan corpus |
| OQ-09 | Which lattice cell types and texture displacement options are in scope for engineering use, and where does lattice design end and additive preparation begin? | SB-FORM-040, SB-FORM-041 | Decision coordinated with additive manufacturing and generative design specifications |
| OQ-10 | Which hem variants and join-by-bend behaviors are required, and what default layer set should flat pattern DXF exports use? | SB-SHEET-013, SB-SHEET-016, SB-SHEET-034 | Survey of fabrication shop requirements |
| OQ-11 | Which options do plastic part features (boss, rest, lip and groove, snap fit, grille) need, and which rule defaults apply per material? | SB-MOLD-001–007 | Review with injection molding practitioners |
| OQ-12 | How far can history reconstruction from imported solids go before results become unreliable? | SB-DIRECT-018, SB-DIRECT-020 | Prototype on the import corpus with deviation measurement |
| OQ-13 | Which assembly constraint kinds beyond joints are needed for imported assemblies, and how do constraints and joints interact in conflict reporting? | SB-ASM-014, SB-ASM-015, SB-ASM-020 | Solver prototype on imported assemblies |
| OQ-14 | What behavior should contact sets have at high drive speeds (tunneling) and with many members? | SB-ASM-025, SB-ASM-026 | Solver tests with step-size control |
| OQ-15 | How are configuration rules expressed so that they are checkable and fast for large tables, and how do on-demand configurations get stable IDs? | SB-PARAM-022, SB-PARAM-023 | Decision record with a prototype on a 1,000-row table |
| OQ-16 | Which composition states and roll-up semantics for cost and mass overrides match Arc Hub product data? | SB-BOM-012, SB-BOM-024, SB-BOM-027 | Joint specification with Arc Hub product data |
| OQ-17 | Which annotation auto-arrangement rules are acceptable for production drawings? | SB-DRAW-031 | Drawing review with checkers on reference drawings |
| OQ-18 | Which STEP AP242 semantic PMI constructs and 3D PDF content are required for downstream consumers, and which library produces 3D PDF under an allowed license? | SB-MBD-015, SB-MBD-016, SB-MBD-018 | Interoperability tests with independent readers; license review |
| OQ-19 | Which ACIS SAT versions are covered by a published description sufficient for a clean-room reader? | SB-IO-017 | Specification review |
| OQ-20 | Can Parasolid XT and JT readers be implemented from the published references with acceptable effort, or should they run in a bridge process? | SB-IO-015, SB-IO-016 | Specification review and prototype |
| OQ-21 | How are open solver executables distributed to users given their licenses: user-installed, Hub worker only, or a separately distributed bundle? | Simulation capabilities; ADR 0006 | License review and a decision record |
| OQ-22 | What is the granularity of borrowing in multi-user sessions (component, feature, sketch), and how are history order conflicts resolved live? | SB-DATA-033–035 | Prototype with Arc Hub editing sessions |
| OQ-23 | What is the offline behavior for libraries and property definitions hosted on Arc Hub when they change while a user is offline? | SB-DATA-028, SB-DATA-031 | Joint decision with Arc Hub |
| OQ-24 | What generated content does the standard parts library ship, and under which data licenses can standard dimensions be distributed? | SB-LIB-002, SB-LIB-027 | License review of standards data sources |
| OQ-25 | What is the canonical text form grammar, and how are plug-in features and freeform bodies represented in it? | SB-AUTO-011, SB-AUTO-012, SB-AUTO-029 | Grammar draft and round-trip tests |
| OQ-26 | Which subset of commands may an assistant run without per-call confirmation, if any? | SB-AUTO-034–036 | Decision aligned with suite assistance policy |
| OQ-27 | Which merge granularity for tracks, zones and footprints keeps concurrent board edits predictable? | SB-PCB-024, SB-PCB-037 | Prototype with branch pairs on reference boards |
| OQ-28 | Which parts of the ODB++ specification are published in a form that permits a clean-room writer and reader? | SB-EOUT-030, SB-EOUT-031 | Specification and license review |
| OQ-29 | Which open autorouter and SPICE engine are distributed to users given their licenses: user-installed, Hub worker only, or a separate bundle? | SB-PCB-051, SB-EOUT-002 | License review and a decision record |
| OQ-30 | Which feature types of the other applications' feature trees can a connector read and write through public interfaces, and how are unmapped features carried? | SB-CONNECT-011, SB-CONNECT-012 | Connector prototypes per host and a published support table |
| OQ-31 | Which MTConnect and OPC UA node models and security profiles must the machine connection support for common controllers? | SB-MFG-073–077 | Review of controller documentation and tests against simulators |
| OQ-32 | What task-success threshold must each assistance capability meet in replay and live mode before the panel offers it, and which drafting standard defines the dimensioning completeness check? | SB-ASSIST-001–024 | Thresholds recorded with the assistance task suite after its first live runs; a completeness rule set tested on the drawing golden models |
