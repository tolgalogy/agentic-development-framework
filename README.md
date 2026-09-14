# Agentic Development Framework

«A governance-first SDLC framework for AI-assisted software development.»

A lean, risk-based operating model for using coding agents safely — with explicit human authority, bounded agent permissions, independent verification, and risk-based security controls.

This is not another coding agent. It is a governance and execution framework designed to make agent-assisted software development controlled, reviewable, traceable, and proportionate to risk.

It is a kit to copy into a real project repository, not a product or runtime platform itself.

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

This framework defines those operating boundaries before implementation begins.

The goal is not to make agents autonomous at all costs.

The goal is to make agent-assisted development:

```
intentional → bounded → validated → reviewable → auditable → releasable
```

Human authority remains explicit throughout the lifecycle.

---

## What this framework provides

The framework provides a lightweight operating model for agent-assisted software delivery.

It defines:

- explicit Human authority and approval boundaries
- risk-based validation and security review
- Product, Technical Lead, Builder, and Verifier roles
- bounded agent permissions and task scope
- controlled requirement versioning
- independent verification where risk requires it
- bounded repair attempts
- Git as the source of truth
- auditable decisions and release checks
- reusable Claude Code agents and skills
- project initialization and policy templates
- CI templates for web, mobile, and desktop projects

The framework intentionally does not claim to provide a runtime orchestration platform, persistence layer, or automatic permission engine.

Until such capabilities are explicitly added and validated, the protocol is followed manually and evidence is recorded in project artifacts and Git.

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
  ↓
Release
```

This is not an unconditional autonomous pipeline.

Human authority remains required for:

- product intent
- scope
- priorities
- requirement approval
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

| Role | Primary responsibility | Agent | Key boundary |
| --- | --- | --- | --- |
| Human | Intent, scope, priorities, approvals, organizational authority, release decisions | — | Final authority |
| Product | What and why; requirements and acceptance criteria | `product` | Does not implement or silently decide unanswered product questions |
| Technical Lead | How; architecture, decomposition, dependencies, coordination | `tech-lead` | Does not implement product code |
| Builder | Authorized implementation | `builder` | Does not approve or merge medium/high-risk work it wrote |
| Verifier | Independent validation and review | `verifier` | Does not fix or self-review its implementation |
| Security Reviewer | Security analysis for applicable high-risk work | `security-reviewer` | Activated according to risk and project policy |

Specialist capabilities such as performance, accessibility, or platform review are conditional. They are not pre-created unless the project actually requires them.

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

Tools, secrets, repositories, and other resources remain bounded to the current task and project policy.

---

## Product requirements are Human-controlled

The Product workflow begins with Human Product Intent.

The Product role structures that intent into a Product Brief containing:

- goal
- users
- problem
- scope
- out-of-scope items
- success criteria
- risks
- open questions

`/product-brief` is an interactive interview rather than a form that the Human fills in once.

Recommendations may be presented to the Human, but they are never silently applied.

Once the brief is approved, Product proposes a candidate module breakdown. The Human must confirm or redraw that breakdown before implementation proceeds.

Each confirmed module receives its own specification, and only after the required approval does the workflow proceed to planning.

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
├── INITIATION.md
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

- `SKILLS.md` — catalog of in-house and vendored skills, sources, versions, and provenance
- `.claude/skills/` — in-house workflow skills

### Agents

`.claude/agents/` contains the core roles:

- Product
- Technical Lead
- Builder
- Verifier
- Security Reviewer specialist

### Templates

```
templates/
├── docs/
├── ci/
├── agents/
└── skills/
    └── vendor/
```

The documentation templates cover:

- project profile
- project policy
- architecture summary
- decisions
- module specifications

CI templates cover web, mobile, and desktop workflows with staging/production environment gates.

The agent template provides a pattern for adding conditional specialist reviewers only when a project actually requires them.

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

All five are sourced from:

```
github.com/Jeffallan/claude-skills
```

They are pinned to:

```
v0.4.16
commit d5d2dd8ae2ec3842a46e93894655be888fe29446
```

The vendored set includes:

- `feature-forge` — product requirements interview and EARS specification
- `spec-miner` — reverse-engineering specifications from undocumented or legacy code
- `code-reviewer` — broad-scope PR-style delivery review
- `security-reviewer` — SAST, dependency and secret scanning, plus security review workflow
- `test-master` — test generation, mocking, coverage, and test-suite auditing

Skill sourcing is controlled through a closed four-repository trusted-source allowlist, checked in priority order.

The allowlist is reviewed rather than bulk-imported.

Two capability slots are intentionally left open where no suitable trusted implementation currently exists:

- independent plan review for the Technical Lead
- release tagging and publishing

When no suitable trusted skill exists, the workflow falls back to the defined Human/Technical Lead process rather than introducing an unreviewed dependency.

Vendored skills do not automatically receive additional authority. They are invoked when required by the workflow.

See `SKILLS.md` for complete provenance, licensing, source, version, and pinning information.

---

## Bootstrapping a new project

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
5. Copy the appropriate CI template into `.github/workflows/`.
6. Wait for Human Product Intent.
7. Begin the normal development lifecycle.

Initialization deliberately creates only what the project actually needs.

The template does not create runtime infrastructure, event stores, agent contracts, or specialist directories merely because those concepts exist in the framework.

---

## Day-to-day workflow

The normal development loop is:

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

For individual tasks, Build and Verify may repeat until the task satisfies the applicable validation and review requirements.

The default repair limit is 3 attempts per task.

Tasks should define, where appropriate:

- exploration budget
- files in scope
- validation commands
- maximum repair attempts
- review depth

This prevents uncontrolled retry loops, context expansion, and silent scope growth.

---

## Git is the source of truth

The framework treats Git and recorded project artifacts as authoritative evidence.

An agent must not claim that:

- a merge occurred when it did not
- a release occurred when it did not
- validation passed when it did not
- approval was granted when it was not
- security review occurred when it did not

The Definition of Done includes implementation, tests, required independent checks, review, and actual Git merge according to project policy.

Release requires the applicable release checks and approvals.

---

## Why this stays small

The framework intentionally follows an 80/20 approach.

The current kit contains:

- 4 core roles
- 1 default specialist capability
- 5 in-house workflow skills
- 5 vendored supporting skills
- 5 documentation templates
- 3 CI templates
- 1 closed four-repository trusted-source allowlist

The goal is not to create an agent, skill, document, or process for every possible scenario.

Every permanent addition should implement a real framework requirement and remain consistent with the constitution.

Context and token discipline are part of the design: skills load the task packet and project policy by default, then pull in additional context only when the task requires it.

Risk-based validation similarly avoids applying the full security, functional, and quality stack to every low-risk task.

---

## What this project is not

This repository is not:

- another coding agent
- a replacement for Claude Code or another agent runtime
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

The objective is:

«Useful autonomy within explicit boundaries.»

---

## Changing the framework itself

Changes to:

- `CLAUDE.md`
- `INITIATION.md`
- `WORKFLOW.md`
- authority rules
- approval rules
- validation rules
- security rules
- merge rules
- retry limits
- workflow rules

require:

1. Human approval
2. A recorded decision
3. A version increment on the affected document
4. Consistency validation across the bootstrap documents

See `CONTRIBUTING.md` for contribution and review expectations.

---

## Contributing

Contributions are welcome, particularly in:

- agent and tool adapters for other agentic development environments
- governance patterns and risk models
- security controls and verification strategies
- context and token efficiency
- CI/CD templates for additional platforms
- real-world case studies from using the kit on an actual project

For contribution rules, governance changes, skill provenance requirements, and pull-request expectations, see [`CONTRIBUTING.md`](CONTRIBUTING.md).

---

## License

Licensed under the MIT License.

See [`LICENSE`](LICENSE) for the full license text.
