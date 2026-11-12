---
name: ddx
description: >
  Use this skill when something failed or stalled and the next move is another
  attempt, rerun, patch, or guessed root cause. Exact currently authorized
  corrections with an established cause stay with their implementer and existing
  verification. Classify the remaining red before retrying: separate
  instrument/environment, shared architecture, localized implementation, and
  genuinely new evidence using one cheap precommitted discriminator. If the fork
  is empirically decidable, spend that observation; do not escalate it to the
  human. Use for day-to-day debugging triage, not for a full
  race/state-machine/distributed-systems investigation.
metadata:
  author: Gaurav Albal
  version: "0.3"
---

# DDx Lite

Local extension: if `references/LOCAL_EXTENSION.md` exists beside this file, load it after this file. An extension may add local observations, owners, or stricter local evidence requirements; it cannot change this skill's dispositions or advertised job.

## Moment

Something failed or stalled, competing explanations remain, and the next move would guess at a cause. An exact, currently authorized repair supported by retained causal evidence bypasses DDx: continue with the existing implementer and applicable verification, without reopening diagnosis.

## Proposition

What class of cause should the next attempt spend information on: instrument/environment, architecture, localized implementation, or genuinely new evidence?

## Minimum evidence

- all observed red/empty facts;
- prior attempt logs if any;
- the checker/instrument;
- the artifact or worktree it claims to judge.

## Decision contract

```yaml
skill: ddx
requires: [symptom_facts, instrument, claimed_subject, prior_attempts_if_any]
precommit:
  - write 2-3 causal diseases
  - write the cheapest discriminator
  - write outcome -> next action before running it
  - if the fork is empirically decidable, spend that one observation; do not escalate it to the human
decision:
  - when: no discriminator can distinguish the remaining diseases
    then: STOP
  - when: discriminator shows checker/environment disagrees with the claimed subject
    then: INSTRUMENT
  - when: discriminator localizes multiple symptoms to one missing schema/invariant/topology,
          or two or more symptoms fail only when order, deadline, retry, or cleanup changes
    then: ARCHITECTURE
  - when: discriminator remains red on the claimed subject itself
          and no shared architecture cause is established
    then: IMPLEMENTATION
  - when: discriminator adds a fact that changes the next attempt
          but does not yet localize the cause class
    then: NEW_INFO
  - when: proposed next attempt is same-class and spends no new information
    then: SAME_PROMPT_REFUSE
  - when: the proposed repair is a symptom-only workaround of an established shared cause,
          a sleep or longer deadline in place of a missing invariant,
          or a second copy of the same mapping
    then: SAME_PROMPT_REFUSE
non_claims:
  - does not identify the final root cause unless the discriminator establishes it
  - does not auto-repair
stop: spend at most one discriminator before choosing the next class of action
```

## Procedure

1. **Facts first.** List every relevant symptom without interpretation.
2. **Cluster into 2–3 causal diseases.** A disease predicts the same next observation and repair class. Six fail-codes are not six diseases. Two timeouts in different files are not two diseases until a discriminator separates their transitions.
3. **Split instrument from subject before blaming the implementation.** Ask what proposition the checker actually evaluates and what artifact it claims to judge.
4. **Precommit the discriminator.** Write what result A versus B would mean, then run the cheapest one once. If the fork is empirically decidable, spend that observation. Do not ask the human which cause is true.
5. **Honor the result in order:** instrument mismatch first; shared schema/invariant/topology second; localized implementation third; unresolved-but-new evidence fourth.
6. If the next attempt would repeat the same class unchanged, or is a symptom-only workaround of an established shared cause, refuse it.

## Exit

- `INSTRUMENT`
- `ARCHITECTURE`
- `IMPLEMENTATION`
- `NEW_INFO`
- `SAME_PROMPT_REFUSE`
- `STOP`

## Gotchas

- A failing checker is not automatically a failing implementation.
- A successful retry does not erase the original failure class.
- A discriminator that can be reinterpreted after the result did not discriminate.
- An empirically decidable fork is not a principal question. Spend the cheap observation within the one-discriminator bound; do not escalate it to the human.
- If one cheap discriminator is no longer enough because the mechanism itself is thorny—race, state machine, distributed ordering, crash recovery, rare nondeterminism, resource pathology—stop. That is a larger investigation, not an excuse to bloat this skill.
- Scattered timing failures are not several bugs until a discriminator separates their transitions. Closing one and leaving the others is the bandaid.
- A localized caller correction with an established cause and current authorization is not a symptom workaround merely because it changes one caller. Preserve its bounded implementation and verification job.

## Why this shape

- Diseases, not symptom labels: grouping is predictive.
- Precommit prevents hindsight diagnosis.
- Branch precedence prevents architecture from collapsing into generic implementation failure.
- One discriminator is the attention bound. DDx exists to stop blind rerolls, not become a full investigation framework.
- Empirical forks are spent, not escalated. The human is not a substitute discriminator.
