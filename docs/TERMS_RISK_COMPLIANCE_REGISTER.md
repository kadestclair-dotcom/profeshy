# Profeshy — Terms, Risk & Compliance Register

**Created:** 2026-11-14. **Owner:** Kade.
**Purpose:** one place that records what the project's third-party
terms actually say, the risks they create, and the dates and triggers
that need action. Every entry names its source and the date it was
read. If a clause matters to a decision, the decision waits for this
register, not for memory.

## 1. Blizzard Developer API Terms of Use — read 2026-11-14

Source: https://www.blizzard.com/en-us/legal/a2989b50-5f16-43b1-abec-2ae17cc09dd6/blizzard-developer-api-terms-of-use
Terms version: last updated October 1, 2019. These terms govern the
API client Profeshy uses (Decision #001, Route B).

Clauses that bind Profeshy, with their consequences:

- **§1 / §2.11 — Revocable, changeable without notice.** Blizzard may
  limit, change, suspend or terminate any API or our access at any
  time, without notice; everything is provided "as is" with no
  availability guarantee. This applies to the documented APIs. The
  Forever namespaces Profeshy depends on are not published at all,
  so they carry this risk in its strongest form. → Risk R1.
- **§2, licence scope — Registered applications only.** The API key
  may be used only for applications registered with Blizzard when
  the key was requested. The owner's client was created during
  Awesome House setup. → Action A1: confirm or update the client
  registration so it names Profeshy, before any production use.
- **§2.2 — No premiums.** No charging to download the app, no paid
  "premium" tier for features that use the APIs, no donation
  interstitials gating features, no forced video ads before a
  feature. → This is a hard input to open question Q6 (monetization
  intent): any monetization design must be checked against this
  clause at G2. A premium tier for API-data features is prohibited
  as the terms are written.
- **§2.8 / §2.9 — No monetization or transfer of Data.** The Data
  may not be used for marketing, sold, licensed, or transferred to
  any third party (including aggregate or derived data to ad
  networks or data brokers).
- **§2.12 — Key confidentiality.** The API key must be kept
  confidential and never shared or transferred. The project's
  custody pattern (local config, never in chat or shipped code,
  custody-grepped harnesses) is aligned with this clause.
- **§2.13 — Attribution and naming.** The app must clearly identify
  Blizzard as the source of the Data, without implying endorsement,
  and may not use Blizzard trademarks in its title or URL.
  "Profeshy" complies. (The project's own provenance labels already
  implement the attribution half.)
- **§2.15 — Privacy policy required.** The application must post a
  privacy policy governing use of the Data, consistent with
  Blizzard's. → Deliverable D-Priv: required no later than the
  first public release; draft at G2 alongside D4.
- **§2.17 — Rate limit.** 36,000 API calls per hour maximum, and no
  using third-party services to make additional requests on our
  behalf. The S3 collection design (hourly snapshots of up to 15
  houses) is orders of magnitude inside this limit.
- **§2.18 — 30-day data TTL. ⚠️ OPEN COMPLIANCE QUESTION C1.** The
  terms require a maximum 30-day TTL on "all Data obtained through
  our APIs": data pulled from the APIs is to be retained no longer
  than 30 days. The clause's stated rationale is players' data
  withdrawal rights, but its text is not limited to character data.
  Profeshy's core design keeps self-collected auction price history
  indefinitely (Stages 2 and 5; S3 rollup policy R1 keeps daily
  per-item summaries forever). Whether per-item price aggregates
  retained beyond 30 days breach §2.18 is NOT resolved by this
  register. It must be resolved — by a considered reading, by
  Blizzard clarification, or by a retention-capped design — before
  Contract v1 freezes at G2. Until then, no history-retention claim
  is treated as settled.
- **§3 — Licence back to Blizzard.** Blizzard receives a perpetual,
  royalty-free licence to use and publicly display the application
  and the data it compiles. Recorded so it is never a surprise.
- **§2.5 / §2.6 — Community benefit; no under-13 targeting.** The
  app must benefit the player community and must not be designed
  for or marketed to children under 13. Profeshy complies by design.

## 2. ForeverDB terms — read 2026-11-14

Full record: `S4_F3_FOREVERDB_TERMS_2026-11-14.md`. Headlines: key by
manual application; free tier 5,000 requests/day, 30/min, visible
credit link mandatory; over-limit returns 402; paid tier $5.99/mo.
Status: application not yet submitted by the owner (repo URL ready).
Per the owner's instruction 2026-11-14, the ForeverDB thread is
parked until ForeverDB responds to the application — no chasing.

## 3. AHledger terms — recorded 2026-11-14 (fallback only)

Source: D1 decision brief and S3 desk model, from
ahledger.com/developers. AHledger is a named fallback (Decision
#001), never a primary source. Terms tighten on **2026-12-10**:
no-key access drops to 30 req/min with 7 days of history; free key
300/min with 30 days; archiving, datasets and model training require
the Data tier (from $499/mo) at every tier below it. → Dated trigger
T1: if the fallback is ever exercised, what may lawfully be kept is
decided by the terms in force on that date, not by the terms as
first read.

## 4. Name check — "Profeshy", web first pass 2026-11-14

Exact-name searches (web, app stores, general) found no app,
product, or company named "Profeshy". Near names exist and are
distinct: PROFESY (a 2004 academic protein-structure method),
PROPhESY (an academic verification tool), Profesh (an Indian
recruitment startup), Profi.ru. Classification: Tested (web search),
not Verified — this is not a trademark-register search. A formal
clearance search is optional before the Play listing at G3; the
name is clear to keep using in development.

## 5. Risk register

| ID | Risk | Exposure | Mitigation on record |
|---|---|---|---|
| R1 | Blizzard changes or withdraws the undocumented Forever API, or access under §1/§2.11 | Loss of live prices — the product's core | AHledger fallback (Decision #001); charter §10 kill criterion: pivot to guides + manual tracking or stop — never fake "live" |
| R2 | §2.18 TTL reading forbids long-term price history (C1) | Stage 2 history and Stage 5 forecasting shrink or redesign | Resolve C1 before G2; S3 rollup design can be re-cut to a 30-day window if required |
| R3 | Monetization design collides with §2.2/§2.8 | Q6 answered in a way the terms prohibit | Q6 stays open until checked against §1 of this register at G2 |
| R4 | ForeverDB declines or delays the key | F1 unanswered; ~53% of AH items stay unnamed | Charter §7.5 degradation: icon + ID, absence shown as absence; Stage 3 not cut (S4 verdict) |
| R5 | Any provider changes terms (precedent: AHledger, 2026-12-10) | Silent non-compliance | This register is re-read at every gate; dated triggers below |

## 6. Dated triggers and future obligations

- **T1 — 2026-12-10:** AHledger terms tightening takes effect
  (see §3). No action unless the fallback is in use.
- **G2:** resolve C1 (30-day TTL); draft the privacy policy
  (D-Priv); check the Q6 monetization answer against §2.2/§2.8;
  complete A1 (client registration names Profeshy).
- **G3:** Google Play obligations — developer account, data-safety
  declaration, target-API requirements in force at that date;
  optional formal trademark clearance for "Profeshy".

## 7. Records retention rule

Adopted 2026-11-14, mirroring the owner's rule on his other
projects: Profeshy's records — spike designs, results, logs,
decision records, review records, and this register — are retained
for the life of the project plus 7 years. Evidence files are never
edited after the fact; corrections are appended, dated, and say
what they replace.

## 8. Maintenance

This register is re-read at each gate and whenever a provider's
terms change. New findings are appended as dated entries; resolved
questions (like C1) are marked resolved with the resolution and its
evidence, never deleted.
