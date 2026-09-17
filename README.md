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
| 3 | [`accounts/master-register.md`](accounts/master-register.md) | Schema + the credential-pointer rule. The **facts** live in [`accounts/register-entries.md`](accounts/register-entries.md) |
| 4 | [`ops/monthly-financial-close.md`](ops/monthly-financial-close.md) | Business days 1–5: reconcile, compute true margin, refresh unit economics, kill/continue |
| 5 | [`web/property-operations.md`](web/property-operations.md) | Running a site as an asset: property card, performance budgets, analytics minimum set, one-change rule |
| 6 | [`deals/buy-sell-deal-desk.md`](deals/buy-sell-deal-desk.md) | Buy/sell operating system: thesis → screen → value → offer → diligence → close → handoff |

## Executed runs (not templates)

| Artifact | What it is |
|---|---|
| [`log/2026-38.md`](log/2026-38.md) | **First real weekly run**, 2026-09-17. 6/6 stations executed. Three returned empty — and *empty is recorded as a finding, not as a skip*. Registers EXP-001, which carries a kill criterion on this very activity |
| [`accounts/register-entries.md`](accounts/register-entries.md) | **13 populated entries** — 4 accounts/infra + 9 repos. Every unverifiable field says `UNKNOWN` plus how to get it |

## How it fits together

```
ops/compliance-line.md ......... the boundary. Everything below defers to it.
        │
        ├── ops/weekly-operations-checklist.md ..... the heartbeat (weekly)
        │       ├── Station 3 → web/property-operations.md
        │       ├── Station 4 → accounts/master-register.md + register-entries.md
        │       ├── Station 5 → feeds the monthly close
        │       └── output   → log/YYYY-WW.md
        │
        ├── ops/monthly-financial-close.md ......... the truth pass (BD1–5)
        │       └── reconciles against accounts/register-entries.md
        │
        └── deals/buy-sell-deal-desk.md ............ episodic, largest decisions
                └── outputs become register entries + property cards
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
   explicitly continued are killed. *This now includes the weekly run itself* — see EXP-001.
4. **One change at a time, with a written prediction.** Ten simultaneous changes produce zero
   learning.
5. **Decide on the pessimistic column.** If a plan only clears on the optimistic case, it's a bet,
   and it gets labelled as one in the first line.
6. **A silently failing channel is the worst failure mode.** If an escalation tool errors, the
   escalation did not happen — find another channel in the same cycle.
7. **Empty ≠ skipped.** A station run against reality that finds nothing produces a recorded zero.
   A station not run produces a gap. Never let the log blur the two.

---

## Operating note — file size and the 8KB read limit

Discovered and verified on 2026-09-17, and it changes how these files should be written:

**The agent's read tools truncate a response at roughly 8,000 characters.** `github_read_file` and a
plain `web_fetch` of a raw URL both cut off at the same point — so
`ops/weekly-operations-checklist.md` was, for one cycle, a document whose own scorecard, stop rules,
and log format the agent could not see. A runbook longer than the tool that reads it is only
partially executable, and it fails *silently*: the text just stops.

**Verified workaround:** `web_fetch` honours HTTP `Range` headers against
`raw.githubusercontent.com`. `{"Range": "bytes=7500-16000"}` returned HTTP 206 and the missing tail.
That is how the rest of the checklist was recovered and how this run followed the real log format.

**Rules adopted:**
- Keep every operational file **under ~7,000 characters** so it is readable in one call.
- If a file must be longer, split it, or leave a pointer at the top naming the byte offset of the
  remainder.
- When reading any file whose end you have not actually seen, **assume there is more and Range-fetch
  the tail.** An unread tail is indistinguishable from a file that ends there.

---

## Verification record

- **2026-09-16, 23:00Z** — six runbooks written, then the directory re-listed and contents re-read in
  separate calls. Cross-references into `agent-skills` checked against a live listing before being
  written; all 18 skills exist (`business` 4, `sales` 3, `research` 3, `design` 2, `media` 2,
  `thinking` 4).
- **2026-09-17, 00:00Z** — first weekly run executed; `log/2026-38.md` and
  `accounts/register-entries.md` written, then re-read from the repo to confirm. Absences were
  verified, not assumed: `GET /repos/{repo}/pages` → 404 on both public content repos (no web
  property exists); issues API → 2 open escalations, 0 comments.
- **Runtime state, re-confirmed live at 00:00Z:** `write_journal`, `read_journal`,
  `list_journal_dates`, `surface_finding` all still fail with
  `EACCES: permission denied, mkdir '/data/journal'` — **fourth cycle.** Every skill in
  `agent-skills` instructs the agent to output a `surface_finding`; until `/data` is fixed, read that
  as **open a GitHub issue** (`ops/compliance-line.md` §3).

## Not yet built

Honest gaps, so nobody builds on top of something that isn't here:

- `close/` — no monthly close has been run. Format specified, unexecuted. **First one is due at the
  start of October**, and it will be mostly `UNKNOWN` unless the register's cost fields are supplied.
- `web/properties/` — no property cards, because **no property exists** (verified, not assumed).
  Station 3 of the weekly checklist is unexecutable until one does.
- A **complete** register. 13 entries exist; every cost, renewal date, billing account, credential
  location, and key age is still `UNKNOWN`. Those are Carl's to supply — issue
  [#1](https://github.com/cspoerer62/business-operations-library/issues/1)(b), which now has a
  skeleton to fill rather than a blank schema.
- A **secret scan.** Not performed, and deliberately not claimed. "No secrets in these repos" is not
  an assertion this library makes.
