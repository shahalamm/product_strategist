# Example: interactive-map-agent — rendering notes and verification

Continues from `input.md`. `interactive-map.example.html` is the actual
artifact — this file explains the rendering decisions and how it was
verified to actually work, not just parse.

## What's on the map, and what isn't

Per `input.md`'s Step 1 analysis, exactly one axis-pair exists (Approval
Friction × Budget Visibility Immediacy), and exactly two entities qualify
for it:

- **TeamFlo** — (Approval Friction: 1, Budget Visibility Immediacy: 5)
- **Huddleworks** — (Approval Friction: 5, Budget Visibility Immediacy: 1)

RosterPilot and Us are both named in an "Not plotted on this map" box on
the page itself, with the specific score they *do* have and which axis
they're missing — not silently dropped, not forced onto the chart with a
guessed Approval Friction value.

## The "Us" hollow-marker rule exists in the code, but isn't visually exercised here

Per Step 2 of `.claude/agents/interactive-map-agent.md`, "Us" should
render as a hollow marker whenever its `own-position` status is
`"draft"`. The rendered page's JS implements this generically
(`pointBackgroundColor`/`pointBorderColor`/`pointBorderWidth` functions
keyed on `isUs`/`status`) — but **no point on this particular chart has
`isUs: true`**, because Us doesn't qualify for the one pair this store
currently supports. The rule is real, working code, not a placeholder,
but this example's actual data can't visually demonstrate it — see
"Verification" below for direct proof the logic itself is correct.

## Verification

Static syntax and structure checks:
- HTML tag balance checked with Python's `html.parser` — no mismatched
  or unclosed tags.
- The inline `<script>` block's JS was extracted and checked with
  `node --check` — valid syntax.

Real execution checks, in headless Chromium (Playwright, the browser
already installed in this environment):

- Loaded the actual file. Chart.js itself is fetched from
  `cdn.jsdelivr.net`, which this sandbox's own egress policy blocks
  (confirmed via `/root/.ccr/README.md`'s status endpoint — a
  pre-existing, unrelated network restriction, not a defect in this
  file; a normal browser with internet access loads it directly). To
  test *this file's own logic* independent of that restriction, the CDN
  request was intercepted and fulfilled with a small local stub
  implementing only the `Chart` constructor surface this page actually
  calls — everything downstream of that (data building, the click
  handler, the tooltip callback, the point-style functions) is this
  page's real, unmodified code, exercised directly:
  - **Zero console/page errors.**
  - **Exactly 2 points built** for the chart, labeled `TeamFlo` and
    `Huddleworks` — confirming RosterPilot and Us were correctly excluded
    by the page's own data, not just by the prose around it.
  - **Clicking each point** (via the real `onClick` handler) populated
    the detail panel with the full, verbatim rationale text for both
    axes and the correct source record id, for both TeamFlo and
    Huddleworks.
  - **The tooltip label callback**, called directly, returned the same
    full rationale text and source id as a multi-line array — confirming
    hover would show the same information as click.
  - **The point-style functions**, called directly with synthetic
    `isUs`/`status` combinations: `{isUs: true, status: "draft"}` →
    transparent background, colored border, width 3 (hollow, thicker
    border); `{isUs: true, status: "active"}` → filled, same color
    (distinct but not hollow); `{isUs: false, status: "active"}` →
    filled, default color. This confirms the hollow-marker rule is
    correctly implemented and ready for the day a project has "Us" fully
    scored on a shared pair — it just isn't triggered by today's data.

## What's deliberately absent

No score, axis, or position was invented to get RosterPilot or Us onto
this map. No radar/spider view was attempted — this store has never had
more than 2 distinct axes, so there's nothing to build one from yet. No
other artifact type (Capability Matrix, Product Decision Document) was
touched or regenerated.
