# G1 exit demonstration: results

**Date:** 2026-11-18, run by Kade on his PC and phone.
**Written by:** Claude (reviewer), from the evidence files listed below.
**Claim level:** Tested. One live run on one evening, house 113, one item and one recipe.
**Status:** close-out review done. **Review #30 (Gemini, 2026-11-18): APPROVED WITH DIRECTIONS** on version `8445f334bd6a`; directions and recommendations applied (§8). The G1 decision is still Kade's (§7).

## 1. The criterion

Charter, gates list:

> G1 — Data feasibility. S1–S4 evidence in. Exit: a live price and a validated recipe can be produced on a phone, with provenance, through the chosen access model — or the project pivots per §10.

`docs/G1_CLOSEOUT_CHECKLIST.md` lists this as the open item "G1 exit demonstration". Kade chose a fresh live demonstration (option B) over citing the S2/S4 evidence (option A) at 19:39 PST.

## 2. How it was run

- **Access model: Route B** (Decision #001), the owner's own Blizzard API client.
- **The PC as a stand-in backend.**
  - The PC holds the key. It fetches the data and builds one static page, then serves it over home Wi-Fi.
  - The phone opens that page in Chrome. It holds no key and never calls Blizzard.
  - This matches S1's custody aim: no secret in the app.
- **Harness:** `scripts/g1_exit_demo.py` (throwaway evidence tooling), final version 595 lines, SHA-256 `1fd05674bf8b48b4e8be91af35ab337889bc0c5c582f66ce2dcf800bda7dd474`.
  - Built by Claude Code to `TASK_G1_EXIT_DEMO_2026-11-18.md`, with Amendments 1–3.
  - Reviewed by the reviewer four times; Reviews 1–4 are in the task file. Review 1 caught a crash in the serving step before any live run.
- **Network exposure:** a temporary Windows firewall rule allowed inbound TCP 8765 for Python on the Private profile only. Kade removed it after each session.

## 3. Result: run `20261118-210413` (the complete run)

**Live price**

| Field | Value |
|---|---|
| Item | Light Leather (item 2318) |
| Lowest buyout per unit | **65c** |
| Listings / total quantity | 391 / 14,432 (391 of 391 priced); the page shows the quantity unformatted, as `14432` |
| Price source | Commodities route, `/data/wow/ruleset/1/auctions/113/commodities`, namespace `dynamic-forever-us` |
| Fetched | 2026-11-18 21:04:16 PST (05:04:16 UTC), HTTP 200 |
| Snapshot `Last-Modified` | Thu, 19 Nov 2026 04:25:17 GMT |
| House route, same run | HTTP 200, 148,905 auctions, 14,967,923 bytes |
| Commodities route | HTTP 200, 120,295 auctions, 10,482,923 bytes |

**Validated recipe**

| Field | Value |
|---|---|
| Recipe | Handstitched Leather Boots (recipe 1353), Leatherworking |
| Crafted item | Handstitched Leather Boots (item 2302), makes 1 |
| Reagents | Light Leather (2318) × 2, Coarse Thread (2320) × 1 |
| Fetched | 2026-11-18 21:04:22 PST, namespace `static-forever-us` |
| Live enumeration | 27 calls, 0 non-200; **2,494 recipes, the same as S4's `P2_total`** |
| Selection | First recipe examined qualified (rank 1). Its reagent Light Leather was listed live. |

"Validated" means Amendment 1's definition. The recipe passed S4's own checks, re-run live in this run:
- it is listed under its profession
- the recipe returns 200
- every item resolves

It also sits inside S4's tested window: rank 1 of the 400 lowest recipe IDs, whose 192 reagents all resolved in S4 (`s4_run_20261118-123204.json`). **S4 kept counts only, so this is not a field-by-field match against S4.** The page says so. This run's JSON now holds the full recipe record S4 never kept.

**Shown on the phone**

- **The phone's visit:** the phone at `192.168.0.6` requested `GET /` from the PC at `192.168.0.5:8765` at **21:05:35 PST**, answered 200 (`_requests.json`).
- **Screenshots:** Kade's screenshots were taken at 21:05:44, 21:05:49 and 21:05:52. They show both cards with every provenance line, and match the JSON field for field.
- **The served page:** fingerprint `50ec9a45…60df`.
- **How it stopped:** Kade pressed Ctrl+C at 21:08:03.

**Custody**
- Every evidence file passed the script's built-in check for the client ID, secret and token before writing.
- The reviewer's grep of the run's JSON and TXT for `client_secret`, `access_token` and `bearer` found 0 matches.

## 4. Earlier attempts (kept as written)

| Run | What happened | Why it is kept |
|---|---|---|
| `20261118-202305` | Token and house fetch fine (143,158 auctions). All 50 recipes skipped with "no reagent listed"; no page served. | The reviewer's spec omitted the commodities route, where crafting materials are listed (S1b T8, S2 A5). Fixed in Amendment 2. An honest failed attempt. |
| `20261118-203948` | **Partial evidence only** (page and screenshots, no run log or request log). It worked: the same item, price and recipe as the final run, shown on the phone (three screenshots, page `07985bd1…20a7`). | Kade closed the PowerShell window while it was serving, so the JSON/TXT log was never written (it was only written at the end). Fixed in Amendment 3: the log is now written before serving. |

The final run's numbers equal the 203948 run's. Blizzard published no new snapshot between them: `Last-Modified` was 04:25:17 GMT for both. The fetch times are new.

## 5. Limits (what this does not show)

- **The backend is a stand-in.** The PC served a static page over home Wi-Fi. It is not a deployed backend, there is no app, and no phone network was involved. Those are D4, S6 and S7 questions.
- **One house, one item, one recipe, one evening.** This is evidence that the path works, not that coverage is complete. S4 and F1 hold the coverage evidence.
- **One region and ruleset:** US, ruleset 1, house 113, namespaces `dynamic-forever-us` and `static-forever-us`. Other houses, rulesets and regions were not exercised.
- **The recipe was chosen by a deterministic rule,** not sampled: the first recipe in ascending ID order with a listed reagent. It is a simple two-reagent recipe, not a random or high-complexity case.
- **The recipe is validated by re-running S4's checks,** not by comparing against a stored S4 record (none exists).
- **Prices are hourly snapshots.** The page shows the fetch time and Blizzard's own `Last-Modified`, never "real-time".

## 6. Evidence (SHA-256, files in `evidence/`)

| File | SHA-256 |
|---|---|
| `g1_exit_demo_20261118-210413.html` | `50ec9a45cebc695bb6a608a1ef0cb9f952fbd2bf97ae5502022725b23c1860df` |
| `g1_exit_demo_20261118-210413.json` | `71fb0a626ffe262087cd951921bffc0eea0e5a042f5ffed1ba77784277dab03b` |
| `g1_exit_demo_20261118-210413.txt` | `de7816be8e7509bb6b10f03ef653fb85712b510c9d15be28f79357efa537d359` |
| `g1_exit_demo_20261118-210413_requests.json` | `1e0222377ee1edb2d95ae3d1b6c6d3a9b8216ebf4846500c7f18fb0c9a636218` |
| `g1_exit_demo_20261118-210413_phone_1_price.jpg` | `7164a416b0e9b2021dd0e9bd1a96bba3fc7c2dca7b8b4e0e6eab995f62fed446` |
| `g1_exit_demo_20261118-210413_phone_2_recipe.jpg` | `a0715f9a6ddd856389cd984b3e20cb2fd9b0b99680797c6d7385f3acfd2b4fac` |
| `g1_exit_demo_20261118-210413_phone_3_recipe_provenance.jpg` | `d3be2e8df814dbeb334a837b0de233444a7680a0f22ddd6fe3fa4bc04f7caff0` |
| `g1_exit_demo_20261118-203948.html` | `07985bd1e0c61442413cabf11133632c805d2370820652aadc21de6f8f0820a7` |
| `g1_exit_demo_20261118-203948_phone_1_price.jpg` | `4d701d34658efba0443ac2e9f16c6da1f72908d0a1168c1f2ab27f9230dc2590` |
| `g1_exit_demo_20261118-203948_phone_2_recipe.jpg` | `32312fa28b1761b48eac04722239cef7ee67241a0a034c9a45192b840347a444` |
| `g1_exit_demo_20261118-203948_phone_3_recipe_provenance.jpg` | `edd8ed2be9cc143896613166782d1851d61060c933fd8281ca0797e3c3e26b0b` |
| `g1_exit_demo_20261118-202305.json` | `9ef39b6b733efad83162e380afd69f3d510239fad43640864adbc75955eab2c1` |
| `g1_exit_demo_20261118-202305.txt` | `ad5260b3e384eb7d58a3099b5d062d8ac19a63edd21c4717c190c1e7ebda4d79` |

The screenshot fingerprints are those of the copies saved to `evidence/` on the PC. The copies relayed through chat differ by a few KB, most likely stripped metadata; the PC copies are the evidence.

## 7. For the owner

**Reviewer's reading:** the criterion is met, at Tested. A live price and a validated recipe were produced on a phone, with provenance, through Route B. No secret reached the phone.

**Open before the G1 decision:** the checklist says the close-out review confirms whether the demonstration satisfies the criterion as written. That is a peer review of this document; it would be #30 in the log.

**Then Kade decides:** G1 CLOSED, or the specific item that remains. After that, in the master rules' order:
1. tick the checklist
2. update master §8
3. refresh the Notion snapshots

## 8. Review record

**Review #30, Gemini, 2026-11-18. Verdict: APPROVED WITH DIRECTIONS**, on version `8445f334bd6a`. The reviewer checked each clause of the criterion and found all five met:
- live price
- validated recipe, under Amendment 1, with the S4 gap kept as a stated limit
- on a phone
- with provenance
- through Route B, where a PC stand-in backend serving a static page over Wi-Fi is "a fair and proportional reading" at the feasibility stage

The reviewer found the evidence chain consistent across the document, Appendices C–F and the screenshots, and found no overclaiming beyond Tested. Its scope line: **"Supports recording G1 CLOSED"** under Tested and the §5 limits, subject to the owner's sign-off. It does not certify:
- multi-item coverage
- real-time latency
- the production app or a deployed backend
- mobile networks
- compliance with Blizzard's API terms

| # | Finding | Class | Disposition |
|---|---|---|---|
| 1 | Appendix E (`.txt`) does not show its own write confirmation | Recommended | No change. The `.txt` is the log as it stood when written, so it cannot contain the line confirming its own write. That line appears on the console, captured in Kade's screenshot of the run. The JSON confirmation is in the `.txt`. |
| 2 | Quantity `14,432` in §3 vs `14432` on the page | Recommended | Applied: §3 notes the page's unformatted value. |
| 3 | Treat run `203948` as partial evidence | Recommended | Applied: §4 labels it partial evidence only. |
| 4 | Keep the `evidence/` screenshot hashes canonical | Recommended | Already stated in §6; no change. |
| Q7 | Add single-region and ruleset scope, and selection-order determinism, to §5 | Answer to question 7 | Applied: two bullets added to §5. |
| Q5 | Don't print raw auth exception or response bodies | Custody note | Already so: the token failure prints only the HTTP status (Appendix B). |

Answers to the questions: "live price" is honest with both timestamps shown. Amendment 1's "validated" is adequate for G1 with the gap kept as a limit. The earlier attempts are reported honestly and do not weaken the final run.

