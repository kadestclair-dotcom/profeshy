# Guardrail — Feature Research & Comparative Audit (DRAFT v0.2)

**Status: DRAFT FOR PEER REVIEW — NOT ADOPTED.**
**Prepared:** 2026-11-14, by Muse, from the v0.1 proposal
(FEATURE_RESEARCH_GUARDRAIL_PROPOSAL_2026-11-14.md) with the
amendments of peer review #11 (A1–A5) incorporated.
**Review state:** Muse review complete (Approved with amendments,
all incorporated here). Gemini review complete 2026-11-15:
APPROVED, no revisions required (peer review log #12), with two
non-blocking observations — waiver follow-ups to be logged in the
project tracker, and standalone status confirmed correct. A
second independent review of this version remains outstanding:
the owner relayed two messages (labelled ChatGPT and Gemini)
carrying identical text self-attributed to Gemini, so only one
independent approval is established by the evidence. Adoption, if
it happens, is by an owner decision record in the charter — this
draft authorizes nothing and changes no existing rule.
**Form when adopted:** a standalone gate-and-template document,
kept separate from the Master Guardrails and Rules, which it
cross-references rather than restates (A5).

## 1. Core principle

Before working on a feature or significant part of Profeshy, we
must understand:

1. **What is working in the world today?** Identify established
   products, implementations, technical approaches, and industry
   practices addressing the same or closely related problem.
2. **How does it work?** Research the underlying architecture,
   workflows, technical mechanisms, dependencies, strengths,
   limitations, and known failure modes.
3. **How does our work compare?** Audit our proposed or existing
   implementation against those findings.
4. **What should we do differently?** Identify where we match
   established practice, where we improve upon it, where we
   deliberately diverge, and where we have gaps.

The objective is not to copy competitors or follow trends blindly.
It is to make informed engineering decisions based on current
evidence.

**Operating question:** *"What is working today, how does it
actually work, and how does our implementation compare?"*

## 2. Scope — when this guardrail applies (A1)

The research gate applies to a workstream when any of these hold:

- **Class A:** a user-facing feature entering a stage for the
  first time.
- **Class B:** adoption of any third-party dependency, service,
  library, or data source.
- **Class C:** a material change to the subject matter of an
  accepted contract.

It does **not** apply to: spike harnesses themselves (they are the
research instrument and are already design-reviewed); bug fixes
that restore accepted behaviour; or work inside a workstream
already marked READY under this guardrail.

Proportionality: research depth scales with the feature's
complexity, risk, and importance. For anything outside classes
A–C that still seems to warrant a look, a one-page short-form
audit is the maximum required — never a full report by default.

## 3. Required research

For each applicable workstream, a proportionate research exercise
covering:

- **Current landscape:** relevant established solutions and
  credible implementations.
- **How they work:** actual mechanisms, architecture, lifecycle,
  interfaces, and operating assumptions — not merely advertised
  capabilities.
- **Strengths and weaknesses:** what works, what does not,
  trade-offs, edge cases, known limitations and failure modes.
- **Current practice:** whether the approach remains relevant and
  actively used, rather than relying on outdated examples.
- **Enterprise expectations, where relevant:** reliability,
  security, maintainability, observability, scalability,
  compatibility, performance, failure recovery.

## 4. Evidence standards (A2)

The audit is written in the project's existing evidence language —
the claims ladder and provenance rules of the Master Guardrails
(§2) and the charter — not in a parallel taxonomy.

- Every **material claim** in the audit carries a claims-ladder
  level (Hypothesis / Tested / Verified, as operationally defined
  in the master file §2).
- Every material claim cites **at least one primary source**
  (technical documentation, source code, specification,
  engineering publication, or reproducible demonstration).
  Secondary sources may support, never substitute.
- Sources are recorded with access dates, so findings can be
  revisited.
- Claims are separated from demonstrated behaviour: what a product
  *claims* is recorded as a claim. Where a comparable product
  exists and the feature is user-facing, the audit includes a
  **demonstrated-behaviour check** (hands-on use or equivalent
  direct evidence) — or an explicit **recorded waiver** stating
  why the check was not done and what remains unverified as a
  result. (Precedent: the S8 audit, listings-based, whose hands-on
  pass remains owed — this rule exists so that gap is a recorded
  waiver with a due point, not a silent one.)
- Conflicting evidence and unresolved questions are recorded as
  they are, never resolved by preferring the source that supports
  the favoured design.

## 5. Comparative audit

Research is compared explicitly against Profeshy's own work:

| Area | Required assessment |
|---|---|
| Industry approach | What established solutions currently do |
| Mechanism | How those solutions achieve it |
| Profeshy approach | What we have implemented or propose |
| Comparison | Where we match, differ, improve, or fall short |
| Risks | Technical, security, reliability, compatibility, and maintenance risks |
| Gaps | What remains unsupported, untested, or unresolved |
| Recommendation | Adopt, adapt, improve, defer, or reject — with reasons |

Do not claim superiority without evidence. Do not treat a
feature's presence as proof of equivalent quality or behaviour.

## 6. The deliverable (report template)

Before implementation begins, the workstream has a concise
**Feature Research & Comparative Audit** containing:

1. Feature scope, scope class (A/B/C), and intended outcome.
2. Current industry landscape and relevant examples.
3. How the strongest relevant implementations work.
4. Evidence: sources, access dates, claims-ladder levels, and
   any recorded waivers.
5. The comparative audit (§5 table).
6. Gaps, risks, trade-offs, and opportunities.
7. Recommended approach and rationale.
8. Open questions and required verification.
9. Research date, reviewed-as-of date, reviewer identity, and
   review outcome.

The report is part of the workstream's supporting evidence and
handover material, filed with the project's other documents.

## 7. The research gate (A3)

- The audit is peer-reviewed, and the gate outcome is an **owner
  decision** — as with every project gate (master file §2.4,
  §10.5). Reviewers advise; the owner decides.
- Outcomes: **READY** (research sufficient, approach justified,
  known risks addressed or accepted) / **READY WITH CONDITIONS**
  (work proceeds within explicitly documented boundaries) /
  **BLOCKED** (material research, evidence, design, or risk
  questions unresolved).
- The research gate **nests inside the existing stage gates** as a
  precondition for a workstream's implementation. It is not a
  parallel gate system and confers no authorization beyond the
  workstream it names.
- A BLOCKED outcome cannot be bypassed by beginning the work under
  a different task name (master file §7 stopping protocol applies).
- **Exceptions:** emergency fixes and explicitly authorized
  investigative spikes may use the short-form audit, but the
  exception is itself recorded — scope, reason, authorizer, date.
  An exception does not authorize full implementation.

## 8. Reassessment (A4)

- Every audit carries a **reviewed-as-of date**.
- An audit is re-validated at **the next stage gate that depends
  on it**, or when a named trigger fires: a dependency's terms
  change, a competitor ships the feature, an underlying API or
  platform changes materially, or Profeshy's own architecture
  changes in a way the audit speaks to.
- Reassessment confirms or amends the audit. It does not reopen
  the decision the audit supported without new evidence (master
  file §7 regression protection; §9 authority hierarchy).

## 9. Relationship to existing governance (A5)

This guardrail operationalizes rules that already exist; it does
not restate or replace them:

- Claims ladder, provenance, gates, peer review: charter and
  master file §2.
- No silent supersession; authority hierarchy: master file §9.
- Regression protection; blocked-work stopping protocol: master
  file §7.
- AI operating protocol (no invented state, no implied
  authorization): master file §10.
- An accepted contract or decision is never changed merely
  because an alternative exists; changes follow the decision
  process above.

Where this guardrail and the master file appear to conflict, the
master file and the charter's decision records govern, per the
authority hierarchy.
