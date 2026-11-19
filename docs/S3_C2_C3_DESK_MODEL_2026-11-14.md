# Profeshy — S3 desk model (C2 sizing + C3 AHledger comparison)

**Status: DESK WORK — arithmetic on S2's measured numbers, pending the
C1 cadence probe. Every figure scales linearly with the true cadence;
the tables assume hourly snapshots and say so.**
**Date:** 2026-11-14. Inputs (all measured in S2 Part A): per-house
aggregate 533,193 bytes JSON / 66,996 bytes gzipped; 5,015 items per
house; raw fetch 13.0 MB; server processing ~3 s per house-snapshot.

## C2 — Self-collection sizing (assumes hourly cadence)

| Store shape | Per house | 15 houses (one ruleset) |
|---|---|---|
| Raw aggregates kept (533 KB/snapshot) | 12.8 MB/day → **384 MB/mo** | 192 MB/day → **5.8 GB/mo** |
| Time-series rows (~40 B/item/snapshot, 5,015 items) | ~4.8 MB/day → **145 MB/mo** | **~2.2 GB/mo** |
| Rollup policy R1: hourly kept 7 days, then daily per-item summaries | ~34 MB working set + **~6 MB/mo growth** | ~0.5 GB working set + **~90 MB/mo growth** |

Fetch/processing load at hourly cadence, 15 houses: 4.7 GB/day
inbound to the backend, ~18 CPU-minutes/day of processing. Neither
is a cost driver. The real D4 driver is the **always-on requirement**
(a collector that must not miss snapshots), not size or compute — a
small always-on VPS class machine suffices on these numbers (hosting
price itself is a D4 market check, not measured here).

Product consequence, restated for the Contract: with rollup R1,
Stage 2's 24h/7d views are exact and 30d/90d views are daily-grain —
which matches what the charter promises. Anything finer than hourly
does not exist, because the source snapshot is the grain.

## C3 — The AHledger rental, on the same coverage

From the D1 brief's recorded terms (2026-11-14; terms tighten
2026-12-10): AHledger already holds history (24h per-scan, 7d hourly,
30d/90d daily). But below the **Data tier ($499/mo)** the licence
forbids keeping answers past cache time — no archiving, no datasets,
no models. So the rental buys *display* of recent history, never an
owned asset: stop paying, or stay on a lower tier, and Profeshy's
history — including anything Stage 5 forecasting would train on —
is gone or was never collectible. Self-collection at the C2 numbers
costs roughly two orders of magnitude less and owns the data from
day one, at the price of starting empty.

**Desk recommendation (for the S3 report, pending C1):** self-collect
from the earliest moment the Contract allows; treat AHledger history
as a display-only supplement at most, never as the system of record.
This aligns with Decision #001 (Route C is a fallback, not a source).

---

**2026-11-18 (S3 results, review #26):** C2's 4.7 GB/day transfer estimate is superseded by the measured 4.5 GB/day (`S3_RESULTS_2026-11-18.md` §3). C3's statement that self-collection costs roughly two orders of magnitude less than AHledger has no measured hosting price behind it, and is unverified until D4 prices hosting. The text above is otherwise unchanged.
