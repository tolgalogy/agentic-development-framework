---
name: plan
description: Decompose an approved Requirement into one or more task packets with risk level and required checks set, per WORKFLOW.md. Use after a Requirement is approved and before any implementation starts. Technical Lead role — no product code is written here.
---

# Plan

Implements the Technical Lead's part of `WORKFLOW.md` §5–§8: turn an approved Requirement into task packets that a Builder can implement and a Verifier can check.

## Preconditions

- The Requirement referenced must be Human-approved (`/product-brief` output). If it isn't, stop and say so — don't plan against an unapproved requirement.

## Steps

1. Delegate to the `tech-lead` agent with the approved Requirement path.
2. It will produce task packet(s) under `docs/tasks/` with `risk`, `scope`, `validation`, and `review` set per `WORKFLOW.md` §8 — small low-risk work gets the short path, high-risk work (auth/payments/PII/public APIs/migrations/infra/release/reliability-impacting) gets the full checklist including security review.
3. It records an architecture/technical decision in `docs/decisions.md` only if one was actually made — a single-file bug fix task should not produce a decision entry.
4. Review the task breakdown for scope creep beyond the approved Requirement before moving any task to `ready`.

## Output

The task packet(s) created, each one's risk level and why, and which ones are `ready` to build vs. still `blocked` on a dependency.

## Don't

- Don't create more tasks than the requirement needs — one task per coherent unit of work, not one per file.
- Don't set risk higher "to be safe" or lower "to move faster" than `WORKFLOW.md` §8's criteria actually indicate.
