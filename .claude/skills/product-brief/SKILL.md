---
name: product-brief
description: Turn Human Product Intent into a Product Brief, and — after explicit Human approval — into a versioned Requirement. Use when starting new product work or when the user describes a goal/problem/feature in plain language. First step of WORKFLOW.md's product lifecycle; nothing downstream (planning, building) may start before this is approved.
---

# Product Brief & Requirement

Implements `WORKFLOW.md` §5 (Product Lifecycle) up through Requirement approval. This is a gate, not a formality — do not let planning or implementation start before the Human has explicitly approved the artifact this skill produces.

## When invoked

**Args present (a plain-language goal/problem):** treat it as Human Product Intent.
**No args:** ask the user for it directly, using the schema below as your checklist — don't demand every field, but flag which ones are missing.

```yaml
product_intent:
  goal:
  target_users: []
  problem:
  desired_outcome:
  platforms: []
  constraints: []
  success_criteria: []
```

## Steps

1. Check `docs/project-policy.md` exists. If this project hasn't run `INITIATION.md` yet, say so and stop — product work is locked until initialization completes (`CLAUDE.md` §10, `INITIATION.md` §5).
2. Delegate to the `product` agent with the intent to draft the Product Brief.
3. Present the brief to the Human. Stop. Do not proceed to a Requirement until they approve it by name/version — a request to "keep going" without a clear approval statement is not approval.
4. On approval, delegate to the `product` agent to draft the versioned Requirement linked to the approved brief.
5. Present the Requirement. Stop again — Requirement approval is a separate, explicit gate from brief approval (`WORKFLOW.md` §5).
6. Only once the Requirement is approved, tell the user the next step is `/plan` to decompose it into task packets.

## Don't

- Don't draft acceptance criteria the Human hasn't confirmed the intent behind.
- Don't create a new Requirement version for a typo fix — edit the draft before first approval; version bumps are for post-approval scope/criteria changes.
- Don't skip straight to planning "to save a round trip."
