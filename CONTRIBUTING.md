# Contributing

Thanks for your interest in ADF.

## Before you open a pull request

For anything beyond a typo or a small documentation fix, **open an issue first**. The framework is deliberately small, and the most useful discussion usually happens before code is written.

A proposed addition should be able to cite the specific section of `CLAUDE.md` or `WORKFLOW.md` it implements. If it cannot, it is probably scope creep rather than framework.

## Changes to governance rules

Edits to `CLAUDE.md`, `INITIATION.md`, or `WORKFLOW.md` — or to any authority, approval, validation, security, merge, retry, or workflow rule they define — follow the controlled change process those documents describe:

- Human approval
- A recorded decision
- A version increment on the affected document
- Consistency validation across the other bootstrap documents

Pull requests that silently change these rules as a side effect of another change will be asked to split them out.

## Adding or updating a skill

Skills must come from the confirmed four-repository allowlist in `SKILLS.md`, checked in its stated priority order, and must be pinned to an exact tag and commit — never `latest` or `main`. If no repository in the allowlist has a genuine match, the correct outcome is an open gap recorded in `SKILLS.md`, not a force-fitted substitute.

Record the source repository, license, pinned URL, and review notes for anything vendored.

## Licensing of contributions

By submitting a contribution, you agree that it is your own work (or that you have the right to submit it), and that it is provided under the project's MIT license.

You also agree that the maintainer may relicense the project under another OSI-approved license in the future, and that your contribution may be included under that license.

## Areas where help is especially welcome

- Agent and tool adapters for other agentic development environments
- Governance patterns and risk models
- Security controls and verification strategies
- Context and token efficiency
- CI/CD templates for additional platforms
- Real-world case studies from using the kit on an actual project
