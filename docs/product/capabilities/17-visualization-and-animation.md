# Visualization, rendering and animation

The viewport shows models in technical and realistic styles; photoreal rendering is provided by
RenderBuilder through the Arc Scene contract. Exploded states and animation sequences explain how an
assembly goes together and feed drawings and videos.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Appearance | Base color, metalness, roughness, bump or normal map, opacity, emission, texture mapping | Assigned to bodies, faces, features or occurrences; follows a material by default |
| Decal | Image, projection (planar, wrapped), size, position | Applied to faces of a body |
| Environment | HDR image, rotation, brightness, background, ground plane | Part of a scene |
| Light | Kind (point, spot, area, directional, photometric), intensity, color | Part of a scene |
| Camera | Position, target, projection, focal length, depth of field, exposure | Saved in the document; used by views, renders and animations |
| Scene | Environment, lights, cameras, render settings | Exported to RenderBuilder as an Arc Scene |
| Exploded state | Steps of component displacements, trail lines | Owned by an assembly; used by drawings and animation |
| Animation sequence | Tracks of keyed actions over time | Owned by an assembly; exported as video |

## Viewport display

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-VIS-001 | Visual styles | The viewport shows shaded, shaded with edges, wireframe, hidden-line and technical illustration styles, switchable per viewport. | T1 | 3 |
| SB-VIS-002 | Realistic style | A realistic style adds environment lighting, shadows, reflections, ambient occlusion and a ground plane in real time. | T2 | 3 |
| SB-VIS-003 | Level of detail | Display tessellation density adapts to screen size and can be set per document; silhouette edges are computed on the GPU. | T1 | 3 |

## Appearances

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-VIS-004 | Appearance library | Provides metals, plastics, glass, paint, wood and textured appearances on the Arc Scene material model; libraries are documented files and can be hosted by Arc Hub. | T1 | 3 |
| SB-VIS-005 | Assign appearance | Assigns appearances to occurrences, bodies, features or faces; the most specific assignment wins and overrides are listed. | T1 | 3 |
| SB-VIS-006 | Edit appearance | Edits color, metalness, roughness, bump or normal maps, opacity and emission with a live preview. | T2 | 3 |
| SB-VIS-007 | Texture mapping | Positions, scales and rotates textures with planar, box, cylindrical and spherical projections. | T2 | 4 |
| SB-VIS-008 | Decals | Places images on faces as planar or wrapped decals with size and position controls; decals follow the face through regeneration. | T2 | 4 |

## Scenes and rendering

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-VIS-009 | Environments | Selects HDR environments from a library with rotation and brightness, and a solid, image or transparent background. | T2 | 4 |
| SB-VIS-010 | Lights | Adds point, spot, area, directional and photometric lights from IES files. | T2 | 4 |
| SB-VIS-011 | Cameras | Saves cameras with perspective or orthographic projection, focal length, depth of field and exposure. | T1 | 3 |
| SB-VIS-012 | Reflection preview | Shows environment-map reflections on selected faces to judge surface quality. | T2 | 4 |
| SB-VIS-013 | Interactive render view | Streams the scene to RenderBuilder through the Arc Scene contract and shows progressively refined frames; model edits are sent as incremental updates. | T2 | 4 |
| SB-VIS-014 | Final renders | Queues stills, turntables and panoramas at chosen resolution and quality to a local RenderBuilder process or an Arc Hub render service, and keeps results in a render gallery. | T2 | 4 |
| SB-VIS-015 | Image capture | Saves the viewport as PNG or JPEG at a chosen resolution, with transparent background optional. | T1 | 3 |
| SB-VIS-016 | Live visualization link | Sends the assembly to MeshBuilder as a live scene that updates when the model changes. | T3 | 5 |

## Exploded states

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-VIS-017 | Automatic explosion | Explodes an assembly one level or all levels along directions derived from joints and geometry. | T1 | 3 |
| SB-VIS-018 | Manual steps | Moves and rotates occurrences in steps along axes with trail lines; steps can be reordered and edited. | T1 | 3 |
| SB-VIS-019 | Independence from relationships | Exploded states displace occurrences without changing joints, constraints or position states. | T1 | 3 |
| SB-VIS-020 | Multiple exploded states | An assembly can store several named exploded states, each usable by drawing views. | T1 | 3 |

## Animation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| SB-VIS-021 | Animation sequences | A sequence holds tracks of keyed occurrence transforms, explosion steps, visibility and transparency changes, camera moves and callouts on a time line. | T2 | 4 |
| SB-VIS-022 | Joint-driven motion | Drive joints and motion studies can be placed in a sequence so mechanisms move through their kinematics. | T2 | 4 |
| SB-VIS-023 | Parameter and light actions | Sequences can animate parameter values, lights and appearances. | T3 | 4 |
| SB-VIS-024 | Assembly instructions | Generates a sequence from assembly order that shows one step per subassembly or occurrence with callouts. | T3 | 4 |
| SB-VIS-025 | Video export | Exports sequences as MP4, WebM or PNG image sequences from the viewport or through RenderBuilder. | T2 | 4 |
| SB-VIS-026 | Snapshots for drawings | Any frame of a sequence can be saved as a view state used by drawing views. | T2 | 4 |
