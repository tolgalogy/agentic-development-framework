# Agentic Development Framework

«A governance-first SDLC framework for AI-assisted software development.»

A lean, risk-based operating model for using coding agents safely — with explicit human authority, bounded agent permissions, independent verification, and risk-based security controls.

This is not another coding agent. It is a governance and execution framework designed to make agent-assisted software development controlled, reviewable, traceable, and proportionate to risk.

It is a kit to copy into a real project repository, not a product or runtime platform itself.

Bootstrap document versions: `CLAUDE.md` 3.0.0, `WORKFLOW.md` 3.0.0, `INITIATION.md` 3.0.0.

---

## Why this exists

Coding agents can generate and modify software at unprecedented speed. Speed alone, however, does not provide:

- clear authority boundaries
- controlled product requirements
- independent verification
- security validation
- bounded retries and repair
- auditable decisions
- controlled release authority

The framework addresses those gaps by defining the operating model before implementation begins.

The goal is not to make agents autonomous at all costs.

The goal is to make agent-assisted development:

```
intentional → bounded → validated → reviewable → auditable → releasable
```

Human authority remains explicit throughout the lifecycle.

---

## What this framework provides

The framework defines a lightweight operating system for agent-assisted software delivery.

It provides:

- explicit human authority and approval boundaries
- risk-based validation and security review
- defined Product, Technical Lead, Builder, and Verifier roles
- bounded agent permissions and task scope
- controlled requirement versioning
- independent verification where risk requires it
- bounded repair attempts
- Git as the source of truth
- auditable decisions and release checks
- reusable Claude Code agents and skills
- project initialization and policy templates
- CI templates for web, mobile, and desktop projects

The framework intentionally does not attempt to provide a runtime orchestration platform, persistence layer, or automatic permission engine.

Until such runtime capabilities are explicitly added and validated, the protocol is followed manually and evidence is recorded in project artifacts and Git.

---

## Core operating model

The framework follows a simple authority chain:

```
Human
  ↓
Product
  ↓
Technical Lead
  ↓
Builder
  ↓
Verifier
```

Release is not a role. It is a gate owned by the Technical Lead and the Human, subject to project policy.

This is not an unconditional autonomous pipeline.

Human authority remains required for:

- product intent
- scope
- priorities
- requirement approval
- module breakdown confirmation
- organizational changes
- release decisions reserved by project policy

Agents may analyze, advise, plan, implement, validate, review, and report — but they must not silently replace Human decisions.

---

## Lifecycle

The default product lifecycle is:

```
Human Product Intent
        ↓
Product Brief
        ↓
Human Approval
        ↓
Requirement Version
        ↓
Human Approval
        ↓
Approved Slice
        ↓
Technical Plan
        ↓
Implementation
        ↓
Targeted Validation
        ↓
Risk-Based Review
        ↓
Merge
        ↓
Release Validation
```

Approved requirements are treated as immutable.

If product scope or acceptance criteria change, a new requirement version and appropriate approval are required.

---

## Risk-based validation

Validation depth increases with risk.

**Low risk**

```
Self-test
   +
Targeted review
```

**Medium risk**

```
Self-test
   +
Functional validation
   +
Quality review
```

**High risk**

```
Self-test
   +
Functional validation
   +
Quality review
   +
Security review
```

The effective policy is the strongest applicable rule from project policy, requirement, and task. The implementation agent is never the sole validator for medium- or high-risk work.

Security considerations can include:

- authentication
- authorization and access control
- input validation
- injection risks
- XSS / CSRF
- secrets and sensitive data
- cryptography
- dependency security
- file handling
- rate limiting
- information leakage
- supply-chain risk

The objective is proportional validation rather than applying maximum ceremony to every task.

---

## Role model

| Role | Primary responsibility | Explicit boundary |
| --- | --- | --- |
| Human | Intent, scope, priorities, approvals, organizational authority, release decisions | Final authority |
| Product | What and why; requirements, acceptance criteria, product brief | Cannot implement, cannot decide an unanswered detail, cannot settle the module breakdown alone |
| Technical Lead | How; architecture, decomposition, dependencies, coordination, merge authorization | Does not implement product code |
| Builder | Authorized implementation | Cannot approve or merge its own medium+ risk work |
| Verifier | Independent validation, review, and evidence | Does not implement or fix product code |
| Security Reviewer | Security review for high-risk and security-flagged work | Activated on risk; does not implement fixes |
| Specialists | Activated when task risk or domain requires additional capability | Not created as permanent roles |

One agent may perform multiple roles for low-risk work, but it must not approve its own work when independent review is required.

---

## Authority and permissions

The effective permission of an agent is determined by the intersection of:

```
Role
  ∩
Workflow
  ∩
Task Scope
  ∩
Current State
  ∩
Skill Permission
  ∩
Resource Scope
```

If authority is missing, the default is deny.

Skills cannot grant authority.

Agents cannot self-grant authority.

Tools, repositories, secrets, and other resources remain bounded to the current task and project policy.

---

## Product requirements are human-controlled

The Product workflow begins with Human Product Intent.

The Product role then converts that intent into a structured Product Brief containing:

- goal
- users
- problem
- scope
- out-of-scope items
- success criteria
- risks
- open questions

The Product interview is interactive and runs in the working session itself — it is never delegated wholesale to the `product` subagent, which cannot hold a back-and-forth with the Human. The subagent only drafts files from answers already given.

Recommendations are presented to the Human but are never silently applied.

After the brief is approved, Product proposes a module breakdown. That proposal is confirmed or redrawn by the Human, never decided by the agent. Each confirmed module receives its own specification under `docs/requirements/<slug>-v<n>/modules/`, and implementation begins only after the full requirement set is approved.

---

## Context and token discipline

Context management is treated as an engineering constraint, not an optimization.

Agents load the minimum context needed for the current decision:

```
Always:      task packet + project policy
Usually:     project profile + architecture summary
When needed: relevant source files, tests, ADRs, security or platform policy
```

Unchanged bootstrap documents and unrelated modules are not reread.

---

## Repository structure

The repository is intentionally small:

```
.
├── .claude/
│   ├── agents/
│   └── skills/
├── templates/
├── .gitignore
├── CLAUDE.md
├── CONTRIBUTING.md
├── INITIATION.md
├── LICENSE
├── README.md
├── SKILLS.md
└── WORKFLOW.md
```

### Constitution

- `CLAUDE.md` — principles, authority boundaries, safety, and governance
- `INITIATION.md` — project initialization and bootstrap procedure
- `WORKFLOW.md` — execution lifecycle and risk-based validation

These documents form the framework's operating constitution.

Changes to the constitution require Human approval, a recorded decision, and a version change.

### Skills

- `SKILLS.md` — catalog of in-house and vendored skills, their sources, versions, and provenance
- `.claude/skills/` — in-house workflow skills

### Agents

`.claude/agents/` contains the core roles:

- Product
- Technical Lead
- Builder
- Verifier
- Security Reviewer specialist

### Templates

`templates/` contains reusable project assets:

```
templates/
├── docs/
├── ci/
├── agents/
└── skills/
    └── vendor/
```

The documentation templates cover project profile, project policy, architecture summary, decisions, and module specifications.

CI templates cover web, mobile, and desktop workflows with staging/production environment gates.

---

## Skills and provenance

The framework separates its own workflow skills from externally sourced supporting skills.

### In-house workflow skills

The framework provides five core workflow skills:

```
/product-brief
/plan
/build
/verify
/release
```

### Vendored supporting skills

There are five vendored supporting skills.

All five are sourced from [`github.com/Jeffallan/claude-skills`](https://github.com/Jeffallan/claude-skills), MIT-licensed, pinned to:

```
tag:    v0.4.16
commit: d5d2dd8ae2ec3842a46e93894655be888fe29446
```

| Skill | Capability | Used by |
| --- | --- | --- |
| `feature-forge` | Requirements interview and EARS specification | product |
| `spec-miner` | Reverse-engineering specs from undocumented or legacy code | tech-lead |
| `code-reviewer` | Broad-scope PR-style delivery review | verifier |
| `security-reviewer` | SAST, dependency and secret scanning, security review workflow | security-reviewer |
| `test-master` | Test generation, mocking, coverage, test-suite auditing | verifier + builder |

The upstream MIT license is retained at `templates/skills/vendor/LICENSE-Jeffallan-claude-skills`, and copyright remains with the original author.

Two further skills from [`github.com/anthropics/skills`](https://github.com/anthropics/skills) — `webapp-testing` and `claude-api` — are recorded as conditionally relevant and fetched into a project only when that project actually targets web or integrates the Claude API. They are not vendored here. Check their upstream license terms before use rather than assuming they match this repository's.

### Sourcing rules

The project maintains a closed four-repository trusted-source allowlist, checked in a fixed priority order. The allowlist is reviewed rather than bulk-imported, and no other repository, marketplace, or website may be introduced as a skill source.

Some capabilities intentionally remain unfilled where no suitable trusted skill exists — currently independent plan review and release publication. In those cases, the workflow falls back to Technical Lead or Human judgment and the relevant checklist.

Vendored skills are not automatically granted additional authority. They are invoked by name when required by the workflow.

Anything added later must record its source repository, license, pinned tag and commit, upstream path, and consuming agent in `SKILLS.md`. Never pin to `latest` or `main`.

---

## Bootstrap

The framework is designed to be copied into a real project repository.

Copy:

```
CLAUDE.md
INITIATION.md
WORKFLOW.md
SKILLS.md
.claude/
templates/
```

Then:

1. Run the initialization process.
2. Discover the target project's language, framework, package manager, platform, tests, build, deployment, configuration, and security boundaries.
3. Create or verify the project operating documents.
4. Define project-specific policy.
5. Copy the appropriate CI template into `.github/workflows/` and configure the `staging` and `production` environments.
6. Wait for Human Product Intent.
7. Begin the normal development loop.

Successful initialization emits:

```
ORGANIZATION_INITIALIZED
Product development: LOCKED
Next action: WAIT_FOR_HUMAN_PRODUCT_INTENT
```

If the bootstrap documents conflict on authority, approval, security, or validation, initialization stops with `ORGANIZATION_INITIALIZATION_BLOCKED` rather than silently choosing the weaker rule. Re-running initialization is idempotent: valid artifacts are reused, product code is preserved, and conflicts are reported rather than overwritten.

The default development loop is:

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

Initialization deliberately creates only what the project actually needs.

The template does not create runtime infrastructure, event stores, agent contracts, or specialist directories merely because those concepts exist in the framework.

---

## Bounded repair and execution

Agent execution is deliberately bounded.

The default maximum repair attempts per task is:

```
3
```

Tasks should define, where appropriate:

- exploration budget
- files in scope
- validation commands
- maximum repair attempts
- review depth

This prevents uncontrolled retry loops, context expansion, and silent scope growth.

Repeated failure escalates to the Technical Lead, Product authority, or Human.

---

## Git is the source of truth

The framework treats Git and recorded project artifacts as authoritative evidence.

Authorization to merge (`MERGE_APPROVED`) is distinct from confirmation that a merge occurred (`MERGED`).

An agent must not claim that:

- a merge occurred when it did not
- a release occurred when it did not
- validation passed when it did not
- approval was granted when it was not
- security review occurred when it did not

The Definition of Done includes implementation, tests, required independent checks, review, and actual Git merge according to project policy.

Release requires the applicable release checks and approvals.

---

## What this project is not

This repository is not:

- a replacement for Claude Code or another coding agent
- a hosted autonomous software development platform
- a runtime multi-agent orchestration engine
- an automatic authorization system
- a persistence or event-store implementation
- a promise that agents can safely operate without Human governance

It is a governance-first SDLC framework and project bootstrap kit for agent-assisted software development.

---

## Design philosophy

The framework deliberately favors:

- explicit intent over inferred intent
- bounded authority over unrestricted autonomy
- proportional controls over blanket process
- independent verification over self-certification
- traceability over invisible automation
- reversible changes where practical
- secure defaults
- simple, maintainable mechanisms
- evidence over claims

The objective is not maximum agent autonomy.

The objective is useful autonomy within explicit boundaries.

---

## Current scope

The current framework provides:

- 4 core logical roles
- 1 default specialist security role
- 5 in-house workflow skills
- 5 vendored supporting skills
- 5 documentation templates
- 3 CI workflow templates
- 1 closed four-repository trusted-source allowlist

The framework intentionally leaves some specialist capabilities unfilled where no suitable trusted implementation exists.

This is a design choice, not an accidental gap.

---

## Project status

This project is actively evolving.

The framework is intended to remain:

- small enough to understand
- explicit enough to audit
- modular enough to adapt
- strict enough to protect Human authority
- lightweight enough to use in real projects

The priority is not to accumulate agents or skills.

The priority is to improve the quality, safety, traceability, and reliability of agent-assisted software development.

---

## Contributing

Issues and discussion are welcome — see [`CONTRIBUTING.md`](CONTRIBUTING.md).

For significant changes to the framework's architecture or governance model, please open an issue before implementation. Changes to authority, approval, validation, security, merge, retry, or workflow rules follow the controlled change process defined by `CLAUDE.md` and `WORKFLOW.md`.

---

## License

Licensed under the MIT License. See [`LICENSE`](LICENSE) for the full text.

In practice this means you may copy this kit into a private, commercial, or open-source repository, modify any part of it, and redistribute it — provided the copyright notice and license text are retained in copies or substantial portions of the work. The kit is provided "as is", without warranty of any kind.

### Third-party components

The five vendored skills under `templates/skills/vendor/` are copied verbatim from [`Jeffallan/claude-skills`](https://github.com/Jeffallan/claude-skills) under the MIT License, pinned to `v0.4.16` / `d5d2dd8ae2ec3842a46e93894655be888fe29446`. Their upstream license is included in that directory and copyright remains with the original author.

An earlier version of this kit vendored six skills from [`levnikolaevich/claude-code-skills`](https://github.com/levnikolaevich/claude-code-skills), removed once the trusted-source allowlist was confirmed as closed. That set shaped the current skill layout and is credited here; the full removed-to-replacement mapping is in `templates/skills/vendor/README.md`.

### Attribution

The license does not require public credit beyond retaining the notice, but if this kit was useful, a line like the following is appreciated:

```
Built on the Agentic Development Framework (ADF)
https://github.com/tolgalogy/agentic-development-framework
```
