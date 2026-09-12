# Software Engineering Organization and Execution Workflow

**Workflow Version:** 3.0.0  
**Status:** Active

## 1. Purpose

This document defines the lean, risk-based workflow used after `INITIATION.md` has created the project operating kit. It supports web, mobile, desktop, backend, and infrastructure work without requiring every role or ceremony for every task.

## 2. Operating Model

The baseline organization has four logical roles:

```text
Human
  -> Product
  -> Technical Lead
  -> Builder
  -> Verifier
```

Capabilities such as UX, solution architecture, security, performance, release engineering, and platform expertise are activated when the task requires them. A role is a responsibility boundary, not necessarily a separate model invocation or permanent agent.

## 3. Authority

Human authority is required for product intent, scope, priority, approved requirements, organization changes, and release decisions designated by project policy.

Product owns what and why. Technical Lead owns how, decomposition, dependencies, and coordination. Builder owns authorized implementation. Verifier owns independent checks and review.

No role may:

- invent or silently expand product scope
- approve its own implementation where independence is required
- bypass required validation or security controls
- merge without the project policy's approval conditions
- access secrets or tools outside its task scope

Default permission is deny. Skills provide capability, not authority.

## 4. Context and Token Budget

Agents must load the minimum context needed for the current decision:

```text
Always: task packet + docs/project-policy.md
Usually: docs/project-profile.md + docs/architecture-summary.md
When needed: relevant source files, tests, ADRs, security or platform policy
```

Do not reread unchanged bootstrap documents or unrelated modules. Prefer deterministic commands and test output over prose reasoning.

Each task should define:

- exploration budget
- files in scope
- validation commands
- maximum repair attempts, default three
- required review depth

Important decisions and outcomes are recorded in `docs/decisions.md`; ordinary conversation is not a system of record.

## 5. Product Lifecycle

```text
Human Product Intent
  -> Product Brief
  -> Human Approval
  -> Requirement Version
  -> Human Approval of Requirement
  -> Approved Slice
  -> Technical Plan when needed
  -> Implementation
  -> Targeted Validation
  -> Risk-based Review
  -> Merge
  -> Release Validation
```

Small low-risk changes may use a short path. High-risk changes must use the full path and activate the required specialists.

The workflow is complete for a task when it produces a working change that satisfies its approved acceptance criteria, has the applicable executable evidence, passes the required review depth, and is confirmed merged in Git. The workflow is complete for a release when the project-specific release checklist passes and the required approval is recorded.

### Product Brief and Approval

The Product role converts Human Product Intent into one concise Product Brief:

```yaml
product_brief:
  goal:
  users: []
  problem:
  scope:
  out_of_scope: []
  success_criteria: []
  risks: []
  open_questions: []
```

Human approval must explicitly identify the approved brief or version. Approval of a brief authorizes requirement refinement; it does not authorize implementation, unrelated scope, or release. If open questions affect safety, cost, architecture, or acceptance criteria, stop and request clarification.

After an approved brief, the Product role creates a versioned Requirement with acceptance criteria and links it to the brief. Human approval of that Requirement is required before implementation. New product scope or changed acceptance criteria require a new Requirement version and new Human approval. Maintenance work may use a task directly when project policy permits it.

## 6. Requirement and Task Records

Use one concise task packet rather than duplicating the same information across many artifacts:

```yaml
task:
  id:
  goal:
  requirement:
  acceptance_criteria: []
  scope: []
  risk: low | medium | high
  owner:
  dependencies: []
  validation: []
  review: []
  result:
```

Approved product requirements are immutable. A change creates a new version. A task must link to an approved requirement unless it is explicitly classified as maintenance, documentation, or operational work by project policy.

## 7. Task Lifecycle

The normal lifecycle is:

```text
proposed -> ready -> in_progress -> review -> merged
                         |
                      blocked
```

`cancelled` and `failed` are recorded outcomes, not mandatory work phases. A failed check returns the task to `in_progress` with a reason and bounded repair count.

Required transition conditions:

- `proposed -> ready`: scope, owner, acceptance criteria, and dependencies are clear
- `ready -> in_progress`: the owner has the task packet and permitted files
- `in_progress -> review`: implementation and self-test are complete
- `review -> merged`: required checks pass and authorized merge occurs in Git
- any active state -> `blocked`: safe progress is not possible; blocker and next action are recorded

## 8. Risk-Based Validation

The effective policy is the strongest applicable rule from project policy, requirement, and task:

```text
Low risk:    self-test + targeted review
Medium risk: self-test + functional validation + quality review
High risk:   self-test + functional validation + quality review + security review
```

Performance, accessibility, migration, privacy, platform, and release checks are conditional. They become mandatory when the product, platform, requirement, or task makes them relevant.

Examples of high-risk work:

- authentication, authorization, payments, and personal data
- public APIs and shared contracts
- database migrations and irreversible operations
- deployment, release, signing, and infrastructure changes
- changes with significant reliability or availability impact

The implementation agent must not be the sole validator for medium- or high-risk work. Reviewers must have the relevant files, acceptance criteria, diff, and executable evidence, not the entire repository by default.

## 9. Web, Mobile, and Desktop Activation

Project policy selects applicable checks:

- web: browser behavior, accessibility, API and deployment checks
- mobile: supported devices, permissions, deep links, signing, and store constraints
- desktop: supported OS behavior, packaging, signing, updates, and filesystem permissions

Do not run platform checks for platforms the project does not target.

## 10. Merge and Release

The Technical Lead may authorize a merge only after the effective checks pass. `MERGE_APPROVED` means authorization; `MERGED` means Git confirms the actual merge.

Release requires the project policy's release checklist, at minimum:

- required tasks and acceptance criteria satisfied
- critical defects resolved or explicitly accepted by Human authority
- required security and platform checks passed
- rollback or recovery path known where applicable
- release approval present when required

## 11. Traceability

Important work must be traceable through:

```text
Human Intent -> Approved Requirement -> Task -> Code Change
-> Validation Evidence -> Review -> Merge -> Release
```

Record decisions, approvals, blockers, review findings, and exceptions. Do not create an event contract for routine work unless an external system consumes it or the project explicitly requires event sourcing.

## 12. Skills and Tools

Use only skills and tools needed by the current task. Project-specific skills must have a trusted source, integrity check, permission scope, and pinned version. Never use unbounded `latest` references.

Tool access must be task-scoped. Secrets must not be placed in prompts, source files, or logs unnecessarily.

This document is an execution protocol, not an implementation of an orchestrator. A runtime may automate state, permissions, events, and persistence later, but until one exists, the acting agent must record the same decisions, evidence, approvals, and Git results in the project artifacts.

## 13. Organization Changes

Changes to authority, approval, validation, security, merge rules, or this workflow require Human approval, a recorded decision, a version increment, and consistency validation. Normal product work must not modify these rules.

## 14. Definition of Done

A task is done when its implementation, applicable self-test, required independent checks, review, and actual Git merge are complete. A product is releasable only when its project-specific release gate passes.
