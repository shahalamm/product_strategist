---
name: solution-design
description: >
  Elaborates an already-made, active decision into concrete, testable
  functional requirements — given a specific target decision id, generates
  the requirement records needed to make that decision's recommendation
  buildable. Trigger when a decision (from discovery-interview or
  strategy-agent) is closed and someone needs it broken into buildable
  requirements. Do not use this to reconsider the decision or its
  alternatives (that choice is closed), to make new implementation-level
  decisions (resolve small choices inline in a requirement's details, don't
  spin up a decision record), or when the target decision doesn't exist or
  isn't active — say so and stop rather than guessing at a target.
---

# Solution Design

You elaborate one closed decision into buildable requirements. You do not
reconsider the decision, and you do not make new decisions of your own —
even small ones.

## Scope

- **Input:** a project's memory store, and a specific target `decision`
  id the user points you at. Any `decision` record qualifies — not only
  ones tagged `strategy-recommendation`; a `discovery-interview` scope
  decision (e.g. who can invite) can also need requirement elaboration.
- **Output:** however many `requirement` records are needed to make the
  target decision's recommendation buildable, each conforming to
  `.claude/memory/schema.json`'s required `data` shape for `requirement`.
- **Side effects:** `.claude/memory/<project-slug>.json` only. No output
  document is rendered by this skill — the requirement records themselves
  are the deliverable. `documentation-agent` (not built yet) reads these
  from memory later; whether it can render something coherent from them
  is the actual test of whether this skill captured enough, not a
  document this skill produces itself.
- **Out of scope, explicitly — enforced as strictly as
  `discovery-interview` enforces not-solutioning:**
  - Re-litigating the target decision or its alternatives. That choice is
    closed — this skill elaborates the recommendation, it doesn't
    reconsider it. Don't second-guess `data.recommendation`, don't revisit
    `data.alternatives_considered`.
  - Writing new `decision` records, even for small implementation choices
    that come up while writing requirements (e.g. "email digest vs.
    in-app"). If a genuine small implementation choice needs resolving to
    make a requirement concrete, resolve it and record the reasoning in
    that requirement's `details` field — do not spin up a `decision`
    record with `alternatives_considered` for it. That machinery is for
    the meaningfully-weighed choices `discovery-interview` and
    `strategy-agent` already handle, not for every small call this skill
    has to make to write a concrete requirement.
  - Researching anything, or reassessing impact/tradeoffs beyond what's
    needed to state a requirement and its acceptance criteria — that's
    `research-agent`'s and `strategy-agent`'s job, already done by the
    time this skill runs.

## Step 1: Read the store and check the precondition

Read the full memory store and `.claude/memory/schema.json`. Find the
target decision record by id. Before generating anything, confirm:

1. It exists.
2. Its `type` is `decision`.
3. Its `status` is `"active"`.

If any of these fail — the id isn't found, it's a different type, or its
status is `draft`/`invalidated`/`deprecated`/anything other than
`active` — **stop** and say plainly what's missing: the id wasn't found,
or the decision isn't active and why that matters (an inactive decision
isn't yet something to build against). Do not proceed and do not guess at
a different target.

## Step 2: Carry forward any caveats the decision already flagged

Before writing requirements, check the target decision's own
`data.rationale` and `data.open_questions` for anything flagged as
resting on something unvalidated (an unvalidated assumption, an
unresolved dependency, etc.). If present, carry that caveat forward
**visibly** into the `details` of whichever requirement(s) it actually
bears on — do not silently drop it just because this skill is now several
steps removed from where it was first flagged. A requirement built on a
caveated decision should say so.

## Step 3: Generate requirement records

Generate however many `requirement` records are needed to make the
decision's recommendation buildable — no fixed count. Typically a
handful; not one giant requirement covering everything, and not dozens of
trivial ones splitting normal behavior into needless fragments.

For each:

- `data.statement` — a single concrete, testable functional requirement,
  not a restatement of the decision itself. ("The system sends a weekly
  digest email listing seats added in the prior 7 days" is a requirement;
  "We will notify the budget owner," restated from the decision, is not.)
- `data.acceptance_criteria` — specific, checkable pass/fail conditions. A
  reviewer must be able to check each one against a build and get a
  yes/no answer. Reject vague criteria ("works well," "is reliable")
  before writing them.
- `data.edge_cases` — edge cases this requirement needs to account for, if
  any.
- `data.elaborates_decision_id` — the target decision's id. Every
  requirement from this invocation points to the same target decision.
- **Same evidence-attribution standard `research-agent` and
  `strategy-agent` already hold themselves to:** any specific claim
  inside a requirement's `details` or `edge_cases` either traces to an
  existing record (name it) or is marked
  `"(solution-design's judgment, not evidence)"`. Don't blur invented
  reasoning with sourced fact.
- Set `owner` to `solution-design`, and `related_ids` to include the
  target decision id plus any other record a requirement or edge case
  specifically traces to.

## Step 4: Handle small implementation choices inline, not as decisions

Writing concrete requirements will sometimes surface a small
implementation choice the target decision didn't pin down (exact wording,
a specific default, which of two roughly-equivalent minor mechanisms to
use). Resolve it and record the reasoning directly in the relevant
requirement's `details` field. Do not write a `decision` record for it,
even a small one — `decision` records with `alternatives_considered` are
for the meaningfully-weighed choices `discovery-interview` and
`strategy-agent` already handle; using that machinery for every small
call this skill makes would dilute what a `decision` record signals.

## Example

See `.claude/examples/solution-design/` for a full worked example:
elaborating `decision-2026-09-02-budget-owner-notification-mechanism`
(the weekly-digest recommendation from `strategy-agent`'s example) into
four `requirement` records — including an edge case tracing to
`risk-2026-09-02-digest-misses-burst-spend`, a caveat about an unvalidated
assumption carried forward from the decision, and one small implementation
choice (email vs. in-app delivery) resolved inline rather than spun up as
a new decision.
