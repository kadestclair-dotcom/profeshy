# Profeshy — Spike S2 Re-run (full query battery and refresh-cycle memory)

**Status: PEER REVIEWED 2026-11-18 (review #29, ChatGPT): APPROVED WITH DIRECTIONS on version `33abbad72721`. Review #28's findings A1 and A2 are closed; the three directions are applied in this version (§11). Verdict: S2 PASS, both parts, at Tested, with the limits in §6. Responds to review #28 (NOT APPROVED on the Part B section of `docs/S2_RESULTS_2026-11-14.md` at commit e970c90). Not in the repo yet: `docs/S2_RESULTS_2026-11-14.md` is unchanged until the owner approves the push in `REPO_UPDATE_PLAN_2026-11-18.md` step 6.**
**Owner decision (2026-11-18):** the one phone run at 9–20% battery while charging is accepted; no repeat at normal charge is planned (§6, §10).
**Date:** 2026-11-18
**Author:** Claude Code (assistant), from the run logs in the owner's Profeshy folder
**Design:** `docs/S2_SPIKE_DESIGN_2026-11-14.md` (repo commit e970c90; SHA-256 `54f8bb3a7741…`), §2 A4 and §3
**Task:** `TASK_S2_BATTERY_RERUN_2026-11-18.md`; harness review `REVIEW_S2_BATTERY_r1_2026-11-18.md`
**Harness:** `s2_battery.py` r2, SHA-256 `7c372413c6758c51f7e7f655c5d23a8f8bfd5b2671723970d5aa3da7c210992f`, approved by the reviewer before any real run
**Custody record:** `S2_RERUN_EVIDENCE_2026-11-18.md` (compiled by the reviewer). It is the source for run order, failure causes, custody (8/8 phone files verified) and run conditions; this document cites it and does not re-derive it.

## 0. Answer in one paragraph

Both gaps review #28 found are now measured. On the owner's PC and on the owner's phone (Samsung SM-G975U, Android 12, Python 3.14.10 under Termux), all four design A4 query classes ran against the Part A database: lookup by item ID, prefix search over item IDs, top-50 cheapest, and full-table scan, plus Part A's two extra classes. Every design class passed the 50 ms bar on both machines. The slowest class on the phone was full_table_scan (all 5,015 rows): best 20.1 ms, median 25.6 ms on the original database, and the worst single sample in the run was 38.5 ms, on the refreshed copy. One full refresh cycle on the phone (download the aggregate over the home LAN, decompress and parse, build a new database, replace, query) took 73 + 24 + 283 + 31 ms up to the replace, plus 399 ms for the full query battery. The Python process's resident memory high-water mark (VmHWM) went from 31,100 kB before the cycle to 37,140 kB after it. That is one Termux Python process on one 2019 flagship phone, measured once, at 9–20% battery while charging. It is not a figure for the shipping app and not a lower bound for it. Claim level: Tested.

## 1. What ran

All times PST, 2026-11-18. Hashes are from `S2_RERUN_EVIDENCE_2026-11-18.md`. On 2026-11-18 Claude Code re-hashed every file below on the PC with `sha256sum`, and all 16 matched the record (the 13 listed here, plus `s2_battery.py`, `s2_house113.db` and `s2_aggregate_house113.json.gz` in §1.1).

| Time | Machine | Log | SHA-256 | Outcome | Counted as a result |
|---|---|---|---|---|---|
| 16:48:21 | PC, Windows 11 (10.0.26300), Python 3.12.10 | `s2b_desktop_20261118-164821.json` | `e253d8a441c24e2b4300f179494abdb28f54412f6a385320d43782b13abc2406` | COMPLETE | Yes: Part A completion (§3) |
| | | `s2b_desktop_20261118-164821.txt` | `6da2020078f0690f964bf033dbc5486ddc7b14cbda53e1f12ad3722eeeea668c` | | |
| 16:52:47 | Phone, Termux, Python 3.14.10 | `s2b_phone_20261118-165247.json` | `a1c4c9ca6002c9394af5f82bee5a9d4d00b5af656c21f8ceff7dcbeeeb6183f4` | FAILED: download timed out | No |
| | | `s2b_phone_20261118-165247.txt` | `e67a105c934969e8c3644510a488d430be0a41211d1d7474166437f522415d3b` | | |
| 16:54:50 | PC | `s2b_serve_20261118-165450.json` | `42dc0e1816e163c690d552999347b341e4d0d868ead42c29a0abcfa7a5ce6dde` | COMPLETE: one request, 192.168.0.6, 17:05:56, status 200 | Serving only |
| | | `s2b_serve_20261118-165450.txt` | `c47ac5bacb1dcdb2236c05be49984022bffd02ff3aa0ae7f7f45d168884a91cf` | | |
| 16:55:09 | Phone | `s2b_phone_20261118-165509.json` | `f3b84788f5d1e88c291f83837748f25ee93014063f5cd5a5042a99b8dd501314` | FAILED: download timed out | No |
| | | `s2b_phone_20261118-165509.txt` | `2d680af62430efe2d3a29b3919e087e51aca471cb1372ad81ca19f8a848533d7` | | |
| 17:05:56 | Phone | `s2b_phone_20261118-170556.json` | `c643f2054ba2e0899bb2555ee53f10009052b8ccfdf21e9c17e533328466b3ed` | **COMPLETE: the evidence run** | Yes: Part B re-measure (§4, §5) |
| | | `s2b_phone_20261118-170556.txt` | `3c069efe785f092c6d04f0dd3ef6cf900513672ae3afda9149bd75e4dbd7d576` | | |

**The two failed phone runs** are part of the record and are not hidden. Neither contributes a number to this document. Both stopped at the download step, as the harness is designed to (no fallback to a local file). According to the custody record:

- **16:52:47:** the phone run started before serve mode was running.
- **16:55:09:** serve mode was running, and its log shows no request arrived. The PC's Wi-Fi was set to Public and no inbound firewall rule allowed Python. The owner then set the network to Private and added a temporary rule for Python 3.12's `python.exe` only, on TCP port 8765 and the Private profile. The 17:05:56 run then completed, and the rule was removed afterwards.

Before failing, each of these runs recorded P0 (memory preflight), P1 (input check: the database hash matched) and P2a (memory before the cycle). Those readings appear in §5 only as a cross-check of the start-up baseline.

### 1.1 Harness and inputs

| File | SHA-256 | Note |
|---|---|---|
| `s2_battery.py` | `7c372413c6758c51f7e7f655c5d23a8f8bfd5b2671723970d5aa3da7c210992f` | r2. Every log above records this hash in `meta.script_sha256` |
| `s2_house113.db` | `f765211ca4b7637f2734d6e6386bf5eb040f9563513a16716e8220235991c899` | The Part A database: 184,320 bytes, 5,015 rows. The phone copy had the same hash (phone log P1), so both machines measured the same file |
| `s2_aggregate_house113.json.gz` | `4fd530f63e45a8471e886541d46a209aa33358c8889d7060a7903db8c021caef` | Exported by the desktop run (D2): 64,184 bytes gzip, 533,193 bytes JSON. The phone downloaded it with a matching hash (P2b) |

## 2. Method

**Battery.** Six query classes against `item_stats` (index `idx_min_unit`). Each class was run 5 times in a row on one connection, timed with `time.perf_counter()` around execute plus fetch. The **best** time is the design's method and decides PASS or FAIL against the 50 ms bar. The median is shown alongside it, and all five samples are in the JSON logs.

| Class | SQL | Source |
|---|---|---|
| `lookup_by_id` | `SELECT * FROM item_stats WHERE item_id=?` | design A4 |
| `prefix_search` | `SELECT item_id FROM item_stats WHERE CAST(item_id AS TEXT) LIKE ? LIMIT 50` | design A4 |
| `top50_cheapest` | `SELECT item_id, min_unit_price FROM item_stats WHERE min_unit_price IS NOT NULL ORDER BY min_unit_price ASC LIMIT 50` (verbatim from `s2_spike.py`) | design A4 |
| `full_table_scan` | `SELECT * FROM item_stats`, `fetchall()` | design A4 |
| `count` | `SELECT COUNT(*) FROM item_stats` | Part A extra, no verdict |
| `top50_most_listed` | `SELECT * FROM item_stats ORDER BY listings DESC LIMIT 50` (verbatim from `s2_spike.py`) | Part A extra, no verdict |

- **Parameters.** The sample ID is the median `item_id` (`ORDER BY item_id OFFSET COUNT/2`), 10404 in every battery. The prefix is its first three digits, `104%`, which returned 14 rows each time. `CAST(item_id AS TEXT) LIKE` cannot seek on the key, so SQLite scans entries until it has 50 matches (query plan: `SCAN item_stats USING COVERING INDEX idx_min_unit`, checked by the reviewer on SQLite 3.37.2). `104%` has only 14 matches, so every prefix_search execution examined all 5,015 entries: the slowest case for this query, not a favourable one.
- **Row checks.** Each class's row count is recorded, and the harness stops if any class returns no rows or the full scan returns anything other than 5,015. All batteries returned 1 / 14 / 50 / 5,015 / 1 / 50 rows, and `count` returned 5,015.
- **Read-only access.** Every battery opens its database read-only (`mode=ro` URI). "Open" is the time to connect plus the first read.

**Two changes from the task text, both required by the harness review (`REVIEW_S2_BATTERY_r1_2026-11-18.md`):**

1. **`tracemalloc` was removed.** It adds resident memory of its own and slows the timings, so it would have inflated the VmHWM figure this re-run exists to measure. Memory is therefore read only from `/proc/self/status` (VmHWM, VmRSS) and `resource.getrusage` (`ru_maxrss`).
2. **On the phone, the refresh cycle runs before the battery on the original database** (order P0, P1, P2 cycle, P3 battery). VmHWM is the peak since the process started. With this order, the "after" reading covers only start-up, the input check and the cycle, and is not masked by an earlier full-table scan.

## 3. Part A completion: desktop, all six classes (D1)

Log `s2b_desktop_20261118-164821.json`, battery on `s2_house113.db` (read-only). Open: 0.544 ms.

| Class | Best (ms) | Median (ms) | Rows | Bar ≤ 50 ms (best) | Part A, 2026-11-14 (best, `s2_spike.py`) |
|---|---|---|---|---|---|
| lookup_by_id | 0.077 | 0.080 | 1 | PASS | 0.13 |
| prefix_search | 0.405 | 0.414 | 14 | PASS | not measured |
| top50_cheapest | 0.104 | 0.106 | 50 | PASS | 0.20 |
| full_table_scan | 5.435 | 5.481 | 5,015 | PASS | not measured |
| count | 0.074 | 0.082 | 1 | n/a | 0.16 |
| top50_most_listed | 0.460 | 0.535 | 50 | n/a | 0.67 |

Today's desktop figures are lower than Part A's for the four classes both runs measured. The two runs used different harnesses (`s2_spike.py` vs `s2_battery.py`), so this comparison is recorded, not explained.

**Aggregate export (D2):** 5,015 rows, 533,193 bytes JSON, **64,184 bytes gzip** (level 6). Part A recorded 533,193 and **66,996**. The harness logged the difference and did not adjust the export. The reviewer checked the cause against the original capture (`REVIEW_S2_BATTERY_r1_2026-11-18.md`, "Aggregate size"). The content is identical and only the row order differs. This export sorts rows by `item_id`. Exported in Part A's order (the order in which items first appear in `s2_house113_auctions.json`), the same rows give exactly 533,193 and 66,996 bytes. That check is the reviewer's, by one method: **Tested**. For the sync-size hand-off, one house's aggregate is 64–67 KB gzipped depending on row order. Both figures are far under the 1 MB bar.

## 4. Part B re-measure: phone, all six classes

Log `s2b_phone_20261118-170556.json`, Samsung SM-G975U (Galaxy S10+), Android 12, `Android-12-aarch64-64bit`, Python 3.14.10 under Termux. The database files were in shared storage, `/storage/emulated/0/Download/`.

### 4.1 On the original database (P3, `s2_house113.db`, read-only)

This is the like-for-like re-measure of Part B. Open, including the first read: 27.842 ms.

| Class | Best (ms) | Median (ms) | Slowest sample (ms) | Rows | Bar ≤ 50 ms (best) | Part B, 2026-11-14 (best, `s2_partb.py`) |
|---|---|---|---|---|---|---|
| lookup_by_id | 7.726 | 10.971 | 12.014 | 1 | PASS | 3.3 |
| prefix_search | 8.965 | 10.120 | 15.209 | 14 | PASS | not measured |
| top50_cheapest | 4.218 | 4.669 | 5.735 | 50 | PASS | 3.6 |
| **full_table_scan** | **20.069** | **25.567** | **33.577** | 5,015 | PASS | not measured |
| count | 3.507 | 4.272 | 5.783 | 1 | n/a | 3.55 |
| top50_most_listed | 9.890 | 10.852 | 12.049 | 50 | n/a | 5.17 |

### 4.2 On the refreshed copy (P2c, `s2_refresh_work.db`, built from the downloaded aggregate)

This is the "query" step of the refresh cycle. Open, including the first read: 20.029 ms.

| Class | Best (ms) | Median (ms) | Slowest sample (ms) | Rows | Bar ≤ 50 ms (best) |
|---|---|---|---|---|---|
| lookup_by_id | 5.368 | 8.352 | 8.980 | 1 | PASS |
| prefix_search | 9.193 | 10.749 | 15.952 | 14 | PASS |
| top50_cheapest | 5.138 | 7.415 | 15.723 | 50 | PASS |
| **full_table_scan** | **20.853** | **25.150** | **38.494** | 5,015 | PASS |
| count | 4.870 | 6.434 | 9.487 | 1 | n/a |
| top50_most_listed | 6.327 | 8.192 | 9.851 | 50 | n/a |

**Reading the phone numbers:**

- **Every design A4 class passes on both databases** (`design_classes_all_pass: true` in P2c and P3). This holds on the slowest sample as well as the best: none of the 40 design-class samples on the phone exceeded 50 ms. The highest was 38.494 ms, full_table_scan on the refreshed copy, 11.5 ms under the bar.
- **full_table_scan is the class closest to the bar.** Its best times are 20.1 and 20.9 ms, its medians 25.6 and 25.2 ms, and its five samples range from 20.1 to 33.6 ms (original) and 20.9 to 38.5 ms (refreshed). Whether the product ever loads all 5,015 rows at once is a product-design question, not answered here.
- **Phone versus desktop.** By best time, the phone was 3.7× slower on full_table_scan and up to 100× slower on lookup_by_id. The large ratios are on sub-0.5 ms desktop queries, where fixed per-call cost dominates, so they say little about scaling. Part B's "8–25×" statement compared the 2026-11-14 harnesses (`s2_partb.py` against `s2_spike.py`), and `s2_partb.py` is not in the evidence set, so it is not carried forward.
- **Phone versus Part B (2026-11-14).** lookup_by_id is 7.7 ms today against 3.3 ms then, top50_most_listed 9.9 against 5.2 ms, and open 27.8 against 5.42 ms. top50_cheapest and count are close (4.2 vs 3.6, 3.5 vs 3.55 ms). The causes are not tested. Known differences: a different harness, and `s2_partb.py` is not in the evidence set, so its timing and open method are UNKNOWN. Today's phone battery was at 9–20% and charging. Today's battery on the original database ran after the refresh cycle in the same process. Low-battery throttling is a possible cause: **Hypothesis**, not tested (custody record, "Run conditions"). The 2026-11-14 database location is UNKNOWN, so whether storage location differed is also UNKNOWN.

## 5. Refresh cycle on the phone (P2, one pass)

The design §3 cycle: download aggregate → replace → query. The harness worked on a copy of the input (`s2_refresh_work.db`) and never modified `s2_house113.db`.

| Step | What is timed | ms |
|---|---|---|
| Download (P2b) | `urllib.request` GET of `http://192.168.0.5:8765/aggregate.json.gz` over home Wi-Fi, PC to phone: connect plus read of 64,184 bytes. Status 200, hash matched | 73.273 |
| Decompress and parse | `gzip.decompress` plus `json.loads`, giving 5,015 rows | 24.126 |
| Build | new SQLite file next to the working copy: schema, `executemany` insert of 5,015 rows, `CREATE INDEX`, commit, close | 282.625 |
| Replace | `os.replace(temp, s2_refresh_work.db)` | 30.750 |
| **Subtotal: download through replace** | | **410.774** |
| Query | reopen read-only and run the full six-class battery (§4.2): 30 timed executions plus open, the sample-ID queries and the harness's log writes | 399.064 |
| **Total cycle** | | **809.838** |

The refreshed database had 5,015 rows, 176,128 bytes, SHA-256 `148d76e69de3…` (P2d). The custody record confirms the uploaded working file matches that hash, and keeps it as a working file, not evidence. It is 8,192 bytes smaller than `s2_house113.db` with the same row count. Row-by-row equality with the original was not checked.

### 5.1 Memory (P0, P2a, P2e, P2f)

| Reading | VmHWM (kB) | VmRSS (kB) | ru_maxrss (kB) |
|---|---|---|---|
| P0 preflight (process start) | 30,172 | 30,156 | 30,172 |
| P2a before the cycle (after the input check) | 31,100 | 31,016 | 31,100 |
| P2e after the cycle (after the query step) | **37,140** | 37,080 | 37,140 |
| Change across the cycle | +6,040 | +6,064 | +6,040 |

- **What these figures are.** The resident memory of one Python 3.14.10 process under Termux, including the interpreter itself (about 30 MB at start). They are not the shipping app's memory. The design sets no threshold, so the figures are recorded and not judged.
- **What the peak covers.** VmHWM is the process peak since start. Because the cycle ran before any other query (§2, change 2), the 37,140 kB peak covers start-up, the input check and the cycle, including the query step. The later battery on the original database (P3) is not covered by any memory reading.
- **Split by step: UNKNOWN.** The harness takes readings only before and after the whole cycle, so the +6,040 kB is not split between the parsed rows, the SQLite build and the query.
- **Start-up baseline cross-check.** P0 in the two failed runs read 30,160 and 30,512 kB VmHWM, and P2a read 31,088 and 31,544 kB. The start-up baseline was similar in three separate processes. Those runs never reached the cycle, so this does not corroborate the cycle figure.

## 6. Run conditions and limits (read before relying on §3–§5)

- **One house, one small database.** Every figure here is house 113's aggregate: 5,015 rows, a 184,320-byte database, a 64,184-byte download. Nothing here establishes performance for several houses held at once, for history tables, or for substantially larger databases (review #29, direction 6). Carry this scope into any later design decision that uses these numbers.
- **One device, one run.** One Samsung SM-G975U, a 2019 flagship. Whether it stands in for a mid-range phone is not tested. Review #28's scope already excludes mid-range devices.
- **Termux Python, not the shipping runtime.** A real Android app would use its own SQLite build and runtime. Its timings and memory could be higher or lower; neither direction is shown here. These results are **not a floor and not a lower bound** for the shipping app.
- **Phone battery at 9–20% and charging** during the runs (owner's screenshots). Whether battery saver was on was not recorded. The slower lookups compared with 2026-11-14 may be throttling: Hypothesis.
- **Shared storage.** The databases were in `/storage/emulated/0/Download/`, Android shared storage, not Termux's private home and not an app's private storage. Its effect on build, replace and open times is not tested.
- **LAN download.** The 73 ms download was PC to phone on the same home Wi-Fi (192.168.0.5 to 192.168.0.6). Internet and cellular transfer were not measured.
- **No name search.** The product's real search is by item name. This database holds no names, so only the design's prefix search over item IDs was measured (recorded in every log's `meta.limits`).
- **SQLite library versions** on the PC and the phone were not recorded: UNKNOWN.
- **Five samples per class, back to back on one connection.** Repeat queries probably hit SQLite's page cache, which favours the best time. The design specifies best-of; medians and slowest samples are given so the spread is visible.

## 7. Review #28 findings: disposition

Finding labels A1–A4 follow `PEER_REVIEW_LOG_ROWS_26-28_2026-11-18.md`, row 28.

| # | Finding (review #28) | Kind | Disposition |
|---|---|---|---|
| A1 | The battery does not match design A4: prefix search over item IDs and the full-table scan were never measured. Author's note in the log row: Part A had the same gap | Required | **Fixed.** `s2_battery.py` runs all four A4 classes plus Part A's two extras on both machines: desktop §3 (completes Part A), phone §4 (re-measures Part B). All four A4 classes PASS on both. Results are best and median of 5, with row counts |
| A2 | The refresh-cycle memory high-water mark (download aggregate → replace → query, design §3) was not measured | Required | **Fixed.** One full cycle on the phone with step timings and VmHWM / VmRSS / `ru_maxrss` before and after (§5). The cycle runs first in the process so its peak is not masked (§2). Recorded, not judged: the design sets no threshold |
| A3 | Drop "floor check", since no lower bound for the shipping app was demonstrated | Recommended | **Applied.** This document does not call the result a floor or a lower bound. It is one Termux Python process on one 2019 flagship, measured once, and not a lower bound for the shipping app (§0, §6). The 2026-11-14 wording stays as it is in `docs/S2_RESULTS_2026-11-14.md`, an accepted document. Once this document is reviewed, that file gets a dated note pointing here (`REPO_UPDATE_PLAN_2026-11-18.md` step 6), and this document's wording supersedes "floor check" |
| A4 | Keep the raw Part B logs in the evidence record | Recommended | **Applied.** The 2026-11-14 logs are now in the Profeshy folder and the custody record: `s2_partb_log.json` `d7d9cb95eaf8f596bb9c0d5da2420c9403bec1ade1d0dc4404e0434fc6859a09`, `s2_partb_log.txt` `55fcc286391548f8587f10ca0bf271c343942a8bf6ebd364b3c1e9c24549b259` (re-hashed 2026-11-18, both match). Their figures match the Part B section exactly (open 5.42; 3.3 / 3.6 / 3.55 / 5.17 ms). Limits: they carry no script hash, start and finish share one timestamp (20:41:38), `s2_partb.py` is not in the evidence set, and the text log's last line says to send the logs "back to Muse" (the prior assistant of record) |

## 8. Claim levels and proposed verdict

| Claim | Level |
|---|---|
| On desktop SQLite (PC, Python 3.12.10), all four design A4 classes are ≤ 50 ms on `s2_house113.db` (best of 5; slowest, full_table_scan, 5.4 ms) | Tested |
| On the SM-G975U under Termux Python 3.14.10, all four design A4 classes are ≤ 50 ms on the original database and on a copy rebuilt from the aggregate (best of 5; slowest, full_table_scan, 20.1 / 20.9 ms best; no sample above 38.5 ms) | Tested (one device, one run) |
| One refresh cycle on that phone: 410.8 ms from download through replace, plus 399.1 ms for the full battery; VmHWM 31,100 → 37,140 kB | Tested (one device, one run) |
| The 64,184 vs 66,996 byte aggregate difference is due to row order only | Tested (the reviewer's check, one method) |
| Low battery explains the slower phone lookups compared with 2026-11-14 | Hypothesis |
| Any of the above for the shipping app, other devices, name search, or internet download | Not tested |

Nothing here is Verified. A second run on the same phone with the same harness would be a repeat, not verification.

**Verdict (review #29, approved with directions):** S2 Part A completion PASS on all four design A4 classes; S2 Part B PASS on all four design A4 classes, with the refresh-cycle memory recorded. S2 returns to **PASS, both parts, at Tested**, with the limits in §6. Changing the status of `docs/S2_RESULTS_2026-11-14.md` waits for the owner's approval of that push.

## 9. Hand-offs (if approved)

- **Scope for every hand-off below:** one house (113), 5,015 rows, one phone, one run (§6).
- **D4/G2 (serving contract):** one house's aggregate is 64–67 KB gzipped depending on row order (§3). One phone cycle takes about 0.4 s from download through replace on a LAN (§5).
- **Stage 1 design:** the battery is the acceptance test the design names for "search feels instant". Every design class is under 50 ms on this phone, and the full scan has the least headroom. Name search remains unmeasured and needs its own test once names are in the store.

## 10. Questions for reviewers

1. Do §3–§5 satisfy findings A1 and A2 of review #28 as the design specifies them?
2. *Decided by the owner, 2026-11-18:* the one phone run at 9–20% battery while charging is accepted, and no repeat is planned. Reviewers may still comment on the limit this leaves (§6), and may say if they consider a repeat necessary.
3. The memory reading covers the whole cycle and is not split by step (§5.1). Is a before-and-after high-water mark enough for design §3's "memory high-water mark during a full refresh cycle"?
4. Is the treatment of the two failed phone runs adequate: listed with hashes and causes, and not counted as results (§1)?

## 11. Review record

**Reviewer's check before submission (Claude, 2026-11-18).** Every number in §3–§5 was recomputed from the JSON logs and matches, as do the totals, memory changes, ratios and the 40-sample maximum. Three edits were made to Claude Code's draft (`c1ed0ff786e6…`): the prefix-search scan note in §2, the source of Part B's "8–25×" in §4, and the wording of the A3 disposition in §7.

**Review #29, ChatGPT, 2026-11-18. Verdict: APPROVED WITH DIRECTIONS**, on version `33abbad72721` (hash cited; the reviewer also recomputed the full SHA-256 of the embedded document). The reviewer independently recalculated every figure in §3–§5 against the appended logs, including all 40 phone design-class samples, the cycle arithmetic and the memory changes, and found no discrepancies.

| # | Finding | Class | Disposition |
|---|---|---|---|
| 1 | Review #28 A1 satisfied: prefix search and full-table scan measured on desktop and phone; all 40 phone design-class samples under 50 ms (max 38.494 ms) | Required (met) | **A1 closed** |
| 2 | Review #28 A2 satisfied: full refresh cycle timed, VmHWM 31,100 → 37,140 kB | Required (met) | **A2 closed** |
| 3 | Figures in §3–§5 match the logs | Required (met) | No change |
| 4 | The row-order cause of 64,184 vs 66,996 bytes is UNKNOWN to this review (the capture and the earlier check were not in the package); keep the attribution and retain the supporting review | Direction | Applied: attribution unchanged (§3, §8); the supporting check is kept in `archive/REVIEW_S2_BATTERY_r1_2026-11-18.md`, "Aggregate size" |
| 5 | Keep custody and battery statements attributed to their evidence records | Direction | Already so (§1, §6 cite `S2_RERUN_EVIDENCE_2026-11-18.md`); no change |
| 6 | Make the single-house, 5,015-row scope explicit for later design decisions | Direction | Applied: new first limit in §6 and a scope line at the head of §9 |

**Reviewer's answers.** Q1: yes, §3–§5 satisfy A1 and A2. Q2: no repeat phone run is needed for this bounded approval; the results stay scoped to that phone, that run and those conditions. Q3: the before-and-after VmHWM satisfies design §3; the missing per-step split is a limit, not a failure. Q4: the failed runs are handled appropriately.

**Reviewer's scope.** Certifies: the re-run addresses both of #28's missing measurements; the batteries cover all four design A4 classes; the phone refresh-cycle timing and memory satisfy design §3; §3–§5 agree with the logs; the claim levels and main limits are appropriately restrained. Does not certify: the shipping app, mid-range devices, several houses or larger databases, name search, internet or cellular download, row-by-row equality of the refreshed database, independent execution or source verification of the harness, or Part A's original review (#6). Carry-forward note: S2 has measured evidence for both missing requirements; it has not shown that other devices or the eventual app will perform the same.
