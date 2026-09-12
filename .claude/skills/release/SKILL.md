---
name: release
description: Check the project-specific release checklist from WORKFLOW.md §10 before a release — required tasks/acceptance criteria satisfied, critical defects resolved or accepted, required security/platform checks passed, rollback path known, approval recorded. Use when preparing to cut a release, not per task.
---

# Release

Implements `WORKFLOW.md` §10. This is a gate over a batch of already-merged tasks, not a substitute for per-task `/verify`.

## Steps

1. Read `docs/project-policy.md`'s release rules and `docs/decisions.md`.
2. Confirm, with evidence, each item from `WORKFLOW.md` §10:
   - required tasks and acceptance criteria for this release are satisfied and actually `MERGED` in Git (not just `MERGE_APPROVED`)
   - critical defects are resolved, or explicitly accepted by Human authority (recorded, not assumed)
   - required security and platform checks passed (per project-policy for the platforms this release targets — web/mobile/desktop as applicable, `WORKFLOW.md` §9)
   - rollback or recovery path is known where applicable
   - release approval is present when project policy requires it
3. If anything is missing, report exactly what and stop — do not declare a release ready to paper over a gap.
4. If everything is satisfied, report `RELEASE_READY` and record the checklist result in `docs/decisions.md`. Actual deployment is a separate, explicitly authorized action (see `templates/ci/` for the platform pipeline) — this skill confirms readiness, it does not deploy.
5. If the release includes tagging and publishing a GitHub Release (as opposed to just a deploy), the vendored `ln-63-release-publisher` skill (see `SKILLS.md`) handles the actual tag/notes/publish mechanics — it still stops for explicit Human approval of the exact tag and notes before publishing anything, so it doesn't bypass this checklist.

## Don't

- Don't run this per task — it's a release-level gate.
- Don't treat `MERGE_APPROVED` as equivalent to `MERGED`; verify the Git history directly.
