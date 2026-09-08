---
name: eve-gpt-image
description: >-
  Compile Eve scene state and visual canon into native ChatGPT image generation.
  Use when the user asks to draw, render, illustrate, or visualize Eve or the
  current Eve scene with GPT/ChatGPT image generation. Preserve Eve identity,
  source precedence, and Eve Scene Runtime boundaries. Do not substitute
  Higgsfield or another external image provider.
---

# Eve GPT Image

Render Eve with the host's native ChatGPT image generator while reusing the existing Eve visual compiler semantics.

This skill is a renderer adapter, not a new source of scene canon.

## Activation

Use this skill when the user explicitly invokes this plugin or asks for an Eve image, Eve illustration, current-scene visualization, or GPT/ChatGPT-native rendering.

Distinguish two modes:

- `prepare_only`: the user asks only for an image prompt or render handoff.
- `direct_generate`: the user asks to make/draw/render/generate the image now.

Do not silently turn `prepare_only` into image generation.

## Canonical routing

When the Notion connector is available, fetch known page IDs directly. Do not begin with workspace-wide search.

Canonical entrypoints:

- Eve boot/index: `3cc4b985-2216-8146-b2a4-cd376dc6c980`
- visual/outfit operator: `3cc4b985-2216-810a-8645-eb270dfa2378`
- image prompt compiler: `3d24b985-2216-81d7-8f28-f080337f2a75`
- renderer/runtime binding boundary: `3d34b985-2216-8120-8fd4-ddf3ac52b78e`

Read only the minimum active sources that can change the visible result. Follow the active registry from Eve `00` rather than copying stale values into this skill.

Primary scene precedence is fixed:

1. a new scene explicitly specified in the current user turn
2. the active Eve scene when the user asks for `지금 장면`, `이 장면`, or equivalent visualization
3. visual canon used only to enrich that scene
4. current-state fallback only when neither a new scene nor an active scene sufficiently defines the image

Never let current real-world state overwrite a past or continuing RP scene.
Never merge an old active scene into a newly specified scene merely because it exists.

If Notion is unavailable, use only concrete scene/canon already present in the current context. Do not invent missing canon and do not claim that live Notion was read.

## Visual source selection

For Eve, resolve only what materially affects the image:

- current visible body/face/hair identity from the active `32` source
- scene-time outfit from `26A`
- individual clothing details from `26` only when needed
- visual identity/rendering rules from `27`
- image-reference provenance from `29` only when it changes which reference is authoritative

Do not mutate appearance, wardrobe, ownership, scene state, or Notion merely to make the image more attractive or easier to render.

## CompiledIllustrationV1

Use the semantics of `27A` and assemble an internal `CompiledIllustrationV1` before direct generation.

Minimum fields:

- `schema: CompiledIllustrationV1`
- `mode: prepare_only | direct_generate`
- `prompt_en`
- `aspect_ratio`
- `reference_slots.eve` when Eve appears
- `reference_slots.user` only when the user's character appears
- optional style/environment references only when they materially affect identity or scene continuity
- `scene_basis.source_type`
- `scene_basis.source_refs`
- `scene_basis.resolved_time_context`
- `scene_basis.resolved_location_context`
- short diagnostics only for unresolved conflicts or missing references

Do not expose this internal payload unless the user asks for inspection/debugging.

## Prompt compilation

`prompt_en` must stand on its own in a fresh image-generation context.

Recommended order:

1. scene type and visible characters
2. reference-image identity instruction
3. scene-specific outfit and temporary appearance changes
4. action, pose, expression, and interpersonal spacing
5. camera and framing
6. environment, props, and lighting
7. rendering/style requirements
8. short identity-preservation constraints

Keep only information that changes the visible image.
Do not translate entire Notion pages into the prompt.
Do not leak Notion IDs, database names, runtime state, tool names, checkpoints, registry metadata, or implementation details into the image scene.

When a reference image already establishes face, hair, eyes, skin, and overall identity, avoid re-describing all of it. Text should reinforce only easy-to-lose identity traits and the elements that change in this scene.

Identity outranks generic beautification. Preserve the supplied reference image's body proportions and silhouette rather than drifting toward generic anime heroine proportions.

## Reference-image rules

Treat an actually available image in the current conversation as an image reference. A Notion page ID, provider element ID, filename guess, semantic search hit, or old opaque attachment ID is not a usable image target by itself.

If Eve appears and a usable Eve reference image is present, use it as the authoritative identity reference.

If `direct_generate` requires editing or transforming a specific existing image, first verify that the target image is actually available in the current conversation. If it is missing, ask the user to attach or identify the image instead of pretending it is available.

If exact Eve identity continuity is required but no usable Eve reference is available, do not claim exact visual continuity. Generate reference-free only when the user explicitly permits that loss of identity fidelity; otherwise request the Eve reference image.

Keep Eve and the user's character in separate reference slots. Never blend their appearance rules.

## Sexual or revealing scenes

Follow the current image-safety policy and `27A`'s neutral visual normalization.

Translate subjective words such as `야하게`, `섹시하게`, or `도발적으로` into observable visual facts only when allowed: garment structure, coverage, pose geometry, hand position, gaze direction, camera position, and similar visible details.

Do not amplify sexual intensity beyond the user's request. Do not invent extra exposure, transparency, wet skin, opened zippers, body contact, flushed skin, panting, or seductive expressions merely because a scene is revealing.

Use adult-age wording once when needed for identity clarity; do not repeat it as prompt padding.

## Native GPT rendering

For `direct_generate`, call the host's native ChatGPT image-generation capability, such as `image_gen` when that tool is exposed.

Do not call Higgsfield, web image search, or another image provider as a substitute.
Do not inject Higgsfield Element IDs from `27R` into a GPT prompt.
Do not surface tool arguments, JSON payloads, or provider internals to the user.

Use the current conversation's usable reference images through the host's native image-reference mechanism.
Respect a user-specified aspect ratio when the native generator supports it; otherwise choose the closest supported framing without changing scene meaning.

If native image generation is not exposed in the current host, do not pretend generation succeeded and do not silently fall back to another provider. Return the compiled prompt/handoff and state that native image generation is unavailable in this host.

After a successful direct image generation, do not append a redundant prose description unless the user asked for one.

## Scene Runtime boundary

Eve Scene Runtime keeps its existing seven-tool public contract. This skill must not add a new illustration-specific runtime tool.

Do not reuse `27R`'s Higgsfield-specific renderer bindings as though they were GPT bindings.

Automatic asset registration is allowed only when ALL of the following are actually available:

1. a real Runtime `scene_id`
2. a stable checkpoint `source_turn_key`
3. an exact image reference exposed by the GPT renderer that the runtime file resolver can resolve
4. a genuine stable generation/occurrence identifier when the runtime contract requires one

If the native GPT image result does not expose an exact resolvable `file_ref`, skip automatic `scene_register_asset` registration. Never invent `result_url`, `generation_id`, `source_turn_key`, or `scene_id`.

If a valid exact reference is later available, use the existing `scene_register_asset` path rather than implementing storage inside this skill.

A rendered image does not become new appearance, outfit, location, or world canon merely because it was generated. Canon changes require their normal explicit confirmation/mutation path.

## Failure checks

Before direct generation, verify:

- the selected scene source follows the fixed precedence
- the prompt describes one coherent scene
- Eve identity is anchored to the real provided reference when available
- current-state fallback has not overwritten an RP scene
- no runtime/provider metadata leaked into the prompt
- requested sexual intensity was not amplified
- no unavailable reference or opaque ID is being treated as an actual image

If any of these fail, fix the compiled request before rendering.

## User-visible output

For `prepare_only`, return the copyable prompt only unless the user asked for diagnostics or the compiled payload.

For successful `direct_generate`, render the image and keep text minimal.

For a blocked direct generation, state the exact missing prerequisite in one short sentence and do not fabricate success.
