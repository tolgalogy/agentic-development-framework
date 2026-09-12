# Vendored skills

These are **verbatim copies** from confirmed third-party (and one first-party) repositories, kept unmodified so their content can be diffed against upstream. They are not our own workflow skills (those live in `.claude/skills/` and implement `WORKFLOW.md` directly) — they're optional, deeper-coverage tools the agents can reach for on high-risk or otherwise warranted work. See `SKILLS.md` at the repo root for which agent uses which, and when.

## Trusted source allowlist (priority order) — CONFIRMED, closed list

Human-confirmed: these four repositories are the **entire** set of sources a skill may ever be sourced from for this framework. No other repository, marketplace, or site is permitted — not for a better license, a better match, or any other reason. When a skill is needed, check them in this exact order, and take the highest-priority repo with a real, safe match; if none of the four has one at an actual pinned release, that slot stays unfilled rather than reaching outside the list:

1. `github.com/anthropics/skills` — official, first-party.
2. `github.com/vercel-labs/agent-skills` — checked; **no LICENSE file at all** (repo metadata reports `license: null`), which blocks redistribution regardless of content, and its skills are Vercel/React/Next.js-stack-specific rather than SDLC-role skills. Nothing adopted from here.
3. `github.com/Jeffallan/claude-skills` — MIT-licensed, third-party, tagged releases. Current source for every vendored skill below.
4. `github.com/stillquietlyloud/claude_skills` — MIT-licensed, third-party, tagged releases, but the repo is **archived** (no longer maintained) with no independent community validation (0 stars at the time of review), and at least one checked file (`release-manager`) lacks the frontmatter a `SKILL.md` needs to be invocable at all. Lowest-priority tier; checked for every gap below and found nothing adoptable.

## History: replaced the pre-allowlist vendored set

An earlier version of this kit vendored six skills from `levnikolaevich/claude-code-skills`, sourced before this allowlist existed. That repo was never on the allowlist. Per Human decision, all six were removed and replaced with matches from the confirmed list where one existed. Four had a real, same-purpose replacement in `Jeffallan/claude-skills`; two did not, and are recorded as open gaps below rather than filled with a mismatched pick.

| Removed (levnikolaevich) | Used for | Replacement | Status |
|---|---|---|---|
| `ln-12-delivery-reviewer` | verifier | `code-reviewer` | Replaced |
| `ln-22-codebase-auditor` | security-reviewer | `security-reviewer` (skill) | Replaced |
| `ln-23-test-suite-auditor` | verifier | `test-master` | Replaced |
| `ln-42-acceptance-test-builder` | builder | `test-master` | Replaced (same skill covers both) |
| `ln-11-plan-reviewer` | tech-lead | *none* | **Open gap** — nothing in any of the four repos does independent, evidence-first review of an implementation plan without also being a design/authoring tool (closest were `architecture-designer` in Jeffallan and `senior-architect` in stillquietlyloud — both build/author architecture, they don't audit an existing plan). |
| `ln-63-release-publisher` | release | *none* | **Open gap** — `stillquietlyloud`'s `release-manager` is the closest concept but its `SKILL.md` has no `name`/`description` frontmatter, so it isn't a functioning skill file at all. |

The two open gaps aren't blocking: `plan` and `release` fall back to the `tech-lead` agent's own judgment and the Technical Lead/Human handling tag/release publication directly. Revisit if a newer release of any allowlisted repo ships something that actually fits.

## Provenance (WORKFLOW.md §12: trusted source, integrity check, pinned version)

Every entry below was read in full, end to end, before being copied in — checked for destructive defaults, prompt injection, secret exfiltration, or instructions that bypass the approval/evidence discipline this framework requires. All entries read as evidence-based and appropriately conservative for what they claim to do.

### `Jeffallan/claude-skills` (allowlist #3)

- MIT License (see `LICENSE-Jeffallan-claude-skills`).
- **Pinned at:** tag `v0.4.16`, commit `d5d2dd8ae2ec3842a46e93894655be888fe29446` (lightweight tag — this SHA *is* the commit, no dereferencing needed).
- Files, each with its own `references/*.md` support files:
  - `feature-forge` — fills the Product gap: structured requirements interview producing EARS-format specs; explicitly forbids generating a spec without conducting the interview first.
  - `spec-miner` — fills a Tech-lead need: reverse-engineering specs from an undocumented/legacy codebase during `INITIATION.md` Phase 1 discovery.
  - `code-reviewer` — broad-scope PR-style review (correctness, security, performance, maintainability, test coverage) for the `verifier` agent's deeper pass.
  - `security-reviewer` — **a skill, distinct from this kit's `security-reviewer` agent** (same name, different thing) — adds concrete SAST/dependency/secret-scan tool commands and a pentest workflow with explicit scope-authorization gates, for the agent's deeper/periodic audit.
  - `test-master` — test generation, mocking strategy, coverage analysis across functional/performance/security testing; used by both `verifier` (test-suite audit) and `builder` (writing acceptance-test evidence).

### `anthropics/skills` (allowlist #1) — checked, not vendored

Official and first-party, so it clears the trust bar automatically, but almost all 18 skills are office-document/design/comms tooling, not SDLC-role skills. `webapp-testing` (Playwright-based) fits Builder conditionally for web-targeting projects, and `claude-api` fits Builder conditionally if the project integrates the Claude/Anthropic API. Not vendored here — `webapp-testing` ships its own Python helper scripts specific to that one stack, and both are narrow/conditional enough that fetching them fresh into a project that actually needs them beats carrying them in every copy of this generic kit. See `SKILLS.md` for the pinned commit and URLs.

## Updating

Don't repoint any of these to `latest`/`main` casually. To take a newer version: pick a new tag, resolve it to its commit SHA (dereference an annotated tag via `git/tags/<sha>` if needed — a lightweight tag's ref SHA already *is* the commit), re-fetch and re-read every changed file in full, update the pin recorded here and in `SKILLS.md`, and record the change as a decision (`docs/decisions.md` in a project that adopted this kit) since it's a change to what an agent is allowed to run.

## Files

| File | Source repo | Upstream path at the pinned commit |
|---|---|---|
| `feature-forge/SKILL.md` + `references/*.md` | Jeffallan/claude-skills | `skills/feature-forge/` |
| `spec-miner/SKILL.md` + `references/*.md` | Jeffallan/claude-skills | `skills/spec-miner/` |
| `code-reviewer/SKILL.md` + `references/*.md` | Jeffallan/claude-skills | `skills/code-reviewer/` |
| `security-reviewer/SKILL.md` + `references/*.md` | Jeffallan/claude-skills | `skills/security-reviewer/` |
| `test-master/SKILL.md` + `references/*.md` | Jeffallan/claude-skills | `skills/test-master/` |
