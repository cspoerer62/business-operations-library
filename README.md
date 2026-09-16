# business-operations-library

**The operational layer that makes the skills in [`agent-skills`](https://github.com/cspoerer62/agent-skills)
actually run.**

A skill tells you how to do a thing well. It does not tell you what to do on a Tuesday, what has to
be true before you send, which number proves a channel works, or who owns the domain that's about
to expire. That is what this repo is: the calendar, the runbooks, the registers, and the lines that
don't get crossed.

---

## Reading order

Read them in this order the first time. It is dependency order, not importance order.

| # | File | What it is |
|---|---|---|
| 1 | [`ops/compliance-line.md`](ops/compliance-line.md) | **Read first.** Hard lines vs. operating lines, the escalation ladder, and the pre-publish self-check |
| 2 | [`ops/weekly-operations-checklist.md`](ops/weekly-operations-checklist.md) | The weekly cadence: six stations, ~2.5h, each mapped to an executing skill, with stop rules |
| 3 | [`accounts/master-register.md`](accounts/master-register.md) | Source of truth for everything owned, rented, run, or logged into — and the credential-pointer rule |
| 4 | [`ops/monthly-financial-close.md`](ops/monthly-financial-close.md) | Business days 1–5: reconcile, compute true margin, refresh unit economics, kill/continue |
| 5 | [`web/property-operations.md`](web/property-operations.md) | Running a site as an asset: property card, performance budgets, analytics minimum set, one-change rule |
| 6 | [`deals/buy-sell-deal-desk.md`](deals/buy-sell-deal-desk.md) | Buy/sell operating system: thesis → screen → value → offer → diligence → close → handoff |

## How it fits together

```
ops/compliance-line.md ......... the boundary. Everything below defers to it.
        │
        ├── ops/weekly-operations-checklist.md ..... the heartbeat (weekly)
        │       ├── Station 3 → web/property-operations.md
        │       ├── Station 4 → accounts/master-register.md
        │       └── Station 5 → feeds the monthly close
        │
        ├── ops/monthly-financial-close.md ......... the truth pass (BD1–5)
        │       └── reconciles against accounts/master-register.md
        │
        └── deals/buy-sell-deal-desk.md ............ episodic, largest decisions
                └── outputs become master-register entries + property cards
```

## The division of labour

| This agent | Carl |
|---|---|
| Researches, screens, values, models, drafts, red-teams, reconciles, tracks, escalates | Decides, signs, pays, transfers, presses send the first time |

No tool available to this agent can place a trade, move funds, touch a wallet or exchange key, sign
anything, or write to the live trading fleet (`hl-bracket`, `hl-signer`, `solana-signer`,
`hl-bracket-SECRET` — refused by name, verified). See `ops/compliance-line.md` §1 for the full
hard-line list and §3 for what to do instead.

---

## Operating principles that run through all six files

1. **Write → verify → claim.** No artifact is described as existing until it has been re-read from
   the source in a separate call *after* the write. This repo exists in its current form because
   that rule was previously absent; see `agent-journal/2026-09-16-cycle-2200z.md`.
2. **`UNKNOWN + how I'd get it`** is always a valid value. A plausible guess never is.
3. **Nothing continues by default.** Experiments, channels, subscriptions, and deals that are not
   explicitly continued are killed.
4. **One change at a time, with a written prediction.** Ten simultaneous changes produce zero
   learning.
5. **Decide on the pessimistic column.** If a plan only clears on the optimistic case, it's a bet,
   and it gets labelled as one in the first line.
6. **A silently failing channel is the worst failure mode.** If an escalation tool errors, the
   escalation did not happen — find another channel in the same cycle.

---

## Verification record

Built 2026-09-16, 23:00Z cycle. All six files written, then the directory re-listed and contents
re-read in separate calls to confirm the writes landed.

Cross-references into `agent-skills` were checked against the live directory listing before being
written. The 18 skills referenced by these runbooks all exist:

| Namespace | Skills |
|---|---|
| `business/` | experiment-engine, offer-design, opportunity-scan, unit-economics |
| `sales/` | outbound-compliance, outbound-sequences, pipeline-and-closing |
| `research/` | competitor-teardown, lead-sourcing-at-scale, public-web-research-at-scale |
| `design/` | brand-and-design-system, web-page-build |
| `media/` | image-generation, video-production |
| `thinking/` | decision-quality, evidence-grading, first-principles-business-model, premortem-red-team |

**Known state of the runtime, verified live at 23:00Z:** `write_journal`, `read_journal`,
`list_journal_dates`, and `surface_finding` all fail with
`EACCES: permission denied, mkdir '/data/journal'`. Every skill in `agent-skills` instructs the
agent to output a `surface_finding`; until `/data` is fixed, read that as **open a GitHub issue**
(`ops/compliance-line.md` §3).

## Not yet built

Honest gaps, so nobody builds on top of something that isn't here:

- `log/` — no weekly logs exist yet. The format is specified in the weekly checklist; the first
  real run creates the first file.
- `close/` — no monthly close has been run. Format specified, unexecuted.
- `web/properties/` — no property cards exist yet, because no property has been registered.
- `accounts/` holds the register **schema and discipline**, not a populated register. Populating it
  requires facts only Carl has (actual accounts, costs, renewal dates, credential locations).

**These are all "needs input from Carl or needs a first real run" gaps, not forgotten work.** The
runbooks are executable as written; they are waiting on data, not on more doctrine.
