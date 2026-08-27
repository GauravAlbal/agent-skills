# Three-minute examples

Each skill answers one bounded question. Give it the named artifacts; stop at its disposition.

## Done Reconciliation

**Moment:** An agent says a task is complete.

```text
Use done. Original ask: add CSV import and document the CLI flag.
Current artifacts: parser + tests landed; CLI docs unchanged.
Current leftovers: none recorded.
```

Expected shape: `NOT_DONE`. The docs obligation is due now, absent, and not explicitly left. This says nothing about parser quality.

## DDx Lite

**Moment:** A rerun is tempting after a red result.

```text
Use ddx. Worker changed the declared file, but the checker rejects because
an unrelated untracked file exists. Precommit one discriminator between
implementation failure and checker/environment failure.
```

Expected shape: classify the causal fork, run one cheap observation, and choose the next repair class. Do not reroll before spending an empirically decidable fork.

## Test the Test

**Moment:** A green check is offered as proof.

```text
Use proof. Claim: state survives a process crash between journal writes.
Check: an in-process unit test calls recover() after clean shutdown.
Revision: current.
```

Expected shape: `WRONG_METHOD`. The check may be real and current; it cannot expose torn persistence or crash recovery.

## Scope Check

**Moment:** An agent changed an extra file and a reviewer calls it out of scope.

```text
Use scope. The original ask names src/auth.ts only.
The agent also edits package.json and explicitly records why in the current handoff.
No allow-list was enforced; later scoring penalizes any extra file.
```

Expected shape: distinguish surfaced drift from an undisclosed scoring boundary. Do not collapse disclosure, enforcement, and quality scoring into one scope label.

## Design Triage

**Moment:** A proposal adds a cache and invalidation service.

```text
Use simplify. The renter offered is “faster,” but no named latency envelope
or measured repeated computation exists. The established direct query already
meets the current product job.
```

Expected shape: stop and return to the named ask. Complexity has no demonstrated renter. This does not prove deletion safe if an unexamined invariant depends on it.
