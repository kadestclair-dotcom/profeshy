# Proposed Rule — Three-Way Review of Governance Changes (DRAFT v0.1)

**Status: PROPOSED — NOT ADOPTED.** Until adoption, the existing
guardrails remain authoritative and this draft changes nothing.
**Prepared:** 2026-11-15, by Muse, from the owner's proposed rule
and peer review #16 (DeepSeek — Approved with amendments). Two of
the reviewer's points were reserved to the owner and are drafted
here as the reviewer recommended, flagged for the owner's
confirmation: the independence mechanism (§4, reviewer's Option 3)
and the roster formulation (§3, owner-designated). If the owner
chooses otherwise, this draft is revised before review.
**Intended home when adopted:** a new subsection of the Master
Guardrails and Rules, adopted by an owner decision record, under
the bootstrap sequence in §10.

## 1. Scope — guardrails only

This rule applies **only to governance changes**: the creation,
amendment, removal, or material reinterpretation of a rule in the
Master Guardrails and Rules, or in an adopted standalone guardrail
document (currently: the Feature Research & Comparative Audit
guardrail).

It does **not** apply to, and three-way approval is **not**
permission to change:

- project decisions (they keep the charter's decision-record
  process);
- technical contracts (they keep the gate process);
- measured findings and spike designs or results (they keep the
  evidence and peer-review processes they already have);
- roadmap, tracker, or status content.

Those retain their existing approval processes, untouched by this
rule.

## 2. Material reinterpretation, defined

A reinterpretation is **material** if it changes what an assistant
would do differently in practice, changes the scope of who or what
a rule applies to, or changes the evidentiary standard a rule
requires. A restatement that preserves operative effect is
editorial and does not trigger this rule. **Tie-break:** if any
reviewer or the owner asserts that a change is material, it is
material for the purposes of this rule. Default toward review,
not away from it.

## 3. The reviewers

The reviewers are **the three independent assistants designated
by the owner — currently ChatGPT, Gemini, and DeepSeek**. The
owner may update the roster by a decision recorded in
PEER_REVIEW_LOG.md; a roster update is an owner decision, not a
governance change under this rule.

## 4. Independence and authorship

- A review is **independent** if the reviewer did not author,
  co-author, or previously endorse the specific version under
  review, and reviews the submitted text itself rather than a
  summary of it.
- Reviewers return their **initial verdicts before seeing one
  another's findings**, so three reviews cannot quietly converge
  into one (master file §10.8: repetition or assistant consensus
  settles nothing).
- **Author or reviewer, never both:** an assistant may author a
  governance proposal or review it, never both. Authorship is
  recorded. If the author is one of the three designated
  reviewers, the owner designates a substitute independent
  reviewer for that cycle, recorded in the log — so a proposal is
  never left structurally unapprovable by its own safeguard.
- **Owner-authored proposals** are reviewed by all three
  assistants normally. The owner is the adopting authority, not a
  reviewer, so the owner's authorship does not taint the review.

## 5. Submission and version identification

One clearly identified version is submitted to all three
reviewers, identified by **repo commit hash and version label**.
Every verdict must cite the same hash; a verdict that does not
cite it is not counted.

## 6. Verdicts and reconciliation

- Each reviewer's verdict is recorded in its own class, never
  paraphrased into a stronger one (existing log rule).
- All findings are reconciled and incorporated into a new version
  where necessary. **Any reviewer who requests amendments must
  review the reconciled version before their approval counts.**
- Reconciliation is **bounded to two revision cycles** per
  proposal. If agreement is not reached after two cycles, the
  owner either adopts the proposal as-is over recorded dissent
  (the dissent preserved in the log) or withdraws it. No reviewer
  holds a veto by attrition.
- A reviewer who does not return a verdict on the identified
  version within the owner's stated window is recorded as
  **non-participating**. Non-participation does not block
  adoption, but the fact and the window are recorded, and the
  owner's adoption decision accounts for the reduced review.

## 7. Adoption

- The three-way review is a **precondition to the owner's adoption
  decision, not a separate gate** and not an adoption in itself.
- The **owner makes the final adoption decision** and records it
  in the charter or the appropriate decision record.
- **Until adoption, the existing guardrails remain authoritative.**

## 8. Recording

For each governance change, PEER_REVIEW_LOG.md records: the
proposal's title and author; the submitted version label and
commit hash for every version reviewed; each reviewer's verdict;
the amendments requested and their disposition; any dissent; any
non-participation and its window; the owner's adoption decision
and its date; and where the adopted text is seated.

## 9. Relationship to the existing guardrails

This rule operationalizes master file §2.5 (peer review) for the
governance layer and strengthens §10.5 (no implied authorization)
there. It sits under the §9 authority hierarchy like everything
else: decision records outrank it, and it never outranks a
measurement.

## 10. Bootstrap — how this rule itself is adopted

This rule is adopted by the owner on the record, under the
governance in force before it exists. Its **first application is
the three-way review of its own text** under its own terms. Any
amendment arising from that first review is adopted by the owner
and recorded the same way. From that adoption onward, the rule
governs all further changes to itself under its own terms — so no
future assistant can argue it was never validly adopted, and none
can quietly weaken, reinterpret, or remove it.

One pre-adoption housekeeping item rides with the adoption
package (review #16, finding B7): PEER_REVIEW_LOG.md is given its
canonical home in the master file §1, since this rule makes the
log load-bearing.
