# Profeshy — Peer Review Log

**Purpose:** the central register of every peer review the project
has received. Each verdict is also stamped in the document it
concerns; this log classifies them in one place so nothing depends
on memory.

**Verdict classes:**

- **Passed** — review gate cleared (used for the charter at G0)
- **Approved as-is** — accepted with no changes requested
- **Approved with directions** — accepted, with instructions to act on
- **Approved for execution** — a design accepted to be run
- **Approved with amendments** — accepted in principle; the named
  revisions are required before the subject is treated as final

## Register

| # | Date | Subject | Verdict | Directions / notes | Recorded in |
|---|---|---|---|---|---|
| 1 | 2026-11-14 | Charter v0.1 | Passed | G0 prerequisites met once Decisions #001 and #002 were recorded; Gate G0 closed on the owner's "keep as is and proceed" | Charter decision record |
| 2 | 2026-11-14 | S2 and S4 spike designs | Approved for execution | Run order set: S2 Part A first, then S4 P1–P5 | S2/S4 designs and results |
| 3 | 2026-11-14 | S8 competitor audit | Approved as-is | Adopted into the charter (§1 market note, §8 status, §10 annotated "not triggered"). The reviewer's D1 implication — incumbents lean on AHledger scans, raising S1b's stakes — recorded as a risk note on S1b; Decision #001 unchanged | Charter S8 review record; S8 audit doc |
| 4 | 2026-11-14 | S1b results report | Approved | S1b closed out; directed: proceed to S2 and S4 | S1B_RESULTS_2026-11-14.md |
| 5 | 2026-11-14 | S4 results report | Approved | Confirmed the two-populations finding (AH item gap vs recipe graph), the media-endpoint finding, ForeverDB's narrowed role; Stage 3 safe to proceed | S4_RESULTS_2026-11-14.md |
| 6 | 2026-11-14 | S2 Part A results | Approved with directions | Schedule Part B to close S2 (done 2026-11-14, passed); proceed to S3 using the verified aggregate footprint | S2_RESULTS_2026-11-14.md |
| 7 | 2026-11-14 | Public repo and transparency layer (README + docs/ paper trail + key custody workflow) | Approved as-is | Verdict: repository "pristine"; correctly sequenced and secure. Reviewer's G1 statement at the time: only F1 and S2 Part B remained (Part B has since passed) | Project tracking and memory; this log |

| 8 | 2026-11-14 | Master Guardrails and Rules (handover document) | Approved with amendments | Seven amendments directed: (1) every measured finding carries its test boundaries, results docs authoritative over summaries; (2) Tested vs Verified operational definitions (materially different method; repetition is not verification); (3) S4/S8 status wording reconciled (part vs whole); (4) explicit authority hierarchy, measurements stay authoritative as evidence, no silent supersession; (5) no-invented-state extended to project status and synchronization claims; (6) synchronization rule (GitHub authoritative for documents, snapshots dated, conflicts recorded not silently resolved); (7) security section strengthened (no requesting secrets in chat, no env/config dumps, no tokens in logs, identifiers vs credentials distinguished). Plus: identifier-resolution rule, stopping protocol, regression protection, charter-status clarification, and a new §10 AI operating protocol. §10 added to the master file 2026-11-14 at the owner's forwarding; the seven amendments, improvements A–D, and the S4/S8 and charter-status reconciliations were incorporated into the master file the same day on the owner's instruction ("Incorporate all seven amendments now"). Remaining step per the reviewer: a final peer review of the amended document. Reviewer: do not reopen technical decisions or closed gates; final peer review after amendments | MASTER_GUARDRAILS_AND_RULES.md (§10); this log |

| 9 | 2026-11-14 | Master Guardrails and Rules, amended version — final review (ChatGPT) | Approved | All twelve concerns from review #8 verified resolved, no blocking objections. Two non-blocking observations: §1's "single handover document" is the single operational entry point, not a replacement for authoritative sources; §4's figures stand provided the results documents remain available with methodology and limitations. Scope: approves the document as a governance/handover specification only — does not certify measurements, repo state, Notion state, or technical decisions. Directions: no further changes before the second review; do not reopen G0 or expand the document; second reviewer to assess this exact amended version and have their verdict recorded in the document | MASTER_GUARDRAILS_AND_RULES.md (adoption record); this log |
| 10 | 2026-11-14 | Master Guardrails and Rules, amended version — second reviewer (Gemini) | Approved | Checklist verified: §2/§4 evidence boundaries and claims ladder; §5 secret custody absolute, local-only custody proof preserved; §7/§9 synchronization and authority hierarchy; §1/§8 scope and gate discipline (G0 closed, G1 progressing, G2 prerequisites C1 and A1 must clear before any contract freeze). Verdict: with this second approval the amended document is fully ratified as the single operational entry point and binding governance framework. Both approvals reconciled into the adoption record in the master file | MASTER_GUARDRAILS_AND_RULES.md (adoption record); this log |

| 11 | 2026-11-14 | Proposed guardrail — Feature Research & Comparative Audit (reviewer: Muse) | Approved with amendments | Answers the owner's four scrutiny questions as amendments: A1 scope classes (applies to first-in-stage user-facing features, third-party adoptions, material contract-subject changes; spikes, fixes, and in-flight READY workstreams excluded; short-form floor); A2 bind audits to the claims ladder instead of a parallel taxonomy, one primary source per material claim, demonstrated-behaviour check or recorded waiver; A3 READY is an owner decision, gate nests inside stage gates, exceptions produce a short-form record; A4 reassessment tied to the gate rhythm with reviewed-as-of dates; A5 adopted form cross-references charter/master instead of restating rules. Suggested pilot: S5. Proposal remains unadopted pending Gemini's review and an owner decision record | FEATURE_RESEARCH_GUARDRAIL_REVIEW_2026-11-14.md; this log |

| 12 | 2026-11-15 | Guardrail DRAFT v0.2 — Feature Research & Comparative Audit (reviewer: Gemini) | Approved | Checklist passes on all five areas: scope classes and proportionality (§2), claims-ladder binding and primary sources (§4), comparative structure and deliverable (§5–§6), gate nesting inside stage gates (§7), governance cross-references (§9). Two non-blocking observations: (1) whenever a §4 waiver is used, its follow-up verification milestone must be logged in the project tracker so it cannot languish; (2) standalone-document status is the correct architecture. Verdict: ready for adoption via an owner decision record; no further revisions required. **Relay note (resolved 2026-11-15):** this Gemini text was initially relayed twice under two labels, so only one approval was recorded at the time; ChatGPT's genuine, different review then arrived and is logged as #13. The duplication was a relay error, and this row records Gemini's single verdict on v0.2 | FEATURE_RESEARCH_GUARDRAIL_DRAFT_v0.2_FOR_PEER_REVIEW.md (header stamp); this log |

| 13 | 2026-11-15 | Guardrail DRAFT v0.2 (reviewer: ChatGPT, review 1 of 2) | Approved with amendments | Four required amendments (ChatGPT's numbering): A1 include significant internal subsystems and foundational architectural components in the full research gate, depth by complexity/risk; A2 the claims ladder classifies the evidence behind each assertion — vendor documentation alone cannot establish demonstrated behaviour for external products; A3 clarify exemptions — spike harness exempt, investigative spike into a feature/technology uses the short-form exception, ordinary restoration exempt, emergency exception only where research is actually needed; A4 reassessment triggers are relevance-based (material effect on an assumption, comparison, dependency, risk, or decision). Plus a non-blocking suggestion: the report template should record the owner decision, its date, the workstream, and READY WITH CONDITIONS conditions. Disposition: reconcile into v0.3 and submit that exact version to Gemini for the second review; keep v0.2 intact; adoption NOT authorized by this review | FEATURE_RESEARCH_GUARDRAIL_DRAFT_v0.3_FOR_PEER_REVIEW.md; this log |

| 14 | 2026-11-15 | Guardrail DRAFT v0.3 (reviewer: Gemini, second review per ChatGPT's disposition) | Approved | Verified each reconciled item: Class D internal subsystems (C-A1), exemption boundaries (C-A3), claims-ladder evidence distinction capping vendor claims below Tested (C-A2), tracker-logged waivers (G-obs), template decision fields (C-minor), relevance-based reassessment triggers (C-A4). Verdict: fully ready for adoption via an owner decision record in the charter; no further revisions required. With this verdict the review chain set by the reviewers themselves is complete (Muse #11 on the proposal; Gemini #12 and ChatGPT #13 on v0.2; Gemini #14 on v0.3) — adoption awaits the owner's decision record | FEATURE_RESEARCH_GUARDRAIL_DRAFT_v0.3_FOR_PEER_REVIEW.md; this log |

## Rules for future reviews

- Every new verdict gets a row here in the same turn it is acted on,
  with its class, its directions verbatim in substance, and the
  document where it is stamped.
- A verdict is never paraphrased into a stronger one. "Approved with
  directions" is not "approved as-is".
- If a review rejects or sends work back, that is recorded with the
  same prominence as an approval.
- Scope note: this log covers Profeshy only. Other projects keep
  their own review records.
