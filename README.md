# The Kit

**Five sharp engineering skills for coding agents.**

No mega-prompt. No giant workflow. Five independent decision tools for recurring engineering moments. Each reads concrete evidence, emits one typed disposition, states what it does **not** establish, and stops.

[![Release](https://img.shields.io/github/v/release/GauravAlbal/agent-skills?label=release)](https://github.com/GauravAlbal/agent-skills/releases)
[![License: CC BY 4.0](https://img.shields.io/badge/license-CC%20BY%204.0-2f6f9f)](LICENSE.md)

[Install](#install) · [Try an example](#try-one-in-three-minutes) · [Why The Kit](docs/WHY_THE_KIT.md)

| When this happens | Use | The bounded question |
| --- | --- | --- |
| The agent says the job is complete | [**Done Reconciliation**](done/SKILL.md) (`done`) | Did it actually finish every requirement due now? |
| Another retry looks tempting | [**DDx Lite**](ddx/SKILL.md) (`ddx`; differential diagnosis) | What failed before you spend another attempt? |
| Green checks are offered as proof | [**Test the Test**](proof/SKILL.md) (`proof`) | Does this check prove this claim on this revision? |
| An extra surface appears | [**Scope Check**](scope/SKILL.md) (`scope`) | Is the change inside the disclosed ask? |
| A new layer wants to exist | [**Design Triage**](simplify/SKILL.md) (`simplify`) | Does this complexity earn its keep? |

```text
skill ≠ gate
guidance ≠ evidence
evidence ≠ acceptance
```

## Typed decisions, not vibes

The result is not “looks good.” Each skill has a small terminal vocabulary.

| Skill | Terminal dispositions |
| --- | --- |
| `done` | `DONE` · `EXPLICITLY_PARTIAL` · `NOT_DONE` · `UNKNOWN` |
| `ddx` | `INSTRUMENT` · `ARCHITECTURE` · `IMPLEMENTATION` · `NEW_INFO` · `SAME_PROMPT_REFUSE` · `STOP` |
| `proof` | `SUPPORTS_CLAIM` · `HOLLOW` · `WRONG_ORACLE` · `WRONG_METHOD` · `UNKNOWN` |
| `scope` | `IN_ASK` · `DRIFT_SURFACED` · `SILENT_EXPAND` · `UNDISCLOSED_SCORE` · `UNKNOWN` |
| `simplify` | `DELETE` · `BORING` · `RENT` · `UNKNOWN` · `STOP` |

`UNKNOWN` is an honest result. Missing evidence does not become approval.

## The five tools

### Done Reconciliation · `done`

**Moment:** A task is reported done, complete, ready, or handled.

**Decides:** Whether every requirement due now is delivered or still explicitly left open. It reconciles the original ask against current artifacts and current leftovers; it does not judge implementation quality.

**Exits:** `DONE` · `EXPLICITLY_PARTIAL` · `NOT_DONE` · `UNKNOWN`

**Tiny case:** A recap says a required CLI document was “handled,” but no document exists and no current leftover records it. → `NOT_DONE`

[Full decision contract](done/SKILL.md) · [Three-minute example](examples/done.md)

### DDx Lite · `ddx`

**Moment:** Something failed or stalled, and the next move would be another attempt.

**Decides:** Which cause class deserves the next unit of information. It precommits one cheap discriminator before choosing instrument, architecture, implementation, or genuinely new evidence; it does not auto-repair.

**Exits:** `INSTRUMENT` · `ARCHITECTURE` · `IMPLEMENTATION` · `NEW_INFO` · `SAME_PROMPT_REFUSE` · `STOP`

**Tiny case:** The requested mutation exists in the target worktree, while the checker log shows it evaluated a different worktree’s untracked file. → `INSTRUMENT`

[Full decision contract](ddx/SKILL.md) · [Three-minute example](examples/ddx.md)

### Test the Test · `proof`

**Moment:** A green test, CI run, coverage number, or check is offered as proof.

**Decides:** Whether one current check is valid evidence for one named behavior. It tests falsifiability, oracle authority, method or seam fitness, and execution provenance; it does not certify the product or test portfolio.

**Exits:** `SUPPORTS_CLAIM` · `HOLLOW` · `WRONG_ORACLE` · `WRONG_METHOD` · `UNKNOWN`

**Tiny case:** A current test can falsify an in-memory recovery contract, but it uses clean shutdown and a fake store to support a real crash-durability claim. → `WRONG_METHOD`

[Full decision contract](proof/SKILL.md) · [Three-minute example](examples/proof.md)

### Scope Check · `scope`

**Moment:** An extra surface appears, or someone is about to score work as out of scope.

**Decides:** Whether the surface is in the named ask, honestly surfaced drift, silent expansion, or judged against an undisclosed boundary. It does not decide whether the work is useful or complete.

**Exits:** `IN_ASK` · `DRIFT_SURFACED` · `SILENT_EXPAND` · `UNDISCLOSED_SCORE` · `UNKNOWN`

**Tiny case:** The ask requires code and docs, but an evaluator rejects the docs change using a path allow-list the agent was never shown. → `UNDISCLOSED_SCORE`

[Full decision contract](scope/SKILL.md) · [Three-minute example](examples/scope.md)

### Design Triage · `simplify`

**Moment:** A layer, abstraction, service, state machine, cache, or new requirement is about to be added.

**Decides:** The simplest surviving design that still satisfies the named job and real invariants. It tries deletion first, then a boring established pattern, then charges explicit rent to complexity that survives; it stops before a broad architecture exam.

**Exits:** `DELETE` · `BORING` · `RENT` · `UNKNOWN` · `STOP`

**Tiny case:** Removing a proposed validation service leaves the named job and every required invariant enforced by the existing typed boundary. → `DELETE`

[Full decision contract](simplify/SKILL.md) · [Three-minute example](examples/simplify.md)

## Try one in three minutes

Each example includes the evidence the skill requires and reaches one disposition without hidden facts.

- [A vanished completion obligation → `NOT_DONE`](examples/done.md)
- [A checker judging the wrong worktree → `INSTRUMENT`](examples/ddx.md)
- [A mock offered as crash evidence → `WRONG_METHOD`](examples/proof.md)
- [A hidden scoring boundary → `UNDISCLOSED_SCORE`](examples/scope.md)
- [A removable validation service → `DELETE`](examples/simplify.md)

One skill, one example, one decision.

## Install

Every tool uses the open Agent Skills directory shape:

```text
<skills-root>/
  <skill-name>/
    SKILL.md
```

Clone The Kit once:

```sh
git clone https://github.com/GauravAlbal/agent-skills.git the-kit
cd the-kit
```

### Generic Agent Skills path

For clients that discover user skills under `~/.agents/skills/`:

```sh
mkdir -p "$HOME/.agents/skills"
for skill in done ddx proof scope simplify; do
  ln -s "$PWD/$skill" "$HOME/.agents/skills/$skill"
done
```

Install only one when that is all you need:

```sh
ln -s "$PWD/proof" "$HOME/.agents/skills/proof"
```

### OMP (Oh My Pi)

OMP discovers user skills under `~/.omp/agent/skills/`:

```sh
mkdir -p "$HOME/.omp/agent/skills"
for skill in done ddx proof scope simplify; do
  ln -s "$PWD/$skill" "$HOME/.omp/agent/skills/$skill"
done
```

Restart the client after installation so it reloads skill metadata. Keep the checkout in place while using symlinks. No adapter or build step is required.

## When the question becomes repository-wide

`proof` owns one claim/check pair:

```text
Does this check prove this named behavior on this revision?
```

[Fix My Tests](https://github.com/GauravAlbal/fix-my-tests) owns a different question:

```text
Does this repository protect the product risks that matter with the right evidence?
```

Use Fix My Tests when the unit of analysis expands from one offered check to the repository-wide risk and evidence portfolio. It is a separate free product, not a sixth skill and not bundled here.

## Why these exist

These skills were extracted from decision procedures used while operating coding agents on real repositories. That provenance supports their shape, not claims about customer validation or effectiveness.

| Recurring failure | What the bounded tool exposed |
| --- | --- |
| A green test survived removal of the event named in its own title | `proof` → `HOLLOW` |
| The requested change existed, but the checker judged the wrong surface | `ddx` → `INSTRUMENT` |
| A proposed validation service added no required capability | `simplify` → `DELETE` |

Read [Why The Kit](docs/WHY_THE_KIT.md) for the design argument.

## Product boundary

- Skills are guidance. They do not enforce agent behavior.
- A disposition is not evidence unless the named evidence exists.
- A skill does not approve a merge, certify safety, or replace acceptance controls.
- The tools are independent. Do not chain all five by default.
- The collection contains exactly `done`, `ddx`, `proof`, `scope`, and `simplify`.

## Repository map

```text
README.md
LICENSE.md
done/SKILL.md
ddx/SKILL.md
proof/SKILL.md
scope/SKILL.md
simplify/SKILL.md
examples/{done,ddx,proof,scope,simplify}.md
docs/WHY_THE_KIT.md
```

## License

The skill documents and public explanatory material are available under [Creative Commons Attribution 4.0 International](LICENSE.md).