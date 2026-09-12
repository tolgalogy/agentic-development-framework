# Organization Initialization Protocol

**Protocol Version:** 3.0.0  
**Status:** Active

## 1. Purpose

`INITIATION.md` is the first document executed when this repository is opened for agentic development. It creates the smallest usable operating system for the project, then stops and waits for Human Product Intent.

Initialization is not product development. It must be fast, repeatable, and proportional to the repository and its risks.

## 2. Authority

Read these documents in this order:

```text
INITIATION.md  -> how to start
CLAUDE.md      -> non-negotiable principles and safety
WORKFLOW.md    -> day-to-day execution rules
```

`CLAUDE.md` is authoritative for principles and safety. `WORKFLOW.md` is authoritative for execution. This document is authoritative only for initialization.

If the documents conflict on authority, approval, security, or validation, stop with:

```text
ORGANIZATION_INITIALIZATION_BLOCKED
```

Do not silently choose the weaker rule.

## 3. Initialization Output

Initialization must create or verify only the following operating kit, using existing equivalent files when available:

```text
docs/project-profile.md       # discovered facts and unknowns
docs/project-policy.md        # risk, validation, release, and platform rules
docs/architecture-summary.md  # concise technical context, when applicable
docs/decisions.md             # important decisions and approvals
```

Optional directories and artifacts are created only when the repository or approved product work needs them:

```text
contracts/     # machine-readable rules for externally consumed contracts
skills/        # pinned project-specific skills
architecture/  # detailed architecture and ADRs
modules/       # product module structure
```

Do not create a runtime, event store, agent contract set, or specialist directory structure merely because the template mentions it.

## 4. Required Sequence

### Phase 0: Read and protect sources

1. Read `INITIATION.md`, `CLAUDE.md`, and `WORKFLOW.md` completely.
2. Confirm versions and authority boundaries.
3. Treat the three bootstrap documents as protected. Do not modify them during initialization unless the Human explicitly requests it.

### Phase 1: Discover the repository

Inspect enough to identify:

- language, framework, package manager, and platform targets
- source, test, build, deployment, and configuration boundaries
- existing product, architecture, policy, CI, and agent artifacts
- available test, lint, typecheck, security, and release commands
- secrets or sensitive-data boundaries that affect the project

Unknown values remain unknown. Do not invent technology, product scope, or requirements.

### Phase 2: Build the project profile

Record discovered facts in `docs/project-profile.md`. Keep it concise. The profile is planning context, not authority.

At minimum record:

```yaml
project:
  platforms: []
  languages: []
  frameworks: []
  package_manager: unknown
  existing_product: unknown
  tests: unknown
  ci: unknown
  deployment: unknown
  risk_level: unknown
```

### Phase 3: Set proportional policy

Create or update `docs/project-policy.md` with the minimum effective policy:

- default risk level: low, medium, or high
- required checks by risk level
- platform-specific checks when applicable
- definition of ready and done
- release and rollback expectations
- retry limit, normally three repair attempts per task
- data, security, accessibility, and performance requirements when applicable

Use this baseline:

```yaml
checks:
  low: [self_test, targeted_review]
  medium: [self_test, functional_validation, quality_review]
  high: [self_test, functional_validation, quality_review, security_review]
performance: conditional
```

High-risk work includes authentication, authorization, payments, personal data, migrations, public APIs, infrastructure, releases, and irreversible operations.

### Phase 4: Activate capabilities

Use logical roles, not permanent agents:

```text
Product        -> intent, scope, acceptance criteria
Technical Lead -> architecture, planning, coordination
Builder        -> web, mobile, desktop, backend, or infrastructure code
Verifier       -> tests, quality, security, and release checks
```

Activate UX, security, performance, platform, or specialist capabilities only when the task needs them. One agent may perform multiple roles for low-risk work, but it may not approve its own work where independent review is required.

### Phase 5: Validate the operating kit

Check that:

- the project profile contains facts rather than assumptions
- policy does not weaken `CLAUDE.md` or `WORKFLOW.md`
- required commands are identified or explicitly unknown
- high-risk work has independent validation available
- retry limits are bounded
- no unrestricted tool, merge, or secret access is introduced
- existing product code and valid artifacts are preserved

If a deterministic inconsistency is found, repair the dependent artifact and rerun the check. Allow at most three repair cycles.

### Phase 6: Complete initialization

Emit:

```text
ORGANIZATION_INITIALIZED
Product development: LOCKED
Next action: WAIT_FOR_HUMAN_PRODUCT_INTENT
```

Initialization is successful when the operating kit is coherent and the repository profile is complete enough to plan safely. It does not require every optional contract, event, skill, or runtime component to exist.

The initialization acceptance criteria are:

- a Human can provide Product Intent as the next input
- a task can be represented with one concise task packet
- the project risk level and applicable checks are known or explicitly unknown
- the repository's relevant validation commands are recorded
- the workflow can trace intent to task, code change, evidence, review, merge, and release
- the system can explain what context to load without rereading the entire repository
- product implementation remains locked until Human Product Intent is received

## 5. Product Development Boundary

Before Human Product Intent:

- do not implement product functionality
- do not invent requirements, roadmap items, modules, or acceptance criteria
- do not select architecture solely from assumptions
- do not install skills without project need and version pinning

After Human Product Intent, follow `WORKFLOW.md` from product brief to approved slice.

Human Product Intent may be supplied in plain language, but it must contain enough information to start a Product Brief:

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

Unknown fields remain open questions. The Product role may clarify and structure the intent, but may not silently invent answers.

## 6. Idempotency

Running initialization again must:

- reuse valid artifacts
- preserve existing product code
- avoid duplicate roles, contracts, and documentation
- refresh only stale discovery facts
- report conflicts instead of overwriting valid human decisions

If the project is already initialized and healthy, validate the operating kit and stop. Do not rebuild it.

## 7. Failure Output

```text
ORGANIZATION_INITIALIZATION_BLOCKED
Phase: <phase>
Reason: <reason>
Required Human action: <action>
```
