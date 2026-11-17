# Tolerance and inspection

This area covers tolerance stack-up analysis in one and more dimensions with worst-case and statistical methods, contribution reports, probing cycles and in-process inspection on machines, surface inspection against the model, manual inspection plans and reports, and import of measurement results. Tolerance data lives on the model and drives both analysis and inspection.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Tolerance data | Nominal, limits, fit class, distribution, geometric tolerance | Stored on dimensions and features; read by stack-ups and inspection |
| Stack-up study | Requirement, contributors, method, results | Owned by a document; references model geometry |
| Requirement | Measured gap or clearance, direction, limits | Belongs to a stack-up study |
| Contributor | Source, sign, nominal, tolerance, distribution, process capability | Belongs to a stack chain |
| Stack chain | Ordered contributors in one, two or three dimensions | Belongs to a stack-up study |
| Contribution report | Contribution per contributor, sensitivity | Derived from a stack-up result |
| Probe | Type, stylus length, tip diameter, repeatability | Stored in the tool library |
| Probing cycle | Type, target feature, approach and tolerance settings, action on error | An operation in a probing setup |
| Inspection characteristic | Identifier, nominal, tolerance, method, balloon number | Derived from model dimensions and annotations |
| Inspection plan | Ordered characteristics, instruments, sampling | Owned by a document or part |
| Inspection report | Measured values, deviations, status, signatures | Derived from a plan and a measurement set |
| Measurement set | Source, units, values keyed by characteristic or point | Imported or entered; feeds reports and deviation maps |
| Scan | Point cloud or mesh, alignment, source | Compared with model surfaces |
| Deviation map | Per-point distance, thresholds, statistics | Derived from a scan and the model |

## Tolerance data

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-TOL-001 | Tolerance data on the model | Stores symmetric, bilateral, limit and fit-class tolerances and geometric tolerances on dimensions and features, with a default distribution per tolerance. Stack-ups, drawings and inspection read the same data. | T1 | 4 |
| SB-TOL-002 | Limits and fits | Computes limits for hole and shaft fit classes from ISO 286 tables and shows the resulting clearance or interference. | T1 | 4 |
| SB-TOL-003 | Tolerance data exchange | Reads and writes tolerance and annotation data in STEP AP242 and reports data it cannot map. | T2 | 4 |

## Tolerance stack-up

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-TOL-004 | Stack-up study | Creates a study with a requirement between two geometry references, a chain of contributors and an analysis method. The study updates when the model changes. | T1 | 4 |
| SB-TOL-005 | One-dimensional chain | Builds a chain from a sequence of dimensions along a direction, each with a sign. Nominal values come from the model and tolerances from the tolerance data. | T1 | 4 |
| SB-TOL-006 | Contributor sources | Takes contributors from model dimensions, feature parameters, geometric tolerances, hole and shaft fits, standard part tolerances and manual entries. | T1 | 4 |
| SB-TOL-007 | Loop detection | Proposes the chain between a start and end reference by following mates and joints of the assembly. The user confirms or edits the proposal before it is stored. | T2 | 4 |
| SB-TOL-008 | Worst-case method | Computes the minimum and maximum of the requirement from the extremes of all contributors and reports pass or fail against the limits. | T1 | 4 |
| SB-TOL-009 | Root-sum-square method | Computes mean, standard deviation, the fraction outside the limits and the process capability index for normally distributed contributors. | T1 | 4 |
| SB-TOL-010 | Monte Carlo method | Samples contributor distributions of normal, uniform, triangular, truncated and user-defined type with a set seed and run count, and reports a histogram, percentiles and the fraction outside the limits. | T2 | 4 |
| SB-TOL-011 | Process capability inputs | Accepts capability indices and mean shifts per contributor and reports the resulting parts per million outside the limits. | T2 | 4 |
| SB-TOL-012 | Contribution report | Reports each contributor's share of the variance, its sensitivity and a Pareto ranking, with a what-if table for changed tolerances. | T1 | 4 |
| SB-TOL-013 | Tolerance allocation | Distributes a target requirement over contributors equally or by weights, such as process cost, and shows the resulting tolerances before applying them. | T3 | 4 |
| SB-TOL-014 | Nominal centering | Proposes nominal changes that center the mean of the requirement within its limits. | T3 | 4 |
| SB-TOL-015 | Geometric tolerance contributors | Converts position, parallelism, perpendicularity and flatness tolerances into contributors, including bonus tolerance at maximum and least material conditions. | T2 | 4 |
| SB-TOL-016 | Parameter-linked stacks | Links nominals and tolerances to parameters and configurations and re-evaluates the stack when they change, highlighting changed results. | T1 | 4 |
| SB-TOL-017 | Change alerts | Re-runs affected stack-ups after a design change and marks those that no longer meet their limits. | T2 | 4 |
| SB-TOL-018 | Stack-up report | Produces a report with the chain diagram, contributor table, results and contributions as PDF or HTML, headless from the command line. | T1 | 4 |
| SB-TOL-019 | Results in drawings and models | Places stack results and limits as annotations on drawings and model views. | T2 | 4 |

## Multi-dimensional stack-up

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-TOL-020 | Two-dimensional and three-dimensional chains | Builds vector chains with angles and orientations and projects each contribution onto the requirement direction. | T3 | 4 |
| SB-TOL-021 | Float and clearance contributors | Includes the clearance between a fastener and its holes as a contributor with a float zone and its effect on position and rotation. | T3 | 4 |
| SB-TOL-022 | Datum shift | Includes the shift of a feature within its datum reference frame when the datum features are not at maximum material. | T3 | 4 |
| SB-TOL-023 | Assembly tolerance simulation | Perturbs part positions and features within their tolerance zones under assembly constraints and reports the variation of selected measurements by Monte Carlo sampling. | T3 | 5 |

## Probing and in-process inspection

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-TOL-024 | Probe definition | Defines touch probes, spindle probes and tool setters with stylus length, tip diameter and repeatability in the tool library. | T1 | 5 |
| SB-TOL-025 | Work offset probing | Provides cycles for surface, bore, boss, rectangular pocket and boss, web and slot, inside and outside corner and angle measurement, with protected positioning moves. Results update the work coordinate offsets. | T1 | 5 |
| SB-TOL-026 | Tool probing | Measures tool length and radius and checks for tool breakage with a setter or a spindle probe. | T2 | 5 |
| SB-TOL-027 | Probing parameters | Sets approach distance, overtravel, feed, retract and tolerance bands, and the action on an out-of-tolerance result, such as alarm, offset adjustment or continue. | T1 | 5 |
| SB-TOL-028 | In-process inspection | Measures surfaces, hole diameters, positions and lengths after machining, compares them with model nominals and tolerances, and writes results to machine variables. | T2 | 5 |
| SB-TOL-029 | Probing macros through posts | Maps probing cycles to controller macro calls and variable names in the post-processor definition. | T2 | 5 |
| SB-TOL-030 | Probing simulation | Simulates probe moves and triggers against the stock and checks them for collisions. | T2 | 5 |
| SB-TOL-031 | Offset feedback rules | Updates tool and work offsets from measured errors with limits and averaging over several parts. | T3 | 5 |

## Inspection plans and reports

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-TOL-032 | Inspection characteristics | Derives characteristics with nominal and tolerance from model dimensions, geometric tolerances and annotations, with a numbered identifier for each. The list updates when the model changes. | T1 | 4 |
| SB-TOL-033 | Manual inspection plan | Orders characteristics with method, instrument, frequency and sample size and exports the plan as PDF or CSV. | T1 | 4 |
| SB-TOL-034 | Inspection report | Produces a table of nominal, tolerance, measured value, deviation and pass or fail status, with summary statistics over several samples and signature fields, as PDF or CSV. | T1 | 4 |
| SB-TOL-035 | Ballooned drawings | Numbers characteristics on drawing views and keeps the numbers tied to the plan and report. | T2 | 4 |
| SB-TOL-036 | First-article forms | Provides form templates for first-article reports that fill from the plan and the measured values. | T3 | 4 |
| SB-TOL-037 | Inspection data in the model | Stores inspection characteristics with the model so that exported plans carry the model reference. | T3 | 5 |
| SB-TOL-038 | Statistical summaries | Plots control charts and capability indices over a series of reports. | T3 | 5 |
| SB-TOL-039 | Measured distributions | Uses measured series as the distribution of a contributor in a stack-up. | T3 | 5 |

## Measurement import and surface inspection

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-TOL-040 | Measurement import | Imports results from CSV and from the Quality Information Framework, and maps them to characteristics by identifier. Rows without a match are listed. | T2 | 5 |
| SB-TOL-041 | Inspection program export | Writes inspection plans in standard measurement program and plan formats for coordinate measuring machines. | T3 | 5 |
| SB-TOL-042 | Scan import and alignment | Imports point clouds and meshes in E57, LAS, PLY and STL and aligns them to the model by best fit, datum features or reference points. | T2 | 5 |
| SB-TOL-043 | Surface deviation map | Computes the distance from scan points or mesh to the model surfaces and colors it against thresholds, with a histogram, probes and statistics. | T2 | 5 |
| SB-TOL-044 | Section deviation | Cuts sections through a scan and the model and shows deviation curves with measured dimensions. | T3 | 5 |
| SB-TOL-045 | Feature fitting | Fits planes, cylinders, spheres and cones to scan regions and reports their size, position and orientation against the model. | T3 | 5 |
