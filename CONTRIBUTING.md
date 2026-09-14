# Contributing

Thank you for contributing to the Agentic Development Framework.

This project is intentionally small and governance-focused. Contributions should improve the safety, reliability, clarity, or practical usefulness of agent-assisted software development without adding unnecessary complexity.

## Before you contribute

Please read:

- `README.md` — project purpose, operating model, and contribution scope
- `CLAUDE.md` — principles, authority boundaries, and governance
- `WORKFLOW.md` — execution lifecycle and validation rules
- `SKILLS.md` — skill catalogue, provenance, and trusted-source policy

For significant changes, open an issue before submitting a pull request so the proposed direction can be discussed.

Small fixes such as documentation corrections, broken examples, or obvious bugs can be submitted directly.

---

## What we welcome

Useful contributions include:

- improvements to the core governance model
- clearer agent authority and permission boundaries
- risk-based validation and security practices
- improvements to the Product, Technical Lead, Builder, or Verifier workflows
- improvements to context and token efficiency
- new CI templates for supported project types
- improvements to project initialization
- useful agent or skill integrations
- improvements to documentation and examples
- real-world feedback and case studies
- reproducible demonstrations of the framework being used on actual projects

A contribution should solve a demonstrated problem or provide a clear improvement to the framework.

---

## Keep the framework small

The project follows an 80/20 philosophy.

Please avoid adding:

- agents without a clear responsibility
- skills that duplicate existing capabilities
- abstractions that are not required by the workflow
- process steps that add ceremony without reducing meaningful risk
- runtime infrastructure that does not belong in this bootstrap kit
- speculative integrations
- dependencies without a clear maintenance or security justification

Before adding a permanent component, ask:

«Does this solve a real framework requirement that cannot be addressed more simply?»

If the answer is no, prefer the simpler solution.

---

## Governance changes

Changes to the framework's constitution or authority model require additional care.

This includes changes to:

- `CLAUDE.md`
- `INITIATION.md`
- `WORKFLOW.md`
- agent authority
- approval boundaries
- validation requirements
- security requirements
- merge rules
- release rules
- retry limits
- workflow state transitions

Such changes require:

1. Human approval
2. A recorded decision
3. A version increment on the affected document
4. Consistency validation across the affected bootstrap documents

Do not silently change governance behaviour as part of an unrelated feature or refactor.

---

## Human authority

The framework is intentionally Human-controlled.

Contributors should not introduce behaviour that allows an agent to silently:

- expand product scope
- change approved requirements
- bypass required validation
- bypass security review
- approve its own work where independent review is required
- grant itself additional permissions
- access secrets or resources outside task scope
- claim that an approval, merge, release, or validation occurred when it did not

Automation should make the existing governance model easier to follow, not weaken it.

---

## Skills and external sources

External skills are subject to the trusted-source policy documented in `SKILLS.md`.

The current policy uses a closed four-repository trusted-source allowlist.

Do not introduce a vendored skill from an unapproved source without first updating the trusted-source policy and obtaining the required approval.

For every adopted vendored skill, maintain provenance including:

- source repository
- license
- upstream version or tag
- exact commit where applicable
- reason for adoption
- relevant review or validation

Do not bulk-import external skill repositories.

Only the skills that provide a real capability required by the framework should be vendored.

---

## Vendored skill requirements

A proposed vendored skill should satisfy the following:

- compatible licensing
- identifiable upstream source
- stable version or tag where available
- reproducible pinning
- clear purpose
- useful integration with the existing workflow
- no unnecessary overlap with existing skills
- acceptable security and maintenance characteristics

If no suitable trusted implementation exists, leaving a capability unfilled is preferable to introducing an unreviewed dependency.

---

## Pull requests

A pull request should explain:

- what changed
- why the change is needed
- which part of the framework it affects
- how the change was validated
- whether governance or authority boundaries changed
- whether documentation requires updating
- whether any external dependency or skill was introduced

Keep pull requests focused.

Avoid combining unrelated refactoring, documentation changes, new capabilities, and governance changes into one pull request unless there is a clear reason.

---

## Validation

Before submitting a pull request:

1. Review the changed files against the relevant project policy.
2. Run the applicable validation commands.
3. Verify documentation and examples remain consistent.
4. Check that authority and approval boundaries have not been weakened.
5. Check external skill provenance if applicable.
6. Review the final diff for unintended changes.

Do not report validation as successful unless it actually ran and passed.

If a check cannot be run, state that explicitly in the pull request.

---

## Documentation changes

Documentation is part of the framework, not an afterthought.

If a change affects:

- workflow behaviour
- authority
- validation
- security
- agent responsibilities
- skill usage
- project initialization
- release behaviour

update the relevant documentation in the same change.

When changing the constitution, follow the governance-change process above.

---

## Security

Do not submit secrets, credentials, private keys, tokens, or sensitive project information.

Security-sensitive changes should explicitly describe:

- the risk being addressed
- the affected boundary
- the validation performed
- any remaining limitations

If you discover a potentially serious security vulnerability, avoid publishing sensitive exploit details in a public issue. Report it privately to the maintainer first.

---

## Scope and permissions

Contributions should respect the framework's permission model.

The effective authority of an agent is bounded by:

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

Missing authority means deny.

A skill must not be used as a mechanism for bypassing workflow or project-policy restrictions.

---

## Commit and pull request quality

Use clear commit and pull request descriptions.

Prefer commits that represent a coherent change rather than a sequence of unrelated edits.

A good pull request should allow a reviewer to understand:

```
Problem
  ↓
Proposed change
  ↓
Governance impact
  ↓
Validation
  ↓
Result
```

Keep the review surface as small as practical.

---

## Review expectations

Maintainers may request changes when a contribution:

- increases complexity without sufficient benefit
- weakens Human authority
- introduces unnecessary autonomy
- bypasses validation
- adds an untrusted dependency
- lacks reproducible provenance
- introduces undocumented behaviour
- conflicts with the framework constitution
- duplicates an existing capability
- makes claims that cannot be demonstrated

The goal of review is not to maximize process.

The goal is to keep the framework useful, secure, understandable, and auditable.

---

## License

This project is licensed under the MIT License.

By contributing, you agree that your contributions are provided under the same MIT License, subject to the terms of the license.

See [`LICENSE`](LICENSE) for the complete license text.

---

## Final principle

The project values useful autonomy, but autonomy must remain within explicit boundaries.

When choosing between a more autonomous solution and a simpler, more controlled solution, prefer the solution that provides the required capability with the smallest reasonable increase in authority, complexity, and risk.

Keep it small. Keep it explicit. Keep it verifiable.
