# Example: discovery-interview — four turns

Continues from `input.md`. Each turn shows the internal prioritization
note (what the skill would reason to itself, per SKILL.md Step 1 — not
necessarily said aloud), the question, the user's answer(s), and the exact
memory record(s) written afterward. All JSON below validates against
`.claude/memory/schema.json`.

---

## Turn 1

**Prioritization note (internal):** Of the seven dimensions, *Problem* and
*Evidence* are already anchored by the existing `insight` (support
escalation, 3-day block). *User* is partly stated ("any team member") but
vague. *Constraints* is completely unaddressed — and the idea itself is a
proposal to *remove* an existing control, so understanding what that
control currently does has the highest leverage: almost every other
question (risk, success criteria, scope) depends on knowing what would
actually be lost. The user's own phrase "just friction at this point" is a
self-flagged hedge pointing straight at this gap. Picked over *User*
scoping because scope only matters once we know there's something worth
scoping around.

**Question:** "Right now, when an admin approves a new teammate, what
specifically are they checking or controlling — is it seat/billing cost,
an access/security review, role assignment, or something else?"

**Answer (1st attempt, vague):** "I mean, they're mostly just
rubber-stamping it, it's not really doing much."

*Hedge detected: "mostly," "not really doing much" — no concrete claim.
Probe once.*

**Probe:** "Rubber-stamping suggests it's not fully load-bearing — but
'not really doing much' isn't something we can act on. Concretely: in the
last month, has an admin ever rejected or modified an invite at that
approval step? If you don't know, who would know, or where would we find
that out?"

**Answer (2nd attempt, partially concrete):** "I don't have data on
rejections. I'd guess it's rare but I haven't checked. Billing is
definitely tied to it though — new seats add to the invoice, so someone
with budget visibility should probably still see that."

*Rejection frequency stays unresolved after one probe → logged as an open
assumption rather than probed a second time on that sub-point (the billing
tie-in, by contrast, was stated as fact, not hedged — logged as an
insight). The billing insight also implies a risk if approval is removed
outright, worth logging now while it's fresh rather than waiting.*

**Memory records written:**

```json
{
  "id": "assumption-2026-08-20-approval-rarely-blocks",
  "type": "assumption",
  "title": "Admin approval rarely rejects or changes an invite",
  "summary": "The requester believes admin approval almost never actually rejects or modifies an invite, but has no data confirming this.",
  "status": "draft",
  "confidence": "low",
  "data": {
    "statement": "In the last month, admin approval has not rejected or modified any team invite.",
    "validation_status": "unvalidated",
    "impact_if_wrong": "If rejections/modifications are actually common, removing approval would silently strip a control admins currently rely on, letting unwanted invites or role errors through unchecked."
  },
  "tags": ["self-serve-invites", "approval-flow"],
  "sources": [],
  "related_ids": ["insight-2026-08-18-support-escalation-origin"],
  "owner": "discovery-interview",
  "created_at": "2026-08-20T09:14:00Z",
  "updated_at": "2026-08-20T09:14:00Z"
}
```

```json
{
  "id": "insight-2026-08-20-billing-tied-to-approval",
  "type": "insight",
  "title": "Approval step is also today's billing visibility checkpoint",
  "summary": "New seats add to the account's invoice, so the current approval step is where someone with budget visibility currently sees a new seat being added, whether or not that's its primary purpose.",
  "status": "active",
  "confidence": "medium",
  "tags": ["self-serve-invites", "billing"],
  "sources": [],
  "related_ids": ["assumption-2026-08-20-approval-rarely-blocks"],
  "owner": "discovery-interview",
  "created_at": "2026-08-20T09:15:00Z",
  "updated_at": "2026-08-20T09:15:00Z"
}
```

```json
{
  "id": "risk-2026-08-20-losing-budget-checkpoint",
  "type": "risk",
  "title": "Removing approval removes the only current budget checkpoint",
  "summary": "If admin approval is removed with nothing in its place, there would be no checkpoint left where someone with budget visibility sees a new seat being added before it hits the invoice.",
  "status": "draft",
  "confidence": "medium",
  "tags": ["self-serve-invites", "billing"],
  "sources": [],
  "related_ids": ["insight-2026-08-20-billing-tied-to-approval"],
  "owner": "discovery-interview",
  "created_at": "2026-08-20T09:16:00Z",
  "updated_at": "2026-08-20T09:16:00Z"
}
```

---

## Turn 2

**Prioritization note (internal):** *Constraints* now has useful shape
(billing tie-in, rare-rejection assumption). The next highest-leverage gap
is *User*: the original pitch said "any team member," which is itself a
hedge (an unscoped universal). It directly determines the blast radius of
the risk just logged — "any" including e.g. guests changes the risk
profile a lot more than "any full member" would. Preferred over *Success
criteria*, which is meaningless until scope is fixed.

**Question:** "When you say 'any team member' should be able to invite —
does that include every role in the account (e.g. guests, contractors), or
only full members? Is there a role that should be excluded?"

**Answer (concrete, first try):** "Just full members, not guests.
Contractors are a different role and shouldn't invite."

*Concrete and checkable — no probing needed. This is a stated commitment
among alternatives (all roles vs. full-members-only), so it's a `decision`,
not an `insight`.*

**Memory record written:**

```json
{
  "id": "decision-2026-08-20-restrict-invite-to-full-members",
  "type": "decision",
  "title": "Restrict self-invite ability to the full-member role",
  "summary": "Self-invite is scoped to full members only; guests and contractors cannot invite new teammates.",
  "status": "active",
  "confidence": "high",
  "data": {
    "alternatives_considered": [
      {
        "option": "Allow every role (including guests and contractors) to invite",
        "cons": ["Widens the risk surface beyond what's been examined", "Contractors/guests may not be accountable for who they add"],
        "rejected_because": "Requester explicitly excluded these roles when asked directly."
      },
      {
        "option": "Allow only the full-member role to invite",
        "pros": ["Matches requester's stated intent", "Keeps the risk surface to accountable, permanent teammates"]
      }
    ],
    "recommendation": "Self-invite is available to full members only; guests and contractors are excluded.",
    "rationale": "Requester confirmed directly that 'any team member' was never meant to include guests or contractors — narrowing scope to full members before any further design work avoids designing against a broader, unintended blast radius.",
    "open_questions": [
      "Are there full-member sub-roles (e.g. read-only) that should also be excluded? Not yet asked."
    ]
  },
  "tags": ["self-serve-invites", "scope"],
  "sources": [],
  "related_ids": ["insight-2026-08-18-support-escalation-origin"],
  "owner": "discovery-interview",
  "created_at": "2026-08-20T09:22:00Z",
  "updated_at": "2026-08-20T09:22:00Z"
}
```

---

## Turn 3

**Prioritization note (internal):** The open `risk` from Turn 1 (losing the
budget checkpoint) is still unresolved and now has the highest leverage
left — it's the one thing standing between this idea and being safe to
recommend at all. *Success criteria* and *Evidence* remain lower-leverage
until this is settled, since "success" can't be defined while a known risk
is still open.

**Guardrail check:** resolving *how* the budget owner stays informed —
block vs. notify vs. digest vs. spending cap — would mean proposing and
weighing solution mechanisms, which is out of scope for this skill (see
SKILL.md's guardrail in Step 1). So the question stays on the fact the
skill actually needs: whether this matters to the requester at all, and
how urgently — not which mechanism should handle it.

**Question:** "If admin approval goes away, does whoever owns the budget
need to know about new seats being added at all — and if so, how urgent is
that: do they need to know as it happens, or is finding out later fine?"

**Answer (1st attempt, vague):** "Yeah I guess they should know, but I'm
not sure how urgent — we can figure out the details later."

*Hedge detected: "I guess," "not sure," "figure out the details later."
The "need to know at all" half is answered (yes); the urgency half isn't.
Probe once on urgency specifically — still without offering a mechanism.*

**Probe:** "Just the urgency, roughly: does it need to reach them before or
as the seat is added, or is it fine if they only find out well after the
fact — say, on a weekly or monthly basis?"

**Answer (2nd attempt, concrete):** "Well after the fact is fine, not
urgent. Doesn't need to be immediate."

*Concrete on both need (yes) and urgency (low) — but this is still the
requester's own stated preference, not a decision this skill is entitled
to make or a validated fact from whoever actually owns the budget. Log it
as a low-confidence `assumption`, not a `decision`: no mechanism was
proposed or chosen, so there's nothing to put in
`alternatives_considered`. This is also the second case in this example of
a vague answer getting probed — like Turn 1's rejection-frequency
question, the probe here converts a hedge into something concrete, but
because it's an unverified preference rather than a checked fact, it stays
an `assumption` rather than becoming settled.*

**Memory record written:**

```json
{
  "id": "assumption-2026-08-20-budget-owner-notification-preference",
  "type": "assumption",
  "title": "Budget owner needs to know about new seats, but not urgently",
  "summary": "Requester believes the budget owner needs some visibility into new self-invited seats, but that low-urgency, after-the-fact awareness is sufficient — this hasn't been checked with an actual budget owner, and no mechanism has been decided.",
  "status": "draft",
  "confidence": "low",
  "data": {
    "statement": "The budget owner needs to know when new seats are added via self-invite, but does not need that information in real time — finding out well after the fact (e.g. weekly or monthly) is sufficient.",
    "validation_status": "unvalidated",
    "impact_if_wrong": "If budget owners actually need near-real-time visibility to catch unexpected spend before it compounds, a low-urgency approach would let them find out too late to act — undermining the risk this assumption was meant to address."
  },
  "tags": ["self-serve-invites", "billing"],
  "sources": [],
  "related_ids": ["risk-2026-08-20-losing-budget-checkpoint", "assumption-2026-08-20-approval-rarely-blocks"],
  "owner": "discovery-interview",
  "created_at": "2026-08-20T09:31:00Z",
  "updated_at": "2026-08-20T09:31:00Z"
}
```

No mechanism (notification, spending cap, digest, etc.) is decided here —
that design work belongs to a future skill, once discovery has surfaced
enough to hand off.

---

## Turn 4 — user pauses the interview

**User:** "That's enough for now, I need to check with finance."

**Skill response:** Acknowledges the pause, briefly confirms what was
captured this session (two open assumptions, one risk, one decision, one
insight, all in `.claude/memory/self-serve-team-invites.json`), and stops.
It does **not** assess whether discovery is "complete" — that's explicitly
out of scope for this skill (see SKILL.md). No memory record is written
for this turn; stopping is not itself a finding.
