# Weekly Operations Checklist

**Cadence: once per week, same day, ~2.5 hours total. Same order every time.**

A skill library tells you how to do a thing well. It does not tell you what to do on a Tuesday.
This is that file. Six stations, fixed order, each timeboxed, each with a named executing skill and
a single artifact that proves it ran.

The order is not arbitrary: **money and risk are last because they are the stations most likely to
be skipped when time runs out, and the checklist is designed so that skipping them is visible.**
If a station is skipped, write "SKIPPED — reason" in the log. An unexplained gap in the log is
itself the finding.

Run `ops/compliance-line.md` §4 before anything leaves the building.

---

## Station order and timeboxes

| # | Station | Box | Executing skill | Artifact produced |
|---|---|---|---|---|
| 1 | Inbox & commitments | 20 min | — (triage) | `log/YYYY-WW.md` commitments section |
| 2 | Pipeline | 30 min | `sales/pipeline-and-closing` | Updated stage table + next actions |
| 3 | Web properties | 25 min | `web/property-operations.md` (this repo) | Property scorecard row per property |
| 4 | Accounts & assets | 15 min | `accounts/master-register.md` (this repo) | Register diff |
| 5 | Money | 25 min | `business/unit-economics` | Weekly scorecard numbers |
| 6 | Risk & experiments | 25 min | `business/experiment-engine`, `thinking/decision-quality` | Kill/continue decisions |

Total: 2h20. Budget 2h30 with slack. **If a station overruns its box, stop it and write down what
it would have taken.** An overrunning station every week is a process problem to fix, not a
willpower problem to push through.

---

## Station 1 — Inbox & commitments (20 min)

Purpose: no promise made to a human gets lost.

1. Every inbound message gets exactly one of four dispositions: **reply now** (<2 min),
   **becomes a pipeline record**, **becomes a commitment with a date**, **archive**.
2. List every open commitment made to another person, with the date promised.
3. **Any commitment now past its date is either done, renegotiated this week, or explicitly
   dropped with a message sent.** Silent slippage is the failure this station exists to prevent.
4. Anything requiring a decision the agent cannot make → escalation per
   `ops/compliance-line.md` §3, opened *this station*, not "later".

Fail condition: a commitment appears in two consecutive weeks with no movement and no message sent.

## Station 2 — Pipeline (30 min)

Purpose: the pipeline reflects reality, and every live deal has a dated next action.

1. Pull up every open opportunity. For each: **stage, value, last contact, next action, next date.**
2. **Any record with no next action or no next date is not in the pipeline** — either give it one
   or move it to closed-lost with a reason. A pipeline of records with no next step is a fantasy
   and it inflates the forecast.
3. Advance or kill. Killing is a normal outcome; `sales/pipeline-and-closing` covers the criteria.
4. **Stalled = no movement in 14 days.** Stalled records get one re-engagement attempt, then close.
5. New outbound this week: sequence content per `sales/outbound-sequences`, and the six gates in
   `sales/outbound-compliance` re-checked *even for a program that ran clean last week* whenever the
   list source, message, sending domain, or jurisdiction mix changed.
6. Record actuals into the funnel arithmetic that `business/unit-economics` is holding: sent,
   delivered, replied, conversations, closed. **Replace one benchmark with one measurement.**

Fail condition: pipeline total value changed but no record-level reason exists for the change.

## Station 3 — Web properties (25 min)

Purpose: every property is up, measured, and moving on one metric.

Run per property, using the scorecard defined in `web/property-operations.md`:

1. **Up?** Uptime check result for the last 7 days; any incident noted with duration.
2. **Fast?** Performance budget still met (see the budgets table in `web/property-operations.md`).
3. **Measured?** The minimum event set is still firing. A property with broken analytics is
   **treated as down** — you cannot operate what you cannot see.
4. **Converting?** Primary conversion rate this week vs. the 4-week average.
5. **Growing?** Indexed pages, impressions, top queries — direction only, weekly noise is not signal.
6. **One change.** Exactly one deliberate change per property per week, chosen for the metric that
   is worst. Log it with the date so the next week's number is attributable. Ten changes in a week
   produce zero learning.
7. Forms and inputs: spam volume, and whether anything new accepts user input. New input surface →
   security review before it stays up.

Fail condition: analytics broken for more than one week, or a change shipped with no logged reason.

## Station 4 — Accounts & assets (15 min)

Purpose: nothing is running, costing money, or holding data without being on the register.

1. Diff reality against `accounts/master-register.md`. New account, domain, service, repo, mailbox,
   or subscription since last week → add it, with owner, cost, renewal date, credential location.
2. **Anything on the register with no owner or no purpose is a deprovision candidate.** Flag it.
3. Renewals in the next 30 days: listed, with a keep/cancel recommendation and the annual number.
4. Domains: expiry dates, auto-renew state, DNS unchanged from expected.
5. **Access review, monthly not weekly, but checked for triggers weekly:** anyone who left, any
   contractor whose engagement ended, any key older than the rotation interval → access removed.

Fail condition: a charge appears on a statement that has no register entry. That is Station 5's
detector for this station's failure.

## Station 5 — Money (25 min)

Purpose: the business knows, weekly, whether it is making money on each thing it does.

1. Cash position and 30-day runway, one number each. Not a spreadsheet — a number.
2. Revenue this week by source. Collected, not invoiced.
3. New spend this week, each line reconciled to a register entry (this is the Station 4 detector).
4. Receivables: anything unpaid >14 days past terms gets a chase action in Station 1's commitments.
5. **Refresh the live unit-economics table** per `business/unit-economics`: CAC, payback, LTV:CAC
   per active channel, with the three columns. Update the pessimistic column with real numbers as
   they arrive.
6. **Any channel below 2:1 LTV:CAC on the pessimistic column for two consecutive weeks is a
   Station 6 kill candidate.**

All of this is analysis. **No money moves from this station** — `ops/compliance-line.md` §2.1.
Payments, pricing changes, and cancellations are recommendations to Carl with the number attached.

Fail condition: a channel has run for 30 days and its pessimistic column still holds only
benchmarks, no measurements.

## Station 6 — Risk & experiments (25 min)

Purpose: bets get killed on schedule, and the thing that will break gets seen before it breaks.

1. Every live experiment, per `business/experiment-engine`: hypothesis, metric, kill criterion,
   days running, current reading. **Kill, continue, or scale — no fourth option, no deferrals.**
   An experiment with no kill criterion is not an experiment; give it one now or kill it now.
2. Concentration check: what fraction of revenue is one client, one channel, one platform? Any
   single point above ~40% is named as a risk with a named mitigation.
3. **Single points of failure**: one mailbox, one domain, one contractor, one API key, one payment
   processor. List them. Not all can be fixed; all must be known.
4. Run `thinking/premortem-red-team` on anything shipping next week that is **irreversible** —
   money spent, message sent to a list, contract signed, domain changed, public claim made.
5. Open escalations: still open from last week? Re-state the cost of not deciding
   (`ops/compliance-line.md` §3.4). An escalation that has aged three weeks is being ignored, and
   saying so is the agent's job.

Fail condition: an experiment passed its kill criterion and is still running.

---

## The weekly scorecard

Eight numbers, same eight every week, in one table so trend is visible. Everything else is prose.

| Metric | This wk | Last wk | 4-wk avg | Direction |
|---|---|---|---|---|
| Cash on hand | | | | |
| Runway (days) | | | | |
| Revenue collected | | | | |
| New qualified conversations | | | | |
| Pipeline value (with next actions only) | | | | |
| Blended CAC (pessimistic col) | | | | |
| Primary property conversion rate | | | | |
| Open escalations (and oldest, in days) | | | | |

Rules for the scorecard:
- **`UNKNOWN + how I'd get it`** is a valid cell. A plausible guess is not
  (`agent-skills: business/unit-economics`).
- **Never a single-week conclusion.** Four weeks or it's noise.
- **"Open escalations, oldest in days" is deliberately on the operator's scorecard.** The last audit
  found two cycles of escalations went nowhere silently. This is the tripwire for that.

---

## Stop rules — conditions that end the week's plan immediately

| Trigger | Action |
|---|---|
| Bounce rate >3% or spam complaints >0.1% on any sending program | Stop the program. Clean list. `sales/outbound-compliance` |
| Runway < 60 days | Stop all new spend recommendations; escalate. Cost work only |
| A property is down >4h, or analytics broken 2 weeks | That property becomes the week's only web work |
| A credential is found in a repo, or suspected exposed | Escalate immediately; remediation is rotation, not deletion |
| A legal/regulatory letter, takedown, or platform ban notice | Stop the related activity entirely; escalate. No self-remediation |
| Any claim in a published artifact found to be unevidenced | Correct the artifact this station, then find how it shipped |

---

## Weekly log format

One file per week, `log/YYYY-WW.md`:

```
# Week YYYY-WW
Run on: <date>  |  Stations completed: 6/6  |  Total time: 2h35

## Scorecard
<the eight-row table>

## Station notes
1. Inbox & commitments — <what moved; open commitments and dates>
2. Pipeline — <stage changes with reasons>
3. Web — <scorecard row per property; the one change made, per property>
4. Accounts — <register diff>
5. Money — <cash, revenue, the unit-econ refresh; UNKNOWNs named>
6. Risk & experiments — <kill/continue/scale decisions; SPOFs; premortems run>

## Skipped
<station: reason>   # blank is not allowed if stations completed < 6

## Escalations opened this week
<link, line crossed, decision needed, cost of delay>

## Next week's one priority
<single sentence>
```

**Write the log after the stations run, never before.** `ops/compliance-line.md` §2.6.
