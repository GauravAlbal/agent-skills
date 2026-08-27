# Why The Kit

Coding-agent failures often arrive as ordinary sentences:

- “It says done.”
- “The test is green.”
- “Try the repair again.”
- “That file is out of scope.”
- “We need another service.”

Those moments look small. Each hides a different engineering decision.

The Kit exists because I kept paying for the same mistakes across different projects. I started writing down the rules that survived those mistakes, then compressed the recurring ones into small tools I could reach for without dragging in an entire workflow.

These are not five prompts I brainstormed for a collection. The underlying rules came first.

## The source material was larger than the skills

The first cross-project consolidation pulled recurring engineering principles from roughly **70 design documents across seven project families**. Later work added session-history mining, incident writeups, controlled experiments, and new cases from day-to-day agent operation.

The skill files are the compressed end of that process:

```text
recurring failure
→ rule worth keeping
→ sharpen the evidence boundary
→ reduce it to one decision
→ test routing and branches
→ use it on real work
→ revise only when the evidence earns a change
```

The packaged skills are newer than the disciplines behind them. I do **not** claim that each `SKILL.md` has hundreds of direct invocations. The first formal Kit dogfood campaign recorded 10 fresh uses in one clustered sitting: four changed the intended engineering action and six confirmed it; none exposed a skill-body defect that earned a revision. The standing ledger remains open for natural uses rather than manufacturing a bigger count.

What follows is the more useful history: why each tool exists at all.

## `done`: completion claims kept outrunning the work

The recurring failure was simple: the agent would report completion while a requirement had quietly disappeared, been stubbed, or been reclassified as “for later” without a durable record.

This happened often enough that one of my larger codebases ended up with a permanent **Hollow Implementation Scan** looking for things such as `todo!()`, `unimplemented!()`, bare FIXMEs, empty match arms, and default values standing in for real logic. That tooling tax exists because silent partial delivery repeatedly looked like done work.

The later `done` skill reduces the problem to obligation reconciliation: start from the original ask, compare it with what exists now and what is still explicitly left, then stop.

In live Kit dogfood it caught a setup that was about to be treated as complete even though the required standing ledger did not exist yet. On another program it refused `DONE` and returned `EXPLICITLY_PARTIAL` because real leftovers were still open.

The lesson was not “agents lie.” It was narrower: **the final recap is a bad source of obligations.**

## `ddx`: retries are expensive when you have not classified the failure

DDx predates The Kit as a larger diagnostic protocol for situations where several explanations fit the same symptoms.

One early controlled investigation started with a system performing at **50% vs 83%** on a comparison condition. The diagnostic split competing causes, precommitted a differentiating test, and chose between two interventions before seeing the results. Both hit 80% on the small test, but one regressed a control. The precommitted tree selected the other; the subsequent 30-task run confirmed a **19-point accuracy lift**.

The Kit version is intentionally much smaller. During ordinary engineering I usually do not need a two-hour study. I need to know whether the next attempt should spend information on the worker, the architecture, or the instrument.

That exact distinction mattered in later dogfood: workers had already made the requested mutation, but the checker was evaluating unrelated worktree state. `ddx` returned `INSTRUMENT`, changing the next action from another worker reroll to fixing the judge.

The compressed lesson: **before retrying the worker, establish that the worker is what failed.**

## `proof`: we spent five weeks trusting a green bar that could not protect us

The cleanest origin story for `proof` is a supervisor test suite that shipped with 310 lines of spy-style tests. The mocks always succeeded. The assertions mostly checked that methods had been called.

Those tests stayed green for **37 days**. During that window, **28 unique agent sessions** across Codex, Claude Code, and Gemini touched the affected subsystem. The eventual repair rewrote the tests around behavioral outcomes and failure paths, including 31 higher-fidelity tests.

The important mistake was not “we needed more tests.” We already had tests. The problem was that the checks could not establish the claims we were using them for.

The same pattern appeared again after `proof` existed as a skill. A green portability test had a convincing name, but a new forbidden capability-specific branch could be added and the test would still pass. `proof` returned `HOLLOW`; the engineering action changed from trusting the test to relying on evidence that could actually falsify the claim.

That is why `proof` separates four things that are easy to blur together:

```text
can the check fail?
is the expected answer authoritative?
can this method observe this failure?
did it run on this revision?
```

A test can be perfectly real and still be the wrong evidence.

## `scope`: “while I’m here” was not a rare edge case

Scope had the strongest quantified recurrence before it became a skill.

A Lens analysis mined **180 days of agent sessions across nine projects**. It found **480 spec-drift hits across 48 work windows**, compared with 210 hidden-scope-cut hits across 81 windows. In that corpus, scope expansion showed up roughly twice as often as scope contraction.

The traces were familiar:

- “Now let me also…”
- “while we’re here…”
- “one more thing…”
- adjacent cleanup folded into a fix;
- read-only investigation turning into edits.

The key distinction that survived into the skill is that **disclosure, enforcement, and later scoring are different authorities**. If an evaluator penalizes an agent against a boundary the agent was never shown, that is an instrument defect before it is an agent-scope defect.

In live dogfood, `scope` also caught a genuinely unauthorized write during a read-only sitting and returned `SILENT_EXPAND`.

The lesson: noticing adjacent work is not license to absorb it into the current ask.

## `simplify`: we kept making unnecessary machinery better

`simplify` came from a more embarrassing failure class: doing competent engineering on something that should not exist.

In one SEAR durability effort, we spent **days** building, testing, debugging, and adversarially reviewing a bespoke reserve/submit and two-flag-join mechanism. The work was technically serious. The mistake happened earlier: we had started optimizing the mechanism before questioning the requirement and trying deletion.

When we finally did that in the right order, most of the apparatus disappeared in one pass. The same pattern had appeared in other repositories as well.

The skill therefore starts with the counterfactual, not aesthetics:

```text
What failure does this thing prevent?
What happens if we delete it?
Can a boring pattern satisfy what survives?
What invariant pays the rent for the remaining complexity?
```

In later Kit dogfood, the same procedure deleted a proposed validation-service layer because all named requirements survived without it.

The lesson is not “simple is better.” It is: **do not optimize a requirement or component before it has earned existence.**

## Why one skill, one proposition

These histories overlap, but the decisions do not.

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

### Where Fix My Tests fits

`proof` is a pocket tool: one check, one claim, one revision.

[Fix My Tests](https://github.com/GauravAlbal/fix-my-tests) is what I use when the test system itself becomes the problem. It works across a repository: discover the material risks, derive the evidence each failure needs, inspect the current suite, and decide what to keep, fix, add, delete, or move.

It belongs to the same family of tools. I keep it outside the five-skill Kit because the interaction shape is different: it is a repo-wide analysis session rather than a small decision you reach for in the middle of work.

## Guidance is not authority

```text
skill ≠ gate
guidance ≠ evidence
evidence ≠ acceptance
```

A skill may identify the right evidence to inspect. It does not create that evidence. A disposition may expose a missing obligation or invalid proof method. It does not approve a merge, certify safety, or enforce runtime behavior.

That separation lets the skills remain portable. They can improve a coding agent's judgment without pretending to be the repository's acceptance mechanism.

## Why exactly five

The Kit is intentionally small, not a holding pen for every useful prompt or analysis. A new everyday skill belongs here only when it owns a recurring engineering decision that none of the five can answer cleanly.

Deeper analysis can still belong in the same tool family without pretending it has the same interaction cost. Fix My Tests is the current example.

Small tools stay useful when their stop lines stay sharp.
