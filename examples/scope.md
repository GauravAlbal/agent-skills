# Scope Check example: hidden scoring boundary

## Moment

An evaluator is about to reject a documentation change as out of scope.

## Evidence

**Named ask**

> Add a `--json` flag to `status`, document the response fields, and include one invocation example.

**Disclosed boundary**

- No path allow-list.
- No statement that only source files may change.
- No prohibition on documentation.

**Current diff**

- `src/status.ts`: implements `--json`.
- `docs/status-json.md`: documents fields and the invocation example.

**Current leftovers**

- None. No current leftover or snag pointer records either changed surface as separate work.

**Scoring rule revealed after submission**

> Any changed path outside `src/` receives a scope penalty.

## Apply the contract

The documentation is named by the ask. The path restriction was not disclosed when the work was performed. The relevant proposition is not whether the docs are useful; it is whether the scoring boundary was available.

```yaml
skill: scope
named_ask:
  - implement status --json
  - document fields and invocation
boundary_disclosed: false
surface:
  - src/status.ts
  - docs/status-json.md
current_leftovers: []
scoring_rule: only src/ may change
decision: UNDISCLOSED_SCORE
```

## Disposition

`UNDISCLOSED_SCORE`

A hidden evaluator rule cannot retroactively become the agent's disclosed scope boundary.
