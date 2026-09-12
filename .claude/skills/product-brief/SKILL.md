---
name: product-brief
description: Run the mandatory Product interview — Product Brief, then a confirmed module breakdown, then a per-module Requirement spec — and hand off to planning once approved. Use when starting new product work or when the user describes a goal/problem/feature in plain language. First step of WORKFLOW.md's product lifecycle; nothing downstream may start before this is approved. Runs the interview directly; never delegates the interview itself to a subagent.
---

# Product Brief, Requirement & Module Specs

Implements the Product role's full `WORKFLOW.md` §5 lifecycle through Requirement approval, entirely by asking — never by assumption (`CLAUDE.md` §2, `INITIATION.md` §5).

**Run the interview yourself, in this conversation.** The `product` agent is a subagent — it cannot hold a back-and-forth with the Human. Only ever delegate to it for drafting files from answers you've already gathered; if it comes back with an `open_questions` gap, that's a signal to ask the Human, not to fill the gap yourself either.

## Ground rule

Every field below must come from an explicit Human answer, never an inference — even an obvious-seeming default must be confirmed, not silently applied. Wherever a field has more than one reasonable answer, ask with `AskUserQuestion`: 2-4 concrete options, one labeled "(Recommended)" with a one-line reason, plus the tool's own free-text "Other" for anything not listed. Batch related fields into one `AskUserQuestion` call (up to 4 questions per call) rather than one round-trip per field, but never skip a field to save a round-trip.

## Step 1 — Product Intent interview

Ask through every field, in as few `AskUserQuestion` batches as make sense:

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

If the user's opening message already answers some fields in plain language, don't re-ask those — but do ask everything it left open.

## Step 2 — Draft & approve the Product Brief

1. Delegate to the `product` agent (mode `draft-brief`) with only the confirmed answers.
2. If it returns `open_questions`, ask the Human those specific questions, then re-invoke it with the completed answers — don't proceed with a gap.
3. Present the brief. Stop for explicit Human approval by name/version. "Keep going" without a clear approval statement is not approval.

## Step 3 — Module breakdown

1. Delegate to the `product` agent (mode `propose-modules`) with the approved brief.
2. Confirm the proposal with the Human via `AskUserQuestion` — offer the agent's proposal as the "(Recommended)" option, at least one alternative grouping (e.g. coarser or finer split), and "Other" for the Human to redraw the lines themselves. Do not proceed on an assumed breakdown, even if the proposal looks obviously right.
3. For each confirmed module, interview for its specifics — in-scope features, out-of-scope, acceptance criteria, dependencies on other modules, integrations, data touched, module-specific constraints — using the same ask-with-options-and-a-recommendation rule as Step 1.

## Step 4 — Draft & approve the Requirement

1. Delegate to the `product` agent (mode `draft-requirement`) with the approved brief, confirmed module list, and confirmed per-module answers.
2. It writes `docs/requirements/<slug>-v<n>/overview.md` and one `docs/requirements/<slug>-v<n>/modules/<module-slug>.md` per module. Resolve any `open_questions` it returns with the Human before re-drafting.
3. Present the full set. Stop for explicit Human approval — approval must cover the module breakdown itself, not just each file's wording.
4. Any Human-requested change to scope or acceptance criteria after this point is a new Requirement version (`-v<n+1>/`), never an edit in place. A module that didn't change may be referenced from the new version rather than copied with drift, but say explicitly which modules changed and which didn't.

## Step 5 — Hand off

Once the Requirement is approved, proceed directly into `/plan` for the approved modules — don't wait for a separate hand-off approval. Requirement approval is the authorization `WORKFLOW.md` §5 requires before planning; a second gate here would be ceremony the workflow doesn't call for.

## Don't

- Don't ask a question you can already answer from something the Human said earlier in this conversation — re-ask only what's genuinely still open.
- Don't let the `product` agent's module proposal or drafted file stand in for Human confirmation.
- Don't create a new Requirement version for a pre-approval wording fix — that's still Step 4 drafting.
- Don't run the conditional_checks/platform interview from `docs/project-policy.md` here — that's Technical Lead's job in `/plan`, once the Requirement is approved.
