# Procedural Three.js Object Patterns

Use this reference only when implementing a model.

## Geometry Choices

- box: flat machinery, furniture, panels, blockout masses
- sphere/ellipsoid: fruit, knobs, organic joints, rounded stones
- cylinder/cone/capsule: trunks, pipes, limbs, handles, bottles, rockets
- torus: rings, tires, loops, trim, cable coils
- shape extrude: logos, flat ornamental plates, blades, keys, leaves
- lathe: vases, bottles, bowls, lamps, wheels
- tube along curve: cables, roots, branches, straps, hoses
- section loft: connected torso/head/limb masses defined by ordered elliptical cross-sections
- conforming shell: fitted static clothing, armor skins, bark layers, and covers linked to a section loft
- branch network: tapered tree limbs, roots, horns, antlers, coral, and tentacle graphs with explicit junctions
- instanced mesh: screws, rivets, leaves, needles, scales, pebbles, repeated ornaments
- surface scatter: deterministic leaves, scales, spikes, and tufts placed on a section loft with exclusion masks
- plane cards: thin leaves, feathers, labels, cloth strips, decals
- deformable surface grid: static drapes, fabric panels, membranes, and stylized lace foundations
- fiber ribbon cards: bounded hair tufts, fur accents, feathers, grass, tassels, and thread-like details
- implicit metaballs: smooth merged organic blobs, foam, wax, and stylized liquid masses
- sculpted surface: one welded implicit field for irregular connected anatomy, creature masses, rocks, and embedded silhouette relief
- crossed volume cards: static cloud, smoke, mist, dust, and fire approximations
- deformation stack: bounded bend, taper, bulge, twist, and normal-noise modifiers for standalone geometry

For compound targets, make `assembly` nodes for semantic groups and attach geometry-bearing `part` nodes beneath them. Advanced paths, profiles, and contours are component-local. Repeated systems should name one bounded layout (`explicit`, `grid`, `radial`, `along-path`, or deterministic `scatter`) and one instanced source primitive; do not copy thousands of component records.

## Semantic Component Naming

Every component `id` and `name` must identify the observed construction part or assembly it represents. Use semantic kebab-case IDs shaped like `<system>-<structural-part>[-<side|index|function>]`, for example `fuselage-cockpit-shell`, `main-rotor-blade-01`, `left-landing-gear-strut`, or `rocket-pod-tube-bank`. The one parentless global root may keep the conventional id `root`.

Reject names made only from implementation words, generic nouns, position qualifiers, or indices: `part-01`, `mesh-a`, `component-2`, `main-body`, `object`, and `group` do not explain what must be constructed. Side or index suffixes are valid only after a structural noun. Do not encode primitive, color, or material as a substitute for identity: `black-cylinder-3` is weaker than `tail-rotor-drive-shaft`; geometry and appearance belong in their dedicated fields.

Names must follow the reference and topology plan. A precise-sounding invented name is still invalid when the evidence does not support that construction. Use a bounded assumption or risk when the exact hidden structure is unknown.

Declare the special representation explicitly and keep it within validator caps. These patterns do not provide simulation, dense strand grooming, automatic fur attachment, exact lace topology, caustics, or raymarched volumetrics.

For a fitted shell or surface scatter, the referenced `section-loft` is the final rest surface: make the linked component its child, keep the linked transform identity, and encode the host shape directly in `sections`. The validator rejects post-loft host modifiers because they would detach clothing, leaves, scales, or fur from the rendered body. Use shell `folds` for fitted surface variation.

Kernel descriptor fields:

- `section-loft`: `representation: elliptical-sections`, ordered `sections[{position,radii,twist}]`, `radialSegments`, `segmentsPerSpan`, and cap flags.
- `conforming-shell`: `representation: loft-shell`, `bodyRef`, positive `thickness`, non-negative `clearance`, `coverage{vRange,angleStart,angleLength}`, plus optional normalized elliptical `openings` and directional `folds`.
- `branch-network`: `representation: branch-graph`, unique `nodes[{id,position,radius}]`, one connected acyclic root with single-parent `edges[{from,to,controlPoints}]`, segment counts, and `capEnds`. It emits overlapping branch tubes and junction volumes, so classify it as an intentional assembly; use `sculpted-surface` when the visible object requires one fused hero surface.
- `surface-scatter`: `representation: loft-surface`, `surfaceRef`, instancable `basePrimitive`/`baseParameters`, `count`, `seed`, normalized `uRange`/`vRange`, optional rectangular `excludeMasks`, scale/spin ranges, normal offset, and alignment flag.
- `geometryDescriptor.deformationStack`: ordered `{type,axis,amount,start,end,power}` modifiers; bend adds `direction`, noise adds `frequency` and `seed`.
- `sculpted-surface`: `representation: field-sculpt`, closed `bounds`, bounded `resolution`, positive `isoLevel`, `connectivity: single-surface`, sphere/ellipsoid/capsule `sources`, and local `surfaceModifiers`. Use `inflate`/`pinch` with `position` plus `radius` or `radii`; use `ridge`/`crease` with capsule `start`/`end`/`radius`. `falloff` controls how sharply each operation blends. Keep every radius resolvable by the chosen grid (at least one quarter of its cell size); tighten bounds or raise resolution instead of authoring sub-cell terms.

Resolution and field-term count share a hard evaluation budget. Consolidate overlapping terms instead of raising both without bound; generation also rejects any result that is disconnected, open, or non-manifold.

## Surface Topology Decision

Complete `surfaceTopologyPlan` before visual modules:

- `continuous-sculpt`: uninterrupted soft/organic/rock/tree mass; one connected host.
- `assembled-solid`: real seam, accessory, socket, plate, tooth, eye, lens, or articulated part.
- `conforming-shell`: a separate fitted layer that follows a host surface.
- `surface-relief`: a silhouette ridge, cheek tuft, scar, fold, or raised form embedded in the host—not a floating mesh.
- `fiber-strand`: real hair, whisker, grass, or thread bound to a host.
- `material-only`: color/roughness/normal detail too small to change silhouette.

One semantic module may contain several strategies, and several landmark regions may reference the same host component. Do not use component count as a proxy for descriptive completeness.

## Detail Decomposition Contract

Every geometry-bearing component owns a `detailPlan`. This is a visual inventory, not a mesh-count quota. A continuous shell may stay one component while its windows, frames, panel breaks, recesses, fasteners, vents, and color-only markings remain independently named and reviewable.

- `simple + atomic`: allowed only with a concrete `atomicityReason` and no children/features.
- `children`: every observed sub-part is a real direct child and `childComponentIds` exactly matches the hierarchy.
- `features`: the host remains continuous, but every important sub-detail maps to executable local feature, topology, repetition, material, or numeric geometry data.
- `hybrid`: combines real child parts with host-bound features.
- `compound` or `complex`: `atomic` is invalid.

Each feature records exact host-local position, rotation, and 3D size, plus evidence, confidence, material references, and acceptance criteria. Its realization target must already exist and be supported. Host geometry records such as sculpt sources, ridges, creases, and deformations use `geometry-feature`; a `topology-group` realization also needs an exact geometry `implementationId`, because topology metadata alone does not build detail. A prose-only detail, unknown id, material pretending to change geometry, or missing complex decomposition is a validation error. Reviewer correction plans may target these records as `detail-feature` IDs.

Faces and hands use named assemblies and landmark-bearing child parts, not a generic one-click primitive. Select `explicit-digits` versus `grouped-digits` from the visible reference, and model hand-to-object contact as part of the hand feature system. See `anatomical-regions.md`.

Material contracts, surface-frequency bands, PBR extraction, and lookdev acceptance live only in `material-lighting-realism.md`.

## Local Feature Types

Use `component.localFeatures` for details that matter to recognizability:

- raised ridge
- recessed groove
- seam line
- screw or rivet
- chip or dent
- scratch cluster
- stain or dirt patch
- decal or label area
- hole or socket
- bevel highlight
- fabric stitch
- leaf vein or serrated edge

Each emitted local feature should also have a corresponding `detailPlan.features[]` record with numeric host-local placement/size, geometry effect, evidence, confidence, acceptance criteria, and `realization.mode: local-feature` pointing to its exact id.

The generator can directly emit raised path details (`seam`, `seam-line`, `raised-ridge`, `fabric-stitch`), point details (`button`, `rivet`, `screw`), and planar `decal` details. Recessed grooves, holes, dents, and silhouette-changing features still need explicit components, extrude holes, implicit subtraction, or displacement-capable topology.

## Verification Cues

A procedural object is usually failing when:

- silhouette reads wrong even before material
- every edge is perfectly sharp or perfectly smooth
- material has one flat color and no roughness variation
- lighting hides the form instead of explaining it
- repeated details are too evenly spaced
- close-up details add triangles but not recognizability
