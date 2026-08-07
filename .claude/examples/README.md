# examples/

Reference inputs and outputs used to design, explain, and evaluate skills —
one example per skill at minimum, showing a realistic input and the output
that skill should produce for it.

Examples serve two purposes:

1. **Spec by demonstration.** When a skill's instructions are ambiguous, its
   example is the tiebreaker for what "correct" output looks like.
2. **Regression reference.** When a skill is changed, its existing examples
   should still hold (or be deliberately updated) — they're an informal
   test suite reviewers can check new versions against.

## Status

Empty by design. No skills exist yet to generate examples for — this
repository is currently scaffolding only.

## Conventions (for when examples start being added)

- Mirror the skill's name: `examples/<skill-name>/` containing an
  `input.md` (or similar) and `output.md`.
- Keep examples realistic but minimal — enough to exercise the skill's
  behavior without being a full production case study.
- If a memory record is part of the expected input or output, include it
  as JSON conforming to `.claude/memory/schema.json`.

See `/CONTRIBUTING.md` for how examples fit into the skill contract.
