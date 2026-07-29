# Three.js Object Sculptor

Turn the object in an attached image into a quality-gated, animation-ready procedural Three.js model built entirely with code.

Three.js Object Sculptor is a Codex plugin for rebuilding the visible object in a user-provided or ImageGen-prepared `sourceImage` as a code-only Three.js model. It does not try to do photogrammetry, download an art pack, or extract a perfect mesh from one image. Instead, it guides Codex through a sculpting workflow: validate the image, describe the object precisely, decompose it into geometry and material systems, build from blockout to detail, wire an animation-friendly hierarchy, then compare the browser render against the active source reference.

## Demo

### Tower Ship

[Open the live tower ship demo](https://3dship.harrysoftware.com)

![Procedural Three.js tower ship demo generated from an attached reference image](assets/tower-ship-demo.png)

This tower ship study shows the intended output shape: a browser-rendered, code-sculpted Three.js object rebuilt from an attached reference image, with procedural geometry, articulated parts, material work, and interactive controls.

### Ancient Autumn Tree

[Open the live ancient autumn tree demo](https://tree.harrysoftware.com/)

![Procedural Three.js ancient autumn tree reconstructed from an attached reference image](assets/ancient-autumn-tree-demo.png)

This botanical study reconstructs a complex ancient tree with procedural curves, deterministic branching, layered bark materials, dense autumn foliage, and an animation-ready hierarchy.

## At A Glance

- **Name:** Three.js Object Sculptor
- **Category:** Codex plugin for image-to-procedural-3D workflows
- **Input:** an attached object image, reference screenshot, or local image path
- **Output:** a code-only procedural Three.js object factory, backed by an `ObjectSculptSpec`
- **Primary goal:** recreate the target object's silhouette, component structure, materials, lighting response, and action-ready hierarchy in browser-friendly Three.js code
- **Best for:** animation-ready real-time props, game objects, scene dressing, destructible objects, product-style objects, botanical objects, mechanical parts, and stylized reference reconstructions
- **Not for:** photogrammetry, exact mesh extraction, scanned assets, downloaded art packs, or guaranteed production-perfect geometry from one image

## What It Does

- Optimizes for perceptual identity inside an explicit `viewingContract`: topology
  and physical structure are implementation constraints only where they change
  the required silhouette, parallax, shadows, material response, or motion.
- Stores a `visualIdentitySpec` and salience graph so review fixes the most
  recognizable visible blockers before low-impact construction detail.
- Routes components through composable executable capability packs for
  hard-surface machinery, organic skin/eyes, hair/fur/fiber, fabric, transmissive
  surfaces, vegetation, procedural motion, emissive/volume effects, and
  markings/decals/text.
- Applies typed, impact-assessed correction batches only to a separate challenger
  spec. Unsupported reviewer requests fail as `capability-gap` instead of
  becoming guessed prose or unverified code.
- Defaults CLI-created specs to one final artifact-bound user approval; the
  stricter phase-by-phase approval mode remains available when required.
- Validates whether an image is suitable for procedural 3D reconstruction.
- Uses ImageGen to create a high-contrast solid-white reconstruction target when the object blends into its background, source quality is poor, direct reconstruction is impractically complex, or a real-object photo needs a cleaner buildable 3D-style reference. The generated `sourceImage` becomes the sole reconstruction and acceptance target.
- Integrates the pre-spec complexity assessment into the main `ObjectSculptSpec` before code generation.
- Writes an `ObjectSculptSpec` with component hierarchy, materials, lighting, pivots, sockets, animation anchors, destruction anchors, and quality targets.
- New specs default to one progressive phase-local contract so Blockout can render before Form, PBR, interaction, receipts, or final provenance are authored. Modular v4 manifests remain opt-in for independently isolatable subsystems.
- Surface topology and recursive detail planning are promoted into the spec at Form, where they have an implementation consumer; they no longer block the first silhouette render.
- Requires a planned per-component detail inventory: complex components cannot be one atomic blob, and every visible sub-detail maps by exact ID to executable child geometry, local feature, topology, repetition, material, or numeric parameter data.
- Supports compound objects through nested `assembly` groups plus geometry-bearing `part` nodes.
- Uses one geometry registry for validation and generation, including tube, lathe, extrude, curve sweep, section lofts, fitted shells, branch networks, masked surface scatter, modifiers, and bounded instancing; unsupported geometry is rejected instead of becoming a box silently.
- Supports bounded `sculpted-surface` fields that fuse irregular masses and embedded ridges/creases into one connectivity-checked welded mesh.
- Adds opt-in static approximations for organic bodies, fitted cloth/layers, trees/roots/horns, hair/fur, smooth merged forms, glass/liquid, and soft volumes through bounded special geometry and material profiles; physics simulation and raymarched volumes remain out of scope.
- Uses one quality-first pipeline: `blockout -> form -> lookdev -> interaction`. The Interaction gate disappears after a justified `not-required` assessment; performance is a separate optional audit.
- Provides one `scripts/sculpt.py` command surface while keeping the older individual scripts compatible.
- Generates a code-only Three.js factory skeleton from the current unlocked sculpt pass.
- Assesses ordinary object motion even when the user is silent, preserves exact component pivots during form, and activates interaction only for observed, user-requested, or high-confidence domain-prior behavior.
- Packages reference/render screenshots into one comparison sheet for AI vision review.
- Packs two to four review views into one 2x2 comparison sheet while retaining each original full-resolution image, hash, dimensions, and provenance.
- Requires a user-visible update after every complete visual cycle or named blocker, including current/total gates and a recalculated ETA range; internal schema/build/cache operations are batched and do not masquerade as modeling progress.
- Defaults to one identity-consistent ImageGen 2x2 turnaround before the first source-backed Blockout build, then caches independently hashed `three-quarter`, `side`, `back`, and `front` tiles; only explicitly simple, strongly symmetric objects may skip it. The sheet is planning-veto evidence, never the acceptance authority.
- Separates material rigidity, optical finish, and microrelief in an evidence-bound `surfaceDescriptor`; lookdev cannot proceed with an unassessed or contradictory surface.
- Uses a lightweight visual gate: one composite AI similarity score `>= 0.70`, a hash-bound blind-scout `approve`, then explicit user approval.
- Runs an independent, phase-scoped blind visual scout from image evidence plus only the active `phaseId` and compact visual rubric. It must scan every rubric check—including excessive reference deviation, misaligned or implausible connections, reference-relative balance, signature-detail plausibility, and material/surface fidelity—before reporting at most three highest-impact directions. Passed phases remain improvable rather than frozen; major defects in either current or earlier work can reject, while future-phase issues are deferred. It is a binary gate (`approve`/`reject`) and never receives spec IDs/parameters/scores.
- Keeps the primary reviewer for composite scoring and exact component corrections, then requires explicit user approval before the next phase.
- Requires explicit user approval after deterministic checks and both AI review layers pass each active phase; change requests must identify the visual region, problem, and expected direction before refinement.
- Numeric image-overlap metrics do not participate in acceptance or rollback. Visual regression is decided from the source/current/previous image comparison.
- Supports reference-derived procedural PBR evidence: albedo, roughness estimate, height, normal, and AO maps.
- Emits a disposable PMREM look-dev environment from an external
  equirectangular texture or a procedural studio fallback, applies anisotropy
  to standard reflective materials, supports evidence-bound rust/oxide layers,
  and generates planar, cylindrical, or spherical UV projection for procedural
  component geometry.
- Emits bounded rounded-box edge treatment and supported local seam/ridge/stitch/button/rivet/decal geometry instead of leaving those details as metadata.

## Use Cases

- Convert an attached object image into a procedural Three.js model generated entirely with TypeScript and geometry code.
- Build animation-ready Three.js props with meaningful pivots, sockets, parent-child hierarchy, and transform anchors.
- Recreate reference objects as browser-friendly procedural assets without relying on downloaded meshes or external art packs.
- Generate a structured object spec before implementation, so Codex understands geometry, materials, lighting, local surface features, and interaction readiness.
- Create destructible or transformable objects by planning detachable parts, fracture seams, colliders, and effect emitters before the model is coded.
- Compare the rendered model against `sourceImage` with AI vision and block progress when critical features do not match.
- Produce reusable procedural object factories for Three.js games, WebGPU demos, interactive prototypes, and visual experiments.

## Why This Exists

Procedural 3D generation can fail in a very specific way: the silhouette is "kind of right", but the object loses the details that make it recognizable. This plugin is designed to slow Codex down at the right moments:

- First understand what object class and complexity tier it is dealing with.
- Define what "good enough" means for this specific object.
- Build from coarse structure to fine surface response.
- Fail a pass if an identity-defining feature is wrong, even when the overall score looks acceptable.

The result is less "one-shot generated mesh" and more "Codex as a procedural sculptor with checkpoints": block out the form, attach the moving parts correctly, layer the materials, then keep refining until the model reads like the object in the attachment.

## Requirements

- Codex with local plugin support.
- Python 3.10 or newer.
- A browser project using Three.js when you want to implement the generated factory.
- For visual acceptance: a screenshot from the rendered model and an AI vision reviewer.

The helper scripts use Python standard-library modules and shell image tooling when available. They do not require Playwright or a downloaded Chromium bundle.

## Install For Codex

Clone the plugin source into your local plugin folder. Replace `REPOSITORY_URL` with the Git URL for your copy of this repository:

```bash
mkdir -p ~/plugins
git clone REPOSITORY_URL ~/plugins/threejs-object-sculptor
```

Make sure your local Codex marketplace has an entry for the plugin. If you already have `~/.agents/plugins/marketplace.json`, add this object to its `plugins` array:

```json
{
  "name": "threejs-object-sculptor",
  "source": {
    "source": "local",
    "path": "./plugins/threejs-object-sculptor"
  },
  "policy": {
    "installation": "AVAILABLE",
    "authentication": "ON_INSTALL"
  },
  "category": "Productivity"
}
```

If you do not have a local marketplace file yet, create `~/.agents/plugins/marketplace.json` with:

```json
{
  "name": "local",
  "interface": {
    "displayName": "Local Plugins"
  },
  "plugins": [
    {
      "name": "threejs-object-sculptor",
      "source": {
        "source": "local",
        "path": "./plugins/threejs-object-sculptor"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    }
  ]
}
```

Install it in Codex:

```bash
codex plugin add threejs-object-sculptor@local
```

Start a new Codex thread after installation so the plugin skill is loaded.

## Quick Start

In Codex, attach an object image and ask:

```text
Use Three.js Object Sculptor to turn the object in this attachment into a procedural Three.js model built entirely with code.
```

![Codex prompt example using an attached object image with Three.js Object Sculptor and Browser](assets/codex-prompt-example.png)

Interaction intent is optional. Mention behavior only when you need something beyond the object's normal motion:

```text
Make the visible hatch open on its hinge. Do not add physics or destruction.
```

The plugin will guide Codex through:

1. Image suitability check.
2. A concise stable-core spec.
3. Blockout silhouette convergence.
4. Recursive Form construction with a default 2x2 hidden-view turnaround; only simple, strongly symmetric objects may skip it.
5. Lookdev material/lighting refinement.
6. Object-aware Interaction assessment and implementation when required.
7. Side-by-side AI review, champion promotion/rollback, then explicit user approval in every active phase.

## Progressive Four-Phase Workflow

New work uses one evolving spec and one concise current-phase context packet:

```bash
python3 scripts/sculpt.py init "Ancient Autumn Oak" \
  --image ./reference/oak-tree.png \
  --reference-separation clear \
  --complexity complex \
  --out object-sculpt-spec.json

python3 scripts/sculpt.py context object-sculpt-spec.json
python3 scripts/sculpt.py validate object-sculpt-spec.json \
  --for-pass blockout --strict-quality
python3 scripts/sculpt.py generate object-sculpt-spec.json \
  --out src/AncientOak.generated.ts \
  --wrapper-out src/AncientOak.ts
```

`context` returns the stable core plus only the current phase's editable fields and explicitly lists future work that must remain deferred. The builder applies one correction batch, builds/renders once, creates the `sourceImage` comparison, and submits both AI review layers. Four generated planning views are presented as one identity-consistent 2x2 sheet; complex/ultra assemblies may use an exploded first tile while the other three views remain assembled.

Blockout uses the observed primary view and macro geometry only. Form recursively expands complex components, validates attachments, and uses one ImageGen 2x2 turnaround by default. It may skip that sheet only for an explicitly assessed simple object with strong evidenced symmetry. Lookdev owns PBR, materials, surface descriptors, lighting, and contact shadows. Interaction owns motion inference, pivots, runtime receipts, motion clearance, and final typecheck. A justified `not-required` motion assessment removes the runtime gate.

Each valid challenger is compared with the phase champion using the composite AI similarity score and blind-scout verdict over the source/current/previous images. A failed challenger is checkpointed for audit and rolled back. Three consecutive non-improvements require a materially different representation. Schema, hash, or evidence-scope failures do not spend this quality budget. A system-passed champion remains in `awaiting-user-approval`; only explicit approval of that exact reviewed artifact unlocks the next phase.

Modular layout remains available with `--layout modular` for genuinely isolatable systems. Module comparisons require matching module/component scope and a hash-bound observed crop or mask; a full-object reference beside an isolated module is rejected before scoring.

Legacy schema 2.0/3.0 specs remain readable. Upgrade deliberately with `python3 scripts/sculpt.py migrate <spec> --in-place`; existing review evidence is retained for audit but is not rewritten to manufacture a passing review.

Run `python3 scripts/sculpt.py --help` for the `compare`, `review`, `probe`, and `pbr` commands. The individual scripts below remain available for compatibility.

Inspect the executable capability coverage for the current object or phase:

```bash
python3 scripts/sculpt.py capabilities object-sculpt-spec.json \
  --phase form --in-place
```

Apply a validated perceptual correction batch to a separate challenger. The
command refuses to overwrite the champion path:

```bash
python3 scripts/sculpt.py correct object-sculpt-spec.json \
  --batch review/perceptual-corrections.json \
  --out object-sculpt-challenger.json
```

An ImageGen-cleaned or simplified white-background `sourceImage` is the sole
active acceptance target, and the cached 2x2 planning sheet remains
planning-veto evidence only.

## Review Render Quality

New specs use `viewingContract.version=2` with a global
`viewingContract.renderPipeline`. Anti-aliasing is therefore stable from
Blockout onward instead of being hidden inside a component pattern or deferred
until Lookdev. Legacy viewing-contract v1 specs remain readable; migration adds
the v2 render contract deliberately.

Generated factories export a host-owned WebGL review controller:

```ts
const renderer = new THREE.WebGLRenderer(
  recommendedSculptRendererOptions(),
);
const pipeline = await createSculptReviewPipeline({
  renderer,
  scene,
  camera,
});

pipeline.resize(width, height, window.devicePixelRatio);
pipeline.render();
const renderReceipt = pipeline.receipt();
```

`auto` uses verified native canvas MSAA when the existing context actually has
samples. Otherwise, reference-fidelity selects SMAA and the performance preset
selects FXAA. The generated pass order preserves Three.js color handling:
`RenderPass -> SMAAPass -> OutputPass` for SMAA, and
`RenderPass -> OutputPass -> FXAAPass` for FXAA. Addons are dynamically imported
from the same installed `three` package, so `createSculptReviewPipeline()` is
asynchronous and addon/version incompatibility fails explicitly.

The controller never creates or disposes the host renderer, scene, camera, or
animation loop. `dispose()` is idempotent and releases only its owned passes and
composer. One global pipeline is allowed per scene, including modular previews.

Bind standalone render evidence to the exact AA/output state:

```bash
python3 scripts/sculpt.py compare \
  --reference reference.png \
  --render render.png \
  --render-receipt render-receipt.json \
  --out comparison.png \
  --manifest-out evidence.json
```

A required pipeline cannot pass strict perceptual review without a matching
contract hash, an active AA mode, the correct pass chain, and at least one frame
rendered through the controller. Pixel overlap is still not an acceptance
metric.

## Compatibility / Individual Scripts

The unified progressive `sculpt.py` flow above is recommended. The individual scripts and optional modular layout remain available for advanced or legacy workflows.

Probe a reference image:

```bash
python3 scripts/probe_reference_image.py ./reference/oak-tree.png
```

Create a pre-spec assessment:

```bash
python3 scripts/new_pre_spec_assessment.py "Ancient Autumn Oak" \
  --image ./reference/oak-tree.png \
  --complexity complex \
  --out assessment.json
```

Create a starter sculpt spec:

```bash
python3 scripts/new_sculpt_spec.py "Ancient Autumn Oak" \
  --image ./reference/oak-tree.png \
  --assessment assessment.json \
  --layout monolithic \
  --out object-sculpt-spec.json
```

Validate the spec:

```bash
python3 scripts/validate_sculpt_spec.py object-sculpt-spec.json \
  --for-pass blockout \
  --strict-quality
```

Check which sculpt pass is unlocked:

```bash
python3 scripts/sculpt_pass_orchestrator.py status object-sculpt-spec.json
```

Generate the current pass:

```bash
python3 scripts/generate_threejs_factory.py object-sculpt-spec.json \
  --out src/AncientOak.generated.ts \
  --wrapper-out src/AncientOak.ts
```

Create a comparison sheet after rendering the model:

```bash
python3 scripts/make_visual_comparison_sheet.py \
  --reference ./reference/oak-tree.png \
  --render ./screenshots/oak-render.png \
  --out ./screenshots/oak-comparison.png \
  --manifest-out ./screenshots/oak-evidence.json \
  --diagnostics-dir ./screenshots/oak-diagnostics \
  --json
```

The diagnostic overlay may visualize reference-only silhouette, render-only silhouette, and overlap to help a person inspect framing; it emits no overlap score and has no acceptance authority.

Record an AI vision review:

```bash
python3 scripts/append_sculpt_review.py object-sculpt-spec.json \
  --pass-id blockout \
  --fidelity 0.82 \
  --action continue \
  --summary "Blockout silhouette and primary trunk fork are acceptable." \
  --evidence-set-json ./screenshots/oak-evidence.json \
  --ai-vision-score 0.82 \
  --reviewer-model example-vision-model \
  --layer-scores-json '{"silhouette":0.79}' \
  --feature-reviews-json ./reviews/blockout-features.json \
  --ai-vision-notes "Main proportions pass; canopy microstructure remains deferred." \
  --in-place
```

Sync the pass state:

```bash
python3 scripts/sculpt_pass_orchestrator.py sync object-sculpt-spec.json --in-place
```

## PBR Extraction

The plugin can extract reference-derived procedural PBR evidence from image pixels:

```bash
python3 scripts/extract_reference_pbr.py ./reference/oak-bark.png \
  --material-crop-confirmed \
  --mask ./reference/oak-bark-mask.png \
  --out-dir ./generated/pbr/oak-bark \
  --material-id bark \
  --target-threshold 0.75 \
  --report ./generated/pbr/oak-bark/report.json
```

This produces useful material evidence such as palette, albedo, roughness estimate, height, normal, and AO maps. It uses broad/meso/micro de-lighting and tile-safe border blending; `--mask` is optional but useful when the crop still contains other materials. It is not exact inverse rendering from a single image. An unconfirmed full screenshot remains diagnostic and cannot unlock lookdev, even when its extraction score is high.

## Quality Gates

The plugin uses two levels of visual acceptance:

- Overall match: silhouette, proportions, camera/view, material read, and lighting.
- Semantic feature match: selected critical object features scored from the same full reference/render comparison image.

Examples of critical feature targets:

- Hull shape, cabin blocks, sail rigging, and rails for a boat.
- Trunk fork, major branch sockets, canopy mass, bark material, and root flare for a tree.
- Body shell, wheels, windshield, grille, and headlight clusters for a vehicle.
- Face identity/expression and each visible hand or hand-to-object contact for a character; these use dedicated close-up views and cannot be hidden inside the overall score.

If a critical feature fails its threshold, the pass fails even if the global score is high.

New specs execute the visual gate at composite AI similarity `0.70` + blind-scout approve + user approval. `balanced` remains an explicit profile hint. Polygon, draw-call, FPS, and pixel-overlap metrics never select or approve a modeling pass.

## FAQ

### Is this photogrammetry?

No. Three.js Object Sculptor does not reconstruct a scanned mesh from pixels. It helps Codex infer a procedural model plan and generate Three.js code that approximates the visible object.

### Does it generate a GLB file?

Not by default. The main output is a code-only Three.js factory and an `ObjectSculptSpec`. You can add export tooling in the target Three.js project if you later need GLB output.

### Can the generated model be animated?

Yes. Animation readiness is a core goal. The spec asks for pivots, sockets, parent-child hierarchy, transform channels, collider proxies, and detachable or breakable component roles where relevant.

### Does it use downloaded assets or art packs?

No. The workflow is designed around generated geometry, procedural materials, local image evidence, and code-native Three.js construction.

### Can one image create an exact production model?

No. One image can be enough for a useful procedural reconstruction, but hidden sides, exact dimensions, and fine material behavior may need assumptions, extra reference views, or a lower-fidelity target.

### How does the plugin decide whether the model is good enough?

It uses a quality contract, adaptive build passes, browser screenshots, one reference/render comparison sheet, and AI vision review. Critical features can fail a pass even when the global visual score looks acceptable.

## Project Layout

```text
.codex-plugin/plugin.json
skills/object-to-threejs-procedural/SKILL.md
skills/object-to-threejs-procedural/references/
scripts/
```

Important scripts:

- `sculpt.py`: unified command entry point for new work.
- `sculpt_contract.py`: shared quality-pipeline, evidence, motion, and state rules.
- `sculpt_manifest.py`, `sculpt_module_contract.py`, `sculpt_module_state.py`, and `sculpt_module_review.py`: v4 composition, contracts, risk scheduling, independent review attempts, and cache validity.
- `sculpt_geometry.py`: shared geometry handlers, parameter checks, repetition limits, and TypeScript emitters.
- `sculpt_specialized_regions.py`: face/hand landmark, hierarchy, occlusion, contact, and close-up review contracts.
- `migrate_sculpt_spec.py`: explicit additive migration to schema 3.2.
- `probe_reference_image.py`: technical image metadata probe.
- `new_pre_spec_assessment.py`: compatibility wrapper for the integrated pre-spec.
- `new_sculpt_spec.py`: starter `ObjectSculptSpec` with integrated pre-spec.
- `validate_sculpt_spec.py`: structural and strict quality validation.
- `sculpt_pass_orchestrator.py`: pass locking and pipeline sync.
- `generate_threejs_factory.py`: current-pass Three.js factory generator.
- `make_visual_comparison_sheet.py`: full reference/render comparison image.
- `append_sculpt_review.py`: self-correction review recorder.
- `extract_reference_pbr.py`: reference-derived PBR evidence extraction.
- `sculpt_image_io.py`: shared dependency-free image codec used by comparison and PBR extraction.

## Limitations

- A single image cannot reveal hidden sides or guarantee exact geometry.
- Transparent glass, smoke, liquid, fur, fine cloth, and exact likeness tasks may require extra references or a lower-fidelity target.
- The generated factory is a starting point for procedural construction, not a finished asset pipeline replacement.
- AI vision review is expected for acceptance; the scripts package evidence but do not magically judge visual quality by themselves.

## Development Notes

After changing the plugin, update the cachebuster and reinstall. If you have Codex's `plugin-creator` skill installed, use its `update_plugin_cachebuster.py` helper:

```bash
python3 /path/to/plugin-creator/scripts/update_plugin_cachebuster.py ~/plugins/threejs-object-sculptor
codex plugin add threejs-object-sculptor@local
```

Then open a new Codex thread to pick up the updated skill and scripts.

## Support This Project

If Three.js Object Sculptor helps you, you can support its continued development:

<a href="https://ko-fi.com/harrynguyen112">
  <img height="36" src="https://storage.ko-fi.com/cdn/kofi6.png?v=6" alt="Buy Me a Coffee on Ko-fi">
</a>

## License

MIT
