---
name: done
description: Use this skill when a coding-agent task is reported done, completed, ready, or handled and you need to decide whether every requirement due now was actually delivered or is still explicitly left open. Reconcile the original ask against current artifacts and current leftovers. Do not use it to judge implementation quality or whether offered tests are valid proof.
metadata:
  author: Gaurav Albal
  version: "0.1"
---

# Done Reconciliation

## Moment

It says done / completed / leftover handled.

## Proposition

Did the current result satisfy every requirement that was due now, or explicitly leave it as still-open work?

## Minimum evidence

- original ask;
- current artifacts or diff;
- current leftover list or dated pointers;
- recap only as secondary evidence, never as the obligation source.

## Decision contract

```yaml
skill: done
requires: [original_ask, current_artifacts, current_leftovers]
decision:
  - when: required_now or current artifact state cannot be established
    then: UNKNOWN
  - when: any required_now item is neither delivered nor currently explicitly_left
    then: NOT_DONE
  - when: all required_now items are delivered or currently explicitly_left
          and at least one required_now item is currently explicitly_left
    then: EXPLICITLY_PARTIAL
  - when: all required_now items are delivered
    then: DONE
non_claims:
  - does not certify implementation quality
  - does not certify that offered proof is valid
stop: emit one disposition
```

## Procedure

1. From the **original ask**, write `required_now`. Do not derive obligations from the recap.
2. Write `delivered`: what exists now.
3. Write `explicitly_left`: required work with a **current** dated/not-this-sitting pointer.
4. Walk the decision contract once and stop.

A vanished leftover is not LEFT. “Handled” in the recap is not LEFT. Removing an obligation from the list cannot improve the verdict.

## Exit

- `DONE`
- `EXPLICITLY_PARTIAL`
- `NOT_DONE`
- `UNKNOWN`

## Gotchas

- A non-empty diff can still be `NOT_DONE` if it misses a required obligation.
- A recap cannot create or erase obligations.
- Completion and proof are different propositions. If the next question is whether green evidence supports a claim, use `proof`.

## Why this shape

- Obligation reconciliation comes first because a non-empty artifact can still be the wrong or incomplete job.
- LEFT must be current and explicit so disappearing work cannot masquerade as progress.
- Quality is deliberately excluded. One skill, one decision.
