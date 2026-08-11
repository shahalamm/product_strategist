# Example: research-agent — research and memory records

Continues from `input.md`. Three competitors below — **TeamFlo**,
**Huddleworks**, and **RosterPilot** — are clearly fictional, invented for
this example; no claims about real companies are made anywhere in this
file. All JSON validates against `.claude/memory/schema.json`.

## Step 2: research, with the why for every claim

**TeamFlo**

- *Claim:* No admin approval step at all — any member can add a new
  teammate directly.
  *Why:* TeamFlo appears optimized for fast, viral team growth in small
  teams; removing approval entirely trades a small amount of invite noise
  for maximum signup velocity, consistent with onboarding messaging built
  around getting a whole team working together in under a minute.
- *Claim:* Sends a real-time notification to the billing admin the moment
  a new paid seat is added.
  *Why:* TeamFlo's buyers look billing-sensitive (small teams watching
  cost closely), so instant seat visibility is likely a trust/retention
  feature aimed at avoiding surprise-invoice churn, not a security control.

**Huddleworks**

- *Claim:* Keeps an approval step, but widened who can approve from
  "admins only" to "any full member."
  *Why:* This looks like a direct answer to the exact single-admin
  bottleneck behind our own idea's origin
  (`insight-2026-08-18-support-escalation-origin`) — rather than removing
  the gate, Huddleworks widened who can operate it, removing the
  single-point-of-failure while keeping a check in place.
- *Claim:* No per-seat budget-visibility feature; flat organization-wide
  pricing up to a seat cap, then a plan-upgrade prompt.
  *Why:* Flat, capped pricing sidesteps the need to notify anyone about
  individual seat additions at all — likely chosen to reduce billing
  complexity and support load, not as a considered visibility tradeoff.

**RosterPilot**

- *Claim:* No approval required until an account crosses a soft
  seat-count threshold, after which approval is required again.
  *Why:* Reads as a cheap heuristic guardrail — no per-invite friction for
  the common small-team case, with a size-based threshold catching runaway
  growth without a more nuanced control being designed.
- *Claim:* No per-invite notification; instead a persistent, always-visible
  account-level banner showing seat count and projected monthly cost.
  *Why:* Shifting from "notify on each event" to ambient awareness
  plausibly avoids notification fatigue for larger teams that add seats
  frequently — a different tradeoff than TeamFlo's per-event alert,
  suggesting a bigger-team customer segment.

Every claim above cleared the bar from Step 2 of
`.claude/agents/research-agent.md`: a single falsifiable fact, plus a
plausible reason why, not just a feature listing.

## Step 3: memory records written

```json
{
  "id": "research-source-2026-08-25-teamflo-no-approval",
  "type": "research-source",
  "title": "TeamFlo has no approval step for any invite",
  "summary": "TeamFlo lets any team member invite new teammates directly, with no admin approval step for any role.",
  "details": "TeamFlo appears optimized for fast, viral team growth in small teams — removing approval entirely trades a small amount of invite noise for maximum signup velocity, consistent with their broader 'get to value in under a minute' onboarding messaging.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "claim": "TeamFlo has no admin approval step for team invites; any member can add a new teammate directly.",
    "retrieved_at": "2026-08-25"
  },
  "tags": ["self-serve-invites", "approval-flow", "competitor-research"],
  "sources": [
    { "label": "TeamFlo help center - illustrative example", "reference": "n/a - illustrative example, not a real company or source" }
  ],
  "related_ids": ["assumption-2026-08-20-approval-rarely-blocks", "insight-2026-08-18-support-escalation-origin"],
  "owner": "research-agent",
  "created_at": "2026-08-25T10:00:00Z",
  "updated_at": "2026-08-25T10:00:00Z"
}
```

```json
{
  "id": "research-source-2026-08-25-teamflo-notification",
  "type": "research-source",
  "title": "TeamFlo notifies billing admin in real time on new seats",
  "summary": "TeamFlo sends a real-time notification to the billing admin the moment a new paid seat is added.",
  "details": "TeamFlo's buyers appear to be billing-sensitive (small teams watching cost closely), so instant visibility into new seats is likely a trust/retention feature aimed at avoiding surprise-invoice churn, not a security control.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "claim": "TeamFlo sends a real-time email/push notification to the billing admin whenever a new paid seat is added.",
    "retrieved_at": "2026-08-25"
  },
  "tags": ["self-serve-invites", "billing", "competitor-research"],
  "sources": [
    { "label": "TeamFlo pricing FAQ - illustrative example", "reference": "n/a - illustrative example, not a real company or source" }
  ],
  "related_ids": ["risk-2026-08-20-losing-budget-checkpoint", "assumption-2026-08-20-budget-owner-notification-preference"],
  "owner": "research-agent",
  "created_at": "2026-08-25T10:05:00Z",
  "updated_at": "2026-08-25T10:05:00Z"
}
```

```json
{
  "id": "competitor-2026-08-25-teamflo",
  "type": "competitor",
  "title": "TeamFlo",
  "summary": "Small-team collaboration tool that removes invite friction entirely, including admin approval, in favor of fast team growth.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "product_name": "TeamFlo",
    "positioning": "The fastest way to get your whole team working together, from signup to first project in under a minute.",
    "evidence_ids": [
      "research-source-2026-08-25-teamflo-no-approval",
      "research-source-2026-08-25-teamflo-notification"
    ],
    "scores": [
      {
        "axis": "Approval Friction",
        "axis_low_label": "No approval step required for any invite",
        "axis_high_label": "Every invite requires explicit approval before it completes",
        "value": 1,
        "rationale": "research-source-2026-08-25-teamflo-no-approval: TeamFlo has no admin approval step for team invites at all — any member can add a teammate directly, placing it at the lowest-friction end of this axis."
      },
      {
        "axis": "Budget Visibility Immediacy",
        "axis_low_label": "No signal to the budget owner when a new seat is added",
        "axis_high_label": "Budget owner is notified in real time when a new seat is added",
        "value": 5,
        "rationale": "research-source-2026-08-25-teamflo-notification: TeamFlo sends a real-time email/push notification to the billing admin whenever a new paid seat is added — this matches the high end of this axis directly."
      }
    ]
  },
  "tags": ["self-serve-invites", "competitor-research"],
  "sources": [],
  "related_ids": ["insight-2026-08-18-support-escalation-origin", "decision-2026-08-20-restrict-invite-to-full-members"],
  "owner": "research-agent",
  "created_at": "2026-08-25T10:10:00Z",
  "updated_at": "2026-09-20T11:00:00Z"
}
```

```json
{
  "id": "research-source-2026-08-25-huddleworks-widened-approval",
  "type": "research-source",
  "title": "Huddleworks widened who can approve invites, rather than removing approval",
  "summary": "Huddleworks keeps an approval step, but lets any full member approve a new invite, not just designated admins.",
  "details": "This looks like a direct answer to the same single-admin bottleneck behind our own idea's origin (insight-2026-08-18-support-escalation-origin): rather than removing the gate, Huddleworks widened who can operate it, keeping a check in place while removing the single-point-of-failure.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "claim": "Huddleworks requires approval for new invites, but any full member — not only designated admins — can approve them.",
    "retrieved_at": "2026-08-25"
  },
  "tags": ["self-serve-invites", "approval-flow", "competitor-research"],
  "sources": [
    { "label": "Huddleworks admin docs - illustrative example", "reference": "n/a - illustrative example, not a real company or source" }
  ],
  "related_ids": ["assumption-2026-08-20-approval-rarely-blocks", "insight-2026-08-18-support-escalation-origin"],
  "owner": "research-agent",
  "created_at": "2026-08-25T10:15:00Z",
  "updated_at": "2026-08-25T10:15:00Z"
}
```

```json
{
  "id": "research-source-2026-08-25-huddleworks-flat-billing",
  "type": "research-source",
  "title": "Huddleworks sidesteps per-seat budget visibility with flat pricing",
  "summary": "Huddleworks has no per-seat budget-visibility feature; pricing is flat per organization up to a seat cap, then prompts a plan upgrade.",
  "details": "Flat, capped pricing avoids the need to notify anyone about individual seat additions at all — this looks like it sidesteps the budget-visibility question by pricing structure rather than by a considered notification design, likely to reduce billing complexity and support load.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "claim": "Huddleworks bills a flat rate per organization up to a fixed seat cap, with no per-seat notification; exceeding the cap prompts a plan upgrade.",
    "retrieved_at": "2026-08-25"
  },
  "tags": ["self-serve-invites", "billing", "competitor-research"],
  "sources": [
    { "label": "Huddleworks pricing page - illustrative example", "reference": "n/a - illustrative example, not a real company or source" }
  ],
  "related_ids": ["risk-2026-08-20-losing-budget-checkpoint", "assumption-2026-08-20-budget-owner-notification-preference"],
  "owner": "research-agent",
  "created_at": "2026-08-25T10:20:00Z",
  "updated_at": "2026-08-25T10:20:00Z"
}
```

```json
{
  "id": "competitor-2026-08-25-huddleworks",
  "type": "competitor",
  "title": "Huddleworks",
  "summary": "Team workspace product that keeps an approval step for invites but widens who can operate it, and avoids per-seat billing complexity with a flat, capped price.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "product_name": "Huddleworks",
    "positioning": "One flat price for your whole team's workspace, no per-seat surprises.",
    "evidence_ids": [
      "research-source-2026-08-25-huddleworks-widened-approval",
      "research-source-2026-08-25-huddleworks-flat-billing"
    ],
    "scores": [
      {
        "axis": "Approval Friction",
        "axis_low_label": "No approval step required for any invite",
        "axis_high_label": "Every invite requires explicit approval before it completes",
        "value": 5,
        "rationale": "research-source-2026-08-25-huddleworks-widened-approval: Huddleworks still requires an approval step for every invite — only the pool of who can approve was widened from admins-only to any full member. Every invite still passes through approval, so this sits at the highest-friction end of this axis."
      },
      {
        "axis": "Budget Visibility Immediacy",
        "axis_low_label": "No signal to the budget owner when a new seat is added",
        "axis_high_label": "Budget owner is notified in real time when a new seat is added",
        "value": 1,
        "rationale": "research-source-2026-08-25-huddleworks-flat-billing: Huddleworks bills a flat rate with no per-seat notification at all — there is no signal to the budget owner when a seat is added, matching the low end of this axis directly."
      }
    ]
  },
  "tags": ["self-serve-invites", "competitor-research"],
  "sources": [],
  "related_ids": ["insight-2026-08-18-support-escalation-origin", "decision-2026-08-20-restrict-invite-to-full-members"],
  "owner": "research-agent",
  "created_at": "2026-08-25T10:25:00Z",
  "updated_at": "2026-09-20T11:00:00Z"
}
```

```json
{
  "id": "research-source-2026-08-26-rosterpilot-threshold-approval",
  "type": "research-source",
  "title": "RosterPilot removes approval below a soft seat-count threshold",
  "summary": "RosterPilot requires no approval for new invites until an account crosses a soft seat-count threshold, after which approval is required again.",
  "details": "This reads as a cheap, heuristic guardrail rather than a fully-reasoned policy: no per-invite friction for the common small-team case, but a size-based threshold catches runaway growth without requiring anyone to design a more nuanced control.",
  "status": "active",
  "confidence": "low",
  "data": {
    "claim": "RosterPilot requires no invite approval until an account exceeds a soft seat-count threshold, after which new invites require approval again.",
    "retrieved_at": "2026-08-26"
  },
  "tags": ["self-serve-invites", "approval-flow", "competitor-research"],
  "sources": [
    { "label": "RosterPilot changelog - illustrative example", "reference": "n/a - illustrative example, not a real company or source" }
  ],
  "related_ids": ["assumption-2026-08-20-approval-rarely-blocks", "insight-2026-08-18-support-escalation-origin"],
  "owner": "research-agent",
  "created_at": "2026-08-26T09:00:00Z",
  "updated_at": "2026-08-26T09:00:00Z"
}
```

```json
{
  "id": "research-source-2026-08-26-rosterpilot-ambient-banner",
  "type": "research-source",
  "title": "RosterPilot shows an ambient seat-count banner instead of per-invite alerts",
  "summary": "RosterPilot has no per-invite notification; instead it shows a persistent, always-visible seat count and projected monthly cost banner at the account level.",
  "details": "Shifting from 'notify on each event' to always-visible ambient awareness plausibly avoids notification fatigue for larger teams that add seats frequently — a different tradeoff than TeamFlo's per-event notification, likely aimed at a bigger-team customer segment.",
  "status": "active",
  "confidence": "low",
  "data": {
    "claim": "RosterPilot displays a persistent account-level banner showing current seat count and projected monthly cost, with no per-invite notification.",
    "retrieved_at": "2026-08-26"
  },
  "tags": ["self-serve-invites", "billing", "competitor-research"],
  "sources": [
    { "label": "RosterPilot product tour - illustrative example", "reference": "n/a - illustrative example, not a real company or source" }
  ],
  "related_ids": ["risk-2026-08-20-losing-budget-checkpoint", "assumption-2026-08-20-budget-owner-notification-preference"],
  "owner": "research-agent",
  "created_at": "2026-08-26T09:05:00Z",
  "updated_at": "2026-08-26T09:05:00Z"
}
```

```json
{
  "id": "competitor-2026-08-26-rosterpilot",
  "type": "competitor",
  "title": "RosterPilot",
  "summary": "Team roster/scheduling product that gates invite approval by account size rather than role, and favors ambient cost visibility over per-invite alerts.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "product_name": "RosterPilot",
    "positioning": "Always know who's on your roster and what it costs, without slowing your team down.",
    "evidence_ids": [
      "research-source-2026-08-26-rosterpilot-threshold-approval",
      "research-source-2026-08-26-rosterpilot-ambient-banner"
    ],
    "scores": [
      {
        "axis": "Budget Visibility Immediacy",
        "axis_low_label": "No signal to the budget owner when a new seat is added",
        "axis_high_label": "Budget owner is notified in real time when a new seat is added",
        "value": 3,
        "rationale": "research-source-2026-08-26-rosterpilot-ambient-banner: RosterPilot displays a persistent account-level banner showing current seat count and projected monthly cost, with no per-invite notification. That's a real, always-current signal (ruling out the lowest value), but it's pull-based — the budget owner has to look — rather than pushed in real time (ruling out the highest value), placing it at the midpoint of this axis."
      }
    ]
  },
  "tags": ["self-serve-invites", "competitor-research"],
  "sources": [],
  "related_ids": ["insight-2026-08-18-support-escalation-origin", "decision-2026-08-20-restrict-invite-to-full-members"],
  "owner": "research-agent",
  "created_at": "2026-08-26T09:10:00Z",
  "updated_at": "2026-09-20T11:00:00Z"
}
```

**RosterPilot is deliberately left unscored on "Approval Friction."**
`research-source-2026-08-26-rosterpilot-threshold-approval` says approval
is required only above a soft seat-count threshold — but the evidence
doesn't say where that threshold sits or how often accounts cross it, so
neither end of the axis (nor any specific point between them) can be
honestly justified. Guessing a middling value here would be exactly the
fabrication this platform's evidence-attribution rule exists to prevent;
omitting the axis for this competitor is the correct outcome, not a gap
to fill later.

Note what's absent: no `decision`, `assumption`, `risk`, or `insight`
record is written here, even though the TeamFlo/Huddleworks findings
obviously bear on the store's existing open assumption about approval
rarely blocking anything. Connecting that dot is left for a human or a
future skill — not this agent's call to make unprompted.

## Step 4: rendered Capability Matrix

See `.claude/examples/research-agent/capability-matrix.example.md` for the
full rendered artifact — what `research-agent` would write to
`.claude/outputs/self-serve-team-invites/capability-matrix/capability-matrix.md`,
produced from `.claude/templates/capability-matrix.md` using only the
records above (and the store from `input.md`).

## Step 5: scoring pass (added in Task 11)

A later invocation of `research-agent` against this same store proposed
two axes, per Step 5 of `.claude/agents/research-agent.md`, and added
`data.scores` to the three `competitor` records shown above (see their
updated `updated_at` timestamps). Neither axis is a generic template
default — both trace directly to open questions already in the store:

- **Approval Friction** — ties to `assumption-2026-08-20-approval-rarely-blocks`
  and the still-undecided question of whether admin approval is removed
  at all (see `decision-2026-09-02-budget-owner-notification-mechanism`'s
  own `open_questions`).
- **Budget Visibility Immediacy** — ties directly to
  `risk-2026-08-20-losing-budget-checkpoint` and
  `assumption-2026-08-20-budget-owner-notification-preference`, the exact
  question the whole notification-mechanism decision was about.

**Consistency rule, demonstrated:** every competitor scored on
"Approval Friction" uses the byte-identical `axis_low_label`
("No approval step required for any invite") and `axis_high_label`
("Every invite requires explicit approval before it completes") — TeamFlo
and Huddleworks only, since RosterPilot is omitted on this axis (see
below). Every competitor scored on "Budget Visibility Immediacy" — all
three — uses the byte-identical `axis_low_label`
("No signal to the budget owner when a new seat is added") and
`axis_high_label` ("Budget owner is notified in real time when a new
seat is added"). Neither axis's wording varies by even a word across the
competitors scored on it.

**Scores by axis:**

| Competitor | Approval Friction | Budget Visibility Immediacy |
|---|---|---|
| TeamFlo | 1 | 5 |
| Huddleworks | 5 | 1 |
| RosterPilot | *omitted — see below* | 3 |

RosterPilot's omission on Approval Friction is the deliberate case this
example exists to show: the evidence
(`research-source-2026-08-26-rosterpilot-threshold-approval`) describes a
*conditional* policy (no approval below a threshold, required above it)
with no data on where that threshold sits or how often it's crossed — so
no single 1–5 value can be honestly justified, and none is written,
rather than defaulting to a middling guess.

Positioning "Us" on either of these axes is explicitly out of scope here
— see `.claude/agents/research-agent.md`'s "Explicitly deferred" section.
No such score is written for this project's own stance anywhere in this
example.
