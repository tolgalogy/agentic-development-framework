# Skill Catalog

Every skill each agent uses, where it came from, and why. Two tiers, per `WORKFLOW.md` §12 (skills need a trusted source, an integrity check, a permission scope, and a pinned version — never `latest`):

1. **In-house workflow skills** — authored for this framework, implementing `WORKFLOW.md`'s lifecycle directly. These aren't "found," they're the framework's own control flow.
2. **Vendored supporting skills** — pulled from an existing, already-confirmed source rather than written from scratch, for the deeper/optional capability an agent can reach for on top of its core lifecycle step. Only added where a real match existed at an actual pinned release; no slot was force-filled.

## 1. In-house workflow skills

| Agent | Skill | Source | Path |
|---|---|---|---|
| product | `product-brief` | this repo | `.claude/skills/product-brief/SKILL.md` |
| tech-lead | `plan` | this repo | `.claude/skills/plan/SKILL.md` |
| builder | `build` | this repo | `.claude/skills/build/SKILL.md` |
| verifier | `verify` | this repo | `.claude/skills/verify/SKILL.md` |
| tech-lead (release gate) | `release` | this repo | `.claude/skills/release/SKILL.md` |

## 2. Vendored supporting skills

Source repo for all six: **`levnikolaevich/claude-code-skills`** (GitHub), MIT License, individually maintained. Pinned at tag `v2026.07.12`, commit `331d3b51c3210769de72fac94eb5b83ed559e762` — fetched at that exact commit, not from `main`, and each file was read in full before being adopted (see `templates/skills/vendor/README.md` for the review notes and update procedure).

| Agent | Required capability | Skill used | Downloaded from (repo) | URL (pinned to commit) | Local path |
|---|---|---|---|---|---|
| product | requirements drafting | *none — no safe match at the pinned release* | — | — | — |
| tech-lead | independent plan review before build starts | `ln-11-plan-reviewer` | `levnikolaevich/claude-code-skills` | https://github.com/levnikolaevich/claude-code-skills/blob/331d3b51c3210769de72fac94eb5b83ed559e762/plugins/review-suite/skills/ln-11-plan-reviewer/SKILL.md | `templates/skills/vendor/ln-11-plan-reviewer/SKILL.md` |
| builder | reproducible acceptance-test evidence | *none — no safe match at the pinned release* | — | — | — |
| builder (secondary use) | writing acceptance tests as self-test evidence | `ln-42-acceptance-test-builder` | `levnikolaevich/claude-code-skills` | https://github.com/levnikolaevich/claude-code-skills/blob/331d3b51c3210769de72fac94eb5b83ed559e762/plugins/testing-suite/skills/ln-42-acceptance-test-builder/SKILL.md | `templates/skills/vendor/ln-42-acceptance-test-builder/SKILL.md` |
| verifier | independent multi-perspective delivery review | `ln-12-delivery-reviewer` | `levnikolaevich/claude-code-skills` | https://github.com/levnikolaevich/claude-code-skills/blob/331d3b51c3210769de72fac94eb5b83ed559e762/plugins/review-suite/skills/ln-12-delivery-reviewer/SKILL.md | `templates/skills/vendor/ln-12-delivery-reviewer/SKILL.md` |
| verifier | test-suite quality/confidence audit | `ln-23-test-suite-auditor` | `levnikolaevich/claude-code-skills` | https://github.com/levnikolaevich/claude-code-skills/blob/331d3b51c3210769de72fac94eb5b83ed559e762/plugins/codebase-audit-suite/skills/ln-23-test-suite-auditor/SKILL.md | `templates/skills/vendor/ln-23-test-suite-auditor/SKILL.md` |
| security-reviewer | cross-cutting security/health audit | `ln-22-codebase-auditor` | `levnikolaevich/claude-code-skills` | https://github.com/levnikolaevich/claude-code-skills/blob/331d3b51c3210769de72fac94eb5b83ed559e762/plugins/codebase-audit-suite/skills/ln-22-codebase-auditor/SKILL.md | `templates/skills/vendor/ln-22-codebase-auditor/SKILL.md` |
| tech-lead (`/release`) | preparing and publishing a tagged release | `ln-63-release-publisher` | `levnikolaevich/claude-code-skills` | https://github.com/levnikolaevich/claude-code-skills/blob/331d3b51c3210769de72fac94eb5b83ed559e762/plugins/maintainer-suite/skills/ln-63-release-publisher/SKILL.md | `templates/skills/vendor/ln-63-release-publisher/SKILL.md` |

**Why product and builder have a blank row:** this repo's `main` branch README advertises a much larger 31-skill catalog, including a `product-requirements-builder` and a `surgical-change-implementer` that would have matched those two perfectly — but neither exists at any tagged release yet (checked directly against the tree at `v2026.07.12`). Pulling from `main` would mean running an unpinned, unreleased file with tool access. That's a worse trade than leaving the row honestly blank; `product-brief` and `build` already fully cover each role's core job on their own. Re-check this when a newer tag ships (see `templates/skills/vendor/README.md`'s update procedure).

## 3. Official, first-party, but not vendored (fetch on demand)

Anthropic's own `anthropics/skills` repo (commit `34040c9c568585f6929bedeaad110ad08f079624`) is the highest-trust source available, but almost all 18 of its skills are office-document/design/comms tooling, not SDLC-role skills. Two are conditionally relevant:

| Skill | Fits | Condition | URL |
|---|---|---|---|
| `webapp-testing` | builder | only for web-targeting projects; not vendored here since it ships helper Python/Playwright scripts specific to that stack — fetch it into a project's `.claude/skills/` only when that project actually targets web | https://github.com/anthropics/skills/blob/34040c9c568585f6929bedeaad110ad08f079624/skills/webapp-testing/SKILL.md |
| `claude-api` | builder | only if the project itself integrates the Claude/Anthropic API | https://github.com/anthropics/skills/blob/34040c9c568585f6929bedeaad110ad08f079624/skills/claude-api/SKILL.md |

## How an agent actually uses a vendored skill

These aren't wired into the agent's tool list automatically — they're invoked by name through the `Skill` tool from the orchestrating session when the situation calls for the deeper check (e.g. `/verify` on a high-risk task invoking `ln-12-delivery-reviewer` instead of relying on the `verifier` agent's lighter pass alone). `.claude/skills/verify/SKILL.md`, `plan/SKILL.md`, and `release/SKILL.md` reference the relevant one where applicable — see those files for exactly when.
