# Monthly Financial Close

**Cadence: business days 1–5 of the following month. Done by day 5 or it's late, and late closes
compound into a business that doesn't know what it earned.**

The weekly checklist keeps the business moving. The close is the only point where the numbers are
made *true* rather than approximately known. Weekly numbers are operational estimates; the close
produces the figures that decisions, taxes, and any future sale rest on.

**Everything here is analysis and reconciliation. No money moves from this process**
(`ops/compliance-line.md` §2.1). The output is a one-page statement plus a decision list for Carl.

---

## Preconditions — check before starting

The close will produce garbage if these aren't true. Check them first, fix or flag, then proceed.

- [ ] `accounts/master-register.md` was updated in the last week (Station 4 ran).
- [ ] All bank, card, and processor statements for the month are available and final.
- [ ] Prior month's close exists and its closing balances are recorded (they are this month's
      opening balances — if they disagree, stop and find out why before going further).
- [ ] The list of active experiments and channels is current.

---

## Day 1 — Cash and completeness

Purpose: know what actually arrived and left, with nothing missing.

1. **Opening balance** for every account = prior close's closing balance. Any mismatch is
   investigated before anything else happens. A mismatch is either a missed transaction last month
   or an account nobody is tracking.
2. **Transaction completeness.** Every account on the register has its statement pulled. Every
   statement pulled maps to an account on the register. **A statement with no register entry, or a
   register entry with no statement, is the single most common way a business loses track of money.**
3. **Categorise every line**, no exceptions bucket larger than 2% of spend. Categories:
   revenue, cost of delivery, tooling/subscriptions, advertising, contractors, fees
   (payment/platform), domains/hosting, taxes, owner draws, other.
4. **Closing balance** per account, and the total. Written down, dated.

Output: `close/YYYY-MM/01-cash.md` — opening, all lines categorised, closing, and a list of
unmatched items.

## Day 2 — Revenue, receivables, payables

Purpose: separate what was earned from what was collected, and know who owes whom.

1. **Revenue recognised** vs **revenue collected.** For one-off work these can differ by weeks; for
   subscriptions, recognise over the period served. State both numbers. Never report collected cash
   as earnings in a month where a large prepayment landed — that is how a good month gets invented.
2. **Revenue by source and by customer.** Then the concentration number: largest customer as % of
   month revenue, largest channel as % of month revenue.
3. **Receivables ageing:** 0–14, 15–30, 31–60, 60+ days past terms. Anything 31+ gets a named
   chase action with a date, carried into the weekly checklist's commitments station. Anything 60+
   gets a write-off recommendation or an escalation about terms.
4. **Payables and upcoming obligations** for the next 60 days, including every renewal from the
   register. This is where an annual renewal that nobody budgeted shows up before it bites.
5. **Fees as a line of their own.** Payment processing, platform take, FX. If total fees exceed ~5%
   of revenue, it goes on the decision list — fees are the most quietly compounding cost there is.

Output: `close/YYYY-MM/02-revenue.md`.

## Day 3 — Cost of delivery and true margin

Purpose: compute what it actually cost to serve customers, including the agent's and Carl's hours.

1. **Direct cost per unit delivered**, per offer. Include: contractor cost, payment fees, hosting
   attributable to delivery, licensed data, and **hours at an explicit rate**.
2. **Hours are not free.** Log them at a stated rate, and state the rate in the document
   (`business/unit-economics` uses $50/h as a default placeholder for skilled work — use the real
   number if it is known, and say which it is). Free labour makes almost anything look viable; it
   is the single most common lie in a small business's own accounts.
3. **Gross margin per offer** = revenue per unit − direct cost per unit. In dollars and as a %.
4. **Fixed costs** for the month, from the register: everything that would be charged whether or
   not a customer existed.
5. **Break-even volume** = fixed costs ÷ gross margin per unit, per offer. Compare against actual
   volume. **State how far above or below break-even each offer ran**, in units, not vibes.
6. **Contribution by offer.** Rank the offers. Expect to find that one offer carries the business
   and one loses money quietly. Both findings are actionable; neither is visible weekly.

Output: `close/YYYY-MM/03-margin.md`.

## Day 4 — Unit economics refresh and the kill/continue pass

Purpose: replace last month's assumptions with this month's measurements, then act on them.

### 4a. Refresh the unit-economics table (`business/unit-economics`)

For **every active acquisition channel**, recompute all seven numbers in three columns
(pessimistic / base / optimistic):

| | Pessimistic | Base | Optimistic |
|---|---|---|---|
| Reply or click rate | | | |
| Lead→customer | | | |
| CAC (incl. hours) | | | |
| Gross margin/unit | | | |
| Payback (months) | | | |
| LTV:CAC | | | |
| Break-even customers/mo | | | |

**Rule that makes this refresh meaningful: every benchmark that now has ≥30 days of real data is
replaced by the measured number, and the replacement is noted.** A table that still contains the
same industry benchmarks it started with after two closes is not being instrumented — and that
fact is the finding, ahead of whatever the table says.

### 4b. Kill / continue / scale — every channel and every experiment

Per `business/experiment-engine` and `thinking/decision-quality`:

| Decision | Condition |
|---|---|
| **Kill** | Pessimistic-column LTV:CAC < 2:1 after 30+ days, or it passed its pre-written kill criterion, or CAC exceeds first-purchase margin on a one-off offer |
| **Continue** | Clears the gates but hasn't reached the sample size its hypothesis needs — state the date it will have |
| **Scale** | Clears on the **pessimistic** column, and the binding constraint on scaling is named |

Two rules of hygiene:
- **Nothing continues by default.** An item not explicitly decided is killed. Default-continue is
  how a portfolio fills with zombies.
- **Name the binding constraint** before scaling anything — usually reachable list size, delivery
  hours, or deliverability, rarely price. Scaling without naming it just relocates the bottleneck.

### 4c. Offer-level decisions
Run `business/offer-design` on any offer that ran below break-even two months in a row. The options
are: raise price, cut delivery cost, change the target, or retire it. "Try harder" is not one.

Output: `close/YYYY-MM/04-decisions.md`.

## Day 5 — Statement, reserves, and the decision list

1. **The one-page monthly statement** (format below). One page. If it needs two, the categories
   are wrong.
2. **Tax reserve.** Set aside a stated % of net as a reserve line, tracked cumulatively.
   *The percentage and the jurisdiction treatment are a human/accountant decision — the agent
   tracks the reserve against whatever figure Carl sets, and escalates if no figure is set.*
   An untracked tax reserve is an unfunded liability growing quietly all year.
3. **Trailing trend.** Revenue, gross margin, fixed cost, net, and runway for the last 6 months in
   one small table. Direction matters more than the month.
4. **Runway** = cash ÷ average net burn of last 3 months. Report the number of days, and the date
   it hits zero at current burn.
5. **Subscription audit** (quarterly, and any month fixed costs rose >10%): every recurring line
   from the register, with last-used date. Unused 60+ days → cancel recommendation with the
   annualised number attached, because $19/mo reads as nothing and $228/yr reads as a decision.
6. **The decision list** — the actual deliverable for Carl. Each item: the decision, the number,
   the recommendation, the cost of not deciding.
7. **Prior month's decision list reviewed.** Which were decided, which lapsed. **A decision list
   item that has lapsed twice gets its cost-of-delay restated, larger and in the first line.**

Output: `close/YYYY-MM/05-statement.md` and `close/YYYY-MM/DECISIONS.md`.

---

## The one-page monthly statement

```
# Monthly Statement — YYYY-MM            Closed: <date>  (target: BD5)

CASH          Opening <x>   Closing <y>   Net change <z>
              Runway <n> days at 3-mo avg burn — zero-date <date>

REVENUE       Recognised <a>   Collected <b>
              By source:  <source: amount>  ...
              Concentration: largest customer <p%>, largest channel <q%>

COSTS         Cost of delivery <c>   (of which hours at $<rate>/h: <h>)
              Fixed <d>              Fees <e>  (<f%> of revenue)

MARGIN        Gross margin <g> (<g%>)    Net <n>
              Break-even: needed <u> units, ran <v> units

PER OFFER     <offer>: rev <r>, GM <m> (<m%>), vs break-even <±u>
              ...

CHANNELS      <channel>: CAC <c>, payback <p>mo, LTV:CAC <r> [pessimistic col]
              Decisions: killed <...>  continued <...>  scaled <...>

RESERVES      Tax reserve this month <t>, cumulative <T>   [rate set by Carl: <x%> | UNSET]

UNKNOWNS      <field> — UNKNOWN, would be obtained by <method>

TOP 3 DECISIONS FOR CARL
  1. <decision> — <number> — <recommendation> — cost of delay: <...>
  2. ...
  3. ...
```

---

## Close quality gates — the close is not done until all five pass

1. **Opening balances tie to last month's closing balances**, every account.
2. **Every statement line is categorised**, exceptions bucket ≤2% of spend.
3. **Every register entry has a matching charge, and every charge has a register entry.**
4. **Hours are costed at an explicit, stated rate** — not omitted, not implied.
5. **Every UNKNOWN is written as `UNKNOWN + how I'd get it`** — never a plausible guess
   (`business/unit-economics`).

Then, and only then: re-read each output file from the repo and confirm it exists before declaring
the close complete (`ops/compliance-line.md` §2.6).

---

## Cross-references (all verified present on 2026-09-16)

- `agent-skills: business/unit-economics` — the seven numbers, three columns, the gates
- `agent-skills: business/experiment-engine` — kill criteria and sample size
- `agent-skills: business/offer-design` — what to do with an unprofitable offer
- `agent-skills: thinking/decision-quality` — reversibility, default-kill discipline
- `agent-skills: thinking/evidence-grading` — how much weight a measured number carries
- This repo: `accounts/master-register.md` (the cost side's source of truth),
  `ops/weekly-operations-checklist.md` (Station 5 feeds this), `ops/compliance-line.md`
