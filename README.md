# Agent Skills

Five bounded engineering decision skills for coding-agent work.

```text
done      Is every requirement due now actually delivered?
ddx       What class of cause should the next attempt investigate?
proof     Does this check prove the named claim on this revision?
scope     Is this change inside the disclosed ask?
simplify  Does this complexity pay rent?
```

Each skill owns one decision and stops. They are guidance, not enforcement, gates, or evidence authority.

## Install

Install one or more skill directories with your Agent Skills-compatible client, or symlink them into its skills directory:

```sh
ln -s "$PWD/done" ~/.agents/skills/done
ln -s "$PWD/ddx" ~/.agents/skills/ddx
ln -s "$PWD/proof" ~/.agents/skills/proof
ln -s "$PWD/scope" ~/.agents/skills/scope
ln -s "$PWD/simplify" ~/.agents/skills/simplify
```

For OMP, use `~/.omp/agent/skills/` instead of `~/.agents/skills/`.

Restart the client after installation so activation metadata is reloaded.

## Structure

Every skill follows the open Agent Skills shape:

```text
<skill-name>/
  SKILL.md
```

`SKILL.md` contains activation metadata and the complete decision procedure.

## Boundaries

- No skill claims that its presence guarantees behavior.
- No skill manufactures evidence or substitutes model agreement for proof.
- The collection is deliberately five tools; specialist workbenches are separate products.
- Each skill may be used independently.

## When the question becomes repository-wide

`proof` decides whether one check supports one claim. If the job becomes “does this repository protect the right product risks with the right evidence?”, use the separate free [Fix My Tests](https://github.com/GauravAlbal/fix-my-tests) instrument.

## License

The skill documents are available under [CC BY 4.0](LICENSE.md).
