---
name: verify
description: Independently validate and review a task in the 'review' state against the risk-based checklist in docs/project-policy.md, escalating to security review when required. Use after /build. Verifier role — never the same context that implemented the task.
---

# Verify

Implements `WORKFLOW.md` §8 (Risk-Based Validation) for a task in `review`.

## Preconditions

- The task is in `review` state with self-test evidence from the builder already recorded.
- This must run as an independent invocation from whatever produced the code — if this session also wrote the implementation, launch the `verifier` agent fresh rather than reviewing in the same context.

## Steps

1. Read the task packet, its diff, and `docs/project-policy.md`'s effective checklist for this task's risk level.
2. Delegate to the `verifier` agent: re-run validation commands independently, do quality review against acceptance criteria.
3. If risk is `high`, or the diff touches auth/payments/PII/public APIs/migrations/infra/release, the verifier invokes the `security-reviewer` agent — don't skip this because the builder's self-test already passed; self-test and security review check different things.
4. If any project-policy-activated conditional check applies (performance, accessibility, migration, privacy, platform), run it too — otherwise don't.
5. For `high` risk, or whenever the `verifier` agent's own pass leaves you wanting a second, more thorough look: invoke the vendored `code-reviewer` skill for a broader-scope PR-style review, and/or `test-master` if the task's own test changes leave the suite's trustworthiness in doubt (see `SKILLS.md`). These are additive, not a replacement for the `verifier` agent's check — use them when the risk actually warrants the extra cost, not on every task.
6. Record pass/fail with evidence on the task.
   - **Fail:** task returns to `in_progress` with a specific reason. Counts toward the bounded repair limit (default 3) — if exceeded, escalate to Technical Lead instead of another `/build` → `/verify` loop.
   - **Pass:** report that the task is eligible for `MERGE_APPROVED`. Actual merge authorization is a Technical Lead/Human action — this skill does not merge anything.

## Don't

- Don't accept "I ran it and it passed" from the builder as the verification itself — independently execute the checks.
- Don't run the full conditional checklist (performance/accessibility/platform/etc.) on every task regardless of relevance — that's the over-engineering `WORKFLOW.md` §8 explicitly avoids.
