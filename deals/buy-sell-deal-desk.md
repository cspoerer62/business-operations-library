# Buy / Sell Deal Desk

**The operating system for acquiring and disposing of assets** — websites, domains, small online
businesses, content libraries, customer lists, app or tool properties.

A deal desk exists for one reason: **deals are where a small business makes its largest,
least-reversible decisions with the least practice.** A weekly marketing mistake costs a week. A
bad acquisition costs the balance sheet and eighteen months of attention. So the process here is
deliberately slower than it feels necessary, and structured so that the expensive failure modes
have to be walked past on purpose rather than tripped into.

**What the agent does and does not do here, before anything else:**

| The agent | Carl |
|---|---|
| Sources, screens, values, diligences, drafts, models, red-teams, tracks | Decides, signs, pays, transfers |
| Produces the recommendation with the number and the pessimistic case | Executes every irreversible step |

No agent tooling can move money, sign, or transfer an asset (`ops/compliance-line.md` §1, §2.1).
Every stage gate below that involves money or signature is a **handoff**, marked `[CARL]`.

---

## Pipeline stages

| # | Stage | Exit condition | Typical kill rate |
|---|---|---|---|
| 0 | **Thesis** | A written one-paragraph thesis exists | — |
| 1 | **Intake** | Record created with the eight intake fields | — |
| 2 | **Screen** | Passes all five screening gates | ~80% killed here |
| 3 | **Value** | Three valuation methods computed, range stated | ~40% of survivors killed |
| 4 | **Offer** `[CARL]` | LOI/offer delivered | — |
| 5 | **Diligence** | Every checklist item either verified or listed as an accepted risk | ~25% killed here |
| 6 | **Close** `[CARL]` | Funds and asset exchanged, both confirmed | — |
| 7 | **Handoff** | 30-day plan executed, asset on the register, first close reflects it | — |

**A deal that cannot state which stage it is in is in stage 1.** Ambiguity about stage is how deals
drift for months.

---

## Stage 0 — Thesis (write this before looking at listings)

One paragraph, written *before* browsing, answering:
1. **What kind of asset**, and why that kind fits what this business can actually operate?
2. **What is the edge** — why would this asset be worth more under us than under the seller?
   "It's cheap" is not an edge; it's a hope about the market.
3. **What is the budget ceiling and the cash floor** (the cash level below which no deal happens)?
4. **What is the disqualifier** — the property type we will not buy regardless of price?

Without a thesis, screening becomes reactive and every listing looks interesting. Per
`agent-skills: business/opportunity-scan`, the thesis is what converts a stream of opportunities
into a filter instead of a distraction.

---

## Stage 1 — Intake

Eight fields, no more, for every candidate. If the seller won't provide these, that is itself data.

| Field | Note |
|---|---|
| Source / URL | Where it came from; broker, marketplace, direct approach |
| Asking price | And whether it's a stated multiple |
| Claimed revenue (TTM, monthly) | Claimed — the word matters |
| Claimed profit / SDE | Seller's Discretionary Earnings, and what they added back |
| Age of asset | Under 12 months is a different risk class entirely |
| Traffic / customer source | One channel or several |
| What's actually being sold | Domain? Code? Customers? Contracts? Brand? Staff? |
| Seller's reason for selling | Weak or evasive answers correlate strongly with hidden problems |

---

## Stage 2 — Screen: five gates, all must pass

Fast, cheap, and the most valuable stage in the desk. ~80% should die here, in under 20 minutes each.

| # | Gate | Kill if |
|---|---|---|
| 1 | **Fits the thesis** | It's a different asset class or requires capability we don't have |
| 2 | **Price is in the plausible zone** | Asking price is >2× the top of a rough multiple range for the category (see valuation) |
| 3 | **Revenue is verifiable in principle** | Income is only evidenced by screenshots, or the payment account can't be shown live |
| 4 | **Not single-point-dependent on something we can't inherit** | Traffic is one platform's algorithm, one expiring contract, one person's relationships, or one account that cannot legally transfer |
| 5 | **Clean enough to own** | Prior penalties, trademark conflict, adult/regulated/greyzone content, purchased-list-built email base, or unclear IP/licence provenance |

Gate 5 deserves emphasis. **An inherited compliance problem is worse than an inherited technical
problem**, because you cannot refactor it and it comes with a history. A list built by scraping or
purchase cannot be made lawful after the fact — it has to be re-permissioned, which usually means
losing most of it (`agent-skills: sales/outbound-compliance`).

Run `agent-skills: research/competitor-teardown` on any survivor to understand the competitive
position before spending time on valuation. A property winning a niche nobody is contesting and a
property losing a niche three funded companies are contesting can have identical financials.

---

## Stage 3 — Value: three methods, always, and a range not a number

Never present a single figure. Compute all three, then state a range and which method binds.

### 3a. SDE multiple (the primary method for small profitable assets)
`Value = SDE × multiple`, where SDE = net profit + owner's salary + genuinely discretionary/one-off
addbacks.

**Interrogate every addback.** Sellers add back things that are actually operating costs. An addback
for "owner's time" on an asset that requires 20 hrs/week of owner time is not discretionary — it is
a cost the buyer inherits. Recompute SDE with the buyer's real cost of running it, including hours
at an explicit rate (`agent-skills: business/unit-economics`).

Multiple moves with, in rough order of impact: revenue durability and diversification, growth
direction, owner-hours required, customer concentration, age, and how transferable the traffic is.
**Do not import a multiple from a benchmark table without saying what it's based on and grading it
per `agent-skills: thinking/evidence-grading`.** Marketplace "average multiple" figures are
selection-biased toward what sold, not what was listed.

### 3b. Revenue multiple (for unprofitable or pre-profit assets)
Only meaningful with a credible path to margin. State the path, and the cost of walking it.
**If profitability requires us to do something the seller couldn't, say what it is and why we can.**

### 3c. Asset floor / rebuild cost
What would it cost to build this from zero — domain, content, code, customers, brand? This is the
**walk-away floor**: never pay meaningfully above rebuild cost unless you're buying time, and if
you're buying time, say how many months and what those months are worth.

### The valuation output

```
ASSET: <name>
SDE (seller's): <x>        SDE (recomputed, buyer's costs): <y>
  Addbacks rejected: <list with reason>
Method A — SDE multiple:    <y> × <m..M> = <range>    [multiple basis + evidence grade]
Method B — Revenue multiple: <rev> × <m..M> = <range> [path to margin: ...]
Method C — Rebuild cost:     <range>                   [the walk-away floor]
RANGE: <low>–<high>    BINDING METHOD: <which, and why>
MAX OFFER (pessimistic column only): <number>
PAYBACK at max offer: <months>
WALK-AWAY PRICE: <number>   ← written down before any negotiation
```

**The walk-away price is written down before talking to the seller, and it does not move during
negotiation.** Every mechanism that makes deals go bad — anchoring, sunk diligence cost,
competitive bidding, seller urgency — operates by moving that number after the fact.

---

## Stage 4 — Offer `[CARL]`

Agent drafts; Carl sends and signs. Nothing here is agent-executable.

Structure preferences, in order:
1. **Earn-out or holdback** on any revenue claim that couldn't be independently verified. This is
   the single best defence against overstated income: make the disputed part contingent.
2. **Staged payment** tied to verified post-close performance or completed transfer milestones.
3. **Escrow** for anything above a trivial sum, and for every asset requiring a transfer window
   (domains, accounts, code, customer consent). Escrow releases on *confirmed transfer*, not on
   promised transfer.
4. **Asset purchase over entity purchase** where possible — buying an entity inherits its
   liabilities and history; buying assets generally doesn't. *Which structure is appropriate, and
   its tax and liability consequences, is a lawyer/accountant decision — escalate, don't decide.*

Always in the offer: an exclusivity window for diligence, the diligence list, transfer
responsibilities, a non-compete scope, and what happens to customer data (including the lawful
basis for transferring it at all).

**Before sending: run `agent-skills: thinking/premortem-red-team`.** It is an irreversible act with
a number attached, which is exactly its trigger condition.

---

## Stage 5 — Diligence

The rule: **every item is either verified with evidence, or written down as an accepted risk with a
price attached.** There is no third state. "Seller says" is not verification.

### Financial
- [ ] Live screen-share into the payment processor and bank — not exports, not screenshots
- [ ] 24 months of revenue by month; seasonality visible; any step-change explained
- [ ] Revenue by customer → concentration; by channel → dependency
- [ ] Refund and chargeback rate (assume 5–10% if unknowable, per `business/unit-economics`)
- [ ] Every cost line, including ones the seller absorbs personally and we'd have to pay for
- [ ] Recomputed SDE with buyer's costs and hours at an explicit rate

### Traffic and demand
- [ ] Analytics access, read directly, ≥24 months; compare to the seller's claimed numbers
- [ ] Traffic sources — organic / paid / referral / direct / social split and trend
- [ ] **Search visibility trend, and any penalty or algorithmic-drop history.** A property whose
      organic traffic fell 60% eighteen months ago and stabilised is a different asset than its
      current flat line suggests
- [ ] Top pages and top queries, and whether they're defensible or a single lucky ranking
- [ ] Paid channels: is the reported CAC computed with the same definition we'd use?

### Legal, IP, compliance
- [ ] Ownership of domain, code, content, trademarks — chain of title actually shown
- [ ] Licences for all third-party content, code, images, fonts, and data. **AI-generated or
      stock content with unclear licence terms is a real risk class, not a footnote**
- [ ] Contracts: customers, suppliers, contractors — assignable? on what notice?
- [ ] **Email list provenance.** How was every address obtained? Is there an opt-in record? A list
      without provenance is worth zero and is a liability (`sales/outbound-compliance`)
- [ ] Personal data: what's held, where, under what basis, and is transfer lawful? Privacy policy
      consistent with actual practice?
- [ ] Any disputes, takedowns, platform strikes, or regulator contact, ever

### Technical and operational
- [ ] Code reviewed; dependencies current; anything unmaintainable named
- [ ] **Security review on every user-input surface and every credential path** — use
      `trailofbits/skills: security-review` from the vetted registry
- [ ] Infrastructure and its real monthly cost at current traffic
- [ ] Every account and service that must transfer, and whether each *can* legally transfer
- [ ] Backups exist **and a restore has been performed**, not just claimed
- [ ] Honest owner-hours per week to operate, from the seller's actual calendar where possible

### The four diligence red flags that end a deal
1. **Refusal of live access** to the payment processor or analytics. There is no acceptable reason.
2. **Claimed numbers don't reconcile** to the live accounts, and the gap isn't explained on the spot.
3. **Traffic concentrated in one source that is not transferable** — a personal account, a
   relationship, an expiring contract, a platform that forbids assignment.
4. **List or content provenance cannot be evidenced.**

Any of these: **kill, don't renegotiate.** Discounting a deal with an unverifiable core is buying
the same problem for less money.

---

## Stage 6 — Close `[CARL]`

1. Funds via escrow where the sum warrants it; release on **confirmed** transfer.
2. Transfer sequence, in this order, each confirmed before the next: domain (unlock → auth code →
   transfer → confirm resolution) → hosting/code → analytics and search consoles → payment
   processor and subscriber migration → email/list with its consent records → social and
   directory profiles → supplier and customer notifications.
3. **Credentials are rotated on receipt, every single one.** The seller's credentials are assumed
   compromised by default — not from suspicion, but because their exposure history is unknowable.
4. **Every transferred asset becomes a register entry the same day**
   (`accounts/master-register.md`), with owner, cost, credential location, and data-held fields.
5. The seller's access is revoked and verified revoked, item by item, against the access list.

## Stage 7 — Handoff and the first 30 days

1. **Change nothing structural for 30 days**, except security fixes and broken things. Learn the
   asset's baseline first. Most value destroyed post-acquisition is destroyed in month one by a
   buyer improving something they hadn't yet measured.
2. Instrument it: the minimum analytics event set from `web/property-operations.md` before any
   optimisation. An unmeasured change is not an experiment.
3. Compute real unit economics from *our* costs, and compare against the model that justified the
   purchase. **Write down the variance.** This is the only way the desk's valuation method improves.
4. Add it to the weekly checklist's Station 3 rotation and to the next monthly close.
5. **Post-mortem at day 30, every deal, win or lose**: what the model said, what reality said, which
   diligence item would have caught the gap. Feed it back into this file.

---

## Selling an asset

The same desk, reversed, with three additions:

1. **Prepare the asset for verification.** The single biggest discount a seller takes is for
   unverifiable numbers. Clean books, separated accounts, documented traffic, and a restorable
   backup are worth more than any last-minute growth push.
2. **Separate before selling.** Shared infrastructure, shared mailboxes, shared payment accounts,
   or shared content between the asset and the rest of the business all destroy value and complicate
   transfer. Untangle first — and do not hand over a credential that also unlocks something retained.
3. **Never let the buyer's diligence become a data leak.** Staged disclosure: aggregate figures →
   NDA `[CARL]` → account-level detail → live access only under exclusivity. Customer personal data
   is disclosed last, minimally, and its transfer needs a lawful basis of its own.

Set the walk-away *floor* before listing, for the same reason buyers set a ceiling.

---

## Portfolio-level rules

- **No deal that takes runway below 60 days**, at any price, ever (weekly stop rule).
- **No deal whose worst case is unsurvivable**, regardless of expected value. Ruin is not a number
  you can average against (`agent-skills: thinking/decision-quality`).
- **One deal at a time in diligence.** Two concurrent diligences means one of them got a shallow one.
- **A killed deal stays killed** unless the specific gate that failed has verifiably changed.
  Re-opening a killed deal because the price dropped is how gate 5 problems get bought.

---

## Cross-references (all verified present on 2026-09-16)

- `agent-skills: business/opportunity-scan` — thesis and filtering
- `agent-skills: business/unit-economics` — SDE recomputation, payback, pessimistic column
- `agent-skills: research/competitor-teardown` — competitive position of a target
- `agent-skills: research/public-web-research-at-scale` — pre-contact research, politely
- `agent-skills: thinking/premortem-red-team` — mandatory before any offer
- `agent-skills: thinking/decision-quality` — reversibility, ruin, walk-away discipline
- `agent-skills: thinking/evidence-grading` — "seller says" vs. live account access
- `agent-skills: sales/outbound-compliance` — list provenance, data transfer basis
- Vetted registry: `trailofbits/skills: security-review` — for inherited code and input surfaces
- This repo: `accounts/master-register.md`, `web/property-operations.md`, `ops/compliance-line.md`,
  `ops/monthly-financial-close.md`
