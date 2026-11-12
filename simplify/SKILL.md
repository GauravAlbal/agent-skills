---
name: simplify
description: Use this skill when a coding-agent task is about to add a layer, abstraction, service, state machine, cache, or requirement whose necessity is unclear. Triage the design by naming the renter/failure, testing whether deletion preserves the job and invariants, preferring a boring established pattern, and requiring surviving complexity to pay explicit rent. Stop if the question becomes a real architecture exam, including when independent same-shape implementation deviations or workarounds keep appearing.
metadata:
  author: Gaurav Albal
  version: "0.3"
---

# Design Triage

## Moment

The session is about to add a layer, abstraction, service, state, or requirement whose rent is not yet obvious.

## Proposition

What is the simplest surviving design that still satisfies the named job and its real invariants?

## Minimum evidence

- named ask;
- proposed complexity;
- requirement/invariant it claims to serve;
- current design.

## Decision contract

```yaml
skill: simplify
requires: [named_ask, proposed_complexity, claimed_requirement]
decision:
  - when: the claimed renter/failure or required invariants cannot be established
    then: UNKNOWN
  - when: removing the proposed complexity preserves the named job and required invariants
    then: DELETE
  - when: a boring established pattern satisfies the surviving requirement
    then: BORING
  - when: surviving complexity has a named renter,
          protects a named invariant/failure mode,
          and cannot be collapsed into the boring pattern without losing it
    then: RENT
  - when: deciding requires a broad architecture exam with multiple material designs
    then: STOP
  - when: repeated independent same-shape implementation deviations or workarounds are observed
    then: STOP
non_claims:
  - does not prove the chosen design is globally optimal
  - does not replace a dedicated architecture review when multiple material architectures remain
stop: return to the named ask after one disposition
```

## Procedure

1. **Question the requirement.** Who needs it? What observable failure occurs without it? If unavailable: `UNKNOWN`.
2. **Try deletion before optimization.** Remove the requirement/layer in thought or code shape. If the job and invariants still hold: `DELETE`.
3. **Prefer the boring pattern.** If an established simple topology satisfies the surviving invariant: `BORING`.
4. **Split only for a reason.** A separate component should have a distinct failure mode, lifecycle, authority, or scaling boundary—not merely a new noun.
5. **Charge rent to survivors.** Name the invariant/failure each remaining piece protects. If simpler structure would lose it: `RENT`.
6. **Stop same-shape repair churn.** Repeated independent same-shape deviations or workarounds: `STOP` (same exit as a broad architecture exam). Do not bolt more local repairs. With that `STOP`, fill this handoff and nothing else:

```yaml
what_failed:
why_possible:
what_makes_impossible_or_why_guidance_remains:
how_we_would_know:
what_can_disappear:
```

This is a handoff, not a sixth disposition and not a supervisor skill. Deletion counterfactuals may be thought experiments; do not require a code edit to choose `DELETE`.

## Exit

- `DELETE`
- `BORING`
- `RENT`
- `UNKNOWN`
- `STOP`

## Gotchas

- “Nobody named the renter” is a reason to investigate, not enough evidence to delete.
- Deletion requires a counterfactual.
- “Boring” means a proven/simple topology satisfies the actual invariant; it is not aesthetic conservatism.
- If multiple material architectures survive, or independent repairs keep taking the same shape, use `STOP`. Do not turn this into a sprawling design review, and do not continue repair churn. The five answers are a handoff, not a new Kit job.

## Why this shape

- Deletion comes before optimization because optimization can legitimize a requirement that should not exist.
- Surviving complexity must protect a named property.
- The stop rule preserves the boundary between local triage and a real design exam.
