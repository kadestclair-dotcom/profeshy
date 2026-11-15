# Profeshy — Spike S2 Design (AH payload reality)

**Status: APPROVED FOR EXECUTION (peer review, 2026-11-14). Execution order: S2 Part A first, then S4. Authorizes the spike harnesses only — no feature code.**
**Date:** 2026-11-14
**Depends on:** S1b results (2026-11-14) — house 113 US: 13.0 MB raw,
131,205 auctions, 5,015 distinct items; commodities 8.7 MB / 101,505.
**Charter:** §8 S2, §6 (backend reality), §9 G1.

## 0. The question

S1b killed raw client-side fetching. S2 must now prove the positive
claim: that the normalized form of a full house is small and fast
enough — to store, search, and refresh — on a mid-range Android phone,
and state exactly what the backend must serve so the phone never sees
the 13 MB firehose.

## 1. Step 0 — capture one real payload (owner's machine)

The S1b harness deliberately retained no payloads. S2 needs exactly
one: a fetch-only mini-run (same local channel, owner's key) that
saves house 113's auctions JSON and the commodities JSON to local
files. Auction data contains no credentials; the files may be shared
with the benchmark. Nothing else is fetched.

## 2. Part A — normalization benchmark (runs anywhere)

On the captured payload, a throwaway script measures:

- **A1 Parse cost:** time and peak memory to parse the 13 MB JSON.
- **A2 Aggregate build:** collapse 131,205 auctions → per-item stats
  (distinct items ~5,015: min/mean buyout, total quantity, listing
  count). Record build time and the aggregate table's size as JSON
  and as an SQLite database.
- **A3 Compression:** gzip size of the raw payload vs the aggregate
  payload — the number that sizes a backend→phone sync.
- **A4 Query battery** against the SQLite store: item lookup by ID,
  prefix search over item IDs, top-50 cheapest by unit price,
  full-table scan timing. Record ms per query class.
- **A5 Commodities:** same treatment for the 8.7 MB commodities
  payload (unit_price aggregation is simpler — record the difference).

Pass criteria (proposed, for review): aggregate snapshot ≤ 1 MB
compressed; every A4 query class ≤ 50 ms on desktop SQLite (a floor
estimate — Part B re-measures on Android); parse+build of one house
≤ 10 s server-side.

## 3. Part B — Android reality check (device or emulator)

The A2 SQLite database and A4 query battery are re-run on Android:
the owner's phone if available, else an Android Studio emulator on
the owner's PC with a mid-range device profile (4 GB RAM class).
Record: database open time, per-query latency, and the memory high
-water mark during a full refresh cycle (download aggregate → replace
→ query). If no device/emulator is available, Part B is recorded as
NOT TESTED and S2 closes as PARTIAL — desktop numbers are never
presented as phone numbers.

## 4. What S2 hands forward

- **To D4/G2:** the measured sync size and refresh cost — the backend's
  serving contract in numbers.
- **To S3:** the per-snapshot storage cost after normalization (raw
  13 MB vs aggregate), which sizes history retention.
- **To Stage 1 design:** the query battery becomes the acceptance
  test for "search feels instant" — or the evidence that it doesn't.

## 5. Not in S2

No UI, no app code, no backend build, no cadence claims (T9's
fingerprint comparison remains the open cadence datapoint, consumed
by S3).
