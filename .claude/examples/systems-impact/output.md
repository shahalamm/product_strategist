# Example: systems-impact — findings

Continues from `input.md`. Four `system-impact` records across four
genuinely different `system_area` categories — not padded to cover the
other seven in the schema's enum, which don't apply here — plus one new
`risk` record surfaced while assessing impact. All JSON validates against
`.claude/memory/schema.json`.

## Categories considered and why the rest were skipped

Per Step 2 of `.claude/agents/systems-impact.md`, all eleven
`system_area` values were considered against the four requirements.
`apis`, `events`, `analytics`, `reporting`, `dashboards`, `operations`,
and `observability` produced no genuine, specific finding for this
feature and are correctly absent below — a scheduled digest email with
no new API surface, no analytics event redesign, and no dashboard change
doesn't need an impact record manufactured in those categories just to
look thorough.

## 1. `monitoring` — the finding the task specifically anticipated

```json
{
  "id": "system-impact-2026-09-10-monitoring-silent-failure",
  "type": "system-impact",
  "title": "Failed digest send is indistinguishable from a quiet week",
  "summary": "Monitoring cannot currently tell a failed digest send apart from a week where zero seats were added, since both produce the same observable outcome: no digest sent.",
  "status": "active",
  "confidence": "high",
  "data": {
    "system_area": "monitoring",
    "impact_description": "requirement-2026-09-05-digest-content-and-trigger's own acceptance criteria specify that no digest is sent when zero seats were added in the window — but a failed send (the scheduled job doesn't run, or the email provider errors) produces the exact same observable outcome: no digest received. Without a positive signal that the digest job ran and evaluated the window, as opposed to simply not sending, a real failure and a quiet week are indistinguishable from the outside, so a silent failure could persist indefinitely without detection.",
    "severity": "high",
    "elaborates_requirement_ids": ["requirement-2026-09-05-digest-content-and-trigger"]
  },
  "details": "This is severe enough that it likely warrants a follow-up requirement — e.g. a positive 'ran, found nothing' heartbeat distinct from 'did not run' — but writing that requirement is out of scope for this agent; surfacing the need is as far as it goes.",
  "tags": ["self-serve-invites", "billing", "systems-impact"],
  "sources": [],
  "related_ids": ["requirement-2026-09-05-digest-content-and-trigger", "risk-2026-09-10-silent-digest-failure"],
  "owner": "systems-impact",
  "created_at": "2026-09-10T10:00:00Z",
  "updated_at": "2026-09-10T10:00:00Z"
}
```

This directly references the "no digest if zero seats added" acceptance
criterion already in `requirement-2026-09-05-digest-content-and-trigger`,
rather than inventing an unrelated monitoring concern — the same edge
case, looked at from a different angle. Note it names the follow-up need
in its own `details` without writing that requirement itself, per this
agent's out-of-scope boundary.

## 2. `customer-support`

```json
{
  "id": "system-impact-2026-09-10-customer-support-new-email-confusion",
  "type": "system-impact",
  "title": "New recurring email likely generates 'what is this' support contacts",
  "summary": "Budget owners receiving this new, unfamiliar weekly email — especially listing seats they didn't personally add — are a plausible new source of support contacts asking what it is or whether it's legitimate.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "system_area": "customer-support",
    "impact_description": "requirement-2026-09-05-digest-content-and-trigger lists seats added by anyone on the team, and requirement-2026-09-05-digest-delivery-channel sends this via email to whoever the budget owner is — a role that may not have previously received any product-originated email from this account. A budget owner seeing an unfamiliar automated email listing an action a colleague took, not themselves, is a plausible trigger for a support contact asking what the email is, why they're receiving it, or whether it's a phishing attempt.",
    "severity": "low",
    "elaborates_requirement_ids": ["requirement-2026-09-05-digest-content-and-trigger", "requirement-2026-09-05-digest-delivery-channel"]
  },
  "tags": ["self-serve-invites", "billing", "systems-impact"],
  "sources": [],
  "related_ids": ["requirement-2026-09-05-digest-content-and-trigger", "requirement-2026-09-05-digest-delivery-channel"],
  "owner": "systems-impact",
  "created_at": "2026-09-10T10:05:00Z",
  "updated_at": "2026-09-10T10:05:00Z"
}
```

## 3. `data-model`

```json
{
  "id": "system-impact-2026-09-10-data-model-seat-added-events",
  "type": "system-impact",
  "title": "Digest window computation needs a timestamped seat-added event, not just current membership state",
  "summary": "Computing seats added in a fixed, non-overlapping weekly window requires a queryable, timestamped record of each self-invite addition — not something derivable from current membership state alone.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "system_area": "data-model",
    "impact_description": "requirement-2026-09-05-digest-content-and-trigger needs to enumerate seats added in the preceding 7 days, and requirement-2026-09-05-digest-cadence needs that window to be exact and non-overlapping across consecutive digests. Both require a discrete, timestamped 'seat added via self-invite' event queryable by date range; if current membership data only reflects present state (who is a member now) without a durable added-at timestamp per member, this is a new data requirement, not something the digest can derive from existing membership tables alone.",
    "severity": "medium",
    "elaborates_requirement_ids": ["requirement-2026-09-05-digest-content-and-trigger", "requirement-2026-09-05-digest-cadence"]
  },
  "tags": ["self-serve-invites", "billing", "systems-impact"],
  "sources": [],
  "related_ids": ["requirement-2026-09-05-digest-content-and-trigger", "requirement-2026-09-05-digest-cadence"],
  "owner": "systems-impact",
  "created_at": "2026-09-10T10:10:00Z",
  "updated_at": "2026-09-10T10:10:00Z"
}
```

This is the "you judge genuinely applies" category: two requirements
share the same underlying dependency (a timestamped seat-added event), so
this is one record naming both via `elaborates_requirement_ids`, not two
near-duplicate records.

## 4. `customer-communication`

```json
{
  "id": "system-impact-2026-09-10-customer-communication-new-channel",
  "type": "system-impact",
  "title": "Weekly digest is a new automated communication channel to an unfamiliar recipient role",
  "summary": "Delivering the digest by email introduces a new recurring automated communication to the budget-owner role, requiring its own template and preference/unsubscribe handling distinct from existing transactional email.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "system_area": "customer-communication",
    "impact_description": "requirement-2026-09-05-digest-delivery-channel chose email specifically because it doesn't require the budget owner to be an active product user. That means this is very likely the first recurring, automated email this role receives from the product — it needs its own template, sender identity, and unsubscribe/preference handling, rather than being folded into an existing transactional email type whose unsubscribe/preference toggle the budget owner never explicitly opted into for this purpose.",
    "severity": "medium",
    "elaborates_requirement_ids": ["requirement-2026-09-05-digest-delivery-channel"]
  },
  "tags": ["self-serve-invites", "billing", "systems-impact"],
  "sources": [],
  "related_ids": ["requirement-2026-09-05-digest-delivery-channel"],
  "owner": "systems-impact",
  "created_at": "2026-09-10T10:15:00Z",
  "updated_at": "2026-09-10T10:15:00Z"
}
```

## The new risk this assessment surfaced

The monitoring finding above exposes a failure mode not already captured
by any existing `risk` or `assumption` in the store — not "the digest
cadence has an inherent gap" (`risk-2026-09-02-digest-misses-burst-spend`,
already logged) but "the digest mechanism itself could quietly break."
Per Step 4 of `.claude/agents/systems-impact.md`, that's written as one
new `risk` record, not folded into the `system-impact` finding itself:

```json
{
  "id": "risk-2026-09-10-silent-digest-failure",
  "type": "risk",
  "title": "The digest mechanism could silently stop working without anyone noticing",
  "summary": "Because a failed digest send and a quiet week produce the same observable outcome (no email), the notification mechanism meant to address risk-2026-08-20-losing-budget-checkpoint could itself silently degrade or fail without detection.",
  "status": "draft",
  "confidence": "medium",
  "tags": ["self-serve-invites", "billing", "systems-impact"],
  "sources": [],
  "related_ids": ["system-impact-2026-09-10-monitoring-silent-failure", "requirement-2026-09-05-digest-content-and-trigger", "risk-2026-08-20-losing-budget-checkpoint"],
  "owner": "systems-impact",
  "created_at": "2026-09-10T10:20:00Z",
  "updated_at": "2026-09-10T10:20:00Z"
}
```

## What's deliberately absent

No mitigation, technical solution, or new `requirement`/`decision` record
was written anywhere in this example — five records total (four
`system-impact`, one `risk`), each identifying what's touched and how
seriously. The monitoring finding names the shape of a plausible
follow-up requirement in its own `details` text without writing it;
deciding whether and how to act on any of this is left to a human, or a
future `solution-design` pass against a new decision, neither of which
this agent performs itself.
