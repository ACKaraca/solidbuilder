# Reference workflows

Fourteen end-to-end workflows used as acceptance scenarios for the [roadmap](../roadmap.md). Each step
names the capabilities it exercises; each workflow ends with observable acceptance results.

## W1 Parametric part from sketch to drawing

1. Create a document from a metric template and a part definition (SB-DOC-001, SB-DOC-006, SB-DOC-011).
2. Define user parameters `thickness` and `hole_d` (SB-PARAM-002, SB-PARAM-003).
3. Sketch a profile on the XY datum plane with inferred constraints and dimensions bound to parameters
   until it is fully constrained (SB-SKETCH-001, SB-SKETCH-007, SB-SKETCH-024–027, SB-SKETCH-033).
4. Extrude the profile as a new body by `thickness`; sketch on its top face and cut a pocket
   (SB-FEAT-024, SB-FEAT-068).
5. Add a tapped hole with a standard thread, rule-based fillets and a chamfer
   (SB-FEAT-037, SB-FEAT-041, SB-FEAT-048, SB-FEAT-049).
6. Assign a material and check mass properties (SB-BOM-009, SB-BOM-011).
7. Change `thickness` in the parameter table (SB-PARAM-001, SB-FEAT-001).
8. Create a drawing with base, projected, section and detail views; retrieve dimensions, add a hole
   note, a feature control frame and the title block fields (SB-DRAW-001, SB-DRAW-008–012,
   SB-DRAW-025, SB-DRAW-033, SB-DRAW-036, SB-DRAW-003).
9. Export the drawing to PDF and the part to STEP (SB-DRAW-051, SB-IO-011).

**Acceptance**
- After step 7, the body, mass properties and every drawing view reflect the new thickness without
  failed features or orphaned annotations.
- The hole note text matches the hole specification and changes when the thread size changes.
- The exported STEP file re-read by an independent reader has the same volume within tolerance.

## W2 Upstream edit with persistent references

1. Model a bracket from a sketch with six profile segments; add fillets on four edges, a hole on a side
   face and a chamfer on a cap edge (SB-FEAT-024, SB-FEAT-044, SB-FEAT-037, SB-FEAT-049).
2. Edit the base sketch to add a seventh segment that splits one side face (SB-SKETCH-018,
   SB-FEAT-002).
3. Delete a profile segment that generated a filleted face.
4. Open the health list and remap the broken fillet edge with one pick (SB-FEAT-016, SB-FEAT-017,
   SB-FEAT-019).
5. Run the reference audit (SB-FEAT-022).

**Acceptance**
- After step 2, all fillets, the hole and the chamfer stay on their original geometry.
- After step 3, exactly the references to the removed face are reported broken; no reference is
  attached to a different face.
- After step 4, the model regenerates without errors and the remap is one undoable step.

## W3 Top-down assembly with a layout and joints

1. Create an assembly with a layout component holding a master sketch and key parameters
   (SB-ASM-038, SB-PARAM-011).
2. Create three parts in place, referencing layout geometry (SB-ASM-003, SB-ASM-039, SB-ASM-043).
3. Connect parts with revolute and slider joints with limits, a gear coupling and a rigid group
   (SB-ASM-010, SB-ASM-013, SB-ASM-016, SB-ASM-017).
4. Drive the revolute joint through its range with a contact set enabled (SB-ASM-024, SB-ASM-025).
5. Run an interference check and a clearance check (SB-ASM-027, SB-ASM-028).
6. Change a layout parameter; freeze references at design freeze (SB-ASM-042).
7. Produce the bill of materials and an assembly drawing with parts list and balloons
   (SB-BOM-014, SB-DRAW-044, SB-DRAW-045).

**Acceptance**
- The drive stops at the joint limit or at first contact, whichever comes first.
- After step 6, all three parts update and the assembly solves without conflicts.
- The interference report lists each pair with volume; balloon numbers match parts list item numbers.

## W4 Large assembly

1. Open an assembly of 10,000 occurrences with lightweight loading (SB-ASM-044).
2. Rank components by size and create wrapped simplifications for the three heaviest subassemblies
   (SB-ASM-048, SB-ASM-046).
3. Use the simplifications in a display state for review (SB-ASM-047, SB-PARAM-030).
4. Partially load one subassembly and edit one part in context (SB-ASM-045, SB-ASM-043).
5. Switch to the full state and produce the bill of materials (SB-BOM-015).

**Acceptance**
- The assembly becomes navigable within the open-time target in the architecture overview.
- Measuring an occurrence loads its exact geometry on demand and returns exact values.
- The bill of materials of the full state is identical whether or not simplifications are active.

## W5 Configurable product family

1. Build a part with a configuration table for three sizes, varying parameters, suppression and part
   number (SB-PARAM-016, SB-PARAM-017, SB-PARAM-021).
2. Edit a fillet radius for all configurations using the edit scope (SB-PARAM-018).
3. Place two configurations in an assembly (SB-PARAM-020).
4. Add configuration rules restricting combinations (SB-PARAM-022).
5. Build a form bound to parameters with rules that swap a subassembly and choose configurations
   (SB-AUTO-025, SB-AUTO-022).
6. Publish the configured part as a library family (SB-PARAM-024).
7. Generate a drawing per configuration headlessly (SB-DRAW-054, SB-AUTO-016).

**Acceptance**
- Each configuration has its own part number in the bill of materials (SB-BOM-025).
- An invalid combination is rejected with the rule named.
- Headless publishing produces one PDF per configuration with matching parts lists.

## W6 Sheet metal enclosure

1. Select a sheet metal rule from a library (SB-SHEET-001, SB-SHEET-005).
2. Create a base face, edge flanges, a hem and corner seams with reliefs (SB-SHEET-008, SB-SHEET-009,
   SB-SHEET-013, SB-SHEET-017, SB-SHEET-018).
3. Add a punch from a feature template and a cut across a bend (SB-SHEET-026, SB-SHEET-023).
4. Run manufacturability checks (SB-SHEET-040).
5. Create the flat pattern with A-side and bend order (SB-SHEET-029, SB-SHEET-030).
6. Export the flat pattern to DXF with layer mapping (SB-SHEET-034).
7. Place a flat pattern view and a bend table on a drawing (SB-DRAW-018, SB-DRAW-048).

**Acceptance**
- The flat blank size matches the rule's unfold method for every bend.
- The DXF contains outer profile, inner profiles and bend lines on separate layers.
- Changing the thickness in the rule updates the formed part, flat pattern, bend table and DXF export.

## W7 Welded frame

1. Sketch a 3D skeleton (SB-SKETCH-048).
2. Place frame members from a standard profile library (SB-FRAME-001, SB-FRAME-002).
3. Miter corners, notch crossing members and trim to a face (SB-FRAME-009–011).
4. Convert to a weldment; add preparations, fillet and groove welds and a machining cut
   (SB-FRAME-015–018, SB-FRAME-022).
5. Produce the cut list and a drawing with weld symbols and a cut list table (SB-FRAME-026,
   SB-DRAW-038, SB-DRAW-049).

**Acceptance**
- Cut lengths and end angles in the cut list match the trimmed member geometry.
- Weld symbols on the drawing are generated from weld data and update when a weld size changes.
- The weldment bill of materials includes bead mass (SB-FRAME-027).

## W8 Plastic housing with mold split

1. Assign a plastic rule and shell a housing with rule-based draft (SB-MOLD-001, SB-MOLD-009,
   SB-MOLD-011).
2. Add screw bosses, a lip and groove and snap fits (SB-MOLD-003, SB-MOLD-005, SB-MOLD-006).
3. Run wall thickness and draft and undercut analysis (SB-MOLD-013, SB-MOLD-014).
4. Create the parting line and parting surface, apply shrinkage and split core and cavity
   (SB-MOLD-018, SB-MOLD-019, SB-MOLD-017, SB-MOLD-020).
5. Change the boss height in the part (SB-MOLD-025).

**Acceptance**
- Faces below the minimum draft are highlighted and listed.
- Core and cavity bodies together with the part fill the tool block without gaps.
- After step 5, core and cavity update without manual repair.

## W9 Freeform shape to solid

1. Place a calibrated reference image (SB-SKETCH-056).
2. Create a freeform body from a primitive with symmetry and shape it (SB-FORM-001, SB-FORM-020,
   SB-FORM-005, SB-FORM-012).
3. Convert to an exact body (SB-FORM-023).
4. Add a boundary patch with curvature continuity and stitch to a solid (SB-SURF-005, SB-SURF-016).
5. Check continuity and zebra stripes (SB-SURF-035, SB-SURF-041).
6. Shell and fillet the solid (SB-FEAT-050, SB-FEAT-044).

**Acceptance**
- The continuity check reports the requested continuity across the patch boundary within tolerance.
- Editing the freeform body regenerates the converted body, patch, shell and fillets.

## W10 Supplier STEP import and modification

1. Insert a supplier STEP assembly as a linked import (SB-IO-013, SB-IO-010).
2. Review the loss report and healing report (SB-IO-003, SB-IO-005).
3. Recognize holes and fillets on one part and resize a hole by direct edit (SB-DIRECT-017,
   SB-DIRECT-006).
4. Add own features on top of the base feature (SB-FEAT-010).
5. Replace the supplier file with a newer revision.
6. Export a simplified STEP for the customer (SB-DIRECT-022, SB-DATA-036).

**Acceptance**
- After step 5, own features and joints re-attach; any reference that cannot is reported broken.
- The original STEP file is stored in the document and the import record names reader version and
  options.

## W11 Code-first part and branch merge

1. Write a Python script that creates a parametric part through the automation protocol
   (SB-AUTO-004, SB-AUTO-007, SB-AUTO-008).
2. Export the part's canonical text form and commit it to version control (SB-AUTO-011).
3. Build the document from the text form in CI with the CLI and run model checks (SB-AUTO-013,
   SB-AUTO-015, SB-AUTO-018).
4. Change one dimension in the text form.
5. Create a branch in SolidBuilder, add a feature, while the main line changes a parameter
   (SB-DATA-015).
6. Merge the branch (SB-DATA-016, SB-DATA-018).

**Acceptance**
- Re-exporting an unchanged model yields identical text; the step 4 change produces a one-line
  difference (SB-AUTO-012).
- The CI run exits non-zero when a feature fails and zero otherwise.
- The merge applies both changes automatically; a deliberate conflicting edit shows both values side
  by side and nothing is discarded (SB-DATA-017).

## W12 Model-based definition release

1. Define datum features and a datum reference frame on a machined part (SB-MBD-004).
2. Add 3D dimensions, feature control frames and surface texture on annotation planes
   (SB-MBD-001–003, SB-MBD-005).
3. Run semantic validation and the coverage report (SB-MBD-011, SB-MBD-012).
4. Save annotation views (SB-MBD-013).
5. Export STEP AP242 with semantic PMI and the characteristic list (SB-MBD-015, SB-MBD-014).

**Acceptance**
- Validation reports zero errors before export; a deliberately incomplete datum reference frame is
  reported with a link to the annotation.
- Re-importing the exported file yields the same annotations attached to the same faces
  (SB-MBD-016).

## W13 Collaboration through Arc Hub

1. Connect to an Arc Hub and add a local project (SB-DATA-020).
2. Check out an assembly, edit, and check in with a message (SB-DATA-021).
3. Transfer the bill of materials to Hub items and request a lifecycle transition (SB-DATA-024,
   SB-DATA-023).
4. Pin documents for offline use, disconnect, edit, reconnect and check in (SB-DATA-031).
5. Resolve a comment thread on a 3D view (SB-DATA-029).
6. In a Hub editing session, two users borrow different components and edit concurrently
   (SB-DATA-033, SB-DATA-034).

**Acceptance**
- Without a connection, every local modeling capability remains available.
- After step 3, the released revision is read-only in the document (SB-BOM-027).
- After step 6, both users see each other's committed changes; a conflicting change keeps both
  versions (SB-DATA-035).

## W14 Exploded assembly instructions and render

1. Create an automatic exploded state and adjust steps with trails (SB-VIS-017, SB-VIS-018).
2. Build an animation sequence with explosion steps, camera moves and callouts (SB-VIS-021).
3. Export the sequence to video (SB-VIS-025).
4. Place the exploded state in a drawing view with balloons (SB-DRAW-017, SB-DRAW-045).
5. Assign appearances, choose an environment and render a still through RenderBuilder
   (SB-VIS-005, SB-VIS-009, SB-VIS-014).

**Acceptance**
- The exploded state does not change joints or position states (SB-VIS-019).
- The video and drawing view show the same explosion.
- Changing an appearance in SolidBuilder updates the interactive render view without re-export
  (SB-VIS-013).
