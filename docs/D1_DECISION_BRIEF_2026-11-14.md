# Profeshy — D1 Decision Brief (game target)

**Status: DRAFT FOR PEER REVIEW / OWNER DECISION. Authorizes no code, no accounts, no spending.**
**Date:** 2026-11-14
**Companion to:** `PROFESHY_CHARTER_v0.1_DRAFT_FOR_PEER_REVIEW.md` (§3, §6, §8 S1–S3, §9 G0)
**Purpose:** give Kade (and the peer reviewer) the facts needed to answer D1 — which World of Warcraft Profeshy targets — before G0. The charter itself is **not** amended here; proposed amendments are listed in §6 for review.

## 0. Headline: the charter's §3 premise is already stale

Charter §3 says, for WoW Forever: *"no public auction house API is known to exist."*

That was true when the charter was drafted on 2026-11-14, but it is no longer true:

- A Blizzard Forums thread (posted 2026-11-05, updated 2026-11-06) by a developer probing the Forever API reports **auctions went live on 2026-11-06** on an undocumented Blizzard endpoint — ruleset-based, not realm-based.
- A third-party Forever price service, **AHledger**, already publishes a documented public API (base `https://api.ahledger.com`) with live prices, price history, and a crafting-margin endpoint that ranks recipes by margin — i.e. much of Profeshy's Stage 3 profit engine, as a rented data source.
- **ForeverDB** publishes a separate Forever data API (items, spells, professions, recipes) extracted from the Forever client.

So D1 is no longer "official = live data, Forever = no live data." It is now a choice between three live-data routes with different stability, cost, and honesty trade-offs. Deciding D1 on the old framing would lock the wrong architecture.

All findings below were checked on 2026-11-14 (web search + direct page reads). The Forever API is undocumented and explicitly changeable — treat every Forever-Blizzard fact as *Tested, not Verified*, and re-check it at G1.

## 1. Route A — Official WoW (retail / mainline) via Blizzard's documented API

**What exists:** Blizzard Game Data API, Connected Realm Auctions — `/data/wow/connected-realm/{connectedRealmId}/auctions` (plus `/auctions/commodities` for region-wide commodities), namespace `dynamic-{region}`. Profession/recipe data exists in the same Game Data APIs (`/data/wow/profession/index`, `/data/wow/profession/{id}`, `/data/wow/recipe/{id}`).

**How it behaves (from Blizzard's own API documentation as reflected in current client libraries):**
- Full auction snapshots, updated on a set interval initially set at 1 hour (Blizzard notes the interval may change without notice).
- Responses can be large — sometimes exceeding 10 MB for a busy connected realm.
- Access is OAuth 2.0 client-credentials flow: a Blizzard developer account, Client ID + Client Secret, token via `https://{region}.battle.net/oauth/token`, `Authorization: Bearer` + `Battlenet-Namespace: dynamic-{region}` header. Tokens are valid 24 hours.
- Rate limit reported in current third-party guides: 1,000 requests per 30 seconds (2026) — treat as unverified against Blizzard's current docs; S2 must confirm it from the official docs.

**What this means for Profeshy:**
- The documented, stable route. The charter's backend conclusion stands: the client secret cannot ship in an APK, so a backend proxy holding the key is required for live official data, and price history exists only if the backend collects snapshots on a schedule (the API returns current snapshots, not history).
- Classic Era endpoints are a separate family and have had deprecations — if "official" ever means Classic Era rather than retail, S2 must check that family separately. Do not assume retail endpoints cover it.

**Sources:** Blizzard API reference via current client-library docs (JSR `@pinta365/blizzard-api`, `WowApi.auctions`; Go Blizzard API package, `Auctions` — path `/data/wow/connected-realm/{connectedRealmId}/auctions`, "sometimes exceeding 10 MB"); Blizzard Forums patch notes 2020-04-14 for the professions/recipe endpoints.

## 2. Route B — WoW Forever via Blizzard's undocumented Forever API

**What exists (per the Blizzard Forums thread "WoW: Forever API: what works so far, plus a TS client", 2026-11-05, updated 2026-11-06):**
- Namespaces: `static-forever-{region}` (game data, currently serving version `1.60.1_70399`), `dynamic-forever-{region}`, `profile-forever-{region}`. Works in US, EU, KR, TW.
- **Rulesets replace realms.** There are no realms (realm index 404s). `/data/wow/ruleset/index` and `/data/wow/ruleset/{id}` return `{ id, name, auctions, status }`. EU/US rulesets: 1 PvP, 2 Normal, 3 Roleplay, 15 Roleplay PvP.
- **Auctions, live as of 2026-11-06:** `/data/wow/ruleset/{id}/auctions/` — note the **trailing slash is required**; without it the route 404s. It returns an index of auction houses; `/data/wow/ruleset/{id}/auctions/{houseId}` returns that house's auctions, and `…/commodities` its commodity auctions.
- **15 auction houses per ruleset**, IDs 110–121 and 123–125 — five each of Alliance, Horde, and Blackwater (neutral). Modelled like Classic Era faction houses, but keyed by ruleset instead of connected realm.
- Auction entries look like retail: `id`, `item` (`id`, `context`, `bonus_lists`, `modifiers`), `buyout`, optional `bid`, `quantity`, `time_left`; commodities use `unit_price`.

**Three traps the thread itself flags — these belong in the contract, not in a footnote:**
1. **Every ruleset currently lists the same houses with identical auctions.** House 113 returned the same 68k auction IDs via rulesets 1, 2, and 3. The ruleset ID must be valid (999 → 404) but does not filter. **Do not sum totals across rulesets** or Profeshy will count everything several times. The author notes this may be a bug and may change.
2. **Some houses are empty:** 112, 115, 118 (one per faction) have no `auctions` field at all — treat it as optional.
3. **Item-data coverage is incomplete:** in one EU Alliance house, 652 of 1,515 distinct item IDs (43%) returned 404 from `/data/wow/item/{id}` in `static-forever` (e.g. item 720, Brawler Gloves, and many Classic-era greens/blues). Any name/icon resolution must plan for missing lookups — this is a provenance problem, not a display bug.
4. **Responses are big:** busiest houses hold 50–68k auctions, about 9 MB per response.
5. **Nothing is documented or promised.** The thread's own warning: "Everything can change in a heartbeat since nothing is official and documented yet." A trailing-slash quirk already flipped once (it "used to be the other way around").

**What this means for Profeshy:**
- Forever live data is now *possible* through Blizzard directly — the charter's "impossible from outside the game" fallback (manual entry / addon export / crowdsourced snapshots) is no longer the only Forever shape.
- But it is the least stable route: undocumented, with a known duplication quirk, a 43% item-lookup gap in one sample, and no promise it stays up. Spike S1 must be split: S1a (official, documented) vs S1b (Forever, undocumented) with separate pass/fail criteria. A Forever build that leans on this API needs a fallback data source (see Route C) and a kill criterion if Blizzard changes or withdraws it.
- The backend question does **not** go away for Forever-Blizzard either: the client secret still cannot ship in an APK, and history still requires scheduled snapshot collection.

**Source:** https://us.forums.blizzard.com/en/blizzard/t/wow-forever-api-what-works-so-far-plus-a-ts-client/60334 (read 2026-11-14; post 2026-11-05, auction update 2026-11-06). Corroborated in outline by JSR `@pinta365/blizzard-api` `WowForeverApi` docs (ruleset auctions, commodities, index).

## 3. Route C — WoW Forever via AHledger (third-party, already built)

**What it is:** AHledger aggregates player addon scans into Forever markets and publishes them as a documented public API at `https://api.ahledger.com`. This is the service behind the Timesaver.gg Forever price pages (verified against AHledger market data, Nov 2026).

**What it already offers — note how much of Profeshy's scope this covers:**
- `GET /v1/markets` — 32 Forever markets (normal / pvp / rp / rppvp × alliance / horde × us / eu / cn / kr).
- `GET /v1/prices/{market}/{itemId}?range=7d` — latest price plus history: 24h one point per full scan, 7d hourly buckets, 30d/90d daily buckets; estimated units sold per day.
- `GET /v1/pricetable/{market}` — every priced item as plain text (the format its addon reads), with ETag/304 support.
- `GET /v1/crafting/{market}?profession=Tailoring` — **every priced recipe ranked by margin**: result value after the 5% house cut, reagent cost, reagents, estimated units sold/day, margin × sales (profitPerDay). This is Profeshy's Stage 3 profit engine as an endpoint.
- Also: `/v1/deals`, `/v1/disenchant`, `/v1/structure`, `/v1/timing`, `/v1/signals`, `/v1/economy` — deal scanning, disenchant value, market depth/volatility, best buy/sell hours, buy signals.
- Data provenance is stated on the tin: prices come from player scans (source: "Addon scan", with `observedAt`), not from a Blizzard API; a market with no scans returns null latest values. That fits Profeshy's provenance discipline — but the provenance would be *AHledger's scans*, not Profeshy's own, and must be labelled as such.

**Cost and terms — the part that decides whether this is a shortcut or a trap:**
- Free for personal use and community tools **with a credit link** ("Prices from AHledger" wherever data is shown). A product that makes money from the data needs a commercial licence.
- **From 10 December 2026** (announced 10 November 2026): no-key callers drop to 30 requests/minute and 7 days of history only; a free key gives 300/min and 30 days; a commercial licence gives 600/min and all history (90d).
- Licence prices (from the developers page, read 2026-11-14): **Site $29/month** ($290/year) — a site/bot/newsletter earning from ads/sponsorship/small paid tier; **Product $99/month** ($990/year) — a paid product (subscriptions, paid reports, forecasts, commercial app), 1,200/min; **Data from $499/month** — keeping archives/datasets, training a model, bulk history exports. A credited partner showing the credit prominently pays 50% less for Site/Product.
- **The history/forecasting catch:** from 10 December 2026, "do not keep answers past their cache time" — building an archive, dataset, or model from the answers needs a commercial licence that says so (the Data tier). Profeshy's Stage 2 history and Stage 5 forecasting, if built on AHledger data, are therefore *rented*, not owned, unless Profeshy pays for the Data tier or collects its own history via Route A/B.
- Rate limits: 300/min per address with or without a key until 10 Dec 2026; market-tool endpoints 60/min per account/address. Responses carry `Cache-Control` — honour it.

**What this means for Profeshy:**
- Route C is the fastest credible path to a Forever Profeshy: live-ish prices + history + crafting margins today, no Blizzard secret to custody, no snapshot-collection backend to build on day one.
- It changes the backend decision (D4): the backend becomes optional at Stage 1–2 and the money goes to a licence instead — but the licence is a recurring cost that scales with monetization, and the data can change terms again (it just did, with 30 days' notice). Any charter that leans on AHledger needs a D4 answer that prices the Product tier ($99/mo, or $49.50/mo credited) against a self-collection backend, and a fallback if AHledger withdraws or reprices.
- If Kade ever intends Profeshy to make money (charter Q6 is open), the free tier does **not** cover it — decide monetization before leaning on this route, not after.

**Source:** https://ahledger.com/developers (read 2026-11-14; page crawled 2026-11-14 20:35 UTC). Licence/rate changes announced 2026-11-10, in effect 2026-12-10.

## 4. Supporting source — ForeverDB (recipe / profession data for Forever)

Separate from prices: ForeverDB publishes Forever data as JSON (items, spells, quests, vendors, professions and recipes, gathering) extracted from the Forever client on every build, plus what player addons record. `GET /api/v1/professions/{slug}` returns ranks, skill caps, trainers, and recipes by skill, with skill colours. Free tier: 5,000 requests/day, 30/min, credit to ForeverDB.net required; paid plan $5.99/month for 10,000/day, 60/min, credit optional. This is a candidate source for Spike S4 (recipe/profession data completeness) if D1 = Forever — it does not supply AH prices.

**Source:** https://foreverdb.net/developers (search result, read 2026-11-14).

## 5. Competitor note — S8 starter (not a completed audit)

The charter records Kade's note as unverified: "two WoW companions on the Play Store offer similar value but are mainline-focused." A first search (2026-11-14) does **not** cleanly confirm that framing, and it surfaces a Forever competitor the charter does not mention:

- **Official WoW Companion (Blizzard)** — mainline/retail, on Google Play and App Store. Includes AH browse/search, bid/buyout, favourites, posting and cancelling listings, plus missions/factions. This is one of the two Kade likely meant. It does *not* do profession guides, profit-per-level, or price history/forecasting — that is Profeshy's differentiation against it.
- **Ragetusk: WoW Forever Guide** (ragetusk.com/app) — a Forever companion, **not** mainline: guides and saved plans on iPhone and Android, market prices on iPhone, community auction prices and crafting costs with snapshot dates, offline bundled guides. This overlaps Profeshy's Forever territory directly (professions + market prices + crafting costs), albeit with prices only on iPhone so far.
- **WoW Forever Hub** and **WoW Forever Path** — iOS reference/guide apps for Forever (talents, BiS, maps, guides), launched November 2026. No AH/profit focus found in their listings, but they show how fast the Forever companion space is filling — the same crowding that shelved SomethingFishy.
- **Wowdee** — a Forever addon/app "coming soon" to App Store / Google Play, 104+ features claimed.

**Do not lean on the "two mainline companions" claim yet.** S8 still needs a hands-on Play Store pass naming the exact two apps, their target, feature set, data sources, and pricing. The working hypothesis to test: the differentiation is real against the official Companion (no profession-profit depth), but thinner against Ragetusk if D1 = Forever — Ragetusk already does Forever guides + market prices + crafting costs, and Profeshy would need to beat it on profit-per-level, history/forecasting, or Android market prices specifically.

## 6. What D1 actually decides now (recommendation for the owner / reviewer)

D1 is no longer a binary. The decision is which of these to build on, in order:

| Route | Live AH data | Stability | History | Cost shape | Best fit if… |
|---|---|---|---|---|---|
| A — Official, Blizzard documented API | Yes, hourly snapshots | Documented, stable | Self-collect via backend | Backend hosting + ops | Kade plays retail/mainline and wants the stable, documented path |
| B — Forever, Blizzard undocumented API | Yes, since 2026-11-06 | Undocumented, can change any time; 43% item-lookup gap in one sample; ruleset duplication quirk | Self-collect via backend | Backend hosting + ops | Kade plays Forever and will accept building on an undocumented API with a fallback |
| C — Forever, AHledger | Yes, scan-aggregated | Documented third-party API; terms just changed with 30 days' notice | Rented (licence-gated past 7–30d; archiving needs Data tier) | Free with credit for non-commercial; $29–$99/mo commercial; $499/mo+ to archive | Kade plays Forever and wants the fastest Stage 1–3, and accepts rented data + recurring licence |

**Questions only Kade can answer (not in the evidence):**
1. Which game does he actually play day to day — retail/mainline or Forever? His recent play in the SomethingFishy evidence was Forever (character "Lovely Heals", client 1.60.1, Durotar/Barrens), but the charter does not record a D1 preference.
2. Does Profeshy intend to make money (Q6)? That flips Route C from free to $99/mo Product tier and changes the D4 maths.
3. Is building on an undocumented Blizzard API acceptable, or is "documented or third-party-documented only" a hard rule? That decides B vs C for Forever.
4. Is a small backend (and its monthly cost ceiling, D4) acceptable at all? If not, Route C is the only one that ships live prices without one — and then history/forecasting are rented.

**Proposed charter amendments (for peer review — not applied):**
- §3: rewrite the Forever bullet to state the undocumented API exists since 2026-11-06, with the ruleset model, the duplication quirk, the item-coverage gap, and the "can change any time" warning; add Route C (AHledger) and ForeverDB as named candidates.
- §6: add AHledger as an explicit S3 candidate with its licence tiers and the 2026-12-10 terms change; note that a "no backend" answer now points at Route C, with the rented-history consequence stated.
- §8: split S1 into S1a (official) and S1b (Forever undocumented, incl. the trailing-slash, ruleset-duplication, and item-404 checks); add to S3 a licence-cost check against D4 before 2026-12-10; add ForeverDB to S4.
- §10: add a kill/pivot trigger — if the Forever API is withdrawn/changed incompatibly and no fallback source covers the gap, Profeshy pivots to guides + manual tracking or stops; it does not fake "live."
- §1 market note: replace "two mainline companions" with the named S8 starter list in §5 above, marked unverified until the hands-on Play Store pass.

## 7. What was checked, and what was not

- Checked 2026-11-14: Blizzard Forums Forever API thread (full read, post 2026-11-05 + update 2026-11-06); AHledger developers page (full read, crawled 2026-11-14 20:35 UTC); ForeverDB developers page (search result); Blizzard profession/recipe endpoints (Blizzard Forums patch notes + current client-library docs); Play Store / App Store companion search (Ragetusk, WoW Forever Hub/Path, Wowdee, official WoW Companion).
- Not checked: a live call against any of these APIs (no accounts, no keys, no spending authorized — that is Spike S1/S2 work); the official Blizzard API docs page itself for the current rate limit (third-party guides cite 1,000/30s — S2 must confirm); the exact two Play Store apps Kade meant (S8 hands-on pass still needed); whether Kade plays retail or Forever day to day (owner question above).
- Timing: the AHledger terms change takes effect **2026-12-10** — any decision leaning on AHledger's free tier should be made with that date in view, not after it.
