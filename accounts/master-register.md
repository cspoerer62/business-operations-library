# Master Register

**The single source of truth for everything the business owns, rents, runs, or logs into.**

If it costs money, holds data, receives mail, resolves a domain, or can be logged into, it is on
this register. Nothing else is an acceptable place to remember it.

Two things this register exists to prevent, both of which are silent until they are expensive:

1. **Costs nobody can account for.** The monthly close cannot reconcile a charge to a purpose if
   the purpose was never written down. (`ops/monthly-financial-close.md` Day 1, gate 3.)
2. **Access nobody remembered to remove.** A contractor's login, an old API key, a mailbox that
   still forwards. Every one of these is a live door with nobody watching it.

---

## The one absolute rule

> **This register never contains a secret. It contains a pointer to where the secret lives.**

No password, no API key, no seed phrase, no token, no recovery code, no 2FA backup code — not in a
field, not in a comment, not in an example, not "temporarily", not in a private repo.
`ops/compliance-line.md` §2.2.

The `Credential location` field holds something like `1Password › Business › Stripe` or
`Render env var STRIPE_KEY` or `hardware key in safe`. It describes a location. It never contains
the thing at that location.

**If a secret is ever found committed anywhere: the remediation is rotation, not deletion.**
Deleting the line does not unpublish it — git history, forks, and caches keep it. Escalate first,
rotate, then clean.

---

## Schema

Every entry, every field. `UNKNOWN + how I'd find out` is a valid value; blank is not.

| Field | Meaning | Why it's here |
|---|---|---|
| `ID` | Short stable key, e.g. `SVC-014` | So other docs can reference an entry without ambiguity |
| `Name` | What it's called | — |
| `Type` | `domain` / `hosting` / `saas` / `mailbox` / `repo` / `payment` / `bank` / `registrar` / `api` / `social` / `data-asset` / `legal` / `hardware` | Determines which review rules apply |
| `Purpose` | One sentence: what breaks if this disappears | **An entry with no purpose is a deprovision candidate** |
| `Owner` | The named human accountable | Not "the team". A name |
| `Cost` | Amount + period, or `free` | Feeds the close's fixed-cost line |
| `Annualised` | Cost × periods/yr | Because $19/mo reads as nothing and $228/yr reads as a decision |
| `Billing` | Which card/account it charges | Lets the close match charge → entry |
| `Renewal date` | Next charge or expiry | Drives the 30-day renewal review |
| `Auto-renew` | yes / no | For domains, `no` is a live risk |
| `Credential location` | **Pointer only** — see the absolute rule | — |
| `2FA` | Method, and whether backup codes are stored (where, not what) | 2FA without recoverable backup = one lost phone from lockout |
| `Access list` | Who/what can log in, incl. service accounts | The access review reads this column |
| `Key age` | Date the current key/token was issued | Rotation interval is measured from here |
| `ToS reviewed` | Date the ToS/rate limits were actually read | `ops/compliance-line.md` §2.5 requires this once per API |
| `Data held` | What personal or customer data lives here | Needed for any deletion request, and for knowing blast radius |
| `Criticality` | `critical` / `important` / `nice` | Critical = business stops without it |
| `SPOF?` | yes/no + what the fallback is | Feeds weekly Station 6's single-point-of-failure list |
| `Status` | `active` / `trial` / `deprecating` / `cancelled` | A `trial` with a renewal date is an upcoming surprise charge |
| `Notes` | Free text | — |

### Register table (template — fill, don't admire)

```
| ID | Name | Type | Purpose | Owner | Cost | Annualised | Billing | Renewal | Auto | Cred location | 2FA | Access | Key age | ToS rev | Data held | Crit | SPOF? | Status |
|----|------|------|---------|-------|------|-----------|---------|---------|------|---------------|-----|--------|---------|---------|-----------|------|-------|--------|
| DOM-001 | example.com | domain | Primary identity; email + site | Carl | $x/yr | $x | card-1 | YYYY-MM-DD | yes | <vault path> | TOTP, codes in vault | Carl | n/a | n/a | none | critical | yes — registrar transfer lock only | active |
```

---

## Mandatory sub-registers

Some asset classes have failure modes a generic row won't capture. Each gets a short section of
its own, in addition to its register row.

### Domains
- Registrar, expiry, auto-renew, **registrar lock**, and who holds the registrar login.
- **Which domain is primary and which is the cold-sending domain.** These must never be the same
  (`ops/compliance-line.md` §2.3). A burned cold domain must not take invoices and password resets
  with it.
- DNS records expected: A/AAAA, CNAME, MX, SPF, DKIM selector(s), DMARC policy. Weekly Station 3
  checks these are unchanged; unexplained DNS drift is a security event, not a config quirk.
- Expiry inside 60 days on a `critical` domain is a stop-rule-grade escalation.

### Mailboxes and sending
- Every mailbox: address, provider, purpose, forwarding rules, and whether it sends cold.
- Per sending domain: SPF/DKIM/DMARC state, warmup stage, current daily volume, daily ceiling.
- **The suppression list is a registered asset** with an ID, an owner, and a location. It is
  append-only and survives platform migrations. Losing it is unrecoverable and legally exposed.
- Bounce and complaint rates for the month, against the 3% / 0.1% stop rules.

### Payment and financial accounts
- Processor, bank, card, and any marketplace payout account.
- **Fee rate per processor**, because fees are a close line item (Day 2, gate 5).
- Who can initiate a transfer — a list that should be short and should not include any agent.
- **No agent tooling has payment capability, by construction** (`ops/compliance-line.md` §1).

### Repos and code
- Every repo: visibility, purpose, what secrets it is *supposed* to contain (ideally none), and
  whether an agent may write to it.
- **The four excluded repos — `hl-bracket`, `hl-signer`, `solana-signer`, `hl-bracket-SECRET` —
  are listed with `agent-write: REFUSED BY TOOL`.** This is a hard line, verified, not a policy.
- `trading-research`: `agent-write: technically allowed, self-imposed propose-only`. The register
  records the distinction so a status report never confuses "cannot" with "chooses not to".

### Data assets
- Customer list, suppression list, content library, analytics history, brand assets, deal records.
- For each: where it lives, who can export it, backup location, last backup verified date.
- **"Backup exists" is a claim; "backup restored successfully on <date>" is evidence.** Only the
  second one goes in the field (`agent-skills: thinking/evidence-grading`).

---

## Lifecycle — four transitions, each with a required action

| Transition | Required action | Who |
|---|---|---|
| **Provision** | Register entry created *before* first use, with purpose, owner, cost, credential location, ToS-reviewed date | Agent drafts, Carl signs up — no agent-initiated paid signup (`compliance-line` §2.1) |
| **Operate** | Appears in weekly Station 4 diff; renewal reviewed at 30 days out | Agent |
| **Review** | Monthly access review; quarterly subscription audit with last-used date | Agent proposes, Carl decides |
| **Deprovision** | Cancel → confirm no further charge on next statement → revoke keys → export/delete data → mark `cancelled` with date, **keep the row** | Carl executes, agent verifies on next close |

**Deprovisioned rows are never deleted.** A cancelled row with a date is how you answer "did we
ever use X, and what happened to the data" a year later. A deleted row is an unanswerable question.

---

## Review calendar

| Frequency | Review |
|---|---|
| **Weekly** (Station 4) | Diff reality vs. register; new things added; renewals inside 30 days; domain expiry and DNS drift; bounce/complaint rates |
| **Monthly** (close Day 1–2) | Every charge matched to an entry, both directions; access review — anyone departed, any engagement ended; key age vs. rotation interval |
| **Quarterly** | Full subscription audit with last-used dates and annualised cancel recommendations; backup *restore* test on each critical data asset; ToS re-read for any API whose terms changed |
| **Annually** | Every entry's purpose re-justified from scratch. Anything that cannot be justified in one sentence is a cancel candidate |

### Rotation intervals (defaults — tighten, never loosen)
| Credential type | Interval |
|---|---|
| API keys / tokens for third-party services | 180 days |
| Anything a departing person or ended engagement could have seen | **Immediately, on departure** |
| Anything suspected exposed | Immediately, and it is an escalation |
| Passwords in a vault with unique values + 2FA | On suspicion only; rotation theatre adds no safety |

---

## The detectors — how this register proves it is being maintained

A register is only as good as the check that catches it drifting. Three, and they are cheap:

1. **Charge with no entry** (monthly close, Day 1): something is running unregistered.
2. **Entry with no charge** (monthly close, Day 1): either it's free, or it was cancelled and the
   register wasn't updated, or the billing moved somewhere unwatched.
3. **Entry with no owner or no purpose** (weekly Station 4): nobody is accountable for it.

If all three detectors return clean for two consecutive months, the register is trustworthy enough
to base the close on. Until then, treat its totals as estimates and say so.

---

## Cross-references (all verified present on 2026-09-16)

- This repo: `ops/compliance-line.md` §2.2 (secrets), §1 (excluded repos),
  `ops/weekly-operations-checklist.md` Station 4, `ops/monthly-financial-close.md` Day 1–2,
  `web/property-operations.md` (properties are register entries)
- `agent-skills: sales/outbound-compliance` — suppression list and sending-domain requirements
- `agent-skills: thinking/evidence-grading` — "backup exists" vs "backup restored on <date>"
