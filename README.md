# The Kit

**Five sharp engineering skills for coding agents.**

No mega-prompt. No giant workflow. Each skill handles one recurring engineering problem, returns one typed decision, and stops.

[![Release](https://img.shields.io/github/v/release/GauravAlbal/agent-skills?label=release)](https://github.com/GauravAlbal/agent-skills/releases)
[![License: CC BY 4.0](https://img.shields.io/badge/license-CC%20BY%204.0-2f6f9f)](LICENSE.md)

| When this happens | Use | Question |
| --- | --- | --- |
| The agent says the job is done | [`done`](done/SKILL.md) | Did it actually finish every requirement due now? |
| Another retry looks tempting | [`ddx`](ddx/SKILL.md) | What failed before you spend another attempt? |
| Green tests are offered as proof | [`proof`](proof/SKILL.md) | Does this check actually prove the claim? |
| The agent changed something extra | [`scope`](scope/SKILL.md) | Is the change inside the disclosed ask? |
| A new layer or abstraction wants to exist | [`simplify`](simplify/SKILL.md) | Does this complexity earn its keep? |

```text
skill ≠ gate
guidance ≠ evidence
evidence ≠ acceptance
```

## Install

Clone the repository:

```sh
git clone https://github.com/GauravAlbal/agent-skills.git the-kit
cd the-kit
```

For Agent Skills-compatible clients that discover user skills under `~/.agents/skills/`:

```sh
mkdir -p "$HOME/.agents/skills"
for skill in done ddx proof scope simplify; do
  ln -s "$PWD/$skill" "$HOME/.agents/skills/"
done
```

Install only the skill you want:

```sh
ln -s "$PWD/proof" "$HOME/.agents/skills/"
```

For OMP (Oh My Pi), use `~/.omp/agent/skills/`:

```sh
mkdir -p "$HOME/.omp/agent/skills"
for skill in done ddx proof scope simplify; do
  ln -s "$PWD/$skill" "$HOME/.omp/agent/skills/"
done
```

Restart the client after installation so it reloads skill metadata. Existing destinations are not overwritten.

## Pick the problem you have

### `done` — the agent says it finished

**Done Reconciliation** compares the original ask with what actually exists now and what is still explicitly left open.

A recap says a required CLI document was “handled,” but no document exists and no current leftover records it.

→ `NOT_DONE`

Other exits: `DONE` · `EXPLICITLY_PARTIAL` · `UNKNOWN`

[Skill](done/SKILL.md) · [Three-minute example](examples/done.md)

### `ddx` — you are about to retry

**DDx Lite** separates a bad implementation from a bad checker, a shared architecture problem, or genuinely new information before you burn another attempt.

The requested mutation exists in the target worktree. The checker failed because it inspected a different worktree's untracked file.

→ `INSTRUMENT`

Other exits: `ARCHITECTURE` · `IMPLEMENTATION` · `NEW_INFO` · `SAME_PROMPT_REFUSE` · `STOP`

[Skill](ddx/SKILL.md) · [Three-minute example](examples/ddx.md)

### `proof` — someone says “the tests pass”

**Test the Test** asks whether one current check is valid evidence for one named behavior. A test can be real and falsifiable and still be the wrong kind of test for the failure you care about.

A state-machine unit test exercises an in-memory crash model but never kills a process or exercises filesystem ordering. It is offered as proof of crash durability.

→ `WRONG_METHOD`

Other exits: `SUPPORTS_CLAIM` · `HOLLOW` · `WRONG_ORACLE` · `UNKNOWN`

[Skill](proof/SKILL.md) · [Three-minute example](examples/proof.md)

### `scope` — the agent changed something extra

**Scope Check** distinguishes work that belongs to the ask from surfaced drift, silent expansion, and rules the agent was never shown.

An evaluator rejects a docs change using a path allow-list that was never disclosed to the agent.

→ `UNDISCLOSED_SCORE`

Other exits: `IN_ASK` · `DRIFT_SURFACED` · `SILENT_EXPAND` · `UNKNOWN`

[Skill](scope/SKILL.md) · [Three-minute example](examples/scope.md)

### `simplify` — the design is growing another noun

**Design Triage** tries deletion first, then a boring established pattern, then asks surviving complexity to name the invariant or failure mode it protects.

Removing a proposed validation service leaves the named job and every required invariant enforced by the existing boundary.

→ `DELETE`

Other exits: `BORING` · `RENT` · `UNKNOWN` · `STOP`

[Skill](simplify/SKILL.md) · [Three-minute example](examples/simplify.md)

## Why five small skills?

Coding agents fail in different ways at different moments. A single giant workflow has to guess which problem you have and tends to drag unrelated ceremony into the answer.

The Kit takes the opposite approach: one skill, one decision.

Each skill:

- uses concrete evidence such as the ask, diff, test, log, or current pointer;
- has a small set of explicit outcomes instead of “looks good” prose;
- can return `UNKNOWN` when the evidence is not there;
- says what its result does **not** establish;
- stops before it turns into the next engineering job.

Read [Why The Kit](docs/WHY_THE_KIT.md) for the longer design argument.

## When one test becomes the whole test suite

`proof` answers:

```text
Does this check prove this behavior on this revision?
```

If the question becomes:

```text
Does this repository protect the risks that matter with the right tests?
```

use [Fix My Tests](https://github.com/GauravAlbal/fix-my-tests). It is a separate free tool, not a sixth skill.

## Limits

These skills improve engineering judgment. They do not enforce agent behavior, manufacture evidence, approve merges, or certify software as safe.

Use them independently. Do not chain all five by default.

## License

CC BY 4.0. See [LICENSE.md](LICENSE.md).
