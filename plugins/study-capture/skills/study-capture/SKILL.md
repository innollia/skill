---
name: study-capture
description: >
  Capture school and study materials such as handouts, assignment sheets, exam ranges, classroom notices, notes, images, PDFs, and text. Use when the user asks to save, classify, organize, extract scope, route by subject, or turn a school material into a compact study record. Preserve original source files and keep indexes lightweight.
---

# Study Capture

Treat this as a school-material intake and routing system.

The source artifact is canonical. Derived notes and indexes must point back to it rather than replacing it.

## Storage root

Default root:

`~/study-capture/`

If unavailable, use `<current-workspace>/.study-capture/` and report the fallback location after a successful write.

Use this structure only as needed:

```text
sources/
subjects/
index/
```

Do not create empty folders merely for symmetry.

## Material classes

Classify into the smallest useful set:

- `handout`
- `assignment`
- `exam-scope`
- `notice`
- `note`
- `reference`
- `unknown`

Do not invent a more specific class when the evidence is weak.

## Subject classification

Infer the school subject from visible content, filename, explicit user context, or source metadata.

Examples include:

- Korean
- Math
- English
- Science
- Social Studies
- History
- Computer Science
- Other

Use the user's own subject naming when known.

If confidence is low and misclassification would matter, store under `Unknown` instead of guessing.

## Preserve the original

For an uploaded file, image, PDF, or document:

1. Keep the original bytes unchanged.
2. Copy or materialize it under `sources/<subject>/<material-class>/`.
3. Do not rewrite the source merely to make it prettier or searchable.
4. Use a collision-safe filename.

Recommended naming:

`YYYY-MM-DD_<short-original-name>.<ext>`

If no filename exists, use a concise content-derived name without adding unsupported facts.

## Create a compact derived record

For each captured source, create a sibling Markdown record under:

`subjects/<subject>/<material-class>/<source-stem>.md`

Use only fields that are actually supported by the source.

Suggested frontmatter:

```yaml
source: ../../../sources/Math/exam-scope/2026-09-08_probability.pdf
subject: Math
material_class: exam-scope
captured_at: 2026-09-08T15:20:00+09:00
```

Body sections are optional and should be omitted when empty:

```md
## 핵심

## 시험범위

## 수행/과제 조건

## 날짜

## 준비물

## 출처 메모
```

Keep these derived records compact. They are indexes for retrieval, not rewritten textbooks.

## What to extract

Prefer information that changes what the user should study or do:

1. Exam or study scope.
2. Assignment or performance-task requirements.
3. Due dates, test dates, submission windows, and schedule constraints.
4. Required materials or formats.
5. Teacher-specific wording that affects grading or submission.
6. Page/chapter/problem ranges.

Do not summarize generic instructional prose unless the user asks.

## Exact wording rule

When a rubric, exam range, due date, teacher instruction, or submission condition is consequential, preserve its exact wording in a short quote or verbatim field when practical.

Do not silently normalize a condition such as `A4 2장 이내` into a looser phrase like `짧은 보고서`.

## Lightweight index

Maintain one compact line per captured item in:

`index/materials.md`

Format:

```md
- 2026-09-08 | Math | exam-scope | 확률과 통계 9월 시험범위 | `subjects/Math/exam-scope/2026-09-08_probability.md`
```

Do not duplicate the full extracted content in the index.

If the index does not exist, create it with only a title and the first entry.

## Duplicate handling

Before writing a new capture, check for an obvious duplicate source by exact filename plus size/hash when available, or by clearly identical content.

- If exact duplicate: do not create another source copy. Reuse the existing record and tell the user.
- If revised version: preserve both and mark the newer record with `supersedes:` when the relationship is clear.
- Never delete an older version automatically.

## Retrieval

When asked things like `수학 시험범위 뭐였지` or `수행평가 유인물 찾아줘`:

1. Search the lightweight index first.
2. Open the matching derived record.
3. Read the original source only when exact wording, rubric detail, or ambiguity requires it.

This ordering is important for context economy.

## Capture discipline

Do not save ordinary conversation merely because it mentions school.

Write only when the user explicitly asks to save/capture/organize a study material or explicitly invokes this skill for intake.

After a successful capture, answer briefly with:

- subject
- material class
- saved source path
- derived record path
- one-line extracted critical item, if any

Do not reproduce the whole material unless requested.
