# Project Profile

Discovered facts, per `INITIATION.md` §4 Phase 2. Facts only — unknown stays `unknown`, never a guess. This file is planning context, not authority.

```yaml
project:
  platforms: []            # web | mobile | desktop | backend | infrastructure
  languages: []
  frameworks: []
  package_manager: unknown
  existing_product: unknown
  tests: unknown            # how tests are run, e.g. "npm test", or "unknown"
  ci: unknown               # what CI exists today, or "none"
  deployment: unknown       # how/where this deploys today, or "unknown"
  risk_level: unknown       # low | medium | high | unknown, see docs/project-policy.md
```

## Discovery notes

- Source, test, build, deployment, and config boundaries:
- Existing product/architecture/policy/CI/agent artifacts found:
- Available test/lint/typecheck/security/release commands:
- Secrets or sensitive-data boundaries:

_Update this file only when discovery facts actually change (new stack, new deploy target). Don't re-run full discovery on every task._
