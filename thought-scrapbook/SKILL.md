---
name: thought-scrapbook
description: Capture rough, incomplete, strange, speculative, or fleeting thoughts with minimal distortion so they can be retrieved and recombined later. Use when the user wants to save a thought, idea fragment, observation, question, hypothesis, connection, or unfinished line of reasoning without turning it into polished notes, summaries, or conclusions.
---

# Thought Scrapbook

Capture thoughts before they become tidy enough to lose what made them interesting.

## Core Principle

Preserve the thought first.

Organize only enough to make it findable later.

A scrapbook entry is not a summary, essay, task, conclusion, or knowledge-base article.

## When to Activate

Use this skill when the user:

- says to save, capture, scrap, record, or keep a thought
- invokes "Thought Scrapbook"
- gives an unfinished idea they want preserved
- notices an odd connection, hypothesis, question, pattern, or observation
- wants to collect thoughts for later development
- explicitly does not want the idea cleaned up yet

Do not activate merely because the user says something interesting.

The user must indicate capture intent, unless another active workflow explicitly routes material into the scrapbook.

## Capture Rules

1. Preserve the user's original wording whenever practical.
2. Do not silently repair awkward phrasing, unfinished sentences, unusual terminology, speculative leaps, contradictions, or typos that may carry meaning.
3. Never inflate a small thought into a long note.
4. Never manufacture a conclusion.
5. Never turn uncertainty into certainty.
6. Never infer the user's emotional state, motivation, or personal meaning unless explicitly stated.
7. Separate user content from assistant inference.
8. Prefer one compact entry over a taxonomy.
9. Add metadata only when it improves future retrieval.
10. If persistence requires a tool or external store, actually write the entry there before saying it was saved.

If no writable destination is available, return a ready-to-save entry and clearly treat it as unsaved.

## Entry Format

Use the smallest useful representation.

Default:

```text
[SCRAP]
raw: <user's thought, preserved closely>
```

Add fields only when useful:

```text
[SCRAP]
raw: <original thought>

question: <explicit unresolved question>
link: <closely related concept or existing scrap>
tags: <1-3 retrieval terms>
```

Do not add empty fields.

## Raw vs Interpretation

`raw` belongs to the user.

Anything added by the assistant must remain visibly separate.

Allowed:

```text
raw: ai 모델간 결과차가 최대가 되는 입력을 찾아 실험하기?

link: model discrimination / adversarial experimental design
```

Not allowed:

```text
raw: The user proposes adversarial experimental design as the optimal method for distinguishing neural models.
```

The second version rewrites a tentative thought into a polished claim and destroys information.

## Chunking

One conceptual fragment should normally become one scrap.

Split only when the fragments could reasonably be retrieved or developed independently.

Do not split merely because the input contains several sentences.

Do not combine unrelated fragments merely because they arrived in one message.

When uncertain, preserve the original grouping.

## Tags

Tags are optional.

Use at most three by default.

Prefer concrete retrieval hooks over broad categories.

Better:

```text
tags: brain-model-comparison, discriminative-input
```

Worse:

```text
tags: science, research, thoughts, interesting, ai
```

Never build or reorganize a global tag hierarchy during capture.

## Links

A link records a potentially useful connection, not an asserted equivalence.

Add one only when the relationship is strong enough to help future retrieval.

Do not generate chains of speculative links.

If the connection is uncertain, mark it briefly:

```text
link?: active learning
```

## Handling Polished Writing

If the user explicitly says a passage is finished, refined, publishable, or written intentionally as prose, do not reduce it to a Scrapbook entry.

Preserve it in the appropriate writing collection or workflow.

Thought Scrapbook is primarily for pre-document thoughts.

## Later Retrieval

When searching the scrapbook:

- prioritize semantic similarity over exact wording
- preserve the distinction between original scraps and later interpretations
- return the smallest relevant set first
- do not synthesize scraps unless the user asks

## Recombination

When the user asks to connect or develop scraps:

1. retrieve relevant scraps
2. show which raw scraps are being used
3. identify connections separately
4. construct the new reasoning only after that

Never overwrite the original scraps with the synthesis.

Original fragments remain immutable source material unless the user explicitly asks to edit them.

## Compression Discipline

Capturing a short thought should usually require only a few lines.

Do not produce:

- executive summaries
- background sections
- elaborate classifications
- action plans
- key takeaways
- five-part interpretations
- invented context

A six-word thought is allowed to remain a six-word thought.

## Failure Test

Before finishing a capture, ask internally:

"If the user sees this six months later, can they distinguish what they originally thought from what the assistant added?"

If not, simplify the entry.
