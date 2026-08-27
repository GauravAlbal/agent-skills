# Done Reconciliation example: vanished obligation

## Moment

An agent reports: “Implementation and docs handled.”

## Evidence

**Original ask**

1. Add `status --json`.
2. Add `docs/status-json.md` with the response fields and one invocation example.

**Current artifacts**

- `cmd/status.go` implements `--json`.
- A current smoke receipt shows `status --json` emitting valid JSON.
- `docs/status-json.md` does not exist.
- No other current document contains the requested field reference or invocation example.

**Current leftovers**

- None. The final recap declares both requirements complete.

## Apply the contract

The documentation requirement was due now. It is neither delivered nor explicitly left open. A recap cannot erase the original obligation.

```yaml
skill: done
required_now:
  - status --json implementation
  - status JSON documentation
current_artifacts:
  - cmd/status.go
  - current smoke receipt
current_leftovers: []
decision: NOT_DONE
open_items:
  - add docs/status-json.md with fields and invocation example
```

## Disposition

`NOT_DONE`

The implementation may work. That does not make the named documentation requirement disappear.
