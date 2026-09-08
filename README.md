# Innollia Skills

Personal Codex plugin marketplace.

## Plugins

### Context Capsule

`context-capsule` turns the active task into a very small handoff capsule for a fresh chat.

Design goals:

- target 600-900 Unicode characters
- hard ceiling 1200 characters
- active task only by default
- no chronological conversation summary
- preserve decisions, blockers, next actions, and opaque identifiers
- no preface or trailing commentary

Example prompts:

- `캡슐 만들어줘`
- `인계 캡슐`
- `Create a context capsule for this chat.`

### Thought Scrapbook

`thought-scrapbook` captures raw ideas without polishing them away.

Design goals:

- raw wording is immutable
- refinement is always stored separately
- promoted writing stays distinct from raw scraps
- idea links are stored without rewriting source entries
- explicit capture only, never auto-save ordinary conversation

Default storage root: `~/thought-scrapbook/`

Example prompts:

- `이거 글감. 원문 그대로 스크랩해줘.`
- `이 생각 저장해줘.`
- `이 스크랩 정제해. 원문은 건드리지 마.`

### Study Capture

`study-capture` preserves school materials and routes them into a compact subject-based archive.

Design goals:

- original source files remain canonical
- classify by subject and material type
- extract only study-critical metadata
- keep one lightweight retrieval index
- exact wording is preserved for rubrics, ranges, deadlines, and submission conditions

Default storage root: `~/study-capture/`

Example prompts:

- `이거 공부자료로 저장해줘.`
- `이 유인물 과목이랑 유형 분류해서 넣어줘.`
- `시험범위랑 수행평가 조건만 뽑아서 저장해줘.`

### Eve GPT Image

`eve-gpt-image` compiles the current Eve scene and live visual canon into the host's native ChatGPT image generator.

Design goals:

- reuse Eve `27A` / `CompiledIllustrationV1` scene semantics
- user-specified new scene outranks the active scene; active scene outranks current-state fallback
- preserve a supplied Eve reference image as the identity source
- keep Eve and user-character reference slots separate
- never substitute Higgsfield or reuse Higgsfield Element IDs as GPT bindings
- never fabricate Runtime `scene_id`, `source_turn_key`, generation IDs, or image URLs
- register a rendered image with Scene Runtime only when an exact resolvable asset reference really exists

Example prompts:

- `이브 지금 장면 GPT로 그려줘.`
- `이브 이미지 만들어줘.`
- `Render the current Eve scene with ChatGPT image generation.`

## Add this marketplace in Codex

Use the plugin marketplace source dialog with:

- Source: `https://github.com/innollia/skill`
- Git ref: `main`
- Sparse path: leave blank

The repository marketplace manifest is at `.agents/plugins/marketplace.json`.

After adding the marketplace, install the plugin you want and start a new Codex thread before testing newly installed or updated skills.

## Repository layout

```text
.agents/plugins/marketplace.json
plugins/
  context-capsule/
    .codex-plugin/plugin.json
    skills/context-capsule/SKILL.md
  thought-scrapbook/
    .codex-plugin/plugin.json
    skills/thought-scrapbook/SKILL.md
  study-capture/
    .codex-plugin/plugin.json
    skills/study-capture/SKILL.md
  eve-gpt-image/
    .codex-plugin/plugin.json
    skills/eve-gpt-image/SKILL.md
```
