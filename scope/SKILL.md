---
name: scope
description: Use this skill when an agent changed an extra surface, someone says work is out of scope, or an eval/reviewer is about to score scope. Distinguish in-ask work, honestly surfaced drift, silent expansion, undisclosed scoring, and unknown by comparing the named ask, disclosed boundary, current diff, and current leftover pointers. Do not use it to decide whether required work is complete.
metadata:
  author: Gaurav Albal
  version: "0.1"
---

# Scope Check

## Moment

An extra surface appeared, or someone is about to score work as “out of scope.”

## Proposition

Is the extra surface part of the named ask, honestly surfaced as separate work, silently expanded, or being judged against an undisclosed boundary?

## Minimum evidence

- named ask;
- surfaces disclosed to the agent;
- surfaces actually enforced/scored;
- current diff;
- current leftover/snag pointers.

## Decision contract

```yaml
skill: scope
requires: [named_ask, disclosed_surfaces, current_diff, current_leftovers]
decision:
  - when: named ask, disclosed boundary, or current changed surfaces cannot be established
    then: UNKNOWN
  - when: quality/acceptance is scored against a surface or rule the agent was not shown
    then: UNDISCLOSED_SCORE
  - when: changed surface is outside the named ask
          and a current leftover/snag pointer explicitly records it as separate work
    then: DRIFT_SURFACED
  - when: changed surface is outside the named ask
          and no current leftover/snag pointer records it
    then: SILENT_EXPAND
  - when: changed surfaces are within the named ask and disclosed boundary
    then: IN_ASK
non_claims:
  - does not decide whether the extra work is good or useful
  - does not decide completion of vanished required work; that belongs to done
stop: emit one scope disposition; Record & Continue is an action, not another skill
```

## Procedure

1. **Name the ask.** What job was actually requested? If that cannot be recovered, stop at `UNKNOWN`.
2. **Separate three boundaries:** `disclosed` (agent saw it), `enforced` (mechanically constrained), `used_for_quality` (later judged). A quality boundary cannot honestly outrun disclosure.
3. **Compare the current diff to the named ask.** Do not use the recap as the diff.
4. **Check current leftovers.** If an extra is real but not this ask, a live pointer makes the drift explicit; record it and continue the named job.

## Exit

- `IN_ASK`
- `DRIFT_SURFACED`
- `SILENT_EXPAND`
- `UNDISCLOSED_SCORE`
- `UNKNOWN`

## Gotchas

- Disclosure, enforcement, and scoring are different authorities.
- Mentioning an extra in recap prose is not the same as a durable current pointer.
- Missing boundary evidence is not evidence that the work was in or out of scope.
- Vanished required work is a `done` problem, not a scope problem.

## Why this shape

- The current pointer cleanly distinguishes surfaced drift from silent expansion.
- Hidden scoring is an instrument/authority defect, not automatically agent drift.
- Scope is descriptive before evaluative: classify where the work belongs, then stop.
