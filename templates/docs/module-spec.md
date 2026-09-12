# Module: <name>

One file per module under `docs/requirements/<slug>-v<n>/modules/`, per the Product role's module breakdown (`WORKFLOW.md` §5, drafted only from Human-confirmed answers — `CLAUDE.md` §2, `INITIATION.md` §5).

```yaml
module:
  name:
  requirement:          # e.g. docs/requirements/<slug>-v<n>/overview.md
  goal:
  in_scope: []
  out_of_scope: []
  acceptance_criteria: []
  dependencies: []       # other modules this one depends on
  integrations: []       # external systems/services this module touches
  data_entities: []
  constraints: []
  risks: []
  open_questions: []     # must be empty before this module is approved
```

_This file is immutable once its Requirement version is approved. A scope or acceptance-criteria change creates a new Requirement version (`-v<n+1>/`) — copy forward only the modules that actually changed._
