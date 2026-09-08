---
name: thought-scrapbook
description: >
  Capture raw thoughts, fragments, observations, strange phrasings, and writing seeds without rewriting them. Use when the user says things like 이거 글감, 생각 저장, 스크랩, 원문 그대로 저장, 정제해, 승격해, or asks to find or connect previously captured thoughts. Preserve raw text immutably and keep all refinement in separate derived files.
---

# Thought Scrapbook

Treat this as a thought capture system, not a generic note-taking assistant.

The user's original wording is primary evidence. Do not improve it unless the user explicitly asks for a derived version.

## Storage root

Default root:

`~/thought-scrapbook/`

If the environment cannot write there, use `<current-workspace>/.thought-scrapbook/` and tell the user the fallback location after the write succeeds.

Create these folders lazily as needed:

```text
raw/
refined/
promoted/
links/
```

Do not create empty scaffolding unless a write actually needs it.

## Core invariant

Never overwrite, rewrite, clean up, summarize, normalize, translate, or delete a raw entry merely because a refined version exists.

Raw entries are append-only artifacts.

A correction to a raw entry must be stored as a new raw entry unless the user explicitly asks to repair a file-level mistake such as broken encoding or an accidental duplicate write.

## Capture raw thought

Trigger examples include:

- `이거 글감`
- `이 생각 저장`
- `스크랩해`
- `원문 그대로 남겨`
- `Thought Scrapbook에 넣어`

When capturing:

1. Preserve the user's supplied text exactly, including odd grammar, unfinished sentences, slang, punctuation, and line breaks.
2. Do not silently include assistant paraphrases in the raw body.
3. Generate an ID using local time in `YYYYMMDD-HHMMSS` form. If a collision exists, append `-2`, `-3`, and so on.
4. Save to `raw/<id>.md`.
5. Use concise metadata only.

Raw file format:

```md
---
id: 20260908-150100
captured_at: 2026-09-08T15:01:00+09:00
status: raw
---

<exact user text>
```

If the user supplied a title, add `title:` to frontmatter. Otherwise do not invent one merely for neatness.

## Refine without destroying source

When the user asks to refine, clean up, structure, expand, or rewrite a captured thought:

1. Resolve the source raw entry.
2. Read the full raw file before writing.
3. Create a separate file at `refined/<source-id>.md`.
4. Put `source_id: <raw-id>` in frontmatter.
5. Keep the raw file unchanged.
6. Make the transformation requested by the user, no more.

If a refined file already exists, do not overwrite it casually. Create a revision suffix such as `refined/<source-id>-2.md` unless the user explicitly asks to replace the prior refinement.

## Promote to deliberate writing

`promoted/` is for material the user intentionally wants to develop as writing rather than merely preserve as a thought.

When the user says things like `이건 내가 쓸 글로`, `승격`, `글 초안으로`, or clearly distinguishes deliberate writing from raw notes:

- create a new file under `promoted/`
- include `source_id` or `source_ids` when derived from scrapbook material
- do not move or delete the source raw/refined files
- preserve the user's chosen title when provided

## Link thoughts

When the user asks to connect two or more entries, write a small Markdown relation file under `links/` instead of editing every source file.

Example:

```md
---
ids:
  - 20260908-150100
  - 20260908-151430
relation: contrast
---

짧은 연결 설명
```

Use the user's relation wording when supplied. Otherwise choose a plain relation label such as `related`, `contrast`, `extends`, or `same-question`.

## Search and resurface

For requests like `예전에 이런 생각 저장한 거 찾아줘`:

1. Search raw first.
2. Search refined/promoted second if useful.
3. Prefer returning the closest matching original wording with file IDs.
4. Do not present a refined version as though it were the original thought.

## Capture discipline

Do not turn every conversation into a scrapbook entry.

Write only when the user explicitly asks to save/capture/scrapbook something or explicitly invokes this skill for capture.

When a capture succeeds, respond compactly with:

- saved ID
- storage class: raw / refined / promoted / link
- path

Do not repeat the entire saved text unless the user asks.
