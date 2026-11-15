# Profeshy — Project Charter v0.1

**Status: ADOPTED 2026-11-14 (Gate G0 closed; Decisions #001 and #002
recorded at the end of this document). Originally issued the same day
as a DRAFT FOR PEER REVIEW — that draft authorized no code, no
accounts, and no spending; spike harnesses since then were approved
individually. Filename retains its draft-era name so existing links
do not break.**
**Date:** 2026-11-14
**Owner:** Kade
**Supersedes:** the "Awesome House Mobile rewrite" framing. Profeshy is a
new project, not a port.

---

## 0. Relationship to Awesome House

Awesome House is **paused by owner decision (2026-11-14)**. It remains
available at its current release (0.16.1) and is preserved as an evidence
archive: what it does, what its tests prove, and how it got messy are all
useful evidence. Nothing else carries over by default.

**Reuse rule (owner-stated):** No Awesome House code, structure, or habit
is inherited unless it is demonstrably the *best plausible solution* for
Profeshy, judged case by case in a written Reuse Assessment (candidate,
alternatives considered, evidence, verdict: adopt / adapt / reject). The
default verdict is **reject**. As of this charter, zero reuse candidates
have been assessed and zero are approved.

**Habits explicitly banned in Profeshy** (each one is an Awesome House
lesson, recorded so it cannot recur silently):

1. Feature accretion without a plan — every feature enters through a gate.
2. A monolith UI file — no file may become the whole app.
3. Secrets in config files or shipped binaries.
4. Two unreconciled "authoritative" test baselines.
5. A status page that goes stale without a verified-as-of date and owner.

## 1. Product definition

Profeshy is a World of Warcraft **profession companion** for phones. Its
main line of focus, as stated by the owner:

- Profession guides
- Profession profits (what to craft, what it costs, what it earns)
- Live auction house data
- Auction house search
- Item price tracking and wishlisting (with target-price alerts as a
  candidate extension)
- Materials-needed tracking (aggregated shopping lists from recipes and
  guides, minus what the player already has)
- Profession leveling guides
- **Profit per level** — for each skill band of a leveling path, the
  expected cost or profit of the recommended crafts at current prices on
  the player's realm
- Price trend history
- Price forecasting

Platform order: **Android first.** iOS work begins only after the Android
app is final (see §4 for what "final" must mean before iOS starts).

**Market note (per Spike S8 audit, 2026-11-14 — Tested, listings-based;
full audit in `S8_COMPETITOR_AUDIT_2026-11-14.md`):** the owner's earlier
note ("two mainline-focused Play Store companions") could not be
confirmed as stated. The closest competitor is Ragetusk: WoW Forever
Guide, but its live prices, economy, and crafting-market features are on
iPhone and its website only — its Android build explicitly excludes
them. Auction House Tracker for WoW is the one clear current
mainline-focused AH app on Play; its Android build has no alerts, and
its Forever data has no history or alerts. Blizzard's official WoW
Companion was retired in July 2024 and is not a current competitor. No
audited app offers price forecasting, Android Forever AH prices +
alerts, or live-priced profession leveling (profit per level).
Differentiation for an Android-first Forever companion is therefore
evidenced, not assumed. The hands-on device pass remains open (see §8,
S8).

## 2. The discipline (inherited from SomethingFishy)

1. **Spike-first, contract-gated.** No feature code until the unknowns it
   depends on are resolved by recorded evidence. Throwaway spikes answer
   questions; they never quietly become the product.
2. **Claims have statuses.** Every load-bearing claim starts as
   *Hypothesis*, moves to *Tested* with recorded evidence, and to
   *Verified* only after reproducible checks. Statuses are never skipped.
3. **Provenance on all data.** A price is never just a number. Every price,
   profit figure, guide step, and forecast carries its source, realm,
   observation time, and freshness state. Live, cached, stale, estimated,
   and user-entered values are never blended or styled as one another.
4. **Contract before code.** Data schemas, the profit formula, freshness
   rules, and error behavior are frozen in a written contract before the
   features that use them are built.
5. **Scope is earned.** Features ship in the staged order of §5. A later
   stage cannot start because an earlier one is boring.

## 3. The gating question: which World of Warcraft?

**DECIDED 2026-11-14 — Decision Record #001 (end of this document):
Route B, WoW Forever via Blizzard's undocumented Forever API, using
the owner's own Blizzard API client; AHledger is a named fallback
only. The text below is the pre-decision framing, kept for the
record; where it differs from the Decision Record, the Decision
Record governs. Corrections applied at the 2026-11-14 cleanup pass:
WoW Forever is an official Blizzard server (launched 2026-11-04),
not a private server; and an auction house API does exist for it
(undocumented) — see the D1 decision brief and the S1b results.**

Everything about "live data" depends on the game target, and it is **not
yet decided**. The candidates differ fundamentally:

- **Official WoW (Blizzard API):** live auction house data exists through
  documented APIs for realms/connected realms; profession and recipe data
  exists in the Game Data APIs. Costs: API-key custody (§6) and a backend.
- **WoW Forever (official Blizzard server):** no public auction house API is known
  to exist. "Live AH data" may be impossible from outside the game; the
  fallback shapes (manual entry, addon-companion export, crowdsourced
  snapshots) are different products with different honesty rules.

This decision (D1) gates the data architecture, the backend question, and
half the spike list. It is deliberately the first decision requested.

## 4. Platform strategy — Android first, iOS later, decided by evidence

"Android first, iOS after final" is the owner's direction. What it does
*not* yet decide is the stack, because the stack choice is exactly the
kind of decision this discipline refuses to make on vibes:

- **Candidate A — Native Android (Kotlin/Compose), native iOS later
  (Swift/SwiftUI).** Best platform fidelity; the domain/data layer must
  then be kept portable by contract discipline alone, or iOS becomes a
  second full build.
- **Candidate B — Kotlin Multiplatform shared core, native UIs.** Shared
  data/domain logic, native screens; maturity for this app shape must be
  proven by spike.
- **Candidate C — Cross-platform UI (Flutter / React Native).** One
  codebase; risks around background work, store policies, and long-term
  maintenance must be proven acceptable by spike.

**"Final" defined:** iOS work starts when Android reaches a declared
stable release (feature set of the current stage shipped, no open
severity-1 defects, contract stable for one full release cycle) — not
when Android feels done. Decision D2 records the stack after Spike S7.

## 5. Staged scope — the order is the anti-sprawl mechanism

**Stage 1 — Trustworthy prices.** AH search; live price with provenance
(source, realm, fetched-at, expiry); realm selection; offline last-known
prices rendered as stale, never as live. *This stage proves the data
pipeline end to end and nothing else.*

**Stage 2 — Tracking.** Wishlists; price tracking; price history charts
(history begins accumulating when collection begins — see §6; the app
must say so honestly rather than backfill fiction); target-price alerts
if Spike S6 proves Android background limits allow them reliably.

**Stage 3 — Profit engine.** Profession profit calculations for a limited
initial profession set: recipe costs from live material prices, crafted
item value, fees, and assumptions displayed with every figure (§7).
Materials-needed tracking: aggregated lists from selected recipes,
minus user-recorded inventory, with live total cost.

**Stage 4 — Guides.** Profession leveling guides whose steps are priced
live per realm, producing **profit per level**: per skill band, expected
cost/profit of the recommended path. Guide content carries provenance
(source, game version, last-verified date) like any other data.

**Stage 5 — Forecasting.** Price forecasts, only after (a) enough
self-collected history exists, and (b) the forecast method passes a
recorded backtest against that history with stated accuracy. A forecast
is a labeled estimate with a confidence band, never a price.

**Never for v1 (any stage):** feature-for-feature Awesome House parity;
desktop or Windows versions; account sync before the data contract is
stable; any data presented without provenance.

## 6. Data architecture — the backend question, stated plainly

Three of Profeshy's headline features — live prices without exposing a
secret, price *history*, and forecasting — point at a backend:

- **Key custody:** a Blizzard API client secret cannot ship inside an
  APK. Options: a small backend proxy holding the key; user-supplied
  credentials (poor UX, support burden); or no live official data.
- **History:** public APIs return current snapshots, not history. History
  exists only if someone collects snapshots on a schedule from day one —
  realistically the backend — or a third-party history source with a
  license to use it is verified (candidates unverified; Spike S3).
- **Cost and operations:** a backend means hosting cost, uptime, and an
  operator: the owner. A monthly cost ceiling is a decision (D4), not a
  surprise.

If the game target (§3) is not official WoW, this section is rewritten by
the D1 decision; the honesty rules are not.

## 7. Honesty rules (contract-level, not UI polish)

1. **Profit formula is contractual.** A profit figure must state its
   inputs and assumptions with it: materials at current minimum price,
   output valued at current minimum price, auction house cut and deposits
   included or excluded — whichever the contract fixes, displayed.
2. **Freshness labels are exact.** Live / cached / stale / estimated are
   distinct states with distinct rendering. Stale data may never be
   styled as live.
3. **Forecasts are estimates.** Every forecast shows its method version,
   its backtested accuracy, and its confidence band. No backtest, no
   forecast.
4. **Guides are data.** A guide step cites its source and game version;
   a step priced from live data shows the price's provenance too.
5. **Absence is shown as absence.** No price, no history, or no recipe
   data renders as "no data," never as a plausible-looking default.

## 8. Spikes (throwaway, evidence-producing, before Contract v1)

- **S1 — API access & key custody:** prove the chosen access model works
  end to end with no secret in the app binary.
- **S2 — AH payload reality:** real endpoint responses — sizes, rate
  limits, update cadence, latency on phone networks.
- **S3 — History sources:** verify whether any licensable third-party
  price history exists for the target; otherwise size the self-collection
  pipeline (snapshot frequency, storage growth, cost).
- **S4 — Recipe/profession data completeness:** can the full recipe graph
  (materials, skill bands, sources) for the initial profession set be
  obtained and validated against hand-checked examples?
- **S5 — Profit formula validation:** hand-compute profits for a fixture
  set of recipes; the formula contract must reproduce them exactly,
  including fees and edge cases (vendor-bought materials, multi-output
  recipes, no-current-listing items).
- **S6 — Android background reality:** wishlist alert refresh under
  WorkManager and OEM battery savers; measured, not documented.
- **S7 — Platform strategy prototype:** the §4 candidates against the
  real constraints found in S1–S6 (background work, data layer, UI
  needs), producing the evidence for Decision D2.
- **S8 — Competitor audit:** identify the Play Store companions the owner
  referenced; record their target (mainline vs other), feature set, data
  sources, and pricing — from their public listings and hands-on use.
  **Status: Tested (Listings)** — completed 2026-11-14 from public
  listings and developer sites (see §1 market note); the hands-on device
  pass remains open, is due before G3, and is not treated as done.

## 9. Gates

- **G0 — Charter adopted.** Peer review passed; D1 (game target) and D3
  (initial scope confirmation) recorded. Nothing built.
- **G1 — Data feasibility.** S1–S4 evidence in. Exit: a live price and a
  validated recipe can be produced on a phone, with provenance, through
  the chosen access model — or the project pivots per §10.
- **G2 — Contract v1 frozen.** Schemas (PriceQuote, RecipeCost,
  ProfitEstimate, GuideStep, Forecast), freshness rules, profit formula,
  error taxonomy. D2 (stack) and D4 (backend/cost) recorded.
- **G3 — Stage 1 shipped** to a closed test track; each later stage is
  its own gate with its own exit evidence.

## 10. Kill / pivot criteria

- If live data for the chosen target cannot be obtained legally and
  reliably, Profeshy pivots to a guide + manual-tracking companion or
  stops — it does not fake "live."
- If a backend is required and its cost/effort exceeds the D4 ceiling,
  scope cuts (history and forecasting first) before the project does.
- If recipe data cannot be validated (S4), the profit engine and
  profit-per-level do not ship on unvalidated data.
- If the competitor audit (S8) shows the differentiation is imaginary for
  the chosen target, G0 reopens before any build. (S8 outcome,
  2026-11-14: not triggered — differentiation evidenced on Android.)

## 11. Open questions

- Q1: Game target and, within it, which realms/regions at launch?
- Q2: Which professions are in the initial Stage 3 set?
- Q3: Wishlist alerts — in-scope promise or best-effort, given S6?
- Q4: Inventory for materials tracking — manual entry only, or an import
  path (and from what)?
- Q5: Guide authorship — written in-house, derived from data, or imported
  (with what license and provenance)?
- Q6: Monetization, if any — and does it change the backend math?
- Q7: Minimum Android version and device floor?
- Q8: Data retention — how long is price history kept, and who pays for
  its growth?
- Q9: Does Profeshy need user accounts at all before iOS?
- Q10: What accuracy bar must a forecast clear in backtest to ship?

## 12. Decisions requested

- **D1 — Game target** (§3). Blocks G1 spike design. *First.*
- **D2 — Platform stack** (§4). After S7; recorded at G2.
- **D3 — Stage order confirmation** (§5 as written, or amended).
- **D4 — Backend posture & monthly cost ceiling** (§6). Recorded at G2;
  a "no backend" answer forces the §6 scope consequences explicitly.
- **D5 — Awesome House reuse assessments:** none requested now; any
  future candidate enters via §0's rule.
- **D6 — Pause package application:** Awesome House is paused by decision;
  whether/when to post the public notices (README + pinned issue + HQ
  status) and the revisit date are the owner's to set.

## 13. Acceptance criteria for this charter

Peer review should be able to answer, from this document alone: what
Profeshy is and is not; why it is not an Awesome House port and what the
reuse rule is; which single decision gates the data architecture; why a
backend is probably required and what that costs the project; in what
order features ship and what evidence each stage needs; how a displayed
profit or forecast traces back to sourced data and stated assumptions;
and what would make the project stop. Anything less specific is a
brochure, not a charter.

---

## Decision Record

### Decision #001 — D1 Game target (recorded 2026-11-14)

**Decision: Route B — WoW Forever, via Blizzard's undocumented Forever
API, using the owner's own Blizzard API client.** (Basis: D1 Decision
Brief 2026-11-14, which superseded §3's original framing with three
routes: A official documented API, B Forever undocumented API, C Forever
via AHledger. The owner, asked to choose, directed: use the route with
his own API.)

Consequences, recorded with the decision:

- The owner already holds a Blizzard API client (created for Awesome
  House); no new developer account is needed. Custody design is still
  required: the secret never ships in the APK (Spike S1b).
- Spike S1 is executed as **S1b** (Forever undocumented): it must test the
  brief's known traps — trailing-slash route behavior, identical auctions
  across rulesets (never sum across rulesets), empty houses, the item
  lookup 404 gap, and ~9 MB response sizes. S1a (official) is deferred,
  not cancelled: the same client could serve a later official target.
- Route C (AHledger) is **not** the primary source. It stands as the
  named fallback if the undocumented API changes or is withdrawn, under
  its licence terms as recorded in the brief (free with credit while
  non-commercial; archiving/forecasting on its data requires the Data
  tier). Adopting it as more than a fallback requires a new decision.
- Kill criterion attached to this decision: if the Forever API is
  withdrawn or changed incompatibly and no fallback covers the gap,
  Profeshy pivots to guides + manual tracking or stops; it does not fake
  "live" (charter §10).
- §3 and related charter text still carry the pre-brief framing; the
  brief's §6 amendments remain proposed, to be applied at the G0/G2
  contract pass — this record governs where they differ.

### Decision #002 — D3 Stage order confirmed; Gate G0 closed (2026-11-14)

**Decision: the §5 stage order stands as written** (Stage 1 trustworthy
prices → Stage 2 tracking → Stage 3 profit engine → Stage 4 guides with
profit per level → Stage 5 forecasting). Recorded on the owner's
instruction to keep the project as decided and proceed, following
Decision #001 and the S8 adoption.

With peer review passed, D1 recorded (#001), and D3 recorded here, the
§9 conditions for **Gate G0 — Charter adopted** are met. G0 is closed.
Next gate work is G1 (data feasibility): Spikes S1b, S2, S3, S4.

### Decision #003 — Feature Research & Comparative Audit guardrail adopted (2026-11-15)

**Decision: the Feature Research & Comparative Audit guardrail is
ADOPTED as a standalone project document** (text: Draft v0.3,
FEATURE_RESEARCH_GUARDRAIL_DRAFT_v0.3_FOR_PEER_REVIEW.md, whose
header now records this adoption; filename kept so links do not
break). It is kept separate from the Master Guardrails and Rules
and cross-references it, per the master file's one-responsibility-
per-document principle.

What the guardrail establishes: before implementation, a workstream
in scope classes A (first-in-stage user-facing feature), B
(third-party adoption), C (material change to an accepted
contract's subject matter), or D (significant internal subsystem
or foundational architecture) passes a research gate — a Feature
Research & Comparative Audit written to the guardrail's evidence
standards (claims-ladder levels on the evidence itself; primary
sources; demonstrated-behaviour checks or recorded waivers whose
follow-ups are tracker rows), peer-reviewed, and marked READY /
READY WITH CONDITIONS / BLOCKED by owner decision, nested inside
the existing stage gates.

Review chain on record (PEER_REVIEW_LOG.md): the v0.1 proposal was
reviewed by Muse (#11, Approved with amendments); v0.2 by Gemini
(#12, Approved) and ChatGPT (#13, Approved with amendments); the
reconciled v0.3 by Gemini (#14, Approved, no further revisions).
Adoption recorded on the owner's instruction, 2026-11-15.

First scheduled application: pilot the template on Spike S5
(profit formula validation) at G2, per review #11's suggestion.

### Decision #004 — Three-Way Review of Governance Changes adopted (2026-11-15)

The owner adopts the **Three-Way Review of Governance Changes**
rule, seated as §11 of the Master Guardrails and Rules (standalone
text: `docs/THREE_WAY_REVIEW_RULE_ADOPTED.md`). Scope, confirmed
by the owner: **guardrails only** — creation, amendment, removal,
or material reinterpretation of rules in the Master Guardrails or
an adopted standalone guardrail. It is not permission to change
project decisions, technical contracts, or measured findings,
which keep their existing processes.

Review chain: peer reviews #16–#25. Round 3 on Draft v0.4
(commit 5679024): ChatGPT APPROVED WITH AMENDMENTS (#23), Gemini
APPROVED (#24), DeepSeek APPROVED WITH AMENDMENTS (#25). Outcome
reached: **three-way review completed (§6 outcome 1 of the rule).
Three-way approval (outcome 2) is NOT claimed.** Under the
owner's standing instruction of 2026-11-15 (round-3 verdicts of
APPROVED or APPROVED WITH AMENDMENTS are final; proceed with the
amendments), every amendment named in #23 and #25 was
incorporated into the adopted text without a further review
round, and the adoption is recorded in the outcome-3a category
per the rule's §10 stage 3, with composition stated: 1 APPROVED,
2 APPROVED WITH AMENDMENTS, 0 REJECTED, 0 non-participating. No
reviewer dissented and no condition was left unincorporated. The
rule's review-window standing minimum is set by the owner at 3
calendar days (§6 of the rule). Bootstrap stage 1 is this record;
the rule governs changes to itself from this adoption forward.

Adoption recorded on the owner's instruction, 2026-11-15.

### Spike S8 — review record (2026-11-14)

Peer review verdict on the S8 competitor audit: **Approved as-is.** The
audit fulfills its purpose and provides clear evidence supporting
progress past Gate G0 (G0 itself also requires D3, §9). Applied from the
review: the §1 market note now carries the audit's findings, and S8 is
formally **Tested (Listings)** in §8, with the hands-on device pass kept
open for a later gate.

The review also noted an implication for D1 and architecture: incumbent
Forever apps lean heavily on community scanning (AHledger snapshots)
rather than a native public API on Android, so D1 carries even greater
weight. Recorded as a **risk note on S1b, not a change to Decision
#001**: S1b must prove Route B (the owner's own Blizzard API route) can
carry the pipeline. If S1b fails, the pivot paths are the ones already
recorded — the Route C fallback (AHledger, under its licence terms) or
a crowdsourced/companion-export model under §10 — and adopting either
as the primary source requires a new decision superseding #001.
