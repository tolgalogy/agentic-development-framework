# CI/CD templates

Copy only the platform file(s) this project actually targets (`docs/project-profile.md`'s `platforms`) into `.github/workflows/`, then replace the placeholder `run:` steps with real commands. Don't copy all three "for completeness" — an unused pipeline is dead weight that still needs maintaining.

## Environment model

All three templates assume two GitHub Environments configured in repo settings:

- **staging** — no required reviewers; deploys automatically from `main` once checks pass. This is where functional/QA validation for a release candidate happens.
- **production** — required reviewers configured in the environment's protection rules. This is the mechanical enforcement of `WORKFLOW.md` §10's release approval — the workflow file alone does not grant deploy access.

Add more environments (e.g. `preview` per-PR) only if the project actually needs them.

## What these templates enforce vs. what `/verify` and `/release` enforce

- The workflow files enforce **mechanical** gates: lint/typecheck/test/build/dependency-scan must pass, and a human must click approve on the `production` environment.
- The `/verify` and `security-reviewer` agent enforce **judgment** gates: independent functional/quality/security review per `docs/project-policy.md`'s risk-based checklist, which a CI script can't fully automate.

Both are required — green CI is necessary, not sufficient, for `MERGE_APPROVED` (`WORKFLOW.md` §8, §10).
