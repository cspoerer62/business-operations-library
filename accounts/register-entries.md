# Register Entries — populated

**Schema, rules, and the credential-pointer law live in [`master-register.md`](master-register.md).
This file is the actual register.** If the two ever disagree, `master-register.md` wins on rules and
this file wins on facts.

**Populated 2026-09-17, 00:00Z cycle, during the first real weekly run
([`../log/2026-38.md`](../log/2026-38.md)).**

Every row below is a thing I could *verify with a tool call*, not a thing I assumed. Every field I
could not verify says `UNKNOWN` plus how it would be found out — per the schema, that is a valid
value and a guess is not. Carl holds most of the missing fields; issue
[#1](https://github.com/cspoerer62/business-operations-library/issues/1) asks for them.

> **This file contains no credential, token, key, or secret, and must never contain one.**
> Where a credential exists, the row records that it exists and that its location is unknown to me.

---

## A. Accounts and infrastructure (4 entries)

### ACC-001 — GitHub account `cspoerer62`
| Field | Value |
|---|---|
| Type | `hosting` / code platform |
| Purpose | Holds all 9 repos, the agent's **durable memory** (`agent-journal`), and the **only working escalation channel** (issues). If this disappears, the agent loses memory and its only route to Carl simultaneously. |
| Owner | Carl |
| Cost / Annualised | `UNKNOWN` — plan not visible to me. Found at github.com/settings/billing |
| Billing / Renewal / Auto-renew | `UNKNOWN` — same place |
| Credential location | `UNKNOWN` — the agent acts through a token injected by the harness; the agent cannot read it and does not know where it is stored. **Carl should record the location (not the value).** |
| 2FA | `UNKNOWN` — found at github.com/settings/security |
| Access list | Carl + one agent token. Observed scope: read all 9 repos; write all except the 4 refused by tool name |
| Key age | `UNKNOWN` — token issue date not visible to the agent |
| ToS reviewed | Not recorded |
| Data held | Operational doctrine, journals, this register. **No customer or personal data observed.** |
| Criticality | `critical` |
| SPOF? | **yes — no fallback identified.** Account suspension or token revocation ends both memory and escalation |
| Status | `active` |

### ACC-002 — OpenRouter (model routing)
| Field | Value |
|---|---|
| Type | `api` |
| Purpose | Every reasoning/coding call the agent makes runs through it (`set_model` accepts OpenRouter ids only). No alternative provider exists in the toolset. |
| Owner | Carl |
| Cost | Pay-per-token. Rate card is visible to the agent via `list_models` — e.g. on 2026-09-17, `anthropic/claude-opus-5` = $5/M prompt, $25/M completion; `anthropic/claude-sonnet-5` = $2/$10. **Actual spend `UNKNOWN`** — found in the OpenRouter dashboard activity export, or the harness's usage logs |
| Annualised | `UNKNOWN` — computable once one month of actual usage is exported |
| Billing / Renewal | `UNKNOWN` — OpenRouter billing settings |
| Credential location | `UNKNOWN` — key held by the harness, never exposed to the agent |
| Access list | Carl + the harness |
| ToS reviewed | Not recorded. `compliance-line.md` §2.5 requires one read, recorded here |
| Data held | Prompt/completion history on OpenRouter's side — includes the contents of these repos |
| Criticality | `critical` |
| SPOF? | **yes — no second provider reachable** |
| Status | `active` |

### ACC-003 — Agent runtime `/data` volume
| Field | Value |
|---|---|
| Type | `hosting` / runtime storage |
| Purpose | Backs `write_journal`, `read_journal`, `list_journal_dates`, **and `surface_finding`** |
| Owner | Carl (harness operator) |
| Status | **BROKEN.** `EACCES: permission denied, mkdir '/data/journal'` — first recorded 2026-09-16, re-confirmed live 2026-09-17T00:00Z. Four cycles running |
| Criticality | `important` — memory has a working substitute (this GitHub org); the *finding channel* does not |
| SPOF? | **yes, and it already fired.** One filesystem permission took out four tools, one of which was the report channel, silently |
| Fix | Create `/data/journal` writable by the agent user, or repoint the tools at a writable path |

### ACC-004 — Render (trading fleet hosting) — awareness row only
| Field | Value |
|---|---|
| Type | `hosting` |
| Purpose | Runs the live trading fleet (`hl-bracket`, `hl-signer`, `solana-signer`) |
| Evidence | Mission brief refers to "the fleet's live Render services". **Not independently verified — the agent has no network path to it and no account access.** |
| Owner | Carl |
| All other fields | `UNKNOWN` and deliberately not investigated — outside this register's operating scope |
| Why it is here at all | So the monthly close does not treat its charges as unexplained spend. A known-out-of-scope cost is different from a mystery cost |
| Criticality | `critical` (to the trading business, not to this one) |

---

## B. Repositories (9 entries)

Verified by `github_list_my_repos` on 2026-09-17T00:00Z. `agent-write` column distinguishes a
**hard line** (tool refuses) from an **operating line** (tool allows, agent declines) — per
`../ops/compliance-line.md` §1.

| ID | Repo | Visibility | Purpose | agent-write | Crit |
|---|---|---|---|---|---|
| REPO-001 | `agent-journal` | **public** | The agent's durable memory. Cycle entries; the only place decisions and dead ends are recorded | allowed (used) | important |
| REPO-002 | `agent-skills` | **public** | 18-skill business + reasoning library, 6 namespaces, plus the upstream registry and the 2026-09-16 audit | allowed (used) | important |
| REPO-003 | `business-operations-library` | **public** | This repo: runbooks, register, weekly logs | allowed (used) | important |
| REPO-004 | `trading-research` | private | Research station feeding trading decisions. Observed dirs: `doctrine`, `findings`, `ledger`, `missions`, `station`, `tools`, `retros` | **technically allowed — self-imposed propose-only.** Propose by issue, never commit | critical |
| REPO-005 | `agent-workspace` | private | Sandbox. Contains `README.md`, `boundary-test.md` | allowed | nice |
| REPO-006 | `hl-bracket` | private | Live trading | **REFUSED BY TOOL** | critical |
| REPO-007 | `hl-signer` | private | Signing service | **REFUSED BY TOOL** | critical |
| REPO-008 | `solana-signer` | private | Signing service | **REFUSED BY TOOL** | critical |
| REPO-009 | `hl-bracket-SECRET` | private | Holds wallet keys | **REFUSED BY TOOL** | critical |

Fields common to all nine and not repeated per row: Owner = Carl. Cost = `free` (covered by ACC-001's
plan, whatever it is). Renewal = n/a. Credential location = n/a for the repo itself; access is via
ACC-001's token. Data held = code and documents; **no customer or personal data observed in any repo
I can read.** Status = `active`.

**Secret-scan status: not performed.** I have read only a handful of files. "No secrets in these
repos" is *not* a claim this register makes. What it claims is narrower and true: no secret appears
in any file I have read, and none has ever been written by me.

---

## C. What is NOT on this register, and would be if it existed

Checked this cycle, found absent — these are verified absences, not unexamined gaps:

| Class | Finding |
|---|---|
| **Domains** | None found. No repo has a `homepage` set; no registrar account known |
| **Web properties** | None. `GET /repos/{r}/pages` returns **404** for both public content repos — no GitHub Pages site exists |
| **Mailboxes / sending domains** | None known. No outbound program has ever run; no suppression list exists (so the append-only asset the doctrine calls permanent has nothing in it yet) |
| **Payment / bank / processor** | None visible to the agent, by construction — no payment tool exists |
| **SaaS subscriptions** | None known beyond ACC-001/002 |
| **Data assets** | Only these repos. No customer list, no analytics history, no brand assets |

---

## Open flag for Carl — repo visibility

REPO-001/002/003 are **public**. They contain no secrets, but they do publicly describe the shape of
the setup: which private repos exist, what the agent can and cannot write, and that `/data` is broken.
That is low-severity but it is a real exposure and it should be a *decision*, not a default.
Recommendation and cost-of-delay are in `../log/2026-38.md` Station 6.
