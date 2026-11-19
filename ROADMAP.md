# Profeshy roadmap

Where the project is going, in the order it will actually happen.
Profeshy is built in phases separated by gates: a phase starts when
the previous gate closes on evidence, not on a date. No dates are
promised. Anything below that is marked as planned can change if the
evidence says it should, and the change will be recorded here and in
the charter.

Status as of 2026-11-18: Phase 0 complete. Phase 1: every test has
been run and peer reviewed; two steps remain before Gate G1 can close.

## Phase 0 — Charter and decisions (done)

- Project charter written, peer reviewed, and adopted (Gate G0 closed)
- Decision 001: data comes from Blizzard's own API for WoW Forever,
  using the project owner's API client. The AHledger aggregator is a
  fallback only
- Decision 002: the stage order below stands as written
- Public repository opened, with the full paper trail in docs/

## Phase 1 — Data feasibility (Gate G1, in progress)

Throwaway tests that prove the data the app needs actually exists and
behaves. Full designs and results are in docs/.

- S1b, auction house API access and traps: passed. One auction house
  is about 13 MB and 131,000 auctions; the data is identical across
  rulesets, so the app will read one and never add them together
- S2, shrinking that data for a phone: passed, on desktop and on a
  real Android phone. A full house compresses to about 64–67 KB.
  On a 2019 phone every lookup, search and full scan answered in
  under 40 ms, and a full refresh (download, rebuild, swap) took
  under a second. The first phone run missed two of the planned
  measurements; review sent it back, and a re-run on 2026-11-18
  added them and passed
- S3, how often prices change and how history will be collected:
  passed. The auction house measured publishes a new snapshot once
  an hour, at a fixed minute. The API cannot say whether anything
  changed, so every check is a full 10–15 MB download. History will
  be collected by the project itself, once an hour per house,
  because renting it would not allow keeping it. How long it may be
  kept depends on Blizzard's 30-day data rule, which must be
  settled before Gate G2
- S4, recipe and profession data: passed. 2,494 recipes found, and
  every material they need can be looked up. ForeverDB granted API
  access, and it named all 25 sampled auction items that Blizzard's
  API cannot (about half of all items); the rest are not yet
  checked. Whether ForeverDB data may be stored is still an open
  question
- S8, competitor audit: done from public listings. No rival offers
  Android Forever prices with alerts, live-priced leveling, or
  forecasting. A hands-on pass over the rival apps is still owed
  before Gate G3

Gate G1 closes when a live price and a checked recipe can be shown
on a phone, with their sources labelled, through this data route.
What remains: that demonstration (or a review ruling that the
evidence already shows it), then the owner's decision to close G1.

## Phase 2 — Contract and architecture (Gate G2)

- The remaining tests: S5 (the profit formula, checked by hand
  against worked examples), S6 (whether Android battery limits let
  price alerts run reliably), S7 (a prototype that settles the app
  stack)
- Decision D2: the app stack, based on S7
- Decision D4: whether there is a backend, and a monthly cost ceiling
- Contract v1 frozen: the exact shapes for a price quote, a recipe
  cost, a profit estimate, a guide step, and a forecast, plus the
  freshness rules and the profit formula itself

## Phase 3 — Stage 1: Trustworthy prices (Gate G3)

The first shippable version. Auction house search, live prices with
their source and fetch time shown, market selection, and last-known
prices shown honestly as stale when offline. Ships to a closed test
track first. Nothing else is in this stage.

## Phase 4 — Stage 2: Tracking

Wishlists, price tracking, and price history charts. History starts
the day collection starts and the app will say so, rather than
invent a past. Target-price alerts, if S6 showed they can run
reliably on Android.

## Phase 5 — Stage 3: Profit engine

Crafting profit for a first set of professions: material costs from
live prices, what the finished item sells for, fees, and the
assumptions printed next to every number. Plus a materials list that
adds up everything a set of recipes needs.

## Phase 6 — Stage 4: Guides

Profession leveling guides where every step is priced live, giving a
cost and profit figure per skill band: profit per level. Guide steps
carry their source and the date they were last checked.

## Phase 7 — Stage 5: Forecasting

Price forecasts, and only once two things are true: enough
self-collected history exists, and the forecasting method has passed
a recorded test against that history with its accuracy stated. A
forecast is a labelled estimate, never a price.

## After Android

iOS, once the Android app reaches a stable release: current stage
shipped, no severe defects open, and the data contract unchanged for
a full release cycle.

## Not planned for version 1

- Matching Awesome House feature for feature
- Desktop or Windows versions
- Account sync before the data contract is stable
- Any number shown without its source

Game and auction data comes from Blizzard's API. Some item details
come from ForeverDB.

Data from [ForeverDB.net](https://foreverdb.net)
