# Turning and mill-turn

Turning covers profile definition, facing, roughing, finishing, grooving, threading and parting on lathes, plus the work-holding, live-tooling, sub-spindle and multi-channel features of mill-turn machines. Turning operations share setups, tools, stock simulation, posting and associativity with the rest of manufacturing.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Turning profile | Outer, inner and face contours, axis, source revision | Derived from a body about the spindle axis; referenced by operations |
| Spindle | Main or sub, chuck, jaws, speed limits, position | Belongs to a machine definition and a setup |
| Chuck and jaws | Type, jaw profile, clamp diameter and depth, no-cut zone | Attached to a spindle; acts as a collision object |
| Tailstock and steady rest | Position, center or quill geometry, travel | Belongs to a setup |
| Turret | Stations, orientation, offsets, turret type | Belongs to a machine definition; holds tool assemblies |
| Turning tool | Insert shape and size, nose radius, holder style, hand | Stored in the tool library; placed in a turret station |
| Live tool | Powered tool with speed limits and axis | Placed in a station of a mill-turn turret |
| Turning operation | Strategy, profile, tool, cutting parameters, stock to leave | Belongs to a setup |
| Part transfer | Source spindle, destination spindle, mode, positions | Joins two setups; passes stock between them |
| Channel | Program stream with its own turret or spindle | Contains operations; joined by synchronization points |
| Synchronization point | Wait identifier, participating channels | Orders operations across channels |

## Profiles, stock and work-holding

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-TURN-001 | Turning profile | Extracts outer, inner and face contours from a body by cutting along a plane through the spindle axis. The profile updates when the body changes. | T1 | 5 |
| SB-TURN-002 | Spindle axis | Defines the spindle axis from a model axis, a cylindrical face or a work coordinate system, with a choice of diameter or radius programming. | T1 | 5 |
| SB-TURN-003 | Bar and tube stock | Defines stock as a cylinder or tube with stick-out and offsets, or from the result of an earlier setup. | T1 | 5 |
| SB-TURN-004 | Chuck and jaws | Defines the chuck, jaw profile, clamp diameter and depth, and the resulting no-cut zone, and includes them in collision checks. | T1 | 5 |
| SB-TURN-005 | Part position in chuck | Sets the axial position of the part in the chuck and the length of the part that projects from the jaws. | T1 | 5 |
| SB-TURN-006 | Tailstock and steady rest | Places a tailstock center or quill and a steady rest along the axis, and treats them as no-cut and collision objects. | T2 | 5 |
| SB-TURN-007 | Turret and stations | Defines turret stations with orientation, offset numbers and tool assignments for disk and gang turrets. | T1 | 5 |
| SB-TURN-008 | Turning tools | Defines insert-based tools from insert shape, size, relief, nose radius and holder style, with hand and approach angles. The geometry feeds gouge checks against insert relief and holder. | T1 | 5 |
| SB-TURN-009 | Insert codes | Reads insert geometry from ISO insert codes and warns when entered values contradict the code. | T2 | 5 |
| SB-TURN-010 | Turning cutting data | Stores cutting data per insert grade and workpiece material, including surface speed, feed per revolution and depth of cut. | T2 | 5 |

## Turning operations

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-TURN-011 | Face turning | Faces the end of the stock with rough and finish passes from the outside inward or the reverse, with stock to leave. | T1 | 5 |
| SB-TURN-012 | Profile roughing | Removes material along a profile in axial, radial or contour-parallel patterns with stepdown limits, chip-break retracts and handling of undercuts that the tool cannot reach. | T1 | 5 |
| SB-TURN-013 | Profile finishing | Finishes along the profile with tool nose compensation, leads and an optional spring pass. | T1 | 5 |
| SB-TURN-014 | Grooving | Cuts outer, inner and face grooves in rough and finish passes with width steps, peck cycles and side finishing. | T1 | 5 |
| SB-TURN-015 | Single groove | Cuts a groove in one plunge with dwell and retract, using a tool whose width matches the groove. | T2 | 5 |
| SB-TURN-016 | Threading | Cuts external, internal and face threads, single or multi-start, from pitch or threads per inch. Infeed is radial, flank or alternating flank, with depth degression, spring passes and run-in and run-out lengths. | T1 | 5 |
| SB-TURN-017 | Thread from feature | Reads thread data from a modeled or cosmetic thread feature and fills the threading parameters. | T2 | 5 |
| SB-TURN-018 | Parting | Cuts the part off with an end allowance, an optional end chamfer, reduced feed near the center and a retract for part catchers. | T1 | 5 |
| SB-TURN-019 | Boring | Rough and finish bores internal profiles with boring bars, checking bar reach and clearance against the profile. | T1 | 5 |
| SB-TURN-020 | Axial drilling | Drills, taps and reams along the spindle axis with the drilling cycles used in milling. | T1 | 5 |
| SB-TURN-021 | Edge break | Chamfers or rounds profile corners with a finishing tool at selected edges. | T2 | 5 |
| SB-TURN-022 | Turning trace | Follows an arbitrary curve in the turning plane with the insert nose, for contours that are not part of the body. | T3 | 5 |
| SB-TURN-023 | Rest turning | Cuts only the material left by earlier operations, using the simulated stock. | T2 | 5 |

## Turning parameters and checks

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-TURN-024 | Tool hand and spindle direction | Handles right-hand, left-hand and mirrored tools and the spindle direction so that cutting always loads the insert on its supported side. A mismatch is flagged. | T1 | 5 |
| SB-TURN-025 | Constant surface speed | Holds surface speed with a spindle speed limit, and outputs the controller mode for constant surface speed and fixed speed. | T1 | 5 |
| SB-TURN-026 | Feed mode | Sets feed per revolution or per minute and reduces feed on circular moves where the profile curvature requires it. | T1 | 5 |
| SB-TURN-027 | Nose radius compensation | Computes compensated paths or outputs controller compensation with the tool tip orientation number. | T1 | 5 |
| SB-TURN-028 | Cutting-edge collision check | Checks the insert, its relief and the holder against the profile and stock, and warns of undercuts the tool cannot form. | T1 | 5 |
| SB-TURN-029 | Tool change position | Sets the tool change position and turret index behavior and checks the move for collisions with the part and the work-holding. | T2 | 5 |
| SB-TURN-030 | Turning canned cycles | Outputs profile roughing, finishing, grooving and threading as controller cycles where the controller profile has them, and as expanded moves where it does not. | T1 | 5 |

## Live tooling and mill-turn

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-TURN-031 | Live tooling | Places powered tools in turret stations with speed limits and axial or radial orientation. Milling and drilling operations on a lathe select only powered tools. | T2 | 5 |
| SB-TURN-032 | Spindle indexing operations | Machines features on the face and outer surface with the spindle indexed or clamped in position. | T2 | 5 |
| SB-TURN-033 | Polar and cylindrical interpolation | Machines face contours and wrapped features with polar interpolation and cylindrical interpolation, selected by the controller profile. | T2 | 5 |
| SB-TURN-034 | Off-center machining | Drills and mills with the Y axis off the spindle axis on machines that provide one. | T2 | 5 |
| SB-TURN-035 | Tilting-axis machining | Orients tools with a tilting axis on mill-turn machines and outputs the orientation for turning and milling operations. | T3 | 5 |
| SB-TURN-036 | Milling operations on mill-turn machines | Reuses the milling strategies on mill-turn machines with axis mapping through the machine definition. | T2 | 5 |
| SB-TURN-037 | Sub-spindle and part transfer | Defines a main spindle and a sub-spindle and the transfer between them with synchronized pick-off, rotational synchronization and positions. Stock for the second setup is taken from the first. | T2 | 5 |
| SB-TURN-038 | Secondary operations | Machines the back of the part in the sub-spindle with its own coordinate system, chuck and jaws, and includes them in collision checks. | T2 | 5 |
| SB-TURN-039 | Bar feeding and pulling | Models bar feed length, bar pull and part ejection as machine events in the program. | T3 | 5 |
| SB-TURN-040 | Channels and synchronization | Assigns operations to program channels with their own turrets or spindles and joins them with synchronization points, which the post processor outputs as wait codes. | T3 | 5 |
| SB-TURN-041 | Synchronization timeline | Shows operations of all channels on a timeline with idle gaps and lets the user move operations and synchronization points. Collision checks cover simultaneous moves. | T3 | 5 |
| SB-TURN-042 | Simultaneous cutting | Runs two tools at once on opposite sides of the part with a balanced cutting load and checks the pair for collisions. | T3 | 5 |

## Simulation and documents

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-TURN-043 | Turning stock simulation | Simulates stock removal of rotating stock with the insert and holder and reports collisions with the chuck, tailstock and turret. | T1 | 5 |
| SB-TURN-044 | Mill-turn machine simulation | Simulates all axes, turrets and spindles of a mill-turn machine on the posted program. | T2 | 5 |
| SB-TURN-045 | Turning setup sheet | Adds chucking, stick-out, spindle and turret station lists to the setup sheet. | T2 | 5 |
