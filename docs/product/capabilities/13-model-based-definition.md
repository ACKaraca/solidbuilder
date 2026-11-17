# Model-based definition

Model-based definition puts semantic product and manufacturing information directly on the 3D model, so
the model rather than a drawing can carry tolerances, datums and notes downstream. Annotations are
machine-readable objects tied to geometry through persistent references and exchanged through STEP
AP242.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Annotation plane | Plane, orientation, name | Owns 3D annotations; aligned to model geometry |
| 3D annotation | Kind (dimension, feature control frame, datum feature, datum target, surface texture, note, weld callout), semantic values, targets via persistent references, standard | Placed on an annotation plane; exported as semantic PMI |
| Datum feature | Label, target faces or features | Referenced by feature control frames |
| Datum reference frame | Ordered datum features, modifiers | Part of a feature control frame |
| Feature control frame | Characteristic, tolerance value, zone shape, modifiers, datum reference frame | Kind of 3D annotation or drawing annotation |
| Saved view | Camera, visible annotations, section, display state | Groups annotations for consumers |

## Annotation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MBD-001 | Annotation planes | Creates annotation planes on faces, datums or view orientations; annotations on a plane stay readable from its normal direction. | T2 | 4 |
| SB-MBD-002 | 3D dimensions | Adds linear, angular, radial and diameter dimensions between model geometry with tolerances and fits, or promotes feature dimensions to 3D annotations. | T2 | 4 |
| SB-MBD-003 | Geometric tolerances | Adds feature control frames with characteristic, zone, material condition and other modifiers per ASME Y14.5 or ISO 1101, as structured values rather than text. | T2 | 4 |
| SB-MBD-004 | Datum features and targets | Labels datum features and adds point, line and area datum targets on faces; datum reference frames reference them by label. | T2 | 4 |
| SB-MBD-005 | Surface texture and notes | Adds surface texture callouts per ISO 21920 or ASME Y14.36 and general or local notes attached to faces. | T2 | 4 |
| SB-MBD-006 | Hole and thread callouts | Generates 3D callouts from hole and thread specifications that update with the feature. | T2 | 4 |
| SB-MBD-007 | Weld callouts | Shows weld data from weldments as 3D callouts. | T3 | 4 |
| SB-MBD-008 | Sheet metal annotation | Annotates bends and flat pattern geometry; annotations made in the formed state can be shown in the flat state where meaningful. | T3 | 4 |
| SB-MBD-009 | Per-configuration annotation | Annotations can be included or excluded per configuration and model state; their values follow configuration parameters. | T2 | 4 |
| SB-MBD-010 | Attachment survival | Annotations follow geometry through regeneration; an annotation whose target disappears is marked orphaned with its last position and listed for reattachment. | T2 | 4 |

## Validation and views

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MBD-011 | Semantic validation | Checks annotations against the chosen standard: incomplete datum reference frames, modifiers invalid for the characteristic, missing datums, tolerance values out of order, and features without size or location control. Each finding links to the annotation. | T2 | 4 |
| SB-MBD-012 | Coverage report | Lists faces and features with no controlling annotation so the definition can be completed. | T3 | 4 |
| SB-MBD-013 | Saved annotation views | Saves camera, visible annotations, section planes and display state as named views that drawings, exports and Arc Review can show. | T2 | 4 |
| SB-MBD-014 | Characteristic list | Numbers every toleranced characteristic and exports the list as CSV or JSON with balloon numbers shown on the model, for inspection planning in [tolerance and inspection](29-tolerance-and-inspection.md). | T3 | 4 |

## Exchange and publishing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-MBD-015 | Semantic PMI export | Exports 3D annotations, saved views and their geometry links as STEP AP242 semantic PMI with graphical presentation. | T2 | 4 |
| SB-MBD-016 | Semantic PMI import | Imports STEP AP242 semantic PMI as 3D annotations attached to the imported faces; unsupported constructs are kept as preserved foreign data and listed in the loss report. | T2 | 4 |
| SB-MBD-017 | Drawing retrieval | Annotations can be retrieved into drawing views; a retrieved annotation stays linked to the 3D source. | T2 | 4 |
| SB-MBD-018 | 3D PDF publishing | Publishes a 3D PDF with the model, saved views, annotations and a parts list. | T3 | 5 |
| SB-MBD-019 | Headless MBD package | The CLI exports a release package with STEP AP242, characteristic list and saved-view images, failing if semantic validation reports errors. | T3 | 4 |
