# Why The Kit

Coding-agent failures often arrive as ordinary sentences:

- “It says done.”
- “The test is green.”
- “Try the repair again.”
- “That file is out of scope.”
- “We need another service.”

Those moments look small. Each hides a distinct engineering decision with a different subject, evidence burden, and terminal vocabulary. Treating them as one workflow blurs authority and encourages ceremony. The Kit keeps them separate.

## One skill, one proposition

Each tool owns one question:

| Tool | Proposition |
| --- | --- |
| Done Reconciliation | Were all requirements due now delivered or explicitly left open? |
| DDx Lite | Which cause class should receive the next unit of information? |
| Test the Test | Does one current check support one named claim? |
| Scope Check | Is the scored surface inside the disclosed ask? |
| Design Triage | Does the proposed complexity protect a real invariant that simpler structure cannot? |

A tool exits after one typed disposition. It does not become a workflow engine merely because another question could follow.

## Why typed dispositions matter

“Looks reasonable” is hard to challenge and easy to reinterpret. `HOLLOW`, `INSTRUMENT`, `UNDISCLOSED_SCORE`, and `DELETE` make the decision legible. Each result has a branch condition and minimum evidence.

`UNKNOWN` is equally important. If the original ask, current execution receipt, claimed subject, or protected invariant is unavailable, the tool does not manufacture certainty from prose.

## Why the boundaries matter

The five propositions touch nearby concerns without owning them:

- `done` reconciles obligations; it does not judge implementation quality.
- `ddx` classifies the red; it does not auto-repair.
- `proof` judges one claim/check pair; it does not certify a repository.
- `scope` classifies disclosure; it does not decide usefulness.
- `simplify` triages local complexity; it stops before a broad architecture review.

When the proof question becomes “does this repository protect the product risks that matter?”, use the separate [Fix My Tests](https://github.com/GauravAlbal/fix-my-tests) instrument. Repository-wide portfolio design is not a sixth Kit skill.

## Guidance is not authority

```text
skill ≠ gate
guidance ≠ evidence
evidence ≠ acceptance
```

A skill may identify the right evidence to inspect. It does not create that evidence. A disposition may expose a missing obligation or invalid proof method. It does not approve a merge, certify safety, or enforce runtime behavior.

That separation lets the skills remain portable. They can improve a coding agent's judgment without pretending to be the repository's acceptance mechanism.

## Extracted from operating practice

The procedures were compressed from recurring engineering cases: green checks that could not observe the claimed failure, retries aimed at the wrong surface, completion recaps that hid vanished obligations, and abstractions whose deletion changed nothing the user needed.

This is provenance, not market proof. The collection makes no claim that operating use establishes effectiveness, broad adoption, or customer demand.

## Why exactly five

The Kit is a product boundary, not a holding pen. New material belongs here only when it owns a recurring, independent engineering proposition that none of the five can answer without losing its boundary. More specialized investigations and repository-wide instruments should remain separate.

Small tools stay useful when their stop lines stay sharp.
