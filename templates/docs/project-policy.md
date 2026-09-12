# Project Policy

Minimum effective policy for this repository, per `INITIATION.md` §4 Phase 3. Do not weaken `CLAUDE.md` or `WORKFLOW.md`; this file may only make their baseline concrete for this project, never looser.

```yaml
default_risk_level: low   # low | medium | high — the floor when a task doesn't specify otherwise

checks:
  low:    [self_test, targeted_review]
  medium: [self_test, functional_validation, quality_review]
  high:   [self_test, functional_validation, quality_review, security_review]

conditional_checks:        # activate only what this project actually needs (WORKFLOW.md §8)
  performance: false
  accessibility: false
  privacy: false
  migration: false
  platform: false

platforms:                 # WORKFLOW.md §9 — leave empty lists for targets this project doesn't ship
  web: []                  # e.g. [browser_matrix, a11y, deploy_check]
  mobile: []               # e.g. [device_matrix, permissions, deep_links, signing, store_review]
  desktop: []              # e.g. [os_matrix, packaging, signing, auto_update]

retry_limit: 3             # bounded repair attempts per task, CLAUDE.md §8

definition_of_ready:
  - scope, owner, acceptance criteria, and dependencies are clear
definition_of_done:
  - implementation, self-test, required independent checks, review, and actual Git merge complete

release:
  requires_human_approval: true
  rollback_path_required: true
```

## High-risk triggers for this project

List anything specific to this repo beyond `WORKFLOW.md` §8's defaults (auth, payments, PII, public APIs, migrations, infra/release, reliability-impacting changes):

-

_Any change to this file's risk/approval/validation/security rules requires Human approval and a recorded decision (`CLAUDE.md` §11) — not a routine edit._
