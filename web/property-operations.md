# Web Property Operations

**Running a web property as an asset, not as a project.**

A project ends. An asset is operated: it is up, it is fast, it is measured, it converts, and it
improves on one axis at a time. This file is the operating discipline for every site, landing page,
or tool the business runs. It is executed weekly as Station 3 of
`ops/weekly-operations-checklist.md`.

The design and build skills already exist and are not restated here:
- `agent-skills: design/web-page-build` — how to build the page
- `agent-skills: design/brand-and-design-system` — how it should look, consistently
- `agent-skills: media/image-generation`, `agent-skills: media/video-production` — assets

This file covers what happens after it's live, forever.

---

## Every property has a property card

One per property, in `web/properties/<name>.md`. A property without a card is not operated; it's
just something that exists on the internet with the business's name on it.

```
# Property: <name>
URL(s):              <primary + any alternates/redirects>
Register ID:         <accounts/master-register.md entry>
Purpose:             <one sentence: what job this property does>
Primary conversion:  <the ONE action that counts as success>
Secondary:           <at most one>
Owner:               <named human>
Stack:               <host / framework / CMS / builder>
Deploy method:       <how a change goes live; how it rolls back>
Rollback tested:     <date it was actually tested — not "should work">
Analytics:           <tool + property ID + where the event spec lives>
Uptime monitor:      <tool + check URL + alert destination>
Forms/inputs:        <every surface that accepts user input>
Data collected:      <what personal data, stored where, under what basis>
Perf budget status:  <pass/fail per the budgets table>
Content cadence:     <how often, and who writes it>
Current focus metric:<the ONE metric this property is being improved on now>
Baseline (4wk avg):  <that metric's current level>
Change log:          <date → change → metric moved / didn't>
Kill/sell criteria:  <the pre-written conditions to retire or sell it>
```

Two fields do most of the work:

- **`Primary conversion` must be exactly one action.** A property optimising for three things is
  optimising for none, and its conversion rate becomes uninterpretable.
- **`Current focus metric` must be exactly one.** See "the one-change rule" below.

---

## The five weekly checks (Station 3)

### 1. Up?
Uptime over the last 7 days, plus any incident with its duration. Monitoring must alert somewhere a
human sees it — a monitor whose alerts go to an unread inbox is decoration.

### 2. Fast? — performance budgets

**Core Web Vitals thresholds, verified against the primary source
([web.dev/articles/vitals](https://web.dev/articles/vitals), last updated 2024-10-31, read
2026-09-16):**

| Metric | "Good" threshold | Measures |
|---|---|---|
| **LCP** — Largest Contentful Paint | **≤ 2.5 s** | Loading |
| **INP** — Interaction to Next Paint | **≤ 200 ms** | Interactivity |
| **CLS** — Cumulative Layout Shift | **≤ 0.1** | Visual stability |

The measurement rule matters as much as the numbers: **assess at the 75th percentile of page loads,
segmented across mobile and desktop.** A page "passes" only if all three metrics meet their target
at p75. Optimising the median hides the experience of the slowest quarter of real users — and mobile
is usually where a property actually fails.

Additional self-imposed budgets (these are our operating choices, not Google's standard — graded as
such per `agent-skills: thinking/evidence-grading`):

| Budget | Ceiling | Why |
|---|---|---|
| Total page weight | ≤ 1 MB on the primary landing path | Directly drives LCP on mobile networks |
| Third-party scripts | ≤ 3, each named and justified | Each one is a performance and privacy liability |
| Web fonts | ≤ 2 families, ≤ 4 weights, self-hosted or preconnected | Fonts are the most common CLS and LCP cause |
| Above-fold images | Explicit dimensions, correctly sized | Missing dimensions is the classic CLS bug |
| Blocking requests before first paint | 0 avoidable | — |

**Regression rule: if a deploy moves any Core Web Vital out of "good" at p75, that is a rollback
candidate, not a backlog item.** Performance is reclaimed at 10× the cost of not losing it.

### 3. Measured? — the minimum analytics event set

**A property with broken analytics is treated as down.** You cannot operate what you cannot see,
and every optimisation decision made on broken data is worse than no decision.

Minimum event set, verified firing weekly:

| Event | Why it's mandatory |
|---|---|
| `page_view` with path and referrer | Baseline traffic and source |
| `primary_conversion` | The one action that defines success |
| `form_start` and `form_submit` | The gap between them is usually the largest recoverable loss |
| `form_error` with field name | Reveals the specific field killing the form |
| `cta_click` with location | Distinguishes "nobody saw it" from "nobody wanted it" |
| `outbound_click` for payment/booking handoffs | Where the funnel leaves your instrumentation |
| `scroll_depth` at 50% / 90% on content pages | Distinguishes "didn't read" from "read and didn't act" |

Rules:
- **Name events once and never rename them.** A renamed event breaks all historical comparison,
  which is the only thing analytics is for.
- **Verify by firing them**, not by reading the config. A tag that exists and doesn't fire is the
  default state of analytics.
- **No tracking that the privacy policy doesn't describe**, and none at all on cold-email landing
  paths where a pixel is itself a consent question (`agent-skills: sales/outbound-compliance`).
- Keep third-party analytics count at one. Two tools that disagree produce meetings, not decisions.

### 4. Converting?
Primary conversion rate this week vs. the 4-week average, plus the funnel:
`sessions → cta_click → form_start → form_submit → primary_conversion`, each step as a rate.

**Report the step with the largest absolute loss, not the lowest rate.** A 20% step on 1,000 users
loses more than a 60% step on 50.

### 5. Growing?
Indexed pages, impressions, top queries, and direction only. Weekly search data is noise; the value
of looking weekly is catching a *cliff*, not reading a trend. A sudden drop in indexed pages or
impressions is an incident (robots.txt, noindex, a broken deploy, a penalty) and gets investigated
that day.

---

## The one-change rule

**Exactly one deliberate change per property per week, chosen for the worst number, logged with the
date.**

This is the most important operating constraint in this file and the one most likely to be
resented. Ten changes in a week produce zero learning: whichever way the metric moves, nothing is
attributable, and the property accumulates modifications nobody can evaluate or revert.

- The change targets the **current focus metric**, which stays fixed until it moves or is abandoned.
- It is logged: date → what changed → what was predicted → what happened.
- **Write the prediction before shipping.** A change with no prediction can't be wrong, which means
  it can't teach anything (`agent-skills: business/experiment-engine`).
- Exempt from the rule: security fixes, broken-thing fixes, and content published on the normal
  cadence. Those aren't experiments.
- Statistical honesty: most small-site conversion differences over one week are noise. Either run
  to a sample size the hypothesis needs, or label the result as directional. `business/experiment-engine`
  governs; do not declare a winner from 40 sessions.

---

## Deploy and rollback

1. **Every property has a rollback path, and its `Rollback tested` date is real.** Untested rollback
   is a belief about rollback.
2. Preview/staging for anything structural. Content edits can go direct.
3. Post-deploy smoke test, always the same four: homepage renders, primary conversion path
   completes end-to-end, a form actually submits and the submission arrives somewhere a human sees,
   analytics events fire.
4. **Never deploy a structural change and a content change together** — you lose attribution for
   both and double the surface to bisect.
5. Deploys are logged in the property card's change log.

---

## Content cadence

- Consistency beats volume. A stated cadence that is met is worth more than a larger cadence missed.
- Every piece has: a target query or job, one internal link in from an existing page, and a next
  action for the reader. Content with no next action is a cost centre.
- **Refresh beats publish.** Updating a page that already has impressions usually outperforms a new
  page. Station 3's "top queries" output is the input to choosing which.
- Every claim in published content passes `ops/compliance-line.md` §4. No invented proof, no
  invented persona, no regulated-class claim without human sign-off.

---

## Security and input-surface baseline

Every form is an attack surface and a spam target.

- [ ] HTTPS enforced, HSTS on, no mixed content
- [ ] Every form: server-side validation, rate limiting, and a non-CAPTCHA spam control (honeypot
      or timing check) before reaching for a CAPTCHA that costs conversions
- [ ] **Submissions arrive somewhere a human sees.** A silent form is worse than no form: it
      converts a prospect into someone who thinks you ignored them
- [ ] Secrets never in client-side code, never in the repo (`ops/compliance-line.md` §2.2)
- [ ] Dependencies patched; automated advisory alerts enabled
- [ ] Privacy policy matches what is actually collected, and a deletion request can be executed
- [ ] **New input surface → security review before it stays up.** Use
      `trailofbits/skills: security-review` from the vetted registry
- [ ] Domain/DNS expectations recorded in the register; unexplained DNS drift is a security event

---

## Scorecard row (one line per property, weekly)

| Property | Up % | LCP p75 | INP p75 | CLS p75 | Analytics OK | Conv % | Δ vs 4wk | Focus metric | Change made |
|---|---|---|---|---|---|---|---|---|---|

---

## Kill / sell criteria — written when the property launches, not when it disappoints

Every property card names, in advance:

| Condition | Default action |
|---|---|
| No primary conversion in 90 days despite traffic | The offer or the audience is wrong → `business/offer-design`, or retire |
| No traffic in 90 days despite publishing | The channel is wrong → retire or repoint the domain |
| Operating cost exceeds attributable contribution for 2 closes | Retire, or sell via `deals/buy-sell-deal-desk.md` |
| Focus metric unmoved after 8 logged weekly changes | The constraint is elsewhere; stop optimising the page |
| Requires >2 owner-hours/week with no revenue line | It is a hobby. Say so, then decide |

**Retirement is an operation, not an abandonment**: redirect or park the domain deliberately, export
the content and data, revoke integrations, update the register with a `cancelled` date, keep the row.
A property left to rot still costs money, still holds personal data, and still carries the brand.

---

## Cross-references (all verified present on 2026-09-16)

- `agent-skills: design/web-page-build`, `agent-skills: design/brand-and-design-system`
- `agent-skills: media/image-generation`, `agent-skills: media/video-production`
- `agent-skills: business/experiment-engine` — the one-change rule, sample size, predictions
- `agent-skills: business/offer-design` — when conversion is an offer problem, not a page problem
- `agent-skills: business/unit-economics` — attributable contribution vs. operating cost
- `agent-skills: thinking/evidence-grading` — self-imposed budgets vs. published standards
- `agent-skills: sales/outbound-compliance` — tracking and consent on cold landing paths
- Vetted registry: `trailofbits/skills: security-review`
- This repo: `ops/weekly-operations-checklist.md` Station 3, `accounts/master-register.md`,
  `ops/compliance-line.md`, `deals/buy-sell-deal-desk.md`
- External, verified: [web.dev/articles/vitals](https://web.dev/articles/vitals) — LCP/INP/CLS
  thresholds and the p75 measurement rule
