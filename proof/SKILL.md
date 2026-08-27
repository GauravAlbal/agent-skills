---
name: proof
description: Use this skill when green tests, CI, coverage, or another check are offered as proof of a specific behavior. Decide whether that check is valid evidence for the named claim on this revision by testing falsifiability, oracle authority, method/seam fitness, and current execution. Use for one claim/check pair; use Fix My Tests for repo-wide verification portfolio design.
metadata:
  author: Gaurav Albal
  version: "0.2"
---

# Test the Test

## Moment

Someone offers green / “tests passed” / coverage as the reason a behavior is established.

## Proposition

Does this specific check, on this revision, provide valid evidence for the named behavior?

## Minimum evidence

- claimed behavior in one sentence;
- the test/check;
- expected truth/oracle;
- the mechanism or seam the claim depends on;
- log or receipt showing the check ran on this revision.

## Decision contract

```yaml
skill: proof
requires: [claimed_behavior, check, oracle, claim_mechanism_or_seam, current_run_receipt]
decision:
  - when: current execution/provenance, the claimed behavior, oracle authority,
          or the mechanism/seam the claim depends on cannot be established
    then: UNKNOWN
  - when: the check can remain green while the claimed behavior is absent
    then: HOLLOW
  - when: the relevant behavior is exercised but judged against the wrong expected truth
    then: WRONG_ORACLE
  - when: the check is falsifiable but its method/seam cannot expose absence of this kind of behavior
    then: WRONG_METHOD
  - when: the check would fail when the named behavior is absent,
          the oracle is the right authority,
          the method/seam can expose the relevant failure,
          and the receipt is current
    then: SUPPORTS_CLAIM
non_claims:
  - SUPPORTS_CLAIM does not certify the whole product, task, or test portfolio
  - SUPPORTS_CLAIM does not establish that every material risk has evidence
stop: emit one disposition for this claim/check pair
```

## Procedure

1. **Name the behavior.** One sentence.
2. **Name the opposite outcome.** What observable result means the behavior is absent or wrong?
3. **Test falsifiability.** Would this exact check still pass under that opposite outcome? If yes: `HOLLOW`.
4. **Test the oracle.** If the behavior runs but the expected truth/authority is wrong: `WRONG_ORACLE`.
5. **Test the method/seam.** Can this kind of check expose the relevant failure? If not: `WRONG_METHOD`.
6. **Check currentness.** Wrong revision, missing run, or skip-as-green cannot become proof.

## Exit

- `SUPPORTS_CLAIM`
- `HOLLOW`
- `WRONG_ORACLE`
- `WRONG_METHOD`
- `UNKNOWN`

Optional output annotation, not a disposition and not part of the decision contract:

`evidence_depth`: `ASSERTED` | `SOURCE_POINTER` | `FAILURE_TRACED_IMPOSSIBLE` | `EXECUTABLE_REAL_SEAM` | `RUNNING_PRODUCT_REPRODUCTION`

Emit it only to say how far the offered check reached. It is descriptive. It is not a score, not evidence authority, and it never changes the disposition. A deeper annotation cannot rescue a hollow check, a wrong oracle, a wrong method/seam, or a non-current receipt. A shallower annotation cannot demote a check that already satisfies the decision contract.

## Gotchas

- A unit test can be perfectly falsifiable and still be the wrong method for crash recovery, concurrency, or external compatibility.
- A mocked persistence test does not establish crash durability.
- A sleeping thread test does not establish a concurrency invariant.
- An internal fake alone does not establish an external protocol's compatibility.
- Green color, test count, and coverage percentage are not substitutes for the claim/check proposition.
- `evidence_depth` is an optional label on the same claim/check pair. It does not create a sixth disposition and cannot override falsifiability, oracle, method/seam, or currentness.

## Why this shape

- The opposite outcome is the first knife edge between evidence and reassuring green.
- Method is separate from falsifiability.
- Oracle and current execution are separate authorities.
- The positive exit is deliberately narrow: this check supports this claim on this revision, nothing broader.
