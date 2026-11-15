# Profeshy — Spike S4 Design (Recipe & profession data)

**Status: APPROVED FOR EXECUTION (peer review, 2026-11-14), after S2 Part A. Authorizes the spike harness only — no feature code, no paid tiers.**
**Date:** 2026-11-14
**Depends on:** S1b results — the measured **53% item-404 gap** in
`static-forever` is this spike's central problem.
**Charter:** §8 S4, §5 Stage 3/4 (profit engine, guides), §7.5
(absence shown as absence), §9 G1.

## 0. The questions

1. What profession, skill-line, and recipe data does `static-forever`
   actually expose — per profession: recipe counts, tiers, and the
   recipe → reagents structure?
2. Given the 53% item gap, what fraction of recipe reagents resolve
   to item data (name/icon) through Blizzard's static API — measured
   per profession, not assumed?
3. Before accepting the gap: do the 404ing items resolve through any
   *other* Blizzard route (item search, alternate endpoints)? The gap
   must survive an honest attempt to close it.
4. Does ForeverDB (the D1 brief's candidate) cover what Blizzard
   doesn't — items, recipes, reagents — and on what terms?

## 1. Method — harness #2, same custody channel as S1b

A throwaway script on the owner's machine, owner's key, same rules
(nothing secret logged; stop on 429):

- **P1 Profession index:** enumerate professions/skill lines in
  `static-forever-us`; record IDs, names, tier structure.
- **P2 Recipe enumeration:** for each profession, list recipes;
  record counts per profession and per tier; capture one full recipe
  record's shape (reagents, crafted item, quantities).
- **P3 Reagent resolution:** collect every distinct reagent item ID
  across all enumerated recipes; resolve each via `/data/wow/item/{id}`;
  report the resolution rate overall and per profession, with the
  unresolvable IDs kept as evidence.
- **P4 Alternate-route probe:** take 25 known-404 item IDs from S1b's
  T6 evidence (e.g. 1280, 4708, 6266) and try: the item-search
  endpoint, the media endpoint, and any namespace variant the API
  index advertises. Any route that resolves them is a finding that
  re-opens the T6 number.
- **P5 Icon/media check:** for 50 resolved items, fetch media;
  record icon availability rate (the UI's other silent failure mode).

## 2. ForeverDB evaluation (no key purchase; free/public surface only)

- **F1 Coverage sample:** the same 25 known-404 IDs + 25 recipe
  reagents from P3 — do they exist in ForeverDB, with names/icons?
- **F2 Recipe cross-check:** 10 recipes sampled from P2 — does
  ForeverDB's recipe/reagents data agree with Blizzard's?
- **F3 Terms record:** free tier limits (brief: 5,000 req/day,
  30/min, credit required; paid $5.99/mo), licence/credit wording,
  and whether bulk/export use is permitted — recorded verbatim from
  the site on the day, feeding D4 and Q6.

## 3. Pass criteria and outputs

S4 passes when it can state, from data: per-profession recipe counts;
the reagent resolution rate via Blizzard alone; whether any alternate
Blizzard route shrinks the gap; ForeverDB's measured coverage of the
gap sample and its terms. The output is a **source recommendation for
the Contract**: Blizzard-only, ForeverDB-primary for metadata, or
hybrid — each with its measured coverage, so Stage 3/4 planning
(profit engine, leveling guides) builds on numbers.

Failure is also a valid outcome: if neither source resolves reagents
adequately, Stage 3's scope (profit per recipe) is what gets cut —
recorded as a G1 finding, not worked around with invented data (§7).

## 4. Not in S4

No guide content, no profit calculations, no data import into any
store that survives the spike, no ForeverDB paid tier (that is a
spending decision for the owner, informed by F3).
