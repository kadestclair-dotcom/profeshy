# Peer Review — Proposed Guardrail: Feature Research & Comparative Audit

**Reviewer:** Muse (assistant of record on this project)
**Date:** 2026-11-14
**Subject:** FEATURE_RESEARCH_GUARDRAIL_PROPOSAL_2026-11-14.md
**Verdict: APPROVED WITH AMENDMENTS** — the proposal's philosophy
and structure are sound and it generalizes a pattern this project
has already proven (S8). Adoption should be conditional on the
five amendments below, which answer the owner's four scrutiny
questions and one alignment issue. None of them reopens the
proposal's core principle.

## What is strong (preserve)

- It codifies the one research exercise the project has actually
  run — S8 — including S8's hardest lesson: a listing's claim is
  not a demonstrated behaviour. §4's claims-vs-demonstrated rule is
  the right centre of gravity.
- The gate outcomes (READY / READY WITH CONDITIONS / BLOCKED) and
  the anti-bypass rule in §6 match the project's existing gate
  discipline and the master file's stopping protocol.
- §4's rule that an accepted contract or decision is not changed
  "merely because an alternative exists" is consistent with the
  authority hierarchy and regression protection in the adopted
  master file (§7, §9).
- The deliverable (§5) is a report template in embryo, which is
  the form the owner directed: a reusable gate and report
  template, not another paragraph of principles.

## The owner's four questions

### 1. Scope — NOT YET DEFINED (Amendment A1)

The proposal applies to "every feature, subsystem, or significant
project component" and to "significant implementation", but never
says what qualifies. On a solo project with an existing spike
process, an undefined threshold cuts both ways: everything gets a
report (overhead death) or nothing does (the guardrail is decorative).

**A1 — adopt explicit scope classes.** The guardrail applies to:
(a) a user-facing feature entering a stage for the first time;
(b) adoption of any third-party dependency, service, or data
source; (c) a material change to the subject matter of an accepted
contract. It does NOT apply to: spike harnesses themselves (they
are the research instrument, already design-reviewed), bug fixes,
or work inside a workstream already marked READY. Proportionality
gets a floor as well as a ceiling: below class (a)–(c), a one-page
short-form audit is the maximum required.

### 2. Research quality — GOOD STANDARDS, NO VERIFICATION STEP (Amendment A2)

§2 and §4 list the right preferences (primary sources, mechanisms
over marketing), but nothing checks that a given audit actually
met them. S8 is the cautionary example: it was honestly labelled
listings-based, yet its hands-on verification is still owed —
self-assessment alone did not close the gap.

**A2 — bind the audit to the claims ladder.** The proposal invents
a parallel taxonomy ("documented facts, observed behaviour,
informed interpretation, assumptions") instead of using the
project's existing ladder. Require instead: every material claim
in an audit carries a ladder level (Hypothesis / Tested /
Verified, per the master file §2 definitions), at least one
primary source per material claim, and — where a comparable
product exists and the feature is user-facing — a demonstrated-
behaviour check (hands-on or equivalent) or an explicit recorded
waiver. This also fixes the taxonomy duplication.

### 3. Gate authority — UNSPECIFIED, AND IT MATTERS (Amendment A3)

§6 says the audit "must be reviewed" but not by whom, and READY
is left floating. Under the adopted governance, a gate closes only
on recorded evidence plus the owner's explicit decision (master
file §2.4, §10.5). A research gate that an assistant or a reviewer
could mark READY unilaterally would quietly create a second
authority beside the owner's gates.

**A3 — READY is an owner decision.** Peer review advises, as with
every gate; the owner marks READY / READY WITH CONDITIONS /
BLOCKED. The research gate nests inside the existing stage gates
as a precondition for a workstream's implementation — it is not a
parallel gate system. The §6 exception for emergency fixes and
investigative spikes must produce a short-form audit record
(scope, exception, authorizer) rather than no record, or the
exception will swallow the rule.

### 4. Maintenance — RIGHT TRIGGERS, NO RHYTHM (Amendment A4)

§7 lists sensible reassessment triggers but assigns no owner and
no moment. "Revisit when material changes occur" fails silently:
nobody's job is to notice.

**A4 — tie reassessment to the gate rhythm.** Each audit carries
a reviewed-as-of date. An audit is re-validated at the next stage
gate that depends on it, or when a named trigger fires (dependency
terms change — the register's T1 pattern — a competitor ships the
feature, an underlying API changes). Reassessment confirms or
amends the audit; per §7's own last line, it does not reopen the
underlying decision without new evidence. This mirrors how the
compliance register is already maintained.

## One further amendment

### A5 — Cross-reference, don't restate

Parts of §4 restate rules that already live in the charter and the
master file (provenance, decision discipline). The review of the
master file established one responsibility per document. In its
adopted form, the guardrail should cite the charter/master
sections it operationalizes and keep only what is new: the scope
classes, the audit template, and the research gate. A restated
rule is a rule that can drift into two versions.

## Suggested first application

Pilot the template on S5 (profit formula validation) when G2 work
begins: it is research-shaped, has real comparators (Ragetusk's
published margin formula and its stated exclusions), and would
prove the template before Stage 2 features depend on it.

## Limits of this review

I reviewed the proposal text against the charter, the adopted
master file, and the project's recorded practice. I did not test
the guardrail against a live workstream — that is what the S5
pilot is for. Adoption remains the owner's decision, recorded as
its own decision record, after the second reviewer's verdict.
