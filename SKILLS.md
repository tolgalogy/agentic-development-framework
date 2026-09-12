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
| 3 | `github.com/Jeffallan/claude-skills` | MIT-licensed, tagged releases. Two skills adopted (`feature-forge`, `spec-miner`); three more reviewed but not adopted (see the open question below). |
| 4 | `github.com/stillquietlyloud/claude_skills` | MIT-licensed, tagged releases, but **archived** (no longer maintained) and 0 stars — lowest-priority tier, only used when 1–3 have no match. Checked for the Product and Builder gaps specifically; `product-manager-toolkit` exists but `feature-forge` (priority 3) was the better and higher-priority fit, so it wasn't used. |

## 1. In-house workflow skills

| Agent | Skill | Source | Path |
|---|---|---|---|
| product | `product-brief` | this repo | `.claude/skills/product-brief/SKILL.md` |
| tech-lead | `plan` | this repo | `.claude/skills/plan/SKILL.md` |
| builder | `build` | this repo | `.claude/skills/build/SKILL.md` |
| verifier | `verify` | this repo | `.claude/skills/verify/SKILL.md` |
| tech-lead (release gate) | `release` | this repo | `.claude/skills/release/SKILL.md` |

## 2. Vendored supporting skills

Each entry was read in full, end to end, before being adopted (see `templates/skills/vendor/README.md` for review notes and the update procedure). Six of these — everything sourced from `levnikolaevich/claude-code-skills` — predate the allowlist above and that repo isn't on it; see the open question below.

| Agent | Required capability | Skill used | Downloaded from (repo) | URL (pinned to commit) | Local path |
|---|---|---|---|---|---|
| product | requirements interview → EARS-format spec | `feature-forge` | `Jeffallan/claude-skills` | https://github.com/Jeffallan/claude-skills/blob/d5d2dd8ae2ec3842a46e93894655be888fe29446/skills/feature-forge/SKILL.md | `templates/skills/vendor/feature-forge/SKILL.md` |
| tech-lead | reverse-engineering specs from an undocumented/legacy codebase | `spec-miner` | `Jeffallan/claude-skills` | https://github.com/Jeffallan/claude-skills/blob/d5d2dd8ae2ec3842a46e93894655be888fe29446/skills/spec-miner/SKILL.md | `templates/skills/vendor/spec-miner/SKILL.md` |
| tech-lead | independent plan review before build starts | `ln-11-plan-reviewer` | `levnikolaevich/claude-code-skills` *(pre-allowlist)* | https://github.com/levnikolaevich/claude-code-skills/blob/331d3b51c3210769de72fac94eb5b83ed559e762/plugins/review-suite/skills/ln-11-plan-reviewer/SKILL.md | `templates/skills/vendor/ln-11-plan-reviewer/SKILL.md` |
| builder | reproducible acceptance-test evidence | *none — no safe match at any pinned release, checked across all four allowlisted repos* | — | — | — |
| builder (secondary use) | writing acceptance tests as self-test evidence | `ln-42-acceptance-test-builder` | `levnikolaevich/claude-code-skills` *(pre-allowlist)* | https://github.com/levnikolaevich/claude-code-skills/blob/331d3b51c3210769de72fac94eb5b83ed559e762/plugins/testing-suite/skills/ln-42-acceptance-test-builder/SKILL.md | `templates/skills/vendor/ln-42-acceptance-test-builder/SKILL.md` |
| verifier | independent multi-perspective delivery review | `ln-12-delivery-reviewer` | `levnikolaevich/claude-code-skills` *(pre-allowlist)* | https://github.com/levnikolaevich/claude-code-skills/blob/331d3b51c3210769de72fac94eb5b83ed559e762/plugins/review-suite/skills/ln-12-delivery-reviewer/SKILL.md | `templates/skills/vendor/ln-12-delivery-reviewer/SKILL.md` |
| verifier | test-suite quality/confidence audit | `ln-23-test-suite-auditor` | `levnikolaevich/claude-code-skills` *(pre-allowlist)* | https://github.com/levnikolaevich/claude-code-skills/blob/331d3b51c3210769de72fac94eb5b83ed559e762/plugins/codebase-audit-suite/skills/ln-23-test-suite-auditor/SKILL.md | `templates/skills/vendor/ln-23-test-suite-auditor/SKILL.md` |
| security-reviewer | cross-cutting security/health audit | `ln-22-codebase-auditor` | `levnikolaevich/claude-code-skills` *(pre-allowlist)* | https://github.com/levnikolaevich/claude-code-skills/blob/331d3b51c3210769de72fac94eb5b83ed559e762/plugins/codebase-audit-suite/skills/ln-22-codebase-auditor/SKILL.md | `templates/skills/vendor/ln-22-codebase-auditor/SKILL.md` |
| tech-lead (`/release`) | preparing and publishing a tagged release | `ln-63-release-publisher` | `levnikolaevich/claude-code-skills` *(pre-allowlist)* | https://github.com/levnikolaevich/claude-code-skills/blob/331d3b51c3210769de72fac94eb5b83ed559e762/plugins/maintainer-suite/skills/ln-63-release-publisher/SKILL.md | `templates/skills/vendor/ln-63-release-publisher/SKILL.md` |

**Why builder still has a blank row:** none of the four allowlisted repos has a generic "stay in scope, minimal-footprint implementer" skill at an actual pinned release. `levnikolaevich/claude-code-skills`' `main`-branch README advertises a `surgical-change-implementer` and its own `focused-fix`-named skill exists at `stillquietlyloud/claude_skills`' `main` too — but neither exists at either repo's latest tagged release (checked directly against each tag's tree). Pulling from `main` would mean running an unpinned, unreleased file with tool access, which is a worse trade than leaving the row honestly blank; `build` already fully covers the role on its own.

**Reviewed but not adopted, from `Jeffallan/claude-skills`:** `security-reviewer`, `code-reviewer`, `test-master` — all MIT, well-scoped, single-pass checklist skills that would plausibly fit verifier/security-reviewer. Not adopted only because this kit already has a match for those slots from `levnikolaevich` (pre-allowlist) — see the open question below for whether that should change.

## 3. Official, first-party, but not vendored (fetch on demand)

`anthropics/skills` (commit `34040c9c568585f6929bedeaad110ad08f079624`) is the highest-trust source available, but almost all 18 of its skills are office-document/design/comms tooling. Two are conditionally relevant:

| Skill | Fits | Condition | URL |
|---|---|---|---|
| `webapp-testing` | builder | only for web-targeting projects; not vendored here since it ships helper Python/Playwright scripts specific to that stack — fetch it into a project's `.claude/skills/` only when that project actually targets web | https://github.com/anthropics/skills/blob/34040c9c568585f6929bedeaad110ad08f079624/skills/webapp-testing/SKILL.md |
| `claude-api` | builder | only if the project itself integrates the Claude/Anthropic API | https://github.com/anthropics/skills/blob/34040c9c568585f6929bedeaad110ad08f079624/skills/claude-api/SKILL.md |

## Open question for the Human

The six skills sourced from `levnikolaevich/claude-code-skills` were adopted before this priority allowlist existed, and that repo isn't on it. Two ways to resolve this, neither chosen here since it's a real trade-off, not a formality:

1. **Keep them as a grandfathered exception** — they were fully reviewed (see `templates/skills/vendor/README.md`) and offer a more rigorous mechanism (independent multi-perspective panel review) than the closest allowlisted alternatives.
2. **Replace them with the allowlisted alternatives from `Jeffallan/claude-skills`** (`security-reviewer` → security-reviewer agent, `code-reviewer` → verifier, `test-master` → verifier/builder) — simpler single-pass checklists, lower depth but strictly within the new allowlist.

Say which, and this catalog and the vendored files get updated accordingly.

## How an agent actually uses a vendored skill

These aren't wired into the agent's tool list automatically — they're invoked by name through the `Skill` tool from the orchestrating session when the situation calls for the deeper check (e.g. `/verify` on a high-risk task invoking `ln-12-delivery-reviewer` instead of relying on the `verifier` agent's lighter pass alone). `.claude/skills/product-brief/SKILL.md`, `plan/SKILL.md`, `build/SKILL.md`, `verify/SKILL.md`, and `release/SKILL.md`, plus `.claude/agents/security-reviewer.md`, reference the relevant one where applicable — see those files for exactly when.
