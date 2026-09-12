---
name: build
description: Implement a ready task packet within its declared scope, self-test, and hand off for review. Use once a task is in the 'ready' state from /plan. Builder role — does not self-approve medium/high-risk work.
---

# Build

Implements the Builder's part of `WORKFLOW.md` §7: `ready -> in_progress -> review`.

## Preconditions

- The task packet exists, is `ready` (clear scope, owner, acceptance criteria, dependencies satisfied).

## Steps

1. Load only the task packet and `docs/project-policy.md` by default — pull in architecture context or other source files only if the task actually touches them (`WORKFLOW.md` §4 token budget).
2. Delegate to the `builder` agent to implement strictly within `scope`, using the repository's existing patterns and commands.
3. The builder self-tests with real command output as evidence and records it on the task.
4. If self-test fails, the builder repairs and retries — bounded to 3 attempts (`CLAUDE.md` §8). On a 3rd failure, stop and escalate rather than retrying further.
5. Task moves to `review`. Tell the user the next step is `/verify` — building does not include approval or merge.

## Don't

- Don't let the builder touch files outside the task's declared `scope`; if it needs to, that's a signal to go back to `/plan`.
- Don't mark a task done because tests pass — "done" requires Verifier sign-off and an actual Git merge (`CLAUDE.md` §12).
