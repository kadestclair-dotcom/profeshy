# Profeshy — Master Guardrails and Rules

**Purpose:** this file is the single handover document for any AI
assistant (or human) picking up work on Profeshy. It carries the
project's binding rules, its decided questions, its measured facts,
and its working conventions, so a new assistant works the same way
the project has been worked so far: evidence first, gates held,
nothing invented. Read this file first, then the charter, then the
results documents for any numbers you intend to use.

**Owner:** Kade (kadestclair@gmail.com), timezone America/Los_Angeles.
**Last updated:** 2026-11-14.

## 1. What the project is

Profeshy is a profession companion app for World of Warcraft:
Forever. Android first; iOS only after the Android app reaches a
stable release. It is a new project, NOT a port or rewrite of the
owner's earlier app Awesome House (paused 2026-11-14, revisit
February 2027).

Canonical locations:

- Public repo (paper trail in `docs/`):
  https://github.com/kadestclair-dotcom/profeshy
- Charter: `docs/PROFESHY_CHARTER_v0.1_DRAFT_FOR_PEER_REVIEW.md`
- Roadmap: `ROADMAP.md` at the repo root
- Notion hub: page "Profeshy" in the owner's workspace (private;
  snapshot copies of all markdown live there as subpages)
- Tracker: `Profeshy_Tracker.xlsx` (sheets: Tracker, Roadmap,
  Key metrics, Files)

## 2. The discipline (non-negotiable)

1. **Spike-first, contract-gated.** No feature code until the data it
   depends on has been measured by a throwaway spike whose design was
   written down first and whose results are recorded in a results
   document. Designs and results live in `docs/`, named
   `<SPIKE>_SPIKE_DESIGN_<date>.md` and `<SPIKE>_RESULTS_<date>.md`.
2. **Claims ladder.** Every factual claim is one of: Hypothesis,
   Tested (measured once, recorded), Verified (independently
   confirmed a second way). Never state a Tested claim as Verified.
   Never promote a claim because it was repeated or imported.
3. **Provenance on all data.** Every dataset and displayed figure
   carries its origin. Imported data is labelled by source and never
   silently treated as first-party measurement.
4. **Gates.** Work proceeds gate by gate (G0 charter, G1 data
   feasibility, G2 contract frozen, G3 Stage 1 shipped; each later
   stage is its own gate). A gate closes only on recorded evidence
   plus the owner's explicit decision. Do not start a later stage
   because an earlier one is boring.
5. **Peer review.** Designs and results are reviewed before adoption.
   A review verdict is recorded in the document it concerns. Do not
   treat your own draft as adopted.
6. **Scope is earned.** Features ship in the charter's stage order
   (§5). Suggestions outside the current stage are noted, not built.

## 3. Decisions already made — do not reopen

- **Decision #001 (2026-11-14):** Data route is Route B — WoW Forever
  via Blizzard's undocumented Forever API, using the owner's own
  Blizzard API client. AHledger is a named FALLBACK ONLY. The
  official-WoW route is deferred, not cancelled. Making AHledger or
  any crowdsourced model primary requires a NEW decision record
  superseding #001.
- **Decision #002 (2026-11-14):** The charter's stage order stands as
  written (D3). Gate G0 is CLOSED.
- **Source strategy (S4):** Blizzard primary for profession, recipe
  and reagent structure; ForeverDB only for long-tail auction item
  names, pending spike item F1.
- **History strategy (S3 desk model):** price history is
  self-collected. AHledger's licence forbids archiving below its
  $499/mo Data tier, so rented history can never become the project's
  asset.

## 4. Data rules (measured, binding on all future work)

- The Forever API organises auctions by **ruleset, not realm**
  (US rulesets 1, 2, 3, 15). Auction data is **identical across
  rulesets**: read ONE ruleset, NEVER sum across rulesets.
- The auctions endpoint requires a **trailing slash** (404 without).
- House 113 (US) measured 2026-11-14: 131,205 auctions,
  12,999,230 bytes. Commodities: 101,505 auctions, 8.7 MB.
- Blizzard's item endpoint 404s on **53.0%** of items sampled from
  house 113. The recipe graph is unaffected: reagent resolution is
  100%. These are different item populations; do not conflate them.
- The normalised per-item aggregate is the only form that goes to a
  phone: 66,996 bytes gzipped (0.52% of raw), 184,320-byte SQLite
  store, queries 3.3–5.17 ms on a Galaxy S10+ against a 50 ms bar.
- **Freshness honesty:** stale data is never styled as live. History
  displays only data collected since collection began — no backfill
  fiction. Absence is shown as absence (an unnamed item shows as
  unnamed, with its icon and ID), never filled with a guess.
- **Profit figures** must state their inputs and assumptions
  (materials at current minimum price, fees, what is excluded).
  **Forecasts** are labelled estimates with method and confidence,
  and ship only after a recorded backtest (charter Stage 5).

## 5. Secrets and custody

- API keys and secrets NEVER appear in chat, in documents, in code
  that ships, or in logs. Do not ask the owner to paste a secret.
- The owner's Blizzard client ID/secret live in his local config on
  his own PC. Spikes that need them run LOCALLY on his machine via
  throwaway harnesses that read the config or environment variables,
  and each harness greps its own output to prove zero secret leakage
  (the "custody proof" step). Follow this pattern for any new spike.
- The ForeverDB API key, when issued, follows the same rule: the
  owner sets it as an environment variable locally; it is never sent
  to the assistant.
- The GitHub app used by the assistant cannot create repositories
  (403). It can read and push files to the profeshy repo.

## 6. Reuse rule (Awesome House)

Awesome House code and habits are NOT reused by default. Any
candidate for reuse requires a written Reuse Assessment whose
default verdict is REJECT; reuse is allowed only when the candidate
is demonstrably the best plausible solution, judged case by case.
Zero candidates have been assessed or approved so far. The five
banned habits from that project: feature accretion without a plan,
a monolith UI file, secrets in config or shipped binaries, two
unreconciled "authoritative" test baselines, and a stale status page
with no verified-as-of date or owner.

## 7. Working conventions with the owner

- The owner communicates in a direct, technical, specification-led
  register. Execute the stated specification exactly; show concrete
  verification (read-backs, logs, counts) rather than claiming.
- The owner runs spikes on his own machine and returns log files;
  the assistant writes designs, harnesses, and results documents.
- Stop at each gate for the owner's explicit approval. An approval
  covers only the exact action named.
- **Admit errors plainly and immediately**, with the correction and
  its evidence (precedents: a wrong "no project link needed" reading
  of the ForeverDB form, and a harness crash caused by localized
  profession names — both owned and fixed the same day). Never invent
  a decision, a contribution, a measurement, or a document to fill a
  gap; a labelled draft scaffold is the correct placeholder.
- Public-facing project text (README, ROADMAP) is written plainly,
  in a human voice, without asterisk bold or marketing language —
  the owner's explicit preference.
- The Notion page and the Excel tracker are SNAPSHOTS. Refresh them
  when a phase finalizes, not after every edit (owner's rule,
  2026-11-14).
- When a results document changes, update it in the repo `docs/`
  as well and record the push in the local sync record.

## 8. Current state (2026-11-14, evening)

- G0 CLOSED. G1 in progress: S1b PASS (closed), S2 PASS (closed,
  desktop + Android), S4 PASS (item F1 open), S3 RUNNING (cadence
  probe on the owner's PC since 20:04 PST; results report due after
  ~24 h of log), S8 complete from listings (hands-on device pass
  still owed before G3).
- Repo live and peer-review approved, incl. `docs/` paper trail and
  `ROADMAP.md`.
- ForeverDB access form ready to submit by the owner (repo URL is
  the project link). F1/F2 run when the key arrives.
- Still owed by the owner: Q6 monetization intent (unanswered —
  it changes AHledger/ForeverDB tier maths; do not assume an answer).
- Ahead: S5 (profit formula validation), S6 (Android background
  reality), S7 (stack prototype) → D2; D4 (backend posture and cost
  ceiling) at G2; Contract v1 freeze at G2.

## 9. If you are a new assistant starting cold

1. Read this file, then the charter, then the results document for
   any spike you will touch.
2. Check the gate scoreboard (write-up §6 / tracker) before
   proposing work; propose only work inside the current gate.
3. If the owner relays a peer-review verdict, record it in the
   document it concerns and act on its directions exactly.
4. If anything here conflicts with a newer decision record in the
   charter, the decision record wins — and update this file.
