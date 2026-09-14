# Agentic Development Framework

«A lean, risk-based governance and execution framework for agentic software development.»

AI coding agents are increasingly capable of understanding requirements, exploring codebases, implementing changes, running tests, and participating in the software delivery lifecycle.

The challenge is no longer simply whether an AI can write code.

The harder engineering problem is:

«How do we give AI agents meaningful autonomy while preserving human authority, security, independent validation, and traceability?»

Agentic Development Framework (ADF) is a practical, lightweight answer to that problem.

It provides a reusable operating model for web, mobile, desktop, backend, and infrastructure projects, with explicit authority boundaries, risk-based validation, independent review, controlled skills, and human approval at the points where it matters.

ADF is intentionally a kit to copy into a real project repository, not a runtime product or autonomous orchestration platform.

Current bootstrap document versions: `CLAUDE.md` 3.0.0, `WORKFLOW.md` 3.0.0, `INITIATION.md` 3.0.0.

---

## Core Principles

ADF is built around a few simple principles:

- Human authority is explicit.
- Agents do not silently invent requirements or expand scope.
- Permissions are bounded by role, workflow, task scope, state, skill permission, and resource scope.
- Default permission is deny; skills provide capability, not authority.
- Implementation and independent validation are separated where risk requires it.
- Validation depth is proportional to risk.
- Security is mandatory for high-risk work.
- Retries and repair cycles are bounded.
- Important decisions and outcomes are traceable.
- The framework should remain small enough to be maintainable and token-efficient.

The underlying principle is:

«Give agents meaningful autonomy — but make the boundaries explicit.»

---

## The Operating Model

ADF uses four baseline logical roles:

```
Human
  │
  ▼
Product
  │
  ▼
Technical Lead
  │
  ▼
Builder
  │
  ▼
Verifier
```

Additional capabilities such as security, performance, accessibility, platform, UX, solution architecture, and release engineering are activated when the task actually requires them rather than being created as permanent roles.

A role represents a responsibility boundary, not necessarily a separate model invocation or permanent agent.

---

## Role Model

| Role | Owns | Explicit Boundary |
| --- | --- | --- |
| Human | Product intent, scope, priorities, approvals and designated release decisions | Final authority |
| Product | What and why, requirements and acceptance criteria | Cannot implement, cannot decide an unanswered detail, and cannot settle the module breakdown itself |
| Technical Lead | How, decomposition, dependencies, coordination, and merge authorization | Does not implement product code |
| Builder | Authorized implementation | Cannot approve/merge its own medium+ risk work |
| Verifier | Independent validation and review; escalates to security review when required | Does not implement or fix product code at all |
| Security Reviewer | Security review for applicable work | Activated when security risk requires it; does not implement fixes |

The repository provides Claude Code subagents for the four baseline roles plus a built-in `security-reviewer` specialist.

---

## Human Authority

ADF deliberately does not make the AI the ultimate decision-maker.

Human authority is required for areas including:

- Product intent
- Product scope
- Priorities
- Requirement approval
- Module breakdown confirmation
- Organization/workflow changes
- Release decisions designated by project policy

Agents may analyze, advise, plan, implement, validate, review, and report.

They must not silently turn assumptions into requirements or replace explicit human decisions. This is defined as a core authority boundary in `CLAUDE.md`.

---

## Risk-Based Autonomy

ADF does not apply the same amount of governance to every change.

The baseline validation model is:

```
Low Risk
  └── Self-test
      + targeted review

Medium Risk
  └── Self-test
      + functional validation
      + quality review

High Risk
  └── Self-test
      + functional validation
      + quality review
      + security review
```

The effective policy is the strongest applicable rule from project policy, requirement, and task.

High-risk work includes areas such as:

- Authentication
- Authorization
- Payments
- Personal data
- Public APIs
- Database migrations
- Infrastructure
- Releases
- Irreversible operations

Additional validation for performance, accessibility, privacy, platform behavior, migrations, or release concerns is activated when relevant to the project or task.

The implementation agent must never be the sole validator for medium- or high-risk work.

The goal is not maximum ceremony.

The goal is proportional control.

---

## The Development Lifecycle

After initialization, the normal development loop is:

```
Human Product Intent
        │
        ▼
/product-brief  (interview → brief → module breakdown → per-module specs)
        │
        ▼
Human Approval of the Brief
        │
        ▼
Human Confirmation of the Module Breakdown
        │
        ▼
Versioned Requirement (overview + one spec per module)
        │
        ▼
Human Requirement Approval
        │
        ▼
/plan
        │
        ▼
/build
        │
        ▼
/verify
        │
        ▼
Merge
        │
        ▼
/release
```

Requirement approval is itself the authorization to plan, so `/product-brief` hands off directly into `/plan` with no separate hand-off gate.

For individual tasks, the framework supports the shorter iterative loop:

```
/build → /verify → /build → /verify → ...
```

until the approved task is successfully validated and merged.

Tasks move through `proposed → ready → in_progress → review → merged`, with `blocked` available from any active state. A failed check returns the task to `in_progress` with a reason and a bounded repair count.

Low-risk work can take a shorter path, while high-risk work follows the full workflow and activates the required specialist capabilities.

---

## Product Requirements Are Human-Controlled

One of the most important design decisions in ADF is that the Product phase is not an autonomous requirements generator.

`/product-brief` runs as a mandatory interactive interview.

It runs in the interactive session itself and is never delegated wholesale to the `product` subagent, because a subagent cannot hold a back-and-forth with the Human. The subagent is used only to draft files from answers the Human has already given.

Every field is asked via `AskUserQuestion` with concrete options and one answer marked as recommended. A recommendation is never auto-applied — the Human still has to choose.

The interview covers:

- Goal
- Target users
- Problem
- Desired outcome
- Platforms
- Constraints
- Success criteria

and then, per module:

- Module scope and out-of-scope
- Acceptance criteria
- Dependencies, integrations, data touched, module-specific constraints

The Product role may clarify and structure intent, but it cannot silently invent missing answers.

Once the Product Brief is approved:

1. Product proposes a candidate module breakdown.
2. The Human confirms or redraws that breakdown — it is a proposal, never an agent decision.
3. A versioned Requirement is created as `docs/requirements/<slug>-v<n>/overview.md` plus one spec per module under `modules/`.
4. Acceptance criteria are defined per module.
5. The Human approves the Requirement, including the module breakdown itself.
6. Only then can implementation begin.

Approved product requirements are immutable; a change to scope or acceptance criteria creates a new version (`-v<n+1>/`), never an in-place edit.

---

## Initialization Boundary

ADF separates organization initialization from product development.

`INITIATION.md` runs first and creates or verifies the minimum operating kit required for safe development.

Its phases are:

1. Read the framework constitution and workflow, and treat the three bootstrap documents as protected.
2. Discover the target repository — stack, platforms, tests, CI, deployment and security boundaries.
3. Record discovered facts in the project profile; unknown values stay unknown.
4. Establish the project's proportional risk and validation policy.
5. Activate logical roles rather than permanent agents.
6. Validate the operating kit, with at most three repair cycles.
7. Complete initialization and lock product implementation until Human Product Intent is received.

Initialization must not invent product requirements, install unnecessary infrastructure, or begin product implementation. Re-running it is idempotent: valid artifacts are reused, product code is preserved, and conflicts are reported rather than overwritten.

Successful initialization produces:

```
ORGANIZATION_INITIALIZED
Product development: LOCKED
Next action: WAIT_FOR_HUMAN_PRODUCT_INTENT
```

If the bootstrap documents conflict on authority, approval, security, or validation, initialization stops with `ORGANIZATION_INITIALIZATION_BLOCKED` rather than silently choosing the weaker rule.

---

## Repository Structure

The repository itself is intentionally small:

```
.
├── .claude/
│   ├── agents/
│   └── skills/
│
├── templates/
│   ├── agents/
│   ├── ci/
│   ├── docs/
│   └── skills/
│       └── vendor/
│
├── .gitignore
├── CLAUDE.md
├── INITIATION.md
├── SKILLS.md
├── WORKFLOW.md
└── README.md
```

The repository is the framework kit. Some directories and artifacts described below are templates that are copied into the target project rather than being created in this repository itself.

### `CLAUDE.md`

The framework constitution.

Defines:

- Human authority
- Safety principles
- Least privilege
- Authority boundaries
- Product and architecture rules
- Validation principles
- Security requirements
- Failure and repair rules
- Git integrity
- Initialization boundaries
- Organization change controls
- Definition of Done

`CLAUDE.md` is the authoritative source for framework principles and safety.

### `INITIATION.md`

Defines how a target repository is initialized before product development begins.

It discovers the target repository and creates the minimum project operating kit when needed.

### `WORKFLOW.md`

Defines the day-to-day software engineering workflow, including:

- Role responsibilities
- Context/token discipline
- Product lifecycle
- Requirement and task records
- Task states
- Validation
- Review
- Merge
- Release
- Risk handling

### `SKILLS.md`

Provides the complete skill catalog, including:

- In-house lifecycle skills
- Vendored supporting skills
- Source repositories
- Exact versions/commits
- Skill permissions
- Trusted-source allowlist and its priority order
- Open gaps where no adoptable skill exists

### `.claude/agents/`

Contains the Claude Code implementations of the framework's baseline roles and security specialist:

```
product
tech-lead
builder
verifier
security-reviewer
```

### `.claude/skills/`

Contains the framework's in-house lifecycle skills:

```
/product-brief
/plan
/build
/verify
/release
```

Each skill corresponds to a specific lifecycle responsibility.

### `templates/docs/`

Contains templates used during initialization for project-specific operating artifacts:

```
project-profile
project-policy
architecture-summary
decisions
module-spec
```

These are copied into the target project when required.

### `templates/ci/`

Contains GitHub Actions templates for web, mobile, and desktop projects, including staging/production environment gates.

These templates are copied into the target project's `.github/workflows/` directory as appropriate.

### `templates/agents/`

Contains a pattern for adding conditional specialist reviewers such as:

- Performance
- Accessibility
- Platform

Specialists are intentionally not created unless the target project's policy requires them.

### `templates/skills/vendor/`

Contains vendored supporting skills selected from the framework's approved source allowlist and pinned to exact upstream versions, together with the upstream license and review notes.

---

## Skill Governance

Skills are treated as controlled capabilities rather than unrestricted extensions.

ADF uses two skill tiers:

1. In-house workflow skills — authored specifically for this framework.
2. Vendored supporting skills — adopted only where an appropriate, trusted, version-pinned capability exists.

The framework has a closed, Human-confirmed four-repository source allowlist for vendored skills. No arbitrary marketplace, repository, or website may be introduced as a skill source. The four repositories are checked in a fixed priority order, and the highest-priority repository with a real, safe match wins; a lower-priority repository is never used to fill a slot that a higher one already covers.

If none of the four has a safe match at a real pinned release, the answer is "no vendored skill for this slot" — not a widened search and not a force-fitted substitute.

The currently vendored supporting skills all come from:

```
Jeffallan/claude-skills
tag: v0.4.16
commit: d5d2dd8ae2ec3842a46e93894655be888fe29446
license: MIT
```

| Agent | Capability | Skill |
| --- | --- | --- |
| product | Requirements interview → EARS-format spec | `feature-forge` |
| tech-lead | Spec recovery from an undocumented/legacy codebase | `spec-miner` |
| verifier | Broad-scope PR-style delivery review | `code-reviewer` |
| verifier + builder | Test generation, mocking, coverage, test-suite audit | `test-master` |
| security-reviewer | SAST/dependency/secret scanning + pentest workflow | `security-reviewer` (skill) |

Two slots are deliberately left unfilled because nothing in the allowlist is a genuine match:

- Independent review of an implementation plan before build starts. `/plan` falls back to the Technical Lead's own judgment.
- Tagging and publishing an actual release. `/release` falls back to the Technical Lead or Human performing the tag and release notes directly.

`anthropics/skills` is the highest-trust source available but is almost entirely office-document and design tooling. Two of its skills (`webapp-testing`, `claude-api`) are recorded as conditionally relevant and fetched on demand only when a project actually targets web or integrates the Claude API, rather than being vendored here.

`SKILLS.md` records exactly which capability each vendored skill provides, where it came from, and the pinned URL.

---

## Context and Token Discipline

ADF treats context management as an engineering constraint.

Agents should load only the context required for the current decision.

The baseline is:

```
Always:
  task packet
  project policy

Usually:
  project profile
  architecture summary

When needed:
  relevant source files
  tests
  ADRs
  security policy
  platform policy
```

Agents should not repeatedly reread unchanged framework documents or unrelated parts of the repository.

Tasks also define:

- Exploration budget
- Files in scope
- Validation commands
- Maximum repair attempts
- Required review depth

This keeps the framework intentionally lean and helps prevent unnecessary context/token consumption.

---

## Security

Security is a first-class part of the framework.

Applicable work considers areas including:

- Authentication
- Authorization
- Access control
- Input validation
- Injection
- XSS
- CSRF
- Secrets
- Sensitive data
- Cryptography
- Dependencies
- File handling
- Rate limiting
- Logging
- Information leakage
- Supply-chain risk

Security controls must not be weakened for convenience.

High-risk work requires security review, and project-specific policy can require security review for additional tasks. The `verifier` escalates to the `security-reviewer` agent when the task's risk or policy calls for it.

---

## Failure and Repair

ADF does not allow agents to retry indefinitely.

Failures should be:

1. Visible
2. Classified
3. Supported by evidence
4. Repaired only when the cause is understood and the acting role has authority
5. Limited by a bounded retry/repair count

The default repair limit is three attempts per task.

Repeated failure escalates to the appropriate Technical Lead, Product authority, or Human.

---

## Git and Integrity

Git is treated as the source of truth for implementation history.

ADF explicitly distinguishes between:

- Authorization to merge (`MERGE_APPROVED`, given by the Technical Lead once the effective checks pass)
- The actual Git merge (`MERGED`, confirmed by Git)
- Validation evidence
- Release approval

An agent must not claim that a merge, release, validation result, or approval occurred when it did not actually occur.

---

## Bootstrapping a New Project

ADF is designed to be copied into a real project repository.

### 1. Copy the framework

Copy:

```
CLAUDE.md
INITIATION.md
WORKFLOW.md
SKILLS.md
.claude/
templates/
```

into the target repository.

### 2. Initialize the project

Run the protocol defined in `INITIATION.md`.

The framework will discover the target repository and establish the project-specific operating kit from the `templates/docs/` copies.

### 3. Configure CI

Select the templates matching the project's actual target platforms from `templates/ci/` and copy them into the target project's `.github/workflows/`.

Configure the required `staging` and `production` GitHub Environments according to `templates/ci/README.md`.

### 4. Begin product development

Once initialization is complete and Human Product Intent is provided, use:

```
/product-brief
    ↓
/plan
    ↓
/build
    ↓
/verify
    ↓
/release
```

This sequence is the intended day-to-day operating loop of the framework.

---

## Why ADF Stays Small

ADF deliberately resists turning every possible engineering concern into another agent, skill, document, or ceremony.

The current kit contains:

- 4 baseline roles
- 1 default security specialist
- 5 in-house lifecycle skills
- 5 vendored supporting skills
- 5 document templates
- 3 CI template families

Additional specialist capabilities are activated only when justified by project or task requirements.

The framework follows an 80/20 principle:

«If an addition cannot be justified by a specific workflow, safety, validation, or engineering requirement, it probably does not belong in the framework.»

Every gate maps to a specific section of `WORKFLOW.md` or `CLAUDE.md`. If a proposed addition cannot cite the section it implements, it is probably scope creep rather than framework.

---

## What ADF Is

ADF is:

- A governance model for agentic software development
- A reusable SDLC operating kit
- A Claude Code implementation of that operating model
- A risk-based validation framework
- A role and authority model
- A controlled skill model
- A practical bootstrap framework for real software repositories

---

## What ADF Is Not

ADF is not:

- A runtime agent orchestration platform
- An event store
- An autonomous permission-management system
- A replacement for Git
- A replacement for CI/CD
- A replacement for human engineering judgment
- A guarantee that AI-generated code is correct
- A claim that every part of the workflow is automatically enforced by software

The current framework is a protocol and reusable project kit. Until dedicated runtime infrastructure is deliberately added and validated, the workflow is followed by agents and humans through the defined documents, skills, project artifacts, and Git history.

---

## Current Implementation

The current implementation targets Claude Code.

The governance model itself is intentionally broader than a single model provider or coding environment. The repository's current agent and skill implementations use Claude Code subagents and skills, while the underlying principles are designed to be portable to other agentic development environments.

---

## Changing the Framework

ADF treats changes to its own governance rules as controlled changes.

Modifying:

```
CLAUDE.md
INITIATION.md
WORKFLOW.md
```

or changing authority, approval, validation, security, merge, retry, or workflow rules requires:

- Human approval
- A recorded decision
- A version increment
- Consistency validation

This prevents the framework from silently changing the rules that govern the agents operating under it. The same applies in any project copied from this kit.

---

## Project Status

ADF is an early-stage, MIT-licensed open-source project focused on establishing and refining a lean governance model and practical implementation patterns for agentic software development.

It is being developed around:

- Real development workflows
- Risk-based controls
- Practical agent boundaries
- Security
- Independent validation
- Context efficiency
- Reproducibility

Feedback, experimentation, and issues are welcome.

---

## Roadmap

The following are candidate directions, not implemented functionality:

- Additional agent/tool adapters for other coding agents and model providers
- More granular risk policies
- Automated policy enforcement
- Expanded audit and traceability tooling
- Additional security controls
- More project-platform templates
- Production-oriented reference implementations

Nothing on this list should be assumed to exist unless it is present in the repository.

---

## Contributing

Issues and discussion are welcome, particularly around governance patterns, risk models, security controls, verification strategies, context/token efficiency, CI/CD templates, and real-world case studies.

For significant changes to the framework's architecture or governance model, please open an issue before implementation.

Changes to the framework's authority and governance rules require the controlled change process defined by `CLAUDE.md` and `WORKFLOW.md`.

---

## License

ADF is released under the MIT License — see [`LICENSE`](LICENSE).

### What that means in practice

You may:

- Copy this kit into a private, commercial, or open-source repository
- Modify any part of it, including the bootstrap documents
- Redistribute it, with or without changes
- Use it inside a closed-source product

You must:

- Keep the copyright notice and the MIT license text in copies or substantial portions of the work

You should be aware that:

- The kit is provided "as is", without warranty of any kind
- Nothing here guarantees that AI-generated code produced under this framework is correct or secure

If you fork this repository to build your own variant, keeping the original `LICENSE` file satisfies the requirement. Adding your own copyright line above it for your changes is common practice and welcome.

### Attribution

The license does not require public credit beyond retaining the notice, but if the kit was useful, a line like this is appreciated:

```
Built on the Agentic Development Framework (ADF)
https://github.com/tolgalogy/agentic-development-framework
```

---

## Credits and Third-Party Sources

ADF does not pretend to have invented everything it ships. Every skill that came from somewhere else is recorded here, with its license, its exact pin, and what it is used for. `SKILLS.md` and `templates/skills/vendor/README.md` carry the full provenance and review notes.

### Vendored into this repository

Five supporting skills are copied verbatim into `templates/skills/vendor/`, unmodified so they can be diffed against upstream.

| Skill | Used by | Source | License |
| --- | --- | --- | --- |
| `feature-forge` | product | [Jeffallan/claude-skills](https://github.com/Jeffallan/claude-skills) | MIT |
| `spec-miner` | tech-lead | [Jeffallan/claude-skills](https://github.com/Jeffallan/claude-skills) | MIT |
| `code-reviewer` | verifier | [Jeffallan/claude-skills](https://github.com/Jeffallan/claude-skills) | MIT |
| `test-master` | verifier + builder | [Jeffallan/claude-skills](https://github.com/Jeffallan/claude-skills) | MIT |
| `security-reviewer` (skill) | security-reviewer | [Jeffallan/claude-skills](https://github.com/Jeffallan/claude-skills) | MIT |

All five are pinned to tag `v0.4.16`, commit `d5d2dd8ae2ec3842a46e93894655be888fe29446`. The upstream MIT license is included as `templates/skills/vendor/LICENSE-Jeffallan-claude-skills` and copyright remains with its original author. Thanks to [@Jeffallan](https://github.com/Jeffallan) for publishing them under a permissive license.

### Referenced but not vendored

Fetched into a project only when that project actually needs them, rather than carried in every copy of this kit:

| Skill | Condition | Source | License |
| --- | --- | --- | --- |
| `webapp-testing` | Project targets web | [anthropics/skills](https://github.com/anthropics/skills) | See upstream |
| `claude-api` | Project integrates the Claude API | [anthropics/skills](https://github.com/anthropics/skills) | See upstream |

Pinned at commit `34040c9c568585f6929bedeaad110ad08f079624`. If you fetch either into your project, record the pin and the upstream license in your own `SKILLS.md` — check the upstream terms rather than assuming they match this repository's.

### Evaluated and not adopted

Recorded so the sourcing decisions are auditable rather than invisible:

- [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) — no LICENSE file, which blocks redistribution regardless of content; also stack-specific. Nothing adopted.
- [stillquietlyloud/claude_skills](https://github.com/stillquietlyloud/claude_skills) — MIT and tagged, but archived and unmaintained; the one close candidate lacks usable frontmatter. Nothing adopted.

### Previously used

An earlier version of this kit vendored six skills from [levnikolaevich/claude-code-skills](https://github.com/levnikolaevich/claude-code-skills), adopted before the allowlist was confirmed as closed. All six were removed; four were replaced from `Jeffallan/claude-skills` and two are recorded as open gaps rather than force-filled. The full removed-to-replacement mapping is in `templates/skills/vendor/README.md`. Credit is noted here because that set shaped the current skill layout.

### If you add a skill

Source it only from the confirmed allowlist in `SKILLS.md`, in its stated priority order, and pin it to an exact tag and commit — never `latest` or `main`. Then record, in both `SKILLS.md` and your vendor directory's README:

- The source repository and its license
- The pinned tag and commit SHA
- The upstream path and URL at that commit
- Which agent uses it and when
- That the file was read in full before adoption

Copying a skill without recording where it came from is a supply-chain problem, not a shortcut.

---

## Philosophy

ADF is based on a simple principle:

«Autonomous software development should be governed, not unrestricted.»

As AI agents become capable of performing more of the software development lifecycle, engineering teams need mechanisms for maintaining:

```
human authority → controlled autonomy → independent validation → security → traceability
```

ADF explores a practical way to provide those boundaries without turning the development process itself into another heavyweight system.

«Let agents do more. Make the boundaries clearer.»
