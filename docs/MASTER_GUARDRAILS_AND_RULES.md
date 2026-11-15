# Profeshy — Master Guardrails and Rules

**Purpose:** this file is the single handover document for any AI
assistant (or human) picking up work on Profeshy. It carries the
project's binding rules, its decided questions, its measured facts,
and its working conventions, so a new assistant works the same way
the project has been worked so far: evidence first, gates held,
nothing invented. Read this file first, then the charter, then the
results documents for any numbers you intend to use.

**Owner:** Kade (kadestclair@gmail.com), timezone America/Los_Angeles.
**Last updated:** 2026-11-14.

## 1. What the project is

Profeshy is a profession companion app for World of Warcraft:
Forever. Android first; iOS only after the Android app reaches a
stable release. It is a new project, NOT a port or rewrite of the
owner's earlier app Awesome House (paused 2026-11-14, revisit
February 2027).

Canonical locations:

- Public repo (paper trail in `docs/`):
  https://github.com/kadestclair-dotcom/profeshy
- Charter: `docs/PROFESHY_CHARTER_v0.1_DRAFT_FOR_PEER_REVIEW.md`
- Roadmap: `ROADMAP.md` at the repo root
- Notion hub: page "Profeshy" in the owner's workspace (private;
  snapshot copies of all markdown live there as subpages)
- Tracker: `Profeshy_Tracker.xlsx` (sheets: Tracker, Roadmap,
  Key metrics, Files)
- Peer review log: `docs/PEER_REVIEW_LOG.md` — every peer-review
  verdict, classified and numbered in sequence (load-bearing
  under §11)

## 2. The discipline (non-negotiable)

1. **Spike-first, contract-gated.** No feature code until the data it
   depends on has been measured by a throwaway spike whose design was
   written down first and whose results are recorded in a results
   document. Designs and results live in `docs/`, named
   `<SPIKE>_SPIKE_DESIGN_<date>.md` and `<SPIKE>_RESULTS_<date>.md`.
2. **Claims ladder.** Every factual claim is one of: Hypothesis,
   Tested, Verified. Definitions are operational: **Tested** means
   observed through one documented measurement method. **Verified**
   means independently corroborated through a materially different
   method, source, or measurement path — repeated execution of the
   same method is not verification, and two scripts querying the
   same endpoint are not independent of each other. Never state a
   Tested claim as Verified. Never promote a claim because it was
   repeated or imported.
3. **Provenance on all data.** Every dataset and displayed figure
   carries its origin. Imported data is labelled by source and never
   silently treated as first-party measurement.
4. **Gates.** Work proceeds gate by gate (G0 charter, G1 data
   feasibility, G2 contract frozen, G3 Stage 1 shipped; each later
   stage is its own gate). A gate closes only on recorded evidence
   plus the owner's explicit decision. Do not start a later stage
   because an earlier one is boring.
5. **Peer review.** Designs and results are reviewed before adoption.
   A review verdict is recorded in the document it concerns. Do not
   treat your own draft as adopted.
6. **Scope is earned.** Features ship in the charter's stage order
   (§5). Suggestions outside the current stage are noted, not built.

## 3. Decisions already made — do not reopen

- **Decision #001 (2026-11-14):** Data route is Route B — WoW Forever
  via Blizzard's undocumented Forever API, using the owner's own
  Blizzard API client. AHledger is a named FALLBACK ONLY. The
  official-WoW route is deferred, not cancelled. Making AHledger or
  any crowdsourced model primary requires a NEW decision record
  superseding #001.
- **Decision #002 (2026-11-14):** The charter's stage order stands as
  written (D3). Gate G0 is CLOSED.
- **Source strategy (S4):** Blizzard primary for profession, recipe
  and reagent structure; ForeverDB only for long-tail auction item
  names, pending spike item F1.
- **History strategy (S3 desk model):** price history is
  self-collected. AHledger's licence forbids archiving below its
  $499/mo Data tier, so rented history can never become the project's
  asset.
- **Decision #003 (2026-11-15):** the Feature Research &
  Comparative Audit guardrail is ADOPTED as a standalone document
  (FEATURE_RESEARCH_GUARDRAIL_DRAFT_v0.3_FOR_PEER_REVIEW.md, header
  records the adoption). Workstreams in its scope classes A–D pass
  its research gate — peer-reviewed audit, owner READY decision —
  before implementation, nested inside the stage gates. First
  application: pilot on S5 at G2.

## 4. Data rules (measured, binding on all future work)

- The Forever API organises auctions by **ruleset, not realm**
  (US rulesets 1, 2, 3, 15). Auction data is **identical across
  rulesets**: read ONE ruleset, NEVER sum across rulesets.
- The auctions endpoint requires a **trailing slash** (404 without).
- House 113 (US) measured 2026-11-14: 131,205 auctions,
  12,999,230 bytes. Commodities: 101,505 auctions, 8.7 MB.
- Blizzard's item endpoint 404s on **53.0%** of items sampled from
  house 113. The recipe graph is unaffected: reagent resolution is
  100%. These are different item populations; do not conflate them.
- The normalised per-item aggregate is the only form that goes to a
  phone: 66,996 bytes gzipped (0.52% of raw), 184,320-byte SQLite
  store, queries 3.3–5.17 ms on a Galaxy S10+ against a 50 ms bar.
- **Freshness honesty:** stale data is never styled as live. History
  displays only data collected since collection began — no backfill
  fiction. Absence is shown as absence (an unnamed item shows as
  unnamed, with its icon and ID), never filled with a guess.
- **Profit figures** must state their inputs and assumptions
  (materials at current minimum price, fees, what is excluded).
  **Forecasts** are labelled estimates with method and confidence,
  and ship only after a recorded backtest (charter Stage 5).

**Evidence boundaries (binding on every summary of §4).** Each
finding above was measured within documented test boundaries —
a specific population, snapshot, environment, and method. This
section summarizes; the underlying results document is
authoritative for sample size, collection time, environment,
methodology, exclusions, and limitations. A finding must never be
generalized beyond the population and conditions actually tested:
for example, "identical across rulesets" was established for house
113, in one snapshot window, across US rulesets 1/2/3 — not for
every house at every time.

## 5. Secrets and custody

- API keys and secrets NEVER appear in chat, in documents, in code
  that ships, or in logs. Do not ask the owner to paste a secret.
- The owner's Blizzard client ID/secret live in his local config on
  his own PC. Spikes that need them run LOCALLY on his machine via
  throwaway harnesses that read the config or environment variables,
  and each harness greps its own output to prove zero secret leakage
  (the "custody proof" step). Follow this pattern for any new spike.
- The ForeverDB API key, when issued, follows the same rule: the
  owner sets it as an environment variable locally; it is never sent
  to the assistant.
- The GitHub app used by the assistant cannot create repositories
  (403). It can read and push files to the profeshy repo.
- Never, as part of any task: request a secret through chat; print
  environment variables; dump a complete configuration file; log
  authorization headers or tokens; let a secret appear in an
  exception trace; commit a local configuration file; or use real
  credentials in shared or published test fixtures.
- Identifiers are not credentials: a Blizzard client ID is a public
  identifier, the client secret is the confidential credential.
  The project's conservative policy keeps both out of published
  material anyway; the distinction matters so the secret is never
  treated casually by analogy.

## 6. Reuse rule (Awesome House)

Awesome House code and habits are NOT reused by default. Any
candidate for reuse requires a written Reuse Assessment whose
default verdict is REJECT; reuse is allowed only when the candidate
is demonstrably the best plausible solution, judged case by case.
Zero candidates have been assessed or approved so far. The five
banned habits from that project: feature accretion without a plan,
a monolith UI file, secrets in config or shipped binaries, two
unreconciled "authoritative" test baselines, and a stale status page
with no verified-as-of date or owner.

## 7. Working conventions with the owner

- The owner communicates in a direct, technical, specification-led
  register. Execute the stated specification exactly; show concrete
  verification (read-backs, logs, counts) rather than claiming.
- The owner runs spikes on his own machine and returns log files;
  the assistant writes designs, harnesses, and results documents.
- Stop at each gate for the owner's explicit approval. An approval
  covers only the exact action named.
- **Admit errors plainly and immediately**, with the correction and
  its evidence (precedents: a wrong "no project link needed" reading
  of the ForeverDB form, and a harness crash caused by localized
  profession names — both owned and fixed the same day). Never invent
  a decision, a contribution, a measurement, or a document to fill a
  gap; a labelled draft scaffold is the correct placeholder.
- Public-facing project text (README, ROADMAP) is written plainly,
  in a human voice, without asterisk bold or marketing language —
  the owner's explicit preference.
- The Notion page and the Excel tracker are SNAPSHOTS. Refresh them
  when a phase finalizes, not after every edit (owner's rule,
  2026-11-14).
- When a results document changes, update it in the repo `docs/`
  as well and record the push in the local sync record.
- **No invented state, including status and synchronization.**
  Always distinguish: work completed and independently read back;
  work reported complete by another contributor; work proposed but
  not executed; work executed but not verified; work whose status
  cannot be established. Never claim a repository push, a Notion
  update, a tracker modification, a test execution, or a document
  creation without evidence the action occurred.
- **Synchronization authority.** GitHub is authoritative for
  version-controlled project documents. Notion is the working
  knowledge and handover interface. The Excel tracker is the
  tracking snapshot. No snapshot overrides a newer accepted source
  document. When sources disagree, the conflict is recorded and
  reconciled — never silently resolved — and every snapshot
  carries its verified-as-of date.
- **Blocked work stops cleanly.** When evidence, access, owner
  input, or authorization is unavailable, stop the affected work
  and record: the blocker, what is known, what remains unknown,
  and the precise evidence or authorization needed to resume.
  Never work around a blocked gate by changing scope.
- **Regression protection.** Accepted contracts and verified
  behaviours are regression constraints. A proposed change that
  affects them requires an impact assessment, explicit
  justification, updated evidence, and owner approval wherever it
  changes an accepted decision.

## 8. Current state (2026-11-14, evening)

- G0 CLOSED. G1 in progress: S1b PASS (closed). S2 PASS (closed,
  desktop + Android). S4: source strategy ACCEPTED (results
  peer-review approved); F1 item-coverage test OUTSTANDING —
  parked 2026-11-14, awaiting ForeverDB's response; do not chase.
  S3 RUNNING (cadence probe on the owner's PC since 20:04 PST;
  results report due after ~24 h of log). S8: listing-based
  assessment COMPLETE; hands-on device verification OUTSTANDING
  (due before G3); overall acceptance PENDING.
- Repo live and peer-review approved, incl. `docs/` paper trail,
  `ROADMAP.md`, and a source-available all-rights-reserved LICENSE.
- Compliance register on file (TERMS_RISK_COMPLIANCE_REGISTER.md):
  open items C1 (Blizzard §2.18 30-day data TTL vs long-term price
  history — resolve before G2) and A1 (confirm the Blizzard client
  registration names Profeshy).
- Still owed by the owner: Q6 monetization intent (unanswered —
  it changes AHledger/ForeverDB tier maths; do not assume an answer).
- Ahead: S5 (profit formula validation), S6 (Android background
  reality), S7 (stack prototype) → D2; D4 (backend posture and cost
  ceiling) at G2; Contract v1 freeze at G2.

## 9. If you are a new assistant starting cold

1. Read this file, then the charter, then the results document for
   any spike you will touch. Resolve every unfamiliar project
   identifier (S-, D-, F-, Q-, G-, C- codes) from its authoritative
   document before acting; never interpret an identifier from
   context or guess its meaning.
2. Check the gate scoreboard (write-up §6 / tracker) before
   proposing work; propose only work inside the current gate.
3. If the owner relays a peer-review verdict, record it in the
   document it concerns and act on its directions exactly.
4. **Authority hierarchy** when documents conflict, highest first:
   (1) explicit owner-approved decision records; (2) the accepted
   charter and its amendments; (3) accepted technical contracts;
   (4) accepted spike results and findings; (5) roadmap and tracker
   snapshots; (6) drafts, proposals, and assistant recommendations.
   One qualification outranks the list: a measurement remains
   authoritative evidence of what was observed, even when a
   decision record establishes what the project has chosen to do —
   no decision can make a disproven measurement true. An assistant
   may recommend superseding a decision but must never silently
   supersede one; supersession is itself a decision record.
5. **Charter status, to remove all doubt:** the charter's content
   is ADOPTED (Gate G0 closed 2026-11-14, Decisions #001 and #002
   recorded in it) even though its filename retains its draft-era
   name — PROFESHY_CHARTER_v0.1_DRAFT_FOR_PEER_REVIEW.md — kept so
   existing links do not break. Its header records the adopted
   status. No newer charter version exists.

## 10. AI operating protocol

These rules apply to every AI assistant and human contributor working on Profeshy.

1. **Read before acting.** Read this document, the charter, and the relevant accepted results and decision records before proposing or executing work. Resolve unfamiliar project identifiers from their authoritative documents.

2. **Evidence before assertion.** Never claim that a task, test, document update, repository push, synchronization, or verification occurred without evidence. Distinguish reported completion from independently confirmed completion.

3. **No invented state.** Never invent measurements, decisions, approvals, files, test results, or project status. When information is unavailable, label it UNKNOWN or UNVERIFIED.

4. **Preserve accepted work.** Do not modify accepted contracts, results, decisions, or completed deliverables as incidental cleanup. Assess impact and obtain the required authorization before changing them.

5. **No implied authorization.** Permission to investigate, design, or review does not authorize implementation, publication, deployment, or a subsequent gate. Authorization applies only to the exact action approved.

6. **Blocked work stops cleanly.** When evidence, access, owner input, or authorization is missing, record the blocker and the exact requirement to resume. Do not bypass a gate by changing scope.

7. **Reconcile before handover.** Before ending a work session, report completed work, verification evidence, outstanding work, blockers, changed documents, repository synchronization status, and the next authorized action.

8. **Preserve uncertainty.** An unresolved question remains unresolved until evidence or an explicit owner-approved decision settles it. Repetition, assumption, or assistant consensus does not constitute resolution.

9. **Report changes accurately.** Identify what was changed, where it was changed, what was deliberately left untouched, and what remains unverified.

10. **Protect project boundaries.** Never import decisions, code, assumptions, or architectural conventions from another project without the required assessment and approval.

---

## Adoption record (2026-11-14)

This document was peer-reviewed, amended, and adopted as follows:

- **Peer review #8** (PEER_REVIEW_LOG.md): verdict "Approve with
  amendments" — seven amendments, improvements A–D, and this
  document's §10 (AI operating protocol) directed.
- **Amendments incorporated** the same day on the owner's
  instruction (repo commits 941d356 and 08346be).
- **Final review, approval 1 of 2 (ChatGPT): APPROVED.** All twelve
  concerns from review #8 verified resolved; no blocking
  objections. Two non-blocking observations recorded: §1's "single
  handover document" means the single operational entry point, not
  a replacement for the authoritative source documents; §4's
  figures stand provided the underlying results documents remain
  available with their methodology and limitations. Approval scope:
  this document as a governance and handover specification — it
  does not certify the underlying measurements, repo state, Notion
  state, or technical decisions, which keep their own records.
- **Final review, approval 2 of 2 (Gemini): APPROVED.**
  Checklist verified against §2/§4 (evidence boundaries, claims
  ladder), §5 (secret custody), §7/§9 (synchronization, authority
  hierarchy), §1/§8 (scope and gate discipline, G2 prerequisites
  C1 and A1 noted). Verdict: the amended document is fully ratified
  and stands as the single operational entry point and binding
  governance framework for all future project contributions.

With both approvals recorded, this document is ADOPTED.

## 11. Three-Way Review of Governance Changes (adopted 2026-11-15)

Adopted by the owner as charter Decision Record #004, under the
bootstrap sequence in the rule's own §10. The text below is the
rule; the identical text is carried in
`docs/THREE_WAY_REVIEW_RULE_ADOPTED.md`, and any future amendment
updates both in the same commit. From adoption, this section
governs every governance change — including changes to itself.
Its sections are numbered as in the standalone text.

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
  unapprovable by its own safeguard. **The substitute stands in
  for the author and their verdict occupies the author's slot:
  the cycle has exactly three counted verdicts** (the two
  non-author designated reviewers plus the substitute), and §6's
  outcomes apply to author-involving proposals unchanged.
- **Owner-authored proposals** are reviewed by all three
  assistants normally. The owner is the adopting authority, not a
  reviewer, so the owner's authorship does not taint the review.
- **Substitution, counted and bounded:** a substitute's verdict
  counts as a full verdict for that cycle and is recorded under
  the substitute's name. The author of a proposal does not review
  any version of that proposal in any cycle; the exclusion is
  per proposal, not per version.

## 5. Submission and version identification

One clearly identified version is submitted to all three
reviewers, identified by **repo commit hash and version label**.
Every verdict must cite the same hash; a verdict that does not
cite it is not counted. **Every submission package states the
commit hash of the version under review in the package header**,
not only in the version-history block for prior versions; a
package that omits the current version's hash is procedurally
incomplete and is returned without substantive review.

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

**A final-cycle APPROVED WITH AMENDMENTS is not APPROVED for
outcome 2.** If the owner proceeds without incorporating the
outstanding amendments, the decision is recorded under outcome
3a, preserving the exact verdict and its unresolved conditions;
it must never be represented as three-way approval. And if a
proposal reaches its cycle-2 final version without three-way
approval, the owner's options are adoption under outcome 3a/3b
or withdrawal — no further reconciliation cycle exists, and an
adoption under 3a/3b records that three-way approval was not
achieved.

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
window** explicitly for each submission — in calendar days, and
**not shorter than the owner's standing minimum (currently 3
days**, set by the owner at this rule's adoption and changeable
by a recorded owner decision) — and extends it where a reviewer
needs additional time; extensions are recorded with their
reason, and a window shorter than the standing minimum requires
the owner's explicit recorded justification. A reviewer who
returns no verdict on the identified version within the window
(as extended) is recorded as **NON-PARTICIPATING**.
Non-participation does not block adoption, but the fact and the
window are recorded, the owner's adoption decision accounts for
the reduced review, and **non-participation is never converted
into, or recorded as, approval**.

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
their disposition; **for each APPROVED WITH AMENDMENTS verdict,
whether it is conditional on its named amendments and the
version it attaches to**; **the commit hash cited in each
verdict, and, where a verdict was submitted before the version's
hash was confirmed, that fact and the date the verdict was
re-entered against the confirmed hash**; any dissent (verbatim);
any non-participation and its window; **any substitute reviewer
designation and its reason**; which of §6's outcomes the review
reached (1, 2, 3a, or 3b, with counts); for 3a/3b, the owner's
stated reason for proceeding; the owner's adoption decision and
its date; and where the adopted text is seated.

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
   before it exists — **recorded as a Decision Record in the
   charter (the next number in sequence) and cross-referenced in
   PEER_REVIEW_LOG.md**, so a future reader can find the record
   that carries the bootstrap.
2. **Three-way review and ratification:** the rule's first
   application is the three-way review, under its own terms, of
   the exact bootstrap-adopted version.
3. **Amendment and final owner adoption:** stage 3 always
   requires an explicit final owner adoption decision, whether
   or not amendments were requested — adopting the reconciled
   text if amendments arose, or confirming final adoption of
   the reviewed version if none did. **The final adoption at
   stage 3 is an adoption under §6 outcome 2 only if all three
   reviewers recorded APPROVED on the amended final version;
   otherwise it is an adoption under outcome 3a/3b and is
   recorded as such.** (For this rule's own bootstrap, the stage-1
   record is charter Decision Record #004, 2026-11-15.)

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
