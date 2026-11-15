# Profeshy — Spike S8 Competitor Audit

**Status: COMPLETE (listings-based). Hands-on device pass still open.**
**Peer review (2026-11-14): Approved as-is. Adopted into the charter the
same day: §1 market note updated per §4 below, and Spike S8 formally
marked Tested (Listings) in charter §8.**
**Date:** 2026-11-14
**Scope/method:** public Google Play and App Store listings, developer
sites, and data-source pages, read 2026-11-14. No hands-on installs —
no Android device was available; listing claims are recorded as claims,
not as tested behavior. Full sourced research report retained in the
project research notes.

---

## 1. Headline verdict

**Profeshy's differentiation is real on Android, and the charter §10
kill criterion (differentiation imaginary → reopen G0) is NOT
triggered.** The gaps no competitor fills:

1. **Price forecasting — claimed by nobody.**
2. **Android Forever AH prices + alerts — nobody offers this.**
   Ragetusk excludes market data from its Android build entirely;
   Auction House Tracker's Android build has no alerts, and its Forever
   data has no history or alerts on any platform.
3. **Profession leveling guides priced live / profit per level —
   nobody.** Ragetusk explicitly disclaims it ("No cheapest route is
   claimed").
4. **An all-in-one Android bundle** (guides + economy together) — the
   field is split between guide apps with no economy and one economy
   app with no guides.

## 2. Corrections to earlier project documents

These supersede statements in the D1 Decision Brief §5 and the charter
§1 market note:

- **Blizzard's official WoW Companion no longer exists.** It was
  retired in July 2024 (The War Within); its Play listing returns Not
  Found. It is not a current competitor on either store.
- **The owner's "two mainline-focused Play Store companions" note could
  not be confirmed as stated.** Only one current Play app clearly fits
  "mainline-focused with AH value": Auction House Tracker for WoW. The
  second slot is ambiguous post-delisting (weak candidates: Mythic
  Codex — character tracking only; Dewa's Companion for WoW —
  checklists/guides, 0+ downloads).
- **Framing:** 2026 sources treat WoW: Forever as a Blizzard product
  launched 2026-11-04 (subscription-included, partly served by
  Blizzard's Game Data API), not a private server. Project documents
  that say "private server" should be corrected at the next pass.

## 3. The field

### Ragetusk: WoW Forever Guide — Edge Case LLC (closest competitor)
- Android + iOS, Forever only. Android v1.0.10 (Nov 11, 2026), 1K+
  downloads. iOS v1.0.6, 5.0★ (1 rating).
- **Android build explicitly excludes the economy:** the Play listing
  states live auction prices, economy and crafting-market data are
  unavailable in the Android build, and purchases are not included.
  Android = offline bundled guide + talent planner + on-device saves.
- iOS/web: AH search across six US Forever markets, community-scanned
  prices with observation dates, price history, scan medians, crafting
  costs. Crafting margin = market value after the 5% AH fee − reagent
  cost; deposits and sell-through excluded; missing reagents disable
  profit figures.
- Data source: **AHledger community scans** (20 of 32 markets scanned,
  newest Nov 14, 2026; add-on can scan every 15 min, sync every 30 min).
  Ragetusk's own wording: observations "can be delayed or unavailable;
  they are not guaranteed real-time quotes."
- Monetization: free; iOS Premium is a **one-time $4.99** (ad removal);
  all guides/tools free; no subscription.
- No forecasting, no price alerts, no live-priced leveling route.
- ⚠️ Conflict recorded: Ragetusk's support/privacy pages (stamped
  2026-10-31) claim Android v1.0.3 added online auction data and v1.0.4
  adds Premium — contradicted by the current Play listing. The Play
  listing is treated as authoritative; the discrepancy is unexplained.

### Auction House Tracker for WoW — Edge Case LLC (strongest AH app)
- Android + iOS. Retail, Classic family, Era, Anniversary, Forever.
  Android v1.1.1 (Nov 12, 2026) — only 50+ downloads. iOS v1.1.1.
- Android: price snapshots with source + update time shown, search,
  watchlist, available price history. **No Android price alerts, no
  purchases** (per its listing).
- iOS adds 24h/7d/30d history and unlimited price alerts — **but price
  history and alerts are unavailable for WoW Forever** on any platform.
- Data sources as stated: Retail + Forever from Blizzard's Game Data
  API; Classic fallback = TradeSkillMaster AuctionDB snapshots; Forever
  Beta = AHledger snapshots, kept separate.
- No profession guides, no crafting profit, no forecasting.

### Campfire: WoW Forever Guide — Joseph Towne (iOS only)
- $1.99 paid, v1.0.4 (~Nov 9–10, 2026), 3.0★ (2 ratings).
- **Real materials-needed tracking**: crafting plans combine shared
  ingredients and subtract owned supplies — Profeshy's Stage 3 feature,
  present here. But **no prices, no profit, no AH** — the plan is
  unpriced.

### Forge Path — Bjarke Søgaard (iOS only found)
- WoW Classic (not Forever) profession companion: crafting paths +
  materials tracking, offline. Free + $2.99 one-time Premium. Last
  updated May 12, 2026. No AH data.

### Wowdee — Sploosh Games (announced, not available)
- Mobile app is **"coming soon"** on web, App Store, and Google Play —
  no store listings exist. Only the in-game add-on is out (CurseForge,
  112.4K downloads, updated Nov 14, 2026): tooltip AH prices from the
  player's own scans, bargain alerts, 7/30-day history, "Craft for
  gold" estimated-profit comparison, materials/shopping lists — all
  in-game, none of it mobile yet. Watch item: if the app ships with the
  add-on's feature set, it becomes a direct competitor.

### Adjacent Forever companions (no AH / profession economy)
- WoW Forever Hub (iOS, free, no IAP): leveling builds, raid planner,
  757 BiS lists. No professions, no AH.
- WoW Forever Path (iOS, 2.3★/3 ratings): talent planner, profession
  *lookup* only; IAPs are $20–$1,000 "War Chest" donations. No AH.
- WOW Forever Companion by Metaforge (Android, 100+, ads): offline
  database, talent calculator. No professions/AH.
- Companion for WoW Forever by Galaxy Software (Android, 500+, ads):
  talents, leveling routes, gear wishlist. No AH/professions.
- Dewa's two companions (Android, 0+ downloads): timers, lockouts,
  checklists. No AH/professions.

### Defunct
- Blizzard WoW Companion — retired July 2024 (see §2). Historically
  1M+ installs; never had profession guides, profit, history, or
  forecasting even while live.

## 4. What this means for the charter (proposed, not applied)

- §1 market note: replace the "two mainline companions" sentence with
  this audit's §2 corrections.
- S8 status: **Tested** for listing-level claims (this document);
  hands-on verification remains open and should be scheduled before G3,
  not treated as done.
- Differentiation rests on Android + Forever + live economy together,
  profit-per-level, and forecasting — in that order of defensibility.
  Ragetusk's Android exclusion is the single most useful fact in this
  audit: the closest competitor has publicly committed to iPhone-first
  for exactly the features Profeshy leads with.
- Watch items: Wowdee's app launch; any Ragetusk Android build that
  adds market data (their stale support pages suggest it was at least
  planned); AHledger coverage (20/32 markets) as Route C fallback
  quality evidence.

## 5. Could not verify

- The exact identity of the "two" Play Store apps the owner meant.
- Why Ragetusk's support/privacy pages contradict its Play listing.
- Ragetusk's Android rating (none shown — too few ratings).
- Forge Path on Android (none found; absence from search is not proof).
- Wowdee app date, pricing, and final features.
