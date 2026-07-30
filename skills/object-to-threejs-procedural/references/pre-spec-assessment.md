# Pre-Spec Assessment And Quality Contract

Use this reference while filling the integrated `preSpecAssessment` created by `sculpt init`. It is part of the same `ObjectSculptSpec`, not a separate required file.

## Reference preparation evidence

Apply the parent `SKILL.md` preparation policy; record only the assessment evidence here. Judge subject/background separation independently from whether source detail and quality are practical to reconstruct. In `referencePreparation`, state the observed basis for `subjectBackgroundSeparation`. For a generated target, ensure `outputImage` equals `sourceImage`, `outputBackground=solid-white`, `whiteBackgroundValidated` and `subjectContrastValidated` are true, and `modificationPolicy.mode` distinguishes `cleanup-only` from `bounded-simplification`.

Name every intentional simplification in `modificationPolicy.declaredChanges`; keep identity, primary silhouette/proportions, major component layout, signature features, dominant material/color zones, pose, and viewpoint in `protectedTraits`. `sourceImage` remains the sole reconstruction and acceptance reference. `unassessed` preparation, failed background or contrast validation, or undeclared simplification blocks strict quality. Do not use fixed domain profiles; assess observed traits, complexity, and target fidelity.

## Soft Object Classification

Fill every `preSpecAssessment.objectClass` axis. The values below are broad, composable anchors rather than closed enums; use only observed or bounded-inferred descriptors, and use a precise custom value when none fits.

- `primaryType`: a concise noun phrase for what the object is, separate from style, structure, or material
- `representationKind`: solid mesh, surface shell or height field, curve or strand network, point cloud or particle field, sprite or billboard, voxel grid, implicit surface, or volume field; list every applicable kind for a hybrid
- `formLanguage`: organic, geometric, hard-surface, manufactured, mechanical, architectural, botanical, anatomical or creature-like, character-like, sculptural, amorphous, crystalline or faceted, draped or folded, terrain or landform, ornamental, typographic, or symbolic
- `structureKind`: single body, compound or nested assembly, modular assembly, repeated array, branching network, lattice or graph, segmented chain, layered shell, enclosure with internals, articulated hierarchy, deformable continuum, or distributed system
- `motionPotential`: static, whole-object transform, rigid-body, constrained or articulated, skeletal, bend or twist, cloth or soft-body, morphing, detachable or reconfigurable, destructible, particle or fluid flow, volumetric evolution, effect-emitter, or procedural growth or assembly
- `materialFamilies`: metal, mineral/stone/concrete, ceramic/glass, polymer/plastic/rubber, wood/paper, textile/leather, biological tissue, plant matter, hair/fur/fiber, soil/sand, liquid/gel, ice/wax, coating/paint, emissive/energy, volumetric medium, or composite/mixed
- `notes`: optional cross-axis ambiguity or evidence limitation; move actionable uncertainty into structured `assumptions[]` or `risks[]`

`motionPotential` is a pre-spec hypothesis, not authorization to require Interaction or invent hidden mechanisms. Representation kinds classify visible construction without promising unsupported simulation. Material families identify only a broad substance or rendering-medium class; record rigidity, finish, microrelief, and PBR behavior under `material-lighting-realism.md`.

## Sensitive Face And Hand Regions

Inspect visible faces and hands separately from general object complexity. Fill `preSpecAssessment.specializedRegions` and `surfaceTopologyPlan` before creating visual modules: use `declared` with one contract per visible region, or `none` with a reason. A clear region needs a named assembly, landmark-to-geometry mapping, proportion plus expression/pose constraints, dedicated crop views, and its own critical feature target. Landmark names do not require separate meshes; classify continuous tissue, real assemblies, fitted shells, embedded relief, strands, and material-only detail from visible evidence. Partial or occluded anatomy needs explicit unknowns; never infer hidden digits or facial forms as facts.

See `anatomical-regions.md` for the supported landmark, articulation, contact, and evidence contract.

## Complexity Scoring

Score `preSpecAssessment.complexity.scores.*` as ordinal integers `0–3`, from lowest to highest complexity; do not convert normalized `0–1` review scores into this scale. `globalSpec.scores.*` uses the same ordinal scale, with higher better except for `occlusion_risk`. Judge these axes independently:

- silhouette complexity: simple outline to heavily interrupted/organic silhouette
- component count: one piece to many visible subparts
- hierarchy depth: flat object to deep parent-child structure
- repetition density: none to thousands of repeated marks/leaves/scales/rivets
- material layer count: one material to many layered local material responses
- local detail density: plain surface to dense scratches, bumps, moss, seams, chips, pores, or grain
- occlusion risk: fully visible to many hidden/inferred parts
- action readiness need: static to many pivots/sockets/colliders/destruction seams

Map total judgment to:

- `simple`: few parts, low detail, one or two materials
- `moderate`: several parts, visible local detail, shallow hierarchy
- `complex`: many parts, repeated systems, multiple materials, several hierarchy levels
- `ultra`: dense organic/mechanical/architectural structure where fidelity depends on deep hierarchy and repeated microstructure

## Bounded Uncertainty

`preSpecAssessment.unknownsToResolveBeforeImplementation` is a temporary planning queue, not an implementation input. Before building geometry, resolve every entry or move it into exactly one structured record:

- `assumptions[]`: `id`, `statement`, `scope`, `bounds`, `impactIfWrong`, and a concrete `falsifyingCheck`.
- `risks[]`: `id`, `statement`, `scope`, `impact`, and `mitigation`; add `evidenceRefs` when available.

Do not leave uncertainty as a plain sentence. An assumption must state where it applies and what would prove it wrong. A known risk must state its impact and the mitigation used while evidence is missing. `--strict-quality` blocks unresolved unknowns and unbounded legacy strings.

## Quality Contract

Before generating code, define exactly what makes the model good enough:

- definition of done for this object
- minimum macro, meso, and micro feature counts
- required repeated systems and their distribution rules
- required material layers and local overrides
- screenshot viewpoints required for visual comparison
- the resolved `viewHypothesisPolicy` required by `SKILL.md`: record `layoutId` and `layoutMode`, keep `allowedUse=planning-veto` and `acceptanceAuthority=false`, and when skipped require `skipAssessment.objectIsSimple=true`, bilateral/radial/axial symmetry, confidence `>=0.8`, `evidenceRefs`, and a reason
- failure modes that should block `continue`

Make every feature group image-specific and testable: name its visible structure, distribution, material response, and blocking failure instead of using generic goals such as `make leaves look good`.

## Strict Quality Gate

Validate the current pass before generation:

```bash
python3 ../../scripts/sculpt.py validate spec.json --for-pass <current-pass> --strict-quality
```

If strict validation fails:

- refine `preSpecAssessment` if complexity was underestimated
- refine `qualityContract` if definition of done is too generic
- add missing components, material layers, repetition systems, evidence refs, or local features
- only lower the quality bar if the user explicitly accepts a simpler approximation

The gate should block code generation when the spec could describe many different objects instead of the provided reference.

## Suitability Decision

Use `pass` when one target occupies enough of the frame, its silhouette and major materials are readable, and hidden geometry can be bounded honestly.

Use `conditional` when the macro form is clear but one view, partial occlusion, organic simplification, static cloth/fiber/glass/liquid/volume approximations, or missing close-ups limit fidelity. Record the limitation and the evidence needed to remove it.

Use `reject` when the target is ambiguous, badly cropped/blurred/hidden, an identity-critical region cannot be bounded, or the request requires exact mesh extraction, manufacturing dimensions, strand grooming, physical simulation, exact caustics, or dynamic volumetrics that this procedural workflow does not provide.

Request front/side/back views, higher resolution, neutral framing, or material/face/hand close-ups only when that evidence can change the decision. For complex targets, require macro/meso/micro structure, every distinct material layer, local overrides, confidence, and source evidence; otherwise keep suitability `conditional` and list the missing proof.
