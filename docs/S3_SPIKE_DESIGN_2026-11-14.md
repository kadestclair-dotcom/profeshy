# Profeshy — Spike S3 Design (History sources)

**Status: DRAFT FOR OWNER APPROVAL / PEER REVIEW. Authorizes no code, no collection service, no spending.**
**Date:** 2026-11-14
**Depends on:** S1b (API gives snapshots, not history; cadence formally
unmeasured), S2 Part A (per-house aggregate = 533 KB JSON / 180 KB
SQLite per snapshot; incidental datapoint: captures 14 minutes apart
were byte-identical).
**Charter:** §8 S3, §5 Stage 2/5, §6, §9 G1.

## 0. The problem, stated plainly

Route B has **no history and no backfill**. The Forever API returns
the current snapshot only. Every price-trend, alert-baseline, and
forecast feature in the charter therefore depends on Profeshy
collecting its own snapshots from the day collection starts — or on
renting AHledger's history under its licence (Route C terms: history
beyond 7–30 days and any archiving require the Data tier, from
$499/mo — recorded in the D1 brief). S3 must turn that into numbers
and a collection design the Contract can adopt or reject.

## 1. Questions

- **H1 — Cadence:** how often does a house snapshot actually change?
  (Two datapoints exist: S1b's skipped T9 baseline fingerprint, and
  S2's byte-identical captures 14 minutes apart. Neither bounds the
  interval.)
- **H2 — Cost of self-collection:** at the measured cadence, what do
  fetch volume, processing, and storage cost per house per month —
  and for all 15 houses of one ruleset?
- **H3 — Retention shape:** what rollup policy (raw snapshots →
  hourly aggregates → daily aggregates) keeps Stage 2 (tracking,
  24h/7d/30d views) and Stage 5 (forecasting) honest without
  unbounded growth?
- **H4 — The AHledger alternative, priced honestly:** for the same
  coverage, what does renting history cost, what does the licence
  forbid (archiving, models), and what happens to Profeshy's history
  if the subscription lapses?

## 2. Method

- **C1 Cadence probe (owner's machine, owner's key):** a lightweight
  variant of the S1b harness fetches house 113's headers/fingerprint
  every 15 minutes for 24 hours (no payload retention — fingerprint
  and count only, ~96 tiny records). Output: the observed change
  times, from which the true snapshot interval is read directly.
  A 24h window that shows no change is also a result (cadence ≥ 24h
  would itself be a major finding).
- **C2 Sizing model (desk work, from S2's measured numbers):** fetch
  bytes, per-snapshot processing time, and storage under three
  retention policies, per house and ×15 houses, at the C1 cadence.
  No new measurements needed — arithmetic on S2 evidence, with every
  input cited.
- **C3 AHledger comparison (desk work):** the D1 brief's recorded
  terms applied to the same coverage table as C2, including the
  2026-12-10 terms change and the lapse scenario. No AHledger calls
  are needed; if the owner wants a live re-check of terms, it is a
  web read, not an integration.

## 3. Pass criteria and outputs

S3 passes when it states, from evidence: the measured snapshot
interval (H1); a per-month cost table for self-collection at 1 house
and 15 houses under a named retention policy (H2/H3); and a
side-by-side with the AHledger rental, including what each path
forbids (H4). The output is a **collection design recommendation for
the Contract**: collector schedule, storage shape, rollup policy,
and the honest product consequence — *history features can only
ever show data from the day collection starts; the UI must say so*
(§7 freshness/absence rules).

## 4. Not in S3

No collector is built or scheduled (that is a G2/D4 implementation
decision with a cost ceiling), no AHledger signup, no backfill
claims — none exist to claim.
