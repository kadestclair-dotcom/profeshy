# Guardrail — Feature Research & Comparative Audit (v0.3, ADOPTED)

**Status: ADOPTED 2026-11-15 — Decision Record #003 in the
charter.** (Filename retains its draft-era name so existing links
do not break.)
**Prepared:** 2026-11-15, by Muse, reconciling the v0.2 reviews:
peer review #12 (Gemini — Approved) and #13 (ChatGPT — Approved
with amendments). ChatGPT's four required amendments, its
non-blocking template suggestion, and Gemini's waiver-tracking
observation are incorporated and marked (C-A1…C-A4, C-minor, G-obs).
**Review state:** complete. Gemini's second review of this exact
version: APPROVED, no further revisions (peer review log #14).
**Form:** a standalone gate-and-template document, kept separate
from the Master Guardrails and Rules, which it cross-references
rather than restates.

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

## 2. Scope — when this guardrail applies

The research gate applies to a workstream when any of these hold:

- **Class A:** a user-facing feature entering a stage for the
  first time.
- **Class B:** adoption of any third-party dependency, service,
  library, or data source.
- **Class C:** a material change to the subject matter of an
  accepted contract.
- **Class D (C-A1):** a significant internal subsystem or
  foundational architectural component, whether or not it is
  user-facing or third-party. Foundational internal architecture
  can carry as much technical risk as user-facing functionality;
  it does not escape the research standard by being invisible to
  users. Depth is set by complexity and risk, as elsewhere in
  this guardrail.

**Exemptions, stated precisely (C-A3):**

- A spike **harness** — the throwaway measurement instrument
  itself — is exempt; it is already design-reviewed under the
  spike process.
- An **investigative spike into a feature or technology** (a spike
  whose subject is a candidate feature, subsystem, or dependency)
  is not exempt: it uses the short-form exception in §7.
- **Ordinary restoration of accepted behaviour** (a bug fix that
  returns the system to what was accepted) is exempt.
- The **emergency exception** in §7 applies only where the fix
  itself needs research beyond restoration.
- Work inside a workstream already marked READY under this
  guardrail needs no second gate.

Proportionality: research depth scales with the workstream's
complexity, risk, and importance. For anything outside classes
A–D that still seems to warrant a look, a one-page short-form
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

## 4. Evidence standards

The audit is written in the project's existing evidence language —
the claims ladder and provenance rules of the Master Guardrails
(§2) and the charter — not in a parallel taxonomy.

- Every **material claim** in the audit carries a claims-ladder
  level (Hypothesis / Tested / Verified, as operationally defined
  in the master file §2).
- **The ladder classifies the evidence behind an assertion, not
  the assertion's wording (C-A2).** For external products this
  distinction is explicit: a vendor *claims* a capability;
  documentation *describes* how it should work; we *directly
  observe* it working; an independent source *verifies* a
  behaviour. These are not interchangeable. A vendor's
  documentation alone establishes only that a behaviour is
  claimed and described — it cannot establish that the behaviour
  has been demonstrated, and it caps the claim below Tested until
  direct observation or an independent demonstrated source exists.
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
  result. **Every waiver's follow-up verification is logged as a
  row in the project tracker (G-obs)** so it cannot languish
  untracked. (Precedent: the S8 audit, listings-based, whose
  hands-on pass remains owed — this rule exists so that gap is a
  recorded waiver with a due point, not a silent one.)
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

1. Feature scope, scope class (A/B/C/D), and intended outcome.
2. Current industry landscape and relevant examples.
3. How the strongest relevant implementations work.
4. Evidence: sources, access dates, claims-ladder levels, and
   any recorded waivers (with their tracker rows).
5. The comparative audit (§5 table).
6. Gaps, risks, trade-offs, and opportunities.
7. Recommended approach and rationale.
8. Open questions and required verification.
9. Research date, reviewed-as-of date, reviewer identity, and
   review outcome — and, once decided, **the owner decision, its
   date, the workstream it applies to, and any conditions
   attached to a READY WITH CONDITIONS outcome (C-minor)**.

The report is part of the workstream's supporting evidence and
handover material, filed with the project's other documents.

## 7. The research gate

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
- **Exceptions:** emergency fixes and investigative spikes (as
  delimited in §2) may use the short-form audit, but the exception
  is itself recorded — scope, reason, authorizer, date. An
  exception does not authorize full implementation.

## 8. Reassessment

- Every audit carries a **reviewed-as-of date**.
- An audit is re-validated at **the next stage gate that depends
  on it**, or when a trigger event **materially affects an
  assumption, comparison, dependency, risk, or decision recorded
  in the audit (C-A4)** — for example a dependency's terms change,
  an underlying API or platform change, a change in Profeshy's own
  architecture, or a competitor release *that changes the
  comparison the audit relied on*. An event that touches none of
  the audit's recorded substance — a competitor shipping a feature
  our audit did not depend on — is not a trigger.
- Reassessment confirms or amends the audit. It does not reopen
  the decision the audit supported without new evidence (master
  file §7 regression protection; §9 authority hierarchy).

## 9. Relationship to existing governance

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

---

## Review record (2026-11-15)

- Gemini, second review of this exact version: **APPROVED**
  (peer review log #14). All reconciled items verified —
  C-A1 Class D, C-A3 exemptions, C-A2 evidence distinction,
  G-obs tracker-logged waivers, C-minor decision fields,
  C-A4 relevance-based triggers. No further revisions required.
- Review chain complete: Muse #11 (proposal), Gemini #12
  (v0.2 Approved), ChatGPT #13 (v0.2 Approved with amendments,
  reconciled here), Gemini #14 (this version, Approved).
- **Adopted 2026-11-15** by the owner's instruction, recorded as
  Decision Record #003 in the charter. This guardrail is now in
  force for workstreams in its scope classes.
