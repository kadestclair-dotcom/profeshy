# Proposed Rule — Three-Way Review of Governance Changes (DRAFT v0.4)

**Status: PROPOSED — NOT ADOPTED.** Until adoption, the existing
guardrails remain authoritative and this draft changes nothing.
**Version history:** v0.1 (2026-11-15, repo commit 927ae7b) —
reviewed by Gemini (peer review #17: Approved) and ChatGPT
(peer review #18: Approved with amendments). v0.2 (commit
c7f3585) incorporated ChatGPT's amendments but was superseded
before submission when peer review #19 (DeepSeek, reviewing
ChatGPT's review) required further reconciliation first. v0.3
(commit 65d072e) incorporated #18 and #19 and was reviewed by
ChatGPT (peer review #20: Approved with amendments A1–A4).
**This v0.4 incorporates #20 in full and is the cycle-2 —
final — reconciliation (§6):** if amendment requests remain
after v0.4, they do not produce a v0.5; the owner decides on
v0.4 as it stands, over recorded dissent or by withdrawal.
Approvals attach to an exact version and commit hash: no
earlier approval (Gemini on v0.1, ChatGPT on v0.1 or v0.3)
carries to v0.4, and three-way approval of v0.4 requires a
recorded APPROVED from all three reviewers on v0.4 itself (§6,
as amended by #20 A1). v0.4 goes to all three reviewers.
**Prepared:** 2026-11-15, by Muse, from the owner's proposed
rule and peer reviews #16 (DeepSeek), #18 (ChatGPT), #19
(DeepSeek), and #20 (ChatGPT).
Two points in v0.1 were drafted as reviewer recommendations
flagged for the owner's confirmation: the independence mechanism
(§4, DeepSeek's Option 3) and the roster formulation (§3,
owner-designated). Both were endorsed by Gemini and ChatGPT in
their v0.1 reviews.
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
governance change under this rule. **Every roster change records
its reason and its effective date, and states whether it follows
a pending or recent dissent by the outgoing reviewer** — a
required disclosure, not an optional one, so the roster can never
be quietly swapped because a reviewer disagreed with a proposal.

## 4. Independence and authorship

- A review is **independent** if the reviewer did not author,
  co-author, or previously endorse the specific version under
  review, and reviews the submitted text itself rather than a
  summary of it.
- Reviewers return their **initial verdicts before seeing one
  another's findings**, so three reviews cannot quietly converge
  into one (master file §10.8: repetition or assistant consensus
  settles nothing). Historical review records and amendment
  provenance may be supplied as context; what is withheld until
  a reviewer's own initial verdict is submitted is any other
  reviewer's **current-round** verdict or findings on the
  version under review.
- **Author or reviewer, never both:** an assistant may author a
  governance proposal or review it, never both. Authorship is
  recorded. If the author is one of the three designated
  reviewers, the mechanism is **substitution**: the owner
  designates a substitute independent reviewer for that cycle,
  recorded in the log — so a proposal is never left structurally
  unapprovable by its own safeguard. Because substitution still
  yields three independent verdicts, §6's outcomes apply to
  author-involving proposals unchanged.
- **Owner-authored proposals** are reviewed by all three
  assistants normally. The owner is the adopting authority, not a
  reviewer, so the owner's authorship does not taint the review.

## 5. Submission and version identification

One clearly identified version is submitted to all three
reviewers, identified by **repo commit hash and version label**.
Every verdict must cite the same hash; a verdict that does not
cite it is not counted.

## 6. Verdicts, outcomes, and reconciliation

**Verdict vocabulary.** Recorded verdicts use exactly these terms:
**APPROVED**, **APPROVED WITH AMENDMENTS**, **REJECTED**,
**NON-PARTICIPATING**. No other wording is entered in the log for
a reviewer's position. **APPROVED WITH AMENDMENTS is conditional
on the amendments it names and approves only the version
reviewed** — it is never approval of a later version. Proposal
statuses (not verdicts) are recorded as **WITHDRAWN** or
**SUPERSEDED** where they apply.

**Outcomes, kept distinct — and a default bar.** Three reviewers
examining a proposal does not necessarily mean three reviewers
approved it, and the record must always say which happened:

1. **Three-way review completed** — all three reviewers submitted
   independent verdicts on the identified version.
2. **Three-way approval achieved** — all three reviewers
   recorded **APPROVED** on the **same final version and commit
   hash**. No approval carries forward from an earlier version:
   **every reviewer must assess the final version**, regardless
   of their previous verdict and regardless of whether they
   requested amendments. No version may be counted as approved
   by a reviewer who never inspected it.
3. **Owner adoption as an exception**, in two distinct forms:
   **3a — adoption over recorded dissent** (a reviewer engaged
   and said no), and **3b — adoption with non-participation**
   (a reviewer did not engage; absence of evidence, not evidence
   against). The record states which form occurred, and how many
   of each.

**Three-way approval achieved (outcome 2) is the standard bar
for adoption.** Outcomes 3a and 3b are permitted but are
**exceptions, never the default path**: each requires the
dissenting verdicts preserved verbatim (3a) or the
non-participation facts recorded (3b), plus the owner's stated
reason for proceeding. Neither form is ever represented as
three-way approval, in the log or anywhere else.

**Reconciliation.** All findings are reconciled and incorporated
into a new version where necessary. **Any reviewer who requests
amendments must review the reconciled version before their
approval counts** — and, per outcome 2 above, so must every
other reviewer, on the final version. Reconciliation runs in at
most **two cycles, counted per proposal** — not per version and
not per reviewer; producing another version never resets the
count:

- **Cycle 1:** findings are incorporated and reconciled into a
  new version.
- **Cycle 2:** final reconciliation and reviewer reassessment of
  that version.
- **After cycle 2:** any unresolved disagreement is recorded —
  **dissenting verdicts are preserved verbatim**; faithful
  summary is permitted only for non-dissent material — and the
  owner either adopts over the recorded dissent or withdraws
  the proposal. No reviewer holds a veto by attrition.

**Non-participation.** The owner states a **reasonable review
window** explicitly for each submission, and extends it where a
reviewer needs additional time. A reviewer who returns no verdict
on the identified version within the window (as extended) is
recorded as **NON-PARTICIPATING**. Non-participation does not
block adoption, but the fact and the window are recorded, the
owner's adoption decision accounts for the reduced review, and
**non-participation is never converted into, or recorded as,
approval**.

## 7. Adoption

- The three-way review is a **precondition to the owner's adoption
  decision, not a separate gate** and not an adoption in itself.
- The **owner makes the final adoption decision** and records it
  in the charter or the appropriate decision record, naming which
  of §6's outcomes the review reached — and, for outcome 3a or
  3b, the stated reason for proceeding.
- **Until adoption, the existing guardrails remain authoritative.**

## 8. Recording

For each governance change, PEER_REVIEW_LOG.md records: the
proposal's title and author; the submitted version label and
commit hash for every version reviewed; each reviewer's verdict
(in the §6 vocabulary) **together with its stated scope** — a
verdict is scoped to the text as a governance specification and
does not certify the underlying measurements, decisions, or any
prior application of the rule; the amendments requested and
their disposition; any dissent (verbatim); any non-participation
and its window; which of §6's outcomes the review reached (1, 2,
3a, or 3b, with counts); for 3a/3b, the owner's stated reason for
proceeding; the owner's adoption decision and its date; and
where the adopted text is seated.

## 9. Relationship to the existing guardrails

This rule operationalizes master file §2.5 (peer review) for the
governance layer and strengthens §10.5 (no implied authorization)
there. It sits under the §9 authority hierarchy like everything
else: decision records outrank it, and it never outranks a
measurement.

## 10. Bootstrap — how this rule itself is adopted

This rule's own adoption is an **explicit exception to the normal
sequence**, recorded as such — it is not presented as though the
rule had already satisfied its own review requirement before it
existed. The stages, each recorded in the log:

1. **Initial owner-authorized bootstrap adoption:** the owner
   adopts this rule on the record, under the governance in force
   before it exists.
2. **Three-way review and ratification:** the rule's first
   application is the three-way review, under its own terms, of
   the exact bootstrap-adopted version.
3. **Amendment and final owner adoption:** any amendment arising
   from that review is reconciled under §6 and adopted by the
   owner's final decision.

**The bootstrap interval is bounded.** Between stage 1 and stage
3, the bootstrap version operates **only** to govern its own
three-way review and reconciliation; the existing Master
Guardrails remain authoritative for all other governance
matters, and the bootstrap version acquires no unrestricted
authority until the owner's final adoption at stage 3.

From final adoption onward, the rule governs all further changes
to itself under its own terms — so no future assistant can argue
it was never validly adopted, and none can quietly weaken,
reinterpret, or remove it.

One pre-adoption housekeeping item rides with the adoption
package (review #16, finding B7): PEER_REVIEW_LOG.md is given its
canonical home in the master file §1, since this rule makes the
log load-bearing.
