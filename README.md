# Innollia Skills

Personal Codex plugin marketplace.

## Context Capsule

`context-capsule` turns the active task into a very small handoff capsule for a fresh chat.

Design goals:

- target 600-900 Unicode characters
- hard ceiling 1200 characters
- active task only by default
- no chronological conversation summary
- preserve decisions, blockers, next actions, and opaque identifiers
- no preface or trailing commentary

## Add this marketplace in Codex

Use the plugin marketplace source dialog with:

- Source: `https://github.com/innollia/skill`
- Git ref: `main`
- Sparse path: leave blank

The repository marketplace manifest is at `.agents/plugins/marketplace.json` and points to `./plugins/context-capsule`.

After adding the marketplace, install **Context Capsule** and start a new Codex thread before testing it.

Example prompts:

- `캡슐 만들어줘`
- `인계 캡슐`
- `Create a context capsule for this chat.`
- `Compress this work so I can continue in a new chat.`

## Repository layout

```text
.agents/plugins/marketplace.json
plugins/
  context-capsule/
    .codex-plugin/plugin.json
    skills/
      context-capsule/
        SKILL.md
```
