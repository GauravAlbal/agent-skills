# Design Triage example: removable validation service

## Moment

A change proposes a new `ValidationGateway` service before jobs enter an existing queue.

## Named job

> Reject malformed jobs before enqueue while preserving valid-job throughput and the queue's single-writer ordering.

## Current topology

```text
request body
  -> parser returns ValidatedJob or ParseError
  -> queue accepts ValidatedJob only
  -> single writer appends the job
```

The parser already owns schema validation. The queue API cannot accept the unvalidated wire type.

## Proposed topology

```text
request body
  -> parser
  -> ValidationGateway
  -> queue
  -> single writer
```

The proposed service repeats the parser's checks and adds a deployment, network failure, and second error vocabulary.

## Deletion counterfactual

Remove `ValidationGateway`, then exercise the named job:

- malformed body → parser returns `ParseError`; queue call is impossible;
- valid body → parser returns `ValidatedJob`; queue accepts it;
- concurrent valid jobs → the existing single writer preserves ordering.

All named behavior and invariants survive deletion. No distinct authority, lifecycle, or scaling boundary remains.

```yaml
skill: simplify
requirement: reject malformed jobs before enqueue
proposed_component: ValidationGateway
deletion_preserves_job: true
deletion_preserves_invariants:
  - typed queue boundary
  - valid-job throughput
  - single-writer ordering
decision: DELETE
replacement: keep parser -> typed queue topology
```

## Disposition

`DELETE`

The new service protects no property that the existing typed boundary does not already protect.
