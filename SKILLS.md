# Skill Catalog

Every skill each agent uses, where it came from, and why. Two tiers, per `WORKFLOW.md` §12 (skills need a trusted source, an integrity check, a permission scope, and a pinned version — never `latest`):

1. **In-house workflow skills** — authored for this framework, implementing `WORKFLOW.md`'s lifecycle directly. These aren't "found," they're the framework's own control flow.
2. **Vendored supporting skills** — pulled from an existing, already-confirmed source rather than written from scratch, for the deeper/optional capability an agent can reach for on top of its core lifecycle step. Only added where a real match existed at an actual pinned release; no slot was force-filled.

## Trusted source allowlist (priority order) — CONFIRMED, closed list

Human-confirmed: these four repositories are the **entire** set of sources a skill may ever be sourced from for this framework. No other repository, marketplace, or site is permitted, regardless of license, star count, or how good a match it looks like — if none of these four has a safe match at a real pinned release, the answer is "no vendored skill for this slot," not "widen the search." When a skill is needed, check them in this exact order — #1 before #2, #2 before #3, #3 before #4 — and take the highest-priority repo with a real, safe match; never skip ahead to a lower-priority repo when a higher one already covers the need, and never force-fit a low-priority pick just to fill a table cell:

| # | Repo | Status |
|---|---|---|
| 1 | `github.com/anthropics/skills` | Official, first-party. Checked — mostly office-document/design/comms tooling; two skills conditionally relevant (§3 below), nothing else fits our SDLC roles. |
| 2 | `github.com/vercel-labs/agent-skills` | Checked — **no LICENSE file** (repo reports `license: null`), which blocks redistribution regardless of content; skills are also Vercel/React/Next.js-stack-specific, not SDLC-role skills. Nothing adopted. |
| 3 | `github.com/Jeffallan/claude-skills` | MIT-licensed, tagged releases. Current source for all five vendored skills below. |
| 4 | `github.com/stillquietlyloud/claude_skills` | MIT-licensed, tagged releases, but **archived** (no longer maintained), 0 stars, and at least one checked candidate (`release-manager`) lacks the frontmatter a `SKILL.md` needs to be invocable at all. Lowest-priority tier; checked for both open gaps below, nothing adoptable found. |

## 1. In-house workflow skills

| Agent | Skill | Source | Path |
|---|---|---|---|
| product | `product-brief` | this repo | `.claude/skills/product-brief/SKILL.md` |
| tech-lead | `plan` | this repo | `.claude/skills/plan/SKILL.md` |
| builder | `build` | this repo | `.claude/skills/build/SKILL.md` |
| verifier | `verify` | this repo | `.claude/skills/verify/SKILL.md` |
| tech-lead (release gate) | `release` | this repo | `.claude/skills/release/SKILL.md` |

## 2. Vendored supporting skills

All five sourced from **`Jeffallan/claude-skills`**, MIT License, pinned to tag `v0.4.16`, commit `d5d2dd8ae2ec3842a46e93894655be888fe29446`. Each was read in full, end to end, before being adopted (see `templates/skills/vendor/README.md` for review notes and the update procedure).

| Agent | Required capability | Skill used | Downloaded from (repo) | URL (pinned to commit) | Local path |
|---|---|---|---|---|---|
| product | requirements interview → EARS-format spec | `feature-forge` | `Jeffallan/claude-skills` | https://github.com/Jeffallan/claude-skills/blob/d5d2dd8ae2ec3842a46e93894655be888fe29446/skills/feature-forge/SKILL.md | `templates/skills/vendor/feature-forge/SKILL.md` |
| tech-lead | reverse-engineering specs from an undocumented/legacy codebase | `spec-miner` | `Jeffallan/claude-skills` | https://github.com/Jeffallan/claude-skills/blob/d5d2dd8ae2ec3842a46e93894655be888fe29446/skills/spec-miner/SKILL.md | `templates/skills/vendor/spec-miner/SKILL.md` |
| verifier | broad-scope PR-style delivery review | `code-reviewer` | `Jeffallan/claude-skills` | https://github.com/Jeffallan/claude-skills/blob/d5d2dd8ae2ec3842a46e93894655be888fe29446/skills/code-reviewer/SKILL.md | `templates/skills/vendor/code-reviewer/SKILL.md` |
| security-reviewer | SAST/dependency/secret-scan tooling + pentest workflow | `security-reviewer` (skill — distinct from the agent of the same name) | `Jeffallan/claude-skills` | https://github.com/Jeffallan/claude-skills/blob/d5d2dd8ae2ec3842a46e93894655be888fe29446/skills/security-reviewer/SKILL.md | `templates/skills/vendor/security-reviewer/SKILL.md` |
| verifier + builder | test generation, mocking, coverage, and test-suite audit | `test-master` | `Jeffallan/claude-skills` | https://github.com/Jeffallan/claude-skills/blob/d5d2dd8ae2ec3842a46e93894655be888fe29446/skills/test-master/SKILL.md | `templates/skills/vendor/test-master/SKILL.md` |

**Open gaps — no adoptable match in any of the four allowlisted repos:**

- **tech-lead — independent plan review before build starts.** Nothing in any of the four repos does evidence-first review of an implementation plan without also being a design/authoring tool. Closest were `architecture-designer` (Jeffallan) and `senior-architect` (stillquietlyloud) — both *build* architecture, they don't *audit* an existing plan; adopting either would misrepresent what the skill does. `plan` falls back to the `tech-lead` agent's own judgment.
- **release — tagging/publishing a GitHub Release.** `stillquietlyloud`'s `release-manager` is the closest concept but its `SKILL.md` has no `name`/`description` frontmatter, so it isn't a functioning skill file. `release` falls back to the Technical Lead/Human handling the actual tag and release-notes publication directly, using the checklist's `RELEASE_READY` result as the go-ahead.

Re-check both whenever any of the four repos ships a new tagged release (see `templates/skills/vendor/README.md`'s update procedure) — don't fill either from `main`/unreleased content in the meantime.

## 3. Official, first-party, but not vendored (fetch on demand)

`anthropics/skills` (commit `34040c9c568585f6929bedeaad110ad08f079624`) is the highest-trust source available, but almost all 18 of its skills are office-document/design/comms tooling. Two are conditionally relevant:

| Skill | Fits | Condition | URL |
|---|---|---|---|
| `webapp-testing` | builder | only for web-targeting projects; not vendored here since it ships helper Python/Playwright scripts specific to that stack — fetch it into a project's `.claude/skills/` only when that project actually targets web | https://github.com/anthropics/skills/blob/34040c9c568585f6929bedeaad110ad08f079624/skills/webapp-testing/SKILL.md |
| `claude-api` | builder | only if the project itself integrates the Claude/Anthropic API | https://github.com/anthropics/skills/blob/34040c9c568585f6929bedeaad110ad08f079624/skills/claude-api/SKILL.md |

## History

An earlier version of this catalog listed six skills from `levnikolaevich/claude-code-skills`, adopted before the allowlist above was confirmed as closed. Per Human decision, all six were removed; four had a direct replacement in `Jeffallan/claude-skills` (table above), two didn't and are recorded as open gaps instead of being filled with a mismatched pick. See `templates/skills/vendor/README.md` for the full removed→replacement mapping.

## How an agent actually uses a vendored skill

These aren't wired into the agent's tool list automatically — they're invoked by name through the `Skill` tool from the orchestrating session when the situation calls for the deeper check (e.g. `/verify` on a high-risk task invoking `code-reviewer` instead of relying on the `verifier` agent's lighter pass alone). `.claude/skills/product-brief/SKILL.md`, `plan/SKILL.md`, `build/SKILL.md`, `verify/SKILL.md`, and `.claude/agents/tech-lead.md`/`security-reviewer.md` reference the relevant one where applicable — see those files for exactly when.
