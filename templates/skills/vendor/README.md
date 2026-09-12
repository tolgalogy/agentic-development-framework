# Vendored skills

These are **verbatim copies** from third-party (and one first-party) repositories, kept unmodified so their content can be diffed against upstream. They are not our own workflow skills (those live in `.claude/skills/` and implement `WORKFLOW.md` directly) — they're optional, deeper-coverage tools the agents can reach for on high-risk or otherwise warranted work. See `SKILLS.md` at the repo root for which agent uses which, and when.

## Trusted source allowlist (priority order) — CONFIRMED, closed list

Human-confirmed: these four repositories are the **entire** set of sources a skill may ever be sourced from for this framework. No other repository, marketplace, or site is permitted — not for a better license, a better match, or any other reason. When a skill is needed, check them in this exact order, and take the highest-priority repo with a real, safe match; if none of the four has one at an actual pinned release, that slot stays unfilled rather than reaching outside the list:

1. `github.com/anthropics/skills` — official, first-party.
2. `github.com/vercel-labs/agent-skills` — checked; **no LICENSE file at all** (repo metadata reports `license: null`), which blocks redistribution regardless of content, and its skills are Vercel/React/Next.js-stack-specific rather than SDLC-role skills. Nothing adopted from here.
3. `github.com/Jeffallan/claude-skills` — MIT-licensed, third-party, tagged releases.
4. `github.com/stillquietlyloud/claude_skills` — MIT-licensed, third-party, tagged releases, but the repo is **archived** (no longer maintained) with no independent community validation (0 stars at the time of review). Lowest-priority tier; only used when nothing higher up has a match.

This list is now closed and confirmed — it doesn't automatically retroactively invalidate the six skills below sourced from `levnikolaevich/claude-code-skills` before this allowlist existed, since removing already-reviewed content is a different action than not sourcing new content from it. But that repo is **not** on the allowlist, and now that the list is explicitly confirmed as closed, those six are a standing exception to it. See `SKILLS.md`'s "Open question" section — still unresolved, now sharper given this confirmation.

## Provenance (WORKFLOW.md §12: trusted source, integrity check, pinned version)

Every entry below was read in full, end to end, before being copied in — checked for destructive defaults, prompt injection, secret exfiltration, or instructions that bypass the approval/evidence discipline this framework requires. All entries read as evidence-based and appropriately conservative for what they claim to do.

### `levnikolaevich/claude-code-skills` (pre-dates the allowlist above — see note there)

- MIT License (copyright Lev Nikolaevich — see `LICENSE-levnikolaevich-claude-code-skills`; keep it alongside these skills per the license's attribution requirement).
- **Pinned at:** tag `v2026.07.12`, commit `331d3b51c3210769de72fac94eb5b83ed559e762`. Fetched directly from `raw.githubusercontent.com` at that exact commit, not from `main` and not from the tag name alone (tags can move; the commit can't).
- **Integrity caveat:** the tag is unsigned (no GPG signature) — provenance rests on GitHub's custody of the ref, not cryptographic proof of authorship.
- **Known gap:** this repo's `README.md` on `main` describes a much larger, differently-numbered catalog than what's actually tagged — it's roadmap, not shipped. Don't pull from `main`.
- Files: `ln-11-plan-reviewer`, `ln-12-delivery-reviewer`, `ln-22-codebase-auditor`, `ln-23-test-suite-auditor`, `ln-42-acceptance-test-builder`, `ln-63-release-publisher`.

### `Jeffallan/claude-skills` (allowlist #3)

- MIT License (see `LICENSE-Jeffallan-claude-skills`).
- **Pinned at:** tag `v0.4.16`, commit `d5d2dd8ae2ec3842a46e93894655be888fe29446` (lightweight tag — this SHA *is* the commit, no dereferencing needed).
- Files: `feature-forge` (fills the Product gap — structured requirements interview producing EARS-format specs; explicitly forbids generating a spec without conducting the interview first, which reinforces rather than conflicts with `product-brief`'s own no-assumptions rule) and `spec-miner` (fills a Tech-lead need this kit didn't have: reverse-engineering specs from an undocumented/legacy codebase during `INITIATION.md` Phase 1 discovery), each with their `references/*.md` support files.
- Also reviewed but **not adopted** (documented here so the check isn't silently repeated later): `security-reviewer`, `code-reviewer`, `test-master` — all solid, MIT, well-scoped single-pass checklists, but this kit already has an allowlisted-adjacent match for those roles from `levnikolaevich` (see the open question above on whether to keep or replace those). Revisit if that question resolves toward replacement.
- Checked `vercel-labs/agent-skills` (allowlist #2) first per priority — no license and no relevant match, so moved to #3.

### `anthropics/skills` (allowlist #1) — checked, not vendored

Official and first-party, so it clears the trust bar automatically, but almost all 18 skills are office-document/design/comms tooling, not SDLC-role skills. `webapp-testing` (Playwright-based) fits Builder conditionally for web-targeting projects, and `claude-api` fits Builder conditionally if the project integrates the Claude/Anthropic API. Not vendored here — `webapp-testing` ships its own Python helper scripts specific to that one stack, and both are narrow/conditional enough that fetching them fresh into a project that actually needs them beats carrying them in every copy of this generic kit. See `SKILLS.md` for the pinned commit and URLs.

## Updating

Don't repoint any of these to `latest`/`main` casually. To take a newer version: pick a new tag, resolve it to its commit SHA (dereference an annotated tag via `git/tags/<sha>` if needed — a lightweight tag's ref SHA already *is* the commit), re-fetch and re-read every changed file in full, update the pin recorded here and in `SKILLS.md`, and record the change as a decision (`docs/decisions.md` in a project that adopted this kit) since it's a change to what an agent is allowed to run.

## Files

| File | Source repo | Upstream path at the pinned commit |
|---|---|---|
| `ln-11-plan-reviewer/SKILL.md` | levnikolaevich/claude-code-skills | `plugins/review-suite/skills/ln-11-plan-reviewer/SKILL.md` |
| `ln-12-delivery-reviewer/SKILL.md` | levnikolaevich/claude-code-skills | `plugins/review-suite/skills/ln-12-delivery-reviewer/SKILL.md` |
| `ln-22-codebase-auditor/SKILL.md` | levnikolaevich/claude-code-skills | `plugins/codebase-audit-suite/skills/ln-22-codebase-auditor/SKILL.md` |
| `ln-23-test-suite-auditor/SKILL.md` | levnikolaevich/claude-code-skills | `plugins/codebase-audit-suite/skills/ln-23-test-suite-auditor/SKILL.md` |
| `ln-42-acceptance-test-builder/SKILL.md` | levnikolaevich/claude-code-skills | `plugins/testing-suite/skills/ln-42-acceptance-test-builder/SKILL.md` |
| `ln-63-release-publisher/SKILL.md` | levnikolaevich/claude-code-skills | `plugins/maintainer-suite/skills/ln-63-release-publisher/SKILL.md` |
| `feature-forge/SKILL.md` + `references/*.md` | Jeffallan/claude-skills | `skills/feature-forge/SKILL.md` + `skills/feature-forge/references/*.md` |
| `spec-miner/SKILL.md` + `references/*.md` | Jeffallan/claude-skills | `skills/spec-miner/SKILL.md` + `skills/spec-miner/references/*.md` |
