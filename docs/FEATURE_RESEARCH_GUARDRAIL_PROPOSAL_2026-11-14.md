# Proposed Guardrail — Feature Research & Comparative Audit

**Filed:** 2026-11-14, as relayed by the owner.
**Status: PROPOSED — PEER REVIEW REQUIRED.** This is a proposal,
not an adopted rule. Per the owner's recommendation it is kept
SEPARATE from the adopted Master Guardrails and Rules document
until peer review is complete. If accepted, the intent is that it
becomes a reusable gate and report template, adopted by its own
decision record — not folded in as another paragraph of principles.

**Review brief (owner's direction):** the second reviewer (Gemini)
is asked to scrutinize four things before adoption:

1. **Scope:** What qualifies as a feature or significant subsystem,
   so the guardrail does not create excessive research overhead?
2. **Research quality:** Are the source and evidence standards
   strong enough to prevent superficial competitor comparisons?
3. **Gate authority:** Who can mark research READY, and how does
   that interact with the existing project gates?
4. **Maintenance:** How do we keep research current without
   repeatedly reopening settled decisions?

---

**Status:** PROPOSED — PEER REVIEW REQUIRED
**Purpose:** Ensure every feature, subsystem, or significant project component is researched against current industry practice before we design, implement, or materially change it.

## 1. Core principle

Before working on a feature or significant part of Profeshy, we must understand:

1. **What is working in the world today?** Identify established products, implementations, technical approaches, and industry practices addressing the same or closely related problem.
2. **How does it work?** Research the underlying architecture, workflows, technical mechanisms, dependencies, strengths, limitations, and known failure modes.
3. **How does our work compare?** Audit our proposed or existing implementation against those findings.
4. **What should we do differently?** Identify where we match established practice, where we improve upon it, where we deliberately diverge, and where we have gaps.

The objective is not to copy competitors or follow trends blindly. It is to make informed engineering decisions based on current evidence and maintain an enterprise-level standard of research and execution.

## 2. Required research before implementation

For each applicable feature or workstream, conduct a proportionate research exercise covering:

- **Current landscape:** Identify relevant established solutions and credible implementations.
- **How they work:** Understand their actual mechanisms, architecture, lifecycle, interfaces, and operating assumptions—not merely their advertised capabilities.
- **Evidence quality:** Prefer primary technical documentation, source code, specifications, engineering publications, reproducible demonstrations, and credible implementation evidence.
- **Strengths and weaknesses:** Identify what works, what does not, trade-offs, edge cases, and known limitations.
- **Current practice:** Establish whether the approach remains relevant and actively used, rather than relying on outdated examples.
- **Enterprise expectations:** Consider reliability, security, maintainability, observability, scalability, compatibility, performance, and failure recovery where relevant.

Research depth must be proportionate to the feature's complexity, risk, and importance. A small change does not require an exhaustive industry report; a foundational subsystem must not proceed on superficial research.

## 3. Required comparative audit

Research must be compared explicitly against Profeshy's own work.

The audit should identify:

| Area | Required assessment |
|---|---|
| Industry approach | What established solutions currently do |
| Mechanism | How those solutions achieve it |
| Profeshy approach | What we have implemented or propose |
| Comparison | Where we match, differ, improve, or fall short |
| Risks | Technical, security, reliability, compatibility, and maintenance risks |
| Gaps | What remains unsupported, untested, or unresolved |
| Recommendation | Adopt, adapt, improve, defer, or reject—with reasons |

Do not claim superiority without evidence. Do not treat a feature's presence as proof of equivalent quality or behaviour.

## 4. Evidence and decision discipline

- Record sources and access dates so findings can be revisited.
- Separate documented facts, observed behaviour, informed interpretation, and assumptions.
- Verify important technical claims against primary sources wherever possible.
- Record conflicting evidence and unresolved questions rather than selecting whichever source supports the preferred design.
- Distinguish what another product claims to do from what has actually been demonstrated.
- Do not change an accepted contract or established project decision merely because an alternative exists. Any change must follow the existing decision and approval process.

## 5. Research deliverable

Before implementation begins, the workstream should have a concise **Feature Research & Comparative Audit** containing:

1. Feature scope and intended outcome.
2. Current industry landscape and relevant examples.
3. Explanation of how the strongest relevant implementations work.
4. Evidence and source references.
5. Comparison against Profeshy's current or proposed approach.
6. Identified gaps, risks, trade-offs, and opportunities.
7. Recommended approach and rationale.
8. Open questions and required verification.
9. Research date and review status.

The report becomes part of the workstream's supporting evidence and handover material.

## 6. Implementation gate

The research and comparative audit must be reviewed before significant implementation begins.

The outcome must be one of:

- **READY:** Research is sufficient, the approach is justified, and known risks are addressed or accepted.
- **READY WITH CONDITIONS:** Work may proceed within explicitly documented boundaries.
- **BLOCKED:** Material research, evidence, design, or risk questions remain unresolved.

A blocked research gate cannot be bypassed by simply beginning implementation under a different task name.

Emergency fixes and explicitly authorized investigative spikes may use a narrower process, but their scope and exceptions must be documented. They do not automatically authorize full implementation.

## 7. Reassessment

Research is not permanently valid. Revisit it when material changes occur in relevant technology, dependencies, security expectations, industry practice, or Profeshy's own architecture.

Reassessment must be proportionate and must not create unnecessary rework or reopen accepted decisions without new evidence.

## 8. Intended standard

The expected outcome is that Profeshy is not merely functional. Its engineering decisions are informed by current practice, technically justified, independently reviewable, and deliberately designed around reliability, maintainability, and long-term quality.

**Operating question:**

*"What is working today, how does it actually work, and how does our implementation compare?"*

That question should be answered before we commit to building a significant feature or subsystem.
