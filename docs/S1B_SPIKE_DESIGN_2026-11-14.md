# Profeshy — Spike S1b Design (Forever API access & key custody)

**Status: DRAFT FOR OWNER APPROVAL / PEER REVIEW. Authorizes no code, no API calls, no spending.**
**Date:** 2026-11-14
**Companion to:** `PROFESHY_CHARTER_v0.1_DRAFT_FOR_PEER_REVIEW.md` (§3, §6, §8 S1, §9 G1, §10) and `D1_DECISION_BRIEF_2026-11-14.md` (§2 Route B, §6)
**Decision it serves:** Decision #001 (D1) — Route B: WoW Forever via Blizzard's undocumented Forever API, using the owner's own Blizzard API client. S1a (official) is deferred, not cancelled.

## 0. What S1b must prove — and what it must not do

S1b is a **throwaway** spike. It produces evidence, not feature code. It passes only if **all** of these are shown, end to end, with the owner's existing client:

1. A token can be obtained with the owner's client and used against the Forever namespaces.
2. A live Forever auction house can be read, with its quirks measured, not assumed away.
3. The secret is **never** in an app binary, a log, a screenshot, or chat — the custody model is demonstrated, not asserted.

S1b does **not**: write any Profeshy feature code; build the backend; collect price history beyond the spike's own samples; test AHledger as a primary source (it is the named fallback only — a fallback smoke test is listed as optional T11, and adopting it needs a new decision); or make any call with a secret the owner has not supplied through the agreed channel (see §1).

## 1. Custody model for the spike (the point of S1b)

- The owner's Blizzard client ID + secret already exist (created for Awesome House). **No new developer account is needed, and the secret is never typed in chat, pasted into a doc, or committed to a repo.**
- The spike runs as a **local, throwaway script** (not an APK, not a shipped app). The secret reaches it only as a local environment variable on the machine that runs it, or via the secure vault — whichever the owner chooses at approval time. The script reads it from the environment at runtime and never prints, logs, or persists it.
- The spike also builds a **minimal local proxy stub**: a tiny local server that holds the secret, exchanges it for a token, and serves auction data to a dummy client that holds no secret at all. This is the custody pattern the real backend would use, proven at spike scale. The stub is throwaway; choosing the real backend stack is D4, at G2 — not here.
- **Custody proof (T10):** after the run, the dummy client's binary/source and the full run log are searched for the secret (and for the token, after expiry). Any hit is a fail. The token's 24-hour lifetime is recorded; the spike never stores a token past its use.

## 2. Test list

All tests run against the Forever namespaces from the D1 brief: `static-forever-{region}` / `dynamic-forever-{region}`. Region and ruleset for the first run are the owner's call at approval (§5); the brief's samples were EU, so EU is the suggested default unless Kade plays US.

| # | Test | What is done | Pass criterion | Evidence to record |
|---|---|---|---|---|
| T1 | OAuth token | Client-credentials flow against `https://{region}.battle.net/oauth/token` with the owner's client; request a token for the Forever namespaces | Token returned, valid, and accepted by a Forever endpoint (not just by the token endpoint) | Token lifetime observed; scopes/namespaces accepted; time to obtain |
| T2 | Ruleset index | `GET /data/wow/ruleset/index` and `/data/wow/ruleset/{id}` in `dynamic-forever-{region}` | Index returns the expected rulesets (brief: 1 PvP, 2 Normal, 3 Roleplay, 15 Roleplay PvP for EU/US); invalid id (e.g. 999) 404s | Full ruleset list with ids, names, `auctions`, `status` |
| T3 | Trailing-slash route | `GET /data/wow/ruleset/{id}/auctions/` **with** trailing slash vs the same path **without** it | With-slash returns the auction-house index; the without-slash behaviour is recorded as it actually is on the day (the brief says it 404s and that this flipped once — the spike records the current truth, it does not assume it) | Status + body for both forms, verbatim |
| T4 | Auction-house index & empty houses | Read the house index for one ruleset; fetch at least one populated house and the three houses the brief flags as empty (112, 115, 118 — one per faction) | Populated house returns auctions; empty houses are handled as "no `auctions` field" (absence shown as absence, per charter §7.5) — a missing field must not crash the reader or be rendered as zero listings | House ids 110–121, 123–125 as returned; which houses lack the field |
| T5 | Ruleset duplication (the big trap) | Fetch the **same** house (e.g. 113) via rulesets 1, 2, and 3 and compare the returned auction ID sets | Duplication is **measured**: overlap % and identical-ID count recorded. Pass = the spike can state, from data, whether the sets are identical on the day. The design rule it must demonstrate: a consumer that sums across rulesets would over-count, and the spike's reader does not | Auction counts per ruleset; ID-set intersection/union; the brief's figure was 68k identical IDs via rulesets 1–3 |
| T6 | Item-lookup 404 gap | Take the distinct item IDs from one fetched house and resolve a sample of them via `/data/wow/item/{id}` in `static-forever-{region}` | 404 rate is **measured** on the day and reported with the sample size (brief's sample: 652/1,515 = 43% in one EU Alliance house). Pass = a rate + sample size + the list of 404ing ids is recorded, and the reader degrades gracefully (name/icon shown as "no data", never invented) | Sample size, 404 count and %, examples (brief: item 720, Brawler Gloves) |
| T7 | Payload size & latency | Record response bytes, auction count, and wall-clock time for the busiest house fetched, on a normal network | Sizes are **measured** (brief: ~9 MB, 50–68k auctions for the busiest houses). Pass = the numbers are recorded with the house, ruleset, and time — S2 then judges what that means on a phone network; S1b does not pre-judge it | Bytes, auction count, latency, house/ruleset, timestamp |
| T8 | Commodities endpoint | `GET /data/wow/ruleset/{id}/auctions/{houseId}/commodities` for the same house | Returns commodity auctions with `unit_price`, or the actual failure is recorded | Status, count, sample entry shape |
| T9 | Freshness / update cadence (light) | Fetch the same house twice, separated by a stated interval (suggested: 60 minutes), and compare a small fingerprint (auction count + a sample of ids/prices) | Pass = the spike can state whether the snapshot changed between fetches and by how much. This is a first cadence datapoint for S2/S3, not a history pipeline | Two timestamps, counts, changed-sample count |
| T10 | Custody proof | After the run: search the dummy client, its binary, and the full run log for the secret; confirm no secret in any committed file | **Zero hits** for the secret in client-side artefacts and logs. Any hit = fail, and the run is discarded and re-run after fixing the leak | Search method + result (pass/fail), token lifetime observed |
| T11 | Fallback smoke test (optional, only if the owner approves it) | One unauthenticated read of AHledger's public markets/prices endpoint to confirm the fallback is reachable | Reachable and returning data, or the actual failure recorded. This test does **not** adopt AHledger, store its data, or build on it — that needs a new decision | Status + a single sample response shape; licence terms as in the brief (free with credit while non-commercial; terms change 2026-12-10) |

## 3. Evidence log (what "done" looks like)

One run log, kept with the spike, in this shape — numbers, not adjectives:

- Date/time (with timezone), region, ruleset(s), house id(s) used.
- Per test: T-number, endpoint called, HTTP status, counts/sizes/latency, pass/fail against the criterion above.
- T5: the three rulesets' ID-set comparison, stated as counts.
- T6: sample size, 404 count and %, and the 404ing ids (or a file reference if long).
- T10: the custody search result.
- **Verdict line:** S1b PASS / FAIL / PARTIAL, with the failing test numbers named. A partial pass names exactly what blocks G1 and what the next run must re-test.
- **Kill-trigger check:** if the Forever API returns 404s across the board, withdraws the auctions route, or rejects the owner's token for Forever namespaces, that is recorded as a §10 trigger event — the project pivots to guides + manual tracking or stops; it does not fake "live". AHledger as fallback is then a decision for the owner, not an automatic switch.

## 4. What S1b hands to the next spikes

- **S2 (AH payload reality):** T7/T9 numbers — real sizes, latency, and cadence — so S2 judges phone-network feasibility from evidence.
- **S3 (history sources):** T9's cadence datapoint + the fact that history still requires self-collection (the API returns snapshots, not history); AHledger remains the named fallback, licence-gated past 7–30 days from 2026-12-10.
- **S4 (recipe data):** T6's item-coverage gap is the warning S4 must carry — if 43%-scale gaps exist in item data, recipe/material resolution needs the same measurement, and ForeverDB is the candidate source named in the brief.
- **G1 exit:** S1b PASS is one quarter of G1 (S1–S4 evidence in). G1 still needs S2–S4.

## 5. Decisions the owner makes at approval (small, all his)

1. **Region + ruleset for the first run** — suggested default EU / ruleset 2 (Normal), matching the brief's samples, unless Kade plays US (then US / the ruleset he plays on).
2. **Who runs it** — Kade runs the throwaway script locally with his secret in a local env var, or he authorises Muse to run it with the secret supplied through the secure vault. Either way the secret never appears in chat.
3. **T11 yes/no** — include the one AHledger fallback smoke test, or leave the fallback entirely untested for now.
4. **Sample size for T6** — suggested: all distinct item ids in the fetched house if under ~2,000, else a stated random sample of 500, so the 404 rate is comparable to the brief's 1,515-item sample.

## 6. What was not decided here

- No backend stack, hosting, or cost ceiling — that is D4, at G2.
- No monetization answer — Q6 stays open; it changes the AHledger maths if the fallback is ever adopted.
- No charter amendments — the brief's §6 amendments remain proposed, to be applied at the G0/G2 contract pass; Decision #001 governs where they differ.
- The undocumented API can change without notice. Every S1b finding is dated evidence, to be re-checked at G1 — a PASS today is not a promise the endpoint stays up.
