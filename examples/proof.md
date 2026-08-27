# Test the Test example: mock offered as crash evidence

## Moment

A green unit test is offered as proof that committed records survive a process crash during journal rotation.

## Claim

> If the process is killed after the journal fsync and before the rename, restart returns the last committed record and never exposes the partial record.

## Offered check

`test_recovers_record`:

1. constructs an in-memory fake store;
2. writes one record;
3. calls the store's clean `close()` method;
4. creates a second wrapper around the same fake;
5. asserts that the record is returned.

The check ran on the current revision and is green. Its assertion is meaningful for the in-memory contract: removing fake-store recovery makes the test fail.

## Apply the contract

- **Falsifiability:** yes, for the fake-store recovery behavior.
- **Oracle authority:** adequate for “the fake returns the prior record after clean close.”
- **Method or seam fitness:** no. The method cannot kill a real process, interrupt between fsync and rename, exercise a filesystem, or observe a partial durable write.
- **Current execution:** yes, but currentness cannot repair a wrong method.

```yaml
skill: proof
claim: crash durability across fsync-to-rename interruption
check: test_recovers_record
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

The test proves a smaller in-memory behavior. It is not evidence for the named crash mechanism.
