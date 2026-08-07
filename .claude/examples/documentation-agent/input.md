# Example: documentation-agent — input

Unlike every other agent's example in this platform, `documentation-agent`
takes no target record id — it reads the **entire** project memory store.
This example uses the complete, 26-record `self-serve-team-invites` store
accumulated across every prior example: 6 records from
`.claude/examples/discovery-interview/`, 9 from
`.claude/examples/research-agent/`, 2 from
`.claude/examples/strategy-agent/`, 4 from
`.claude/examples/solution-design/`, and 5 from
`.claude/examples/systems-impact/`. Rather than re-embedding all 26
records here, this file lists them by id — the full JSON for each already
lives in the example that produced it.

Project slug: `self-serve-team-invites`. Store:
`.claude/memory/self-serve-team-invites.json`.

| Type | Count | Ids |
|---|---|---|
| `insight` | 2 | `insight-2026-08-18-support-escalation-origin`, `insight-2026-08-20-billing-tied-to-approval` |
| `assumption` | 2 | `assumption-2026-08-20-approval-rarely-blocks`, `assumption-2026-08-20-budget-owner-notification-preference` |
| `risk` | 3 | `risk-2026-08-20-losing-budget-checkpoint`, `risk-2026-09-02-digest-misses-burst-spend`, `risk-2026-09-10-silent-digest-failure` |
| `decision` | 2 | `decision-2026-08-20-restrict-invite-to-full-members`, `decision-2026-09-02-budget-owner-notification-mechanism` (tagged `strategy-recommendation`) |
| `research-source` | 6 | 2 each for TeamFlo, Huddleworks, RosterPilot — see `.claude/examples/research-agent/` |
| `competitor` | 3 | `competitor-2026-08-25-teamflo`, `competitor-2026-08-25-huddleworks`, `competitor-2026-08-26-rosterpilot` |
| `requirement` | 4 | all `elaborates_decision_id: decision-2026-09-02-budget-owner-notification-mechanism` — see `.claude/examples/solution-design/` |
| `system-impact` | 4 | across `monitoring`, `customer-support`, `data-model`, `customer-communication` — see `.claude/examples/systems-impact/` |

No `persona`, `market-signal`, `opportunity`, or `feature-idea` records
exist anywhere in this store — no skill in this platform has ever
produced them. This is deliberate: it's what makes this example a real
test of the honesty rule rather than a store engineered to fill every
section.

## The existing Capability Matrix

`research-agent`'s example describes what it would render to
`.claude/outputs/self-serve-team-invites/capability-matrix/capability-matrix.md`
(kept as `.claude/examples/research-agent/capability-matrix.example.md`
in this repo, since `.claude/outputs/` itself stays empty until a
skill/agent actually runs for real — see `.claude/outputs/README.md`).
For this example, `documentation-agent`'s Competitive Landscape section
treats that path as if the matrix were already sitting there, and points
to it rather than regenerating its content from the `competitor` records.

See `output.md` for how the store's 26 records map onto the 35 canonical
sections, and `product-decision-document.example.md` for the full
rendered document.
