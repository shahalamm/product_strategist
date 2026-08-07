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

**Question:** "If admin approval goes away, should whoever owns the budget
still get some visibility into new seats before or as they're added — e.g.
a notification, a spending cap, a monthly digest — or is silently
increasing the bill acceptable?"

**Answer (1st attempt, vague):** "Yeah I guess they should know somehow,
we can figure out the details later."

*Hedge detected: "I guess," "somehow," "figure out the details later" —
directly on the open risk. Probe once, forcing a provisional concrete
choice rather than accepting deferral.*

**Probe:** "Before logging this as settled, 'know somehow' isn't specific
enough to act on. Minimally: does it need to block the invite (a hard
stop, like today), or is it enough to just inform the budget owner after
the fact (a soft notification)? Pick one, even provisionally."

**Answer (2nd attempt, concrete):** "Soft notification is fine — real-time
is nice to have but not required for v1."

*Concrete enough to log as a decision that directly resolves the open
risk. Note this is the second case in this example of a vague answer being
probed — here the probe succeeded in producing a concrete, actionable
choice, unlike Turn 1's rejection-frequency question, which remained
unresolved after one probe and was logged as an open assumption instead.
Both are correct outcomes of probing — the skill doesn't force every
vague answer to resolve, only makes sure it doesn't get treated as settled
without either resolving or being explicitly marked open.*

**Memory record written:**

```json
{
  "id": "decision-2026-08-20-soft-notification-not-hard-block",
  "type": "decision",
  "title": "Replace hard approval with a soft notification to the budget owner",
  "summary": "Admin approval is replaced by a non-blocking notification to the budget owner when a new seat is added, addressing the budget-visibility risk without reintroducing a hard approval gate.",
  "status": "active",
  "confidence": "medium",
  "data": {
    "alternatives_considered": [
      {
        "option": "Keep a hard approval gate (status quo)",
        "pros": ["Preserves budget visibility exactly as today"],
        "cons": ["Reintroduces the friction and admin-availability bottleneck the idea exists to remove"],
        "rejected_because": "Directly defeats the purpose of the idea; requester wants approval gone, not relocated."
      },
      {
        "option": "Soft, non-blocking notification to the budget owner",
        "pros": ["Preserves visibility without blocking the invite", "Matches requester's stated preference"],
        "cons": ["Doesn't prevent overspend before it happens, only informs after"]
      },
      {
        "option": "No notification at all",
        "cons": ["Leaves the budget-visibility risk from Turn 1 completely unaddressed"],
        "rejected_because": "Requester confirmed budget owner should 'know somehow' — silence was not acceptable even provisionally."
      }
    ],
    "recommendation": "Add a non-blocking notification to the budget owner when a new seat is added via self-invite; real-time delivery is a nice-to-have, not required for v1.",
    "rationale": "Directly answers the open risk from Turn 1 (losing the only budget checkpoint) without reintroducing the approval friction the idea is meant to remove. Requester explicitly chose soft-and-eventual over hard-and-blocking when forced to pick.",
    "open_questions": [
      "Notification channel (email, in-app, Slack) not yet decided.",
      "Whether the budget owner can retroactively remove a seat after the fact is unresolved."
    ]
  },
  "tags": ["self-serve-invites", "billing"],
  "sources": [],
  "related_ids": ["risk-2026-08-20-losing-budget-checkpoint", "assumption-2026-08-20-approval-rarely-blocks"],
  "owner": "discovery-interview",
  "created_at": "2026-08-20T09:31:00Z",
  "updated_at": "2026-08-20T09:31:00Z"
}
```

---

## Turn 4 — user pauses the interview

**User:** "That's enough for now, I need to check with finance."

**Skill response:** Acknowledges the pause, briefly confirms what was
captured this session (one open assumption, one risk, two decisions, one
insight, all in `.claude/memory/self-serve-team-invites.json`), and stops.
It does **not** assess whether discovery is "complete" — that's explicitly
out of scope for this skill (see SKILL.md). No memory record is written
for this turn; stopping is not itself a finding.
