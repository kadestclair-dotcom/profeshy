# Profeshy — S4 input F3: ForeverDB terms (recorded 2026-11-14)

Source: https://foreverdb.net/developers, read in full 2026-11-14.
This completes the desk half of S4's F3; the coverage checks (F1/F2)
need an API key — see the access note at the end.

## Terms as published

- **Access:** API key required. Getting one needs a ForeverDB account
  (Battle.net login) plus an access request stating what you're
  building, an email, and agreement to credit. ForeverDB approves
  manually; up to 3 keys per account. Key sent as `X-API-Key` header.
- **Free tier:** 5,000 requests/day, 30/minute, per account across all
  keys. Over the free limit the API answers **402**
  (`subscription_required`). **Credit is mandatory on the free tier:**
  a visible link to ForeverDB.net wherever the data is shown.
- **Paid plan:** **$5.99/month** — 10,000 requests/day, 60/minute.
  Over-limit answers 429 with `Retry-After`. Credit optional on paid.
  Payments via Stripe.
- **Data:** extracted from the WoW Forever client every build, plus
  player-addon observations. Answers carry `data` + `meta` (game
  build, totals, pagination).
- **Relevant endpoints:** `GET /api/v1/items/{id}` (full item detail),
  `GET /api/v1/professions` (all 12 professions **with recipe
  counts**), `GET /api/v1/professions/{slug}` (ranks, trainers, and
  recipes with reagents, crafted output, learn sources — paginated).
- **Integration gotcha:** Cloudflare rejects some default library
  User-Agents (Python urllib named explicitly) with 403/1010 — API
  clients must send a User-Agent naming the app.

## What this means for S4

- F1/F2 (coverage of the known-404 sample; recipe cross-check) cannot
  run until the owner holds a ForeverDB key. Applying is the owner's
  action (Battle.net login + a short "what I'm building" note) and
  approval timing is ForeverDB's. The S4 harness therefore treats the
  key as optional: P1–P5 (Blizzard side) run regardless; F1/F2 record
  "no key" if absent.
- The free tier's 5,000/day is ample for spike-scale sampling; any
  production use at app scale is a D4/Q6 decision, with the $5.99
  figure now on record.
- Credit wording is a licence condition, not a courtesy — it belongs
  in the Contract if ForeverDB data is ever used.
