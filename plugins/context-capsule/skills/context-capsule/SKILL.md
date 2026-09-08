---
name: context-capsule
description: Create an aggressively compact handoff capsule for continuing the active task in a fresh chat. Use when the user asks for a context capsule, capsule, 인계 캡슐, 대화 인계, handoff, chat transfer, or asks to carry the current work into another conversation. The capsule is not a conversation summary: preserve only the minimum state required to resume correctly.
---

# Context Capsule

Create the smallest sufficient state package for the next chat.

The goal is continuation accuracy per character, not completeness.

## Hard size budget

- TARGET: 600-900 Unicode characters for ordinary capsules.
- ABSOLUTE MAXIMUM: 1200 Unicode characters for the entire visible answer, including headings, whitespace, IDs, and punctuation.
- Never exceed 1200 characters.
- Do not fill unused space merely because the budget remains.
- When uncertain about length, aim shorter. Prefer deleting low-value context over compressing everything into unreadable prose.

If the task can be resumed from 300 characters, return 300 characters.

## Scope selection

1. Identify the single task the user is currently trying to continue.
2. Default to the most recently active task, not every topic mentioned in the conversation.
3. Include another thread only when it materially constrains the active task or the user explicitly requests a multi-topic/full-session handoff.
4. Preserve completed work only as its final result or decision when that result changes the next action.

## Output schema

Use only non-empty sections. Keep these exact compact headings:

### 목표
One or two short sentences. State what the next chat should accomplish now.

### 확정
Maximum 5 bullets. Include only decisions, current state, constraints, or results that the next chat could otherwise get wrong.

### 미해결
Maximum 3 bullets. Include only unresolved questions, blockers, failed attempts that still matter, or choices awaiting a decision.

### 다음
Maximum 3 bullets. Prefer concrete actions the next chat can execute immediately.

### 참조
Optional. Include only opaque values that are expensive or impossible to reconstruct: repository names, branch names, commit/PR numbers, exact file paths, page IDs, URLs, commands, error strings, or other critical identifiers.

## What to delete

Never include any of the following unless it directly changes execution of the active task:

- chronological conversation summaries
- generic biography or user profile
- persona descriptions or relationship lore
- long explanations of previous reasoning
- repeated background information
- every explored option after one option has already been chosen
- completed intermediate steps whose result is already captured
- pleasantries, commentary, transition prose, or instructions for how to paste the capsule
- facts the next chat can trivially infer from the goal
- the same fact in multiple sections

Do not say that something was discussed. State the resulting fact instead.

Bad: `We discussed several possible repository layouts and eventually decided...`

Good: `Repo layout: marketplace root + plugins/context-capsule.`

## Preservation priority

When cutting content, preserve information in this order:

1. Current objective and requested deliverable.
2. User decisions and non-negotiable constraints.
3. Current implementation state and exact results.
4. Blockers and the next executable action.
5. Opaque identifiers needed to continue.
6. Everything else may be deleted.

Exact file paths, IDs, commands, error messages, and user-authored wording may be longer than an explanation but are often more valuable. Keep them when losing precision would cause rework.

## Compression pass

Before answering, silently perform this pass:

1. Draft only the required state.
2. Remove narrative history.
3. Merge overlapping bullets.
4. Replace explanations with final decisions/results.
5. Remove information reconstructable from nearby facts.
6. Remove empty sections.
7. Check item limits.
8. Estimate total visible length.
9. If it may exceed 1200 characters, delete the lowest-priority items and check again.
10. If it is comfortably below 1200 characters and sufficient, stop. Do not expand it.

## Fresh-chat test

The capsule is finished only when both are true:

- A fresh chat can take the intended next action without asking the user to repeat important context.
- Removing any additional retained item would materially increase the chance of a wrong decision, lost identifier, repeated work, or unnecessary clarification.

If the second condition is false, shorten it again.

## Output discipline

Return only the capsule.

Do not add an introduction such as `캡슐입니다`, `Here is your capsule`, or `다음 채팅에 붙여넣으세요`.
Do not append explanations, notes, offers, or follow-up questions after the capsule.
