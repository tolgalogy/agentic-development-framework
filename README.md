# Agentic Development Framework

A lean, risk-based, agentic SDLC template for web, mobile, and desktop projects. It's a **kit to copy into a real project repo**, not a product itself.

## What's here

```text
CLAUDE.md, INITIATION.md, WORKFLOW.md   -> the constitution (authority, safety, execution). Read these first — everything else implements them.
SKILLS.md                                -> catalog of every skill each agent uses: in-house or vendored, source, pinned version, URL
.claude/agents/                          -> the four roles (product, tech-lead, builder, verifier) + the security-reviewer specialist, as Claude Code subagents
.claude/skills/                          -> /product-brief, /plan, /build, /verify, /release — one skill per lifecycle gate
templates/docs/                          -> fill-in templates for the operating kit INITIATION.md asks for (project-profile, project-policy, architecture-summary, decisions, module-spec)
templates/ci/                            -> GitHub Actions templates for web/mobile/desktop, with a staging/production environment gate
templates/agents/                        -> a copyable pattern for adding a conditional specialist reviewer (performance/accessibility/platform/...) only when a project actually needs one
templates/skills/vendor/                 -> the vendored supporting skills listed in SKILLS.md, pinned to an exact upstream commit
```

## Bootstrapping a new project with this kit

1. Copy `CLAUDE.md`, `INITIATION.md`, `WORKFLOW.md`, `SKILLS.md`, `.claude/`, and `templates/` into the target repo.
2. Run `INITIATION.md`'s protocol against the target repo: it discovers the stack and fills `docs/project-profile.md` and `docs/project-policy.md` from the `templates/docs/` copies. This produces `ORGANIZATION_INITIALIZED` and locks product work until Human Product Intent arrives.
3. Copy the CI template(s) matching the project's actual target platforms from `templates/ci/` into `.github/workflows/`, and configure the `staging`/`production` GitHub Environments as described in `templates/ci/README.md`.
4. From here, the day-to-day loop is: `/product-brief` (a full interview — see below, ends with approved per-module specs) → `/plan` → `/build` → `/verify` → (repeat build/verify per task) → `/release`.

## The role model

Four roles, matching `WORKFLOW.md` §2 exactly — no extra permanent roles were added:

| Role | Owns | Agent | Can't do |
|---|---|---|---|
| Product | what & why, acceptance criteria | `product` | implement, approve its own scope, decide an unanswered detail or the module breakdown itself |
| Technical Lead | how, decomposition, coordination, merge authorization | `tech-lead` | implement product code |
| Builder | authorized implementation | `builder` | approve/merge medium+ risk work it wrote |
| Verifier | independent validation & review | `verifier` | fix code, review its own implementation |

`security-reviewer` is the one specialist capability built in by default, because `CLAUDE.md` §7 makes security review mandatory for essentially every high-risk task (auth, payments, PII, public APIs, migrations, infra). Other specialists (performance, accessibility, platform) are **conditional** per `WORKFLOW.md` §8 — don't pre-create agents for them. When a project's `docs/project-policy.md` actually activates one, copy `templates/agents/specialist-reviewer.md.template` and fill it in.

Each agent's core lifecycle skill is authored in-house, but all except Builder also have an optional, deeper vendored skill they can reach for on high-risk or otherwise warranted work (a requirements interview aid, legacy-codebase discovery, an independent plan/delivery review, a test-suite audit, a broader security/health audit, or publishing an actual tagged release) — see `SKILLS.md` for exactly which, sourced from where, and pinned to what version. Skill sourcing is scoped to a small, priority-ordered allowlist of repos (also in `SKILLS.md`) rather than an open-ended search. Builder has none: no safe match existed at an actual pinned release across any allowlisted repo, and `build` already covers the role on its own.

### How Product actually gathers requirements

`/product-brief` is a mandatory interview, not a form the Human fills in once. It runs in the interactive session itself — never delegated wholesale to the `product` subagent, since a subagent can't hold a back-and-forth with the Human. For every field (goal, users, problem, platforms, constraints, success criteria, and later each module's scope/acceptance-criteria/dependencies), it asks via `AskUserQuestion` with concrete options and one recommended answer, and the Human still has to pick — a recommendation is never auto-applied (`CLAUDE.md` §2, `INITIATION.md` §5: Product "may clarify and structure the intent, but may not silently invent answers"). Once the brief is approved, Product proposes a candidate module breakdown, but that's a proposal the Human must confirm or redraw, never a decision the agent gets to make. Each confirmed module gets its own spec file under `docs/requirements/<slug>-v<n>/modules/`, and only once the whole set is approved does Product hand off directly into `/plan` — no separate hand-off gate, since Requirement approval is already the authorization `WORKFLOW.md` §5 requires.

## Why this stays small (80/20)

- 4 roles, 1 default specialist, 5 in-house skills, 8 vendored supporting skills (pinned and reviewed, not a bulk import, sourced from a 4-repo priority allowlist), 5 doc templates, 3 CI templates. That's the whole kit — resist the urge to add a role, skill, or document for every idea; `CLAUDE.md` §3 and `INITIATION.md` §3 exist specifically to block that drift.
- Every gate maps to something in `WORKFLOW.md`/`CLAUDE.md` — if a proposed addition doesn't cite a specific section it's implementing, it's probably scope creep, not framework.
- Context/token discipline is load-bearing, not optional: each skill loads only the task packet + `docs/project-policy.md` by default (`WORKFLOW.md` §4), and pulls in more only when the task actually needs it.
- Risk-based validation means most tasks (low risk) only ever need self-test + targeted review — the full security/functional/quality stack is reserved for the work that's actually high-risk, per `WORKFLOW.md` §8.

## Changing the framework itself

Edits to `CLAUDE.md`, `INITIATION.md`, `WORKFLOW.md`, or the authority/approval/validation/security/merge rules they define require Human approval, a recorded decision, and a version bump (`CLAUDE.md` §11, `WORKFLOW.md` §13) — in this repo or any project copied from it.
