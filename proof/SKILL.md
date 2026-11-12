---
name: proof
description: Use this skill when green tests, CI, coverage, or another check are offered as proof of a specific behavior. Decide whether that check is valid evidence for the named claim on this revision by testing falsifiability, independently grounded oracle authority, method/seam fitness, hermetic instrument validity, and current execution. Use for one claim/check pair; use Fix My Tests for repo-wide verification portfolio design.
metadata:
  author: Gaurav Albal
  version: "0.4"
---

# Test the Test

Local extension: if `references/LOCAL_EXTENSION.md` exists beside this file, load it after this file. An extension may add local observations, owners, or stricter local evidence requirements; it cannot change this skill's dispositions or advertised job.

## Moment

Someone offers green / “tests passed” / coverage as the reason a behavior is established.

## Proposition

Does this specific check, on this revision, provide valid evidence for the named behavior?

## Minimum evidence

- claimed behavior in one sentence;
- the test/check;
- expected truth/oracle and its claim-relative independent basis;
- the mechanism or seam the claim depends on;
- instrument validity, including ownership of dependencies, time, randomness, and mutable state relevant to this claim;
- log or receipt showing the check ran on this revision.

## Decision contract

```yaml
skill: proof
requires: [claimed_behavior, check, independently_grounded_oracle, claim_mechanism_or_seam, instrument_validity, current_run_receipt]
decision:
  - when: current execution/provenance, the claimed behavior, the check identity,
          oracle authority/independence, instrument validity, or the mechanism/seam
          the claim depends on cannot be established (including an invalid instrument)
    then: UNKNOWN
  - when: the exact check can remain green when the asserted expectation
          for the behavior it actually exercises is violated
    then: HOLLOW
  - when: the relevant behavior is exercised but judged against the wrong expected truth
    then: WRONG_ORACLE
  - when: the exact check is falsifiable for its exercised proposition,
          but its method/seam cannot expose absence of the named behavior
    then: WRONG_METHOD
  - when: the check would fail when the named behavior is absent,
          the oracle is the right independently grounded authority for this claim,
          the method/seam can expose the relevant failure,
          the instrument is valid and hermetic for the relevant dependencies/time/randomness/state,
          and the receipt is current
    then: SUPPORTS_CLAIM
non_claims:
  - SUPPORTS_CLAIM does not certify the whole product, task, or test portfolio
  - SUPPORTS_CLAIM does not establish that every material risk has evidence
stop: emit one disposition for this claim/check pair
```

## Procedure

1. **Name the behavior.** One sentence. If the operator did not name **which check**, establish it from the repo this sitting or `UNKNOWN` — do not pick a green test to bless the claim.
2. **Name the exercised proposition and its opposite.**
3. **Test check falsifiability.** Would this exact check still pass when its asserted expectation for the exercised proposition is violated? If yes: `HOLLOW`.
4. **Test the oracle.** Wrong expected truth: `WRONG_ORACLE`. Expected values copied from the same implementation under test (common-mode) are `WRONG_ORACLE` for a correctness claim. An explicit requirement, independently derived relation/reference, or fixed accepted baseline for a named preservation claim can ground the oracle. Candidate-aware inspection is legitimate, but does not itself form an independent oracle; another model's agreement is not acceptance.
5. **Test the method/seam.** Falsifiable for a proxy that cannot expose absence of the named behavior: `WRONG_METHOD`.
6. **Check instrument and currentness.** Establish attributable execution: the check owns or controls relevant dependencies, time, randomness and mutable namespaces/state; it must not pass through uncontrolled shared tenants, lucky timing, skip-as-green, or green retries. Invalid/unestablished instrument → `UNKNOWN`, with the reason. A supplementary live observation can inform the claim but cannot substitute for required hermetic real-seam evidence. Bind the run to this candidate, check identity and environment.

These are minimum conditions for this pair, not a full FMT sitting. A hermetic unit/property check can support a pure claim; no external integration is required when no real seam belongs to the claim.

## Exit

- `SUPPORTS_CLAIM`
- `HOLLOW`
- `WRONG_ORACLE`
- `WRONG_METHOD`
- `UNKNOWN`

Optional output annotation, not a disposition and not part of the decision contract:

`evidence_depth`: `ASSERTED` | `SOURCE_POINTER` | `FAILURE_TRACED_IMPOSSIBLE` | `EXECUTABLE_REAL_SEAM` | `RUNNING_PRODUCT_REPRODUCTION`

Emit it only to say how far the offered check reached. It is descriptive. It is not a score, not evidence authority, and it never changes the disposition. A deeper annotation cannot rescue a hollow check, a wrong oracle, a wrong method/seam, an invalid/unestablished instrument, or a non-current receipt. A shallower annotation cannot demote a check that already satisfies the decision contract.

## Gotchas

- A unit test can be perfectly falsifiable and still be the wrong method for crash recovery, concurrency, or external compatibility.
- `HOLLOW` tests whether the exact check has a real failure knife edge for what it exercises. `WRONG_METHOD` tests whether that exercised proposition and method can expose absence of the named behavior.
- A mocked persistence test does not establish crash durability.
- A sleeping thread test does not establish a concurrency invariant.
- An internal fake alone does not establish an external protocol's compatibility.
- Green color, test count, and coverage percentage are not substitutes for the claim/check proposition.
- Expected values derived from the same implementation under test are not an independent oracle (`WRONG_ORACLE` / common-mode). Preservation baselines are a different claim.
- `evidence_depth` is an optional label on the same claim/check pair. It does not create a sixth disposition and cannot override any evidence-eligibility condition.

## Why this shape

- The opposite outcome is the first knife edge between evidence and reassuring green.
- Method is separate from falsifiability.
- Oracle and current execution are separate authorities.
- The positive exit is deliberately narrow: this check supports this claim on this revision, nothing broader.
