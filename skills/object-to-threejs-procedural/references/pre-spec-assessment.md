# Pre-Spec Assessment And Quality Contract

Use this reference while filling the integrated `preSpecAssessment` created by `sculpt init`. It is part of the same `ObjectSculptSpec`, not a separate required file.

## Reference preparation gate

Assess two independent conditions before describing geometry: subject/background separation, and whether source detail/quality is practical to reconstruct. Use the supplied image directly when its boundary is readable and its construction is manageable; a white, neutral, transparent, or strongly contrasting background is acceptable. Use the `imagegen` skill when the subject mixes with its background, source defects obscure construction, complexity would make direct procedural reconstruction impractical, or a real-object photo needs a cleaner buildable 3D-style target. ImageGen must produce a clean solid-white background with strong contrast, not transparency.

The generated reference may simplify declared non-signature microdetail, surface noise, tiny repeated detail, ambiguous minor geometry, and difficult real-world surface variation into clean buildable masses. It must preserve the recognizable class, macro silhouette and proportions, major component layout, signature features, dominant material/color zones, pose, and viewpoint in the generated target. Store the result as `sourceImage`; it becomes the sole reconstruction and acceptance reference. An `unassessed` preparation, unvalidated white background, or undeclared simplification blocks strict quality.

Do not use fixed domain profiles. Assess the object from observed traits, complexity, and target fidelity.

## Soft Object Classification

Describe the object using multiple axes:

- form language: organic, hard-surface, mechanical, architectural, botanical-like, character-like, amorphous, sculptural, fabric-like, transparent-like
- structure kind: single body, compound object, branching hierarchy, repeated modules, layered shell, articulated assembly, deformable surface
- motion potential: static prop, whole-object transform, articulated, bendable, detachable, destructible, effect-emitter
- material families: wood, bark, leaf, metal, stone, ceramic, plastic, rubber, cloth, glass-like, liquid-like, skin-like, mixed

These are descriptors, not domain templates. Use only what the image supports.

Keep material observations short and executable. For every important material, fill `surfaceDescriptor` with three separate claims: physical `rigidity`, optical `finish`, and tactile `microRelief`. Each claim needs `basis: observed|inferred` and confidence, while the descriptor needs source `evidenceRefs`. Do not use “smooth” to mean glossy: smooth is relief, glossy is low roughness, and a rigid surface may still be matte or pebbled. The numeric `roughness` and selected `normal|bump|displacement` channel must agree with these claims before lookdev.

## Sensitive Face And Hand Regions

Inspect visible faces and hands separately from general object complexity. Fill `preSpecAssessment.specializedRegions` and `surfaceTopologyPlan` before creating visual modules: use `declared` with one contract per visible region, or `none` with a reason. A clear region needs a named assembly, landmark-to-geometry mapping, proportion plus expression/pose constraints, dedicated crop views, and its own critical feature target. Landmark names do not require separate meshes; classify continuous tissue, real assemblies, fitted shells, embedded relief, strands, and material-only detail from visible evidence. Partial or occluded anatomy needs explicit unknowns; never infer hidden digits or facial forms as facts.

See `anatomical-regions.md` for the supported landmark, articulation, contact, and evidence contract.

## Complexity Scoring

Do not mix the two numeric contracts:

- `globalSpec.scores.*` and `preSpecAssessment.complexity.scores.*`: ordinal **integers 0–3**. They measure axis strength or complexity, not quality percentage. For complexity, `0` means lowest/none and `3` means highest. For suitability, higher is better except `occlusion_risk`, where higher means worse.
- Review `overallScore`, `layerScores.*`, `aiVisionScore`, `minimumScore`, and confidence: normalized **numbers 0–1**. Decimals such as `0.96` and `0.82` are valid and expected.

Never convert `0.96 → 3` or otherwise map between these scales mechanically. Judge each ordinal axis from its meaning. Score each complexity axis from integer 0 to 3:

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
- screenshot viewpoints required for visual comparison; package two to four views in one 2x2 sheet
- one ImageGen 2x2 planning sheet for hidden-form planning by default; use an exploded first tile for a complex/ultra assembly with separable or internal parts, and skip only for an explicitly assessed `simple` object with strong evidenced bilateral, radial, or axial symmetry
- failure modes that should block `continue`

Good feature groups are specific to the image:

- weak: `make leaves look good`
- strong: `leaf clusters must form irregular overlapping canopy masses, with varied card size/orientation/color and gaps exposing secondary branches`

- weak: `add bark texture`
- strong: `trunk and primary branches need vertical ridges, cavity-darkened cracks, moss/lichen patches near roots and inner forks, roughness variation, and nonuniform displacement/bump`

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
