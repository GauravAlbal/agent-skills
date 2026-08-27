# Test the Test example: mock offered as crash evidence

## Moment

A green unit test is offered as proof that committed records survive a process crash during journal rotation.

## Claim

> If the process is killed after the journal fsync and before the rename, restart returns the last committed record and never exposes the partial record.

## Offered check

`test_model_recovers_committed_record`:

1. constructs an in-memory journal state machine;
2. seeds committed record `A` and stages partial record `B`;
3. drives the model's explicit `CrashAt(AFTER_FSYNC_BEFORE_RENAME)` transition, which snapshots modeled durable bytes and discards modeled process-local state;
4. creates a fresh model from that snapshot;
5. asserts that recovery returns `A` and never exposes `B`.

The check ran on the current revision and is green. Its mutation receipt changes modeled recovery to return staged record `B`; this exact check then fails with `want A, got B`. The check therefore cannot remain green under the opposite recovery outcome its model represents.

## Apply the contract

- **Falsifiability:** yes. The explicit mutation makes the exact check red when recovery exposes the partial record.
- **Oracle authority:** yes. The expected truth is the last committed record with no partial record exposed.
- **Method or seam fitness:** no. The pure state machine never kills a process, calls a real fsync or rename, exercises filesystem ordering, or restarts the production implementation.
- **Current execution:** yes, but currentness cannot repair a wrong method.

```yaml
skill: proof
claim: crash durability across fsync-to-rename interruption
check: test_model_recovers_committed_record
falsifiable: true
oracle_authority: true
method_fitness: false
current_execution: true
decision: WRONG_METHOD
missing_method:
  - real process boundary
  - real filesystem
  - kill between fsync and rename
  - restart oracle for committed and partial records
```

## Disposition

`WRONG_METHOD`

The test can falsify the modeled opposite outcome. It is not evidence for the real crash mechanism because its method never reaches that seam.
