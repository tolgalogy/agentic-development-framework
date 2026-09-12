# Vendored skills

These six `SKILL.md` files are **verbatim copies** from a third-party repository, kept unmodified so their content can be diffed against upstream. They are not our own workflow skills (those live in `.claude/skills/` and implement `WORKFLOW.md` directly) — they're optional, deeper-coverage tools the agents can reach for on high-risk or otherwise warranted work. See `SKILLS.md` at the repo root for which agent uses which, and when.

## Provenance (WORKFLOW.md §12: trusted source, integrity check, pinned version)

- **Source:** `github.com/levnikolaevich/claude-code-skills`, MIT License (copyright Lev Nikolaevich — see `LICENSE-levnikolaevich-claude-code-skills` in this directory; keep that file alongside these skills per the license's attribution requirement).
- **Pinned at:** tag `v2026.07.12`, commit `331d3b51c3210769de72fac94eb5b83ed559e762`. Fetched directly from `raw.githubusercontent.com` at that exact commit, not from `main` and not from the tag name alone (tags can move; the commit can't).
- **Integrity caveat:** the tag is unsigned (`git tag -v` reports `unsigned` — no GPG signature). That's normal for a small open-source project, but it means provenance rests on GitHub's own custody of the ref, not on cryptographic proof of authorship. Re-verify the commit SHA against the tag if you re-pull.
- **Reviewed:** every file below was read in full before being copied in — checked for destructive defaults, prompt injection, secret exfiltration, or instructions that bypass the approval/evidence discipline this framework requires. All six read as evidence-based, risk-scaled, and appropriately conservative (read-only reviewers stay read-only; the one skill that mutates a real repo — release-publisher — gates every mutating step behind explicit user approval and forbids force-push/tag deletion).
- **Known gap:** this repo's own `README.md` on `main` describes a much larger, differently-numbered catalog (31 skills, e.g. a `product-requirements-builder` and a `surgical-change-implementer`) that does **not exist yet at any tagged release** — it's roadmap, not shipped. Don't be tempted to pull those from `main`; that would mean depending on an unpinned, unreleased file. That's also why Product and Builder have no vendored skill below — there was no safe match at the pinned version, and reaching for one anyway would have violated the pinning rule for the sake of a complete-looking table.

## Updating

Don't repoint these to `latest`/`main` casually. To take a newer version: pick a new tag, dereference it to its commit SHA the same way (`git/refs/tags/<tag>` → `git/tags/<sha>` → the `object.sha` it points to), re-fetch and re-read every changed file in full, update the pin recorded here and in `SKILLS.md`, and record the change as a decision (`docs/decisions.md` in a project that adopted this kit) since it's a change to what an agent is allowed to run.

## Files

| File | Upstream path at the pinned commit |
|---|---|
| `ln-11-plan-reviewer/SKILL.md` | `plugins/review-suite/skills/ln-11-plan-reviewer/SKILL.md` |
| `ln-12-delivery-reviewer/SKILL.md` | `plugins/review-suite/skills/ln-12-delivery-reviewer/SKILL.md` |
| `ln-22-codebase-auditor/SKILL.md` | `plugins/codebase-audit-suite/skills/ln-22-codebase-auditor/SKILL.md` |
| `ln-23-test-suite-auditor/SKILL.md` | `plugins/codebase-audit-suite/skills/ln-23-test-suite-auditor/SKILL.md` |
| `ln-42-acceptance-test-builder/SKILL.md` | `plugins/testing-suite/skills/ln-42-acceptance-test-builder/SKILL.md` |
| `ln-63-release-publisher/SKILL.md` | `plugins/maintainer-suite/skills/ln-63-release-publisher/SKILL.md` |
