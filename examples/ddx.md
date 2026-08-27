# DDx Lite example: checker on the wrong worktree

## Moment

A second identical repair attempt is being proposed after a verifier reports: “unexpected file in candidate worktree.”

## Evidence

**Claimed subject**

- Candidate worktree: `/tmp/feature-42`
- Candidate revision: `7e41c2a`

**Observed candidate state**

- `git -C /tmp/feature-42 diff --name-only 7e41c2a^` returns only `internal/status.go`.
- `internal/status.go` contains the requested mutation.

**Verifier receipt**

- Working directory: `/tmp/base`
- Reported unexpected file: `notes.tmp`
- `/tmp/base/notes.tmp` exists and is untracked.
- `/tmp/feature-42/notes.tmp` does not exist.

**Competing hypotheses**

- A: the implementation failed to isolate its change.
- B: the verifier evaluated the wrong worktree.

**Precommitted discriminator**

Before another repair, compare the verifier's recorded working directory and revision with the claimed subject.

**Result**

- Recorded: `/tmp/base` at `53af901`
- Claimed: `/tmp/feature-42` at `7e41c2a`

The observation separates A from B and identifies the instrument path.

```yaml
skill: ddx
hypotheses:
  - implementation changed an extra file
  - verifier evaluated the wrong subject
discriminator: compare receipt cwd/revision to claimed worktree/revision
result: different subject
decision: INSTRUMENT
next_action: rerun the verifier bound to /tmp/feature-42 at 7e41c2a
```

## Disposition

`INSTRUMENT`

Do not spend another implementation attempt repairing a file that exists only in the verifier's worktree.
