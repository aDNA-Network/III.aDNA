---
type: context_guide
topic: iii_domain_packs
subtopic: whitepaper_communication
created: 2026-04-13
updated: 2026-05-08
token_estimate: 2800
last_edited_by: agent_stanley
status: draft
campaign: campaign_whitepaper_iii_deep_review
tags: [context_guide, iii-review, traps, whitepaper, communication, visual-design, ip-rigor, draft]
banner: "who/assets/banners/banner_context_guide.jpg"
icon: file-text
migration_provenance:
  previous_home: lattice-labs/what/context/iii_domain_packs/context_iii_whitepaper_communication.md
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-2
quality_metric:
  rubric_version: "adr_007_v0"
  scored_at: 2026-05-23
  scored_by: agent_argus
  scoring_mission: campaign_d_federation_adaptive_loop MD-B4
  signal_density: 5
  actionability: 5
  coverage_uniformity: 5
  source_diversity: 3
  cross_topic_coherence: 4
  graduation_yield: 1
  composite: 3.83
  floor_check: triggered
  floor_axes: [graduation_yield]
  notes: "MD-B4 7-pack pilot scoring. Nine whitepaper-specific traps across 3 dimensions (communication clarity / visual design / IP rigor) plus Cross-Trap Escalation Rules + Trap Graduation criteria + Learning Store Integration. Pack is itself draft + campaign-scoped (campaign_whitepaper_iii_deep_review) and tracks its own draft→canonical graduation track separate from the canonical learning store. graduation_yield=1 reflects MB4-2 measurement issue (canonical-jsonl graduations not pack-routed); axis collapse not a pack-quality issue — the pack's own internal cycle tracking is comprehensive. Composite matches MD-B1 exemplar (3.83) coincidentally — different shape (high signal/actionability/coverage, lower source_diversity)."
---

# III Domain Pack: Whitepaper Communication (DRAFT)

Domain-specific trap library for reviewing the Lattice Protocol whitepaper across three dimensions: communication clarity, visual design, and IP rigor/value communication. Extends the `core` pack (Five Traps) with 9 whitepaper-specific failure modes.

**Status: draft, campaign-scoped.** Graduation to canonical status requires 5+ cycles with >=80% user acceptance per the III graduation criteria.

## When to Load

Load alongside `core` (and optionally `mathematics`) when reviewing:
- `~/lattice/whitepaper/whitepaper.tex` (any section)
- `~/lattice/whitepaper/figures/*.tex` (TikZ figures)
- Appendices A-F (especially Appendix E — invention portfolio)
- Any document making claims about the whitepaper's content or positioning

## Canonical Reference

The whitepaper LaTeX source is the ground truth:
`~/lattice/whitepaper/whitepaper.tex` (3,307 lines, v2.0.0)

Protected content (do NOT propose modifications):
- Compliance Supremum Theorem (Section 4.6)
- Proof structure (Lemmas 1-3, Corollary 1)
- Rare disease narrative (Section 1.4)
- Regulatory crosswalk (Chapter 7, Table 6)
- Threat model and security properties (Sections 3.8-3.9)
- "To the authors' knowledge" hedging

## Dimension 1: Communication Clarity Traps

### Trap WP-AUDIENCE-01: Audience-Register Mismatch

| Field | Value |
|-------|-------|
| **Description** | Text shifts between audience registers (engineer-level implementation detail vs. investor-level value proposition vs. academic formalism) without transition or framing, leaving readers outside the active register confused or alienated |
| **Example** | A paragraph opens with "The trust scoring function T(n) computes a weighted sum across six attestation dimensions" (engineer) and closes with "positioning Lattice Protocol as the DNS of AI infrastructure" (investor) without a bridging sentence |
| **Where It Hides** | Chapter openings that mix motivation (investor) with specification (engineer). Section transitions where the audience shifts. Executive summary attempting to serve all audiences in a single paragraph. Case study narratives mixing clinical detail with business outcomes. |
| **Severity Default** | **Medium** |

**Detection method**: For each paragraph, identify the primary audience register (engineer/investor/academic/legal). Flag any paragraph that contains claims at 2+ different registers without explicit transition ("From a technical perspective..." / "For institutional stakeholders...").

**Why it happens**: The whitepaper serves 4 audiences simultaneously. Authors naturally shift registers as they think about different stakeholders. Without explicit framing, transitions feel jarring to any single-audience reader.

---

### Trap WP-JARGON-01: Unglossaried Technical Jargon

| Field | Value |
|-------|-------|
| **Description** | A technical term is used in body text but does not appear in Appendix D (Glossary of Terms, currently 48 entries), or appears in the glossary with a definition that doesn't match its usage |
| **Example** | "LRD" used in Section 4.8 without expansion on first use in that section, or "workspace transfer" used with a different scope than its glossary definition |
| **Where It Hides** | Acronyms introduced in one section and used without expansion in a later section. Terms with multiple senses across chapters (e.g., "lattice" in 3 senses). Novel terms from v2.0.0 additions that weren't added to the glossary. |
| **Severity Default** | **Low** |

**Detection method**: Extract all technical terms from body text (capitalized multi-word phrases, acronyms, terms in `\texttt{}` or `\textsc{}`). Cross-reference against Appendix D glossary entries. Flag: (a) terms used but not glossaried, (b) glossary entries not used in body text, (c) definition mismatches.

**Why it happens**: The glossary was created at v1.0.0 (48 terms) and may not have been updated for v2.0.0's new sections (LRD, marketplace, economic layer, stakeholder guide).

---

### Trap WP-ARC-01: Narrative Arc Gap

| Field | Value |
|-------|-------|
| **Description** | A chapter or major section does not connect to its neighbors — missing "having established X in Chapter N, we now..." bridge sentences at openings, or missing "Chapter N+1 will extend this to..." forward references at closings |
| **Example** | Chapter 5 (DLT) opens with on-chain provenance without referencing how it relates to Chapter 4's mathematical lattice. The reader must infer why provenance matters to the compliance lattice argument. |
| **Where It Hides** | Chapter openings (first paragraph of each chapter). Chapter closings (last paragraph before the next chapter begins). Section transitions within long chapters. The jump from mathematical theory (Ch4) to practical DLT (Ch5) is the most likely gap. |
| **Severity Default** | **Medium** |

**Detection method**: Read the first and last paragraph of each chapter. The opening must reference what came before and why this chapter follows. The closing must preview what comes next. Flag any chapter that opens or closes without connecting to the narrative arc.

**Why it happens**: Chapters were written and revised in separate campaigns. v2.0.0 added new sections (1.2, 4.8, 5.5, 8.7, Appendix E-F) that may not have had their narrative bridges updated.

---

## Dimension 2: Visual Design Traps

### Trap WP-FIGTEXT-01: Figure-Text Inconsistency

| Field | Value |
|-------|-------|
| **Description** | A TikZ figure shows elements, labels, relationships, or quantities that don't match the surrounding body text's description of the same concept |
| **Example** | Figure 2 (architecture stack) shows 7 layers but the text describes them with different names or in a different order. Or a figure labels "Module Registry" but the text calls it "Backend Registry." |
| **Where It Hides** | Terminology drift between figure labels and text (figures frozen at v1.x terminology while text updated to v2.0.0). Count mismatches (figure shows N items, text says M). Direction mismatches (figure shows bottom-up, text describes top-down). Missing elements (text describes 6 components, figure shows 5). |
| **Severity Default** | **High** |

**Detection method**: For each figure, read the TikZ source and extract all node labels, edge labels, and structural elements. Compare against the 2-3 paragraphs of body text that reference the figure. Flag any mismatch in terminology, count, order, or structural relationship.

**Why it happens**: Figures are separate `.tex` files and are easy to skip during text revision passes. The v2.0.0 campaign added 4 new figures but also revised terminology in existing text.

---

### Trap WP-FIGLOAD-01: Overcrowded Figure

| Field | Value |
|-------|-------|
| **Description** | A TikZ figure contains so many elements (nodes, edges, labels, legends) that it is not readable at the paper's print scale (letter-size, two-column or single-column as applicable) |
| **Example** | A figure with 15+ labeled nodes and 20+ edges where edge labels overlap, or where the font size must be reduced below 7pt to fit all labels |
| **Where It Hides** | Architecture diagrams that try to show the complete system in one figure. Pipeline diagrams with many stages. Taxonomy figures with deep nesting. Any figure that was clear on a large screen but unreadable when compiled to PDF at paper scale. |
| **Severity Default** | **Medium** |

**Detection method**: Count nodes and edges in the TikZ source. Heuristic: >12 labeled nodes OR >15 edges OR any `\tiny` or `\scriptsize` font commands for labels → flag for review. Compile the PDF and visually check at 100% zoom: can all labels be read without zooming?

**Why it happens**: Authors design figures at screen resolution with zoom available. Print is unforgiving — what's clear at 200% zoom is illegible at 100%.

---

### Trap WP-FIGREF-01: Orphan Figure

| Field | Value |
|-------|-------|
| **Description** | A figure exists in `figures/` but is not referenced in body text via `\ref{fig:X}`, OR body text references a figure that doesn't exist (phantom reference) |
| **Example** | Text says "as shown in Figure~\ref{fig:token-economics}" but no `\label{fig:token-economics}` exists in any figure file. Or `fig-network-effects-flywheel.tex` exists but no paragraph references it. |
| **Where It Hides** | New figures added in v2.0.0 that may not be woven into all relevant sections. Old figure labels that were renamed but not updated in all `\ref{}` calls. Figures referenced in the conclusion but not introduced earlier. |
| **Severity Default** | **High** |

**Detection method**: Extract all `\label{fig:*}` from figure files. Extract all `\ref{fig:*}` from body text. The two sets must match exactly. Also check that every figure's first reference appears BEFORE its placement in the document (figures should be introduced before they're shown).

**Why it happens**: LaTeX separates figure placement from reference. It's easy to add a figure file without adding the reference, or to rename a label without updating all references.

---

## Dimension 3: IP Rigor Traps

### Trap WP-GCLAIM-01: Innovation Claim Strength Mismatch

| Field | Value |
|-------|-------|
| **Description** | An innovation claim in Appendix E (G1-G6 invention portfolio) uses language that is stronger than what the cited body section actually proves or demonstrates |
| **Example** | Appendix E describes G3 as "a novel algorithm that guarantees optimal compliance resolution" but Section 4.6 only proves it for the restricted case of finite lattices with bounded depth. The guarantee doesn't extend to the general case claimed. |
| **Where It Hides** | Appendix E is a dense summary — each G-label compresses multiple sections into 2-3 sentences. The compression naturally strips qualifiers. Patent-strength language ("novel", "first", "guarantees") may exceed what the proof actually establishes. The gap between "implemented" and "proven" is often elided. |
| **Severity Default** | **Critical** |

**Detection method**: For each G1-G6 entry in Appendix E, identify the body sections it cites. Read those sections and extract the actual claim strength (proven theorem? empirical result? design goal? conjecture?). Compare against Appendix E's language. Flag any claim where Appendix E is stronger than the source.

**Why it happens**: Appendix E was written for patent positioning, which incentivizes strong language. The body text is more carefully hedged because it's in the context of proofs and evidence. Patent counsel review (sharing prep M08) will catch legal issues, but this trap catches the technical accuracy of the claims.

---

### Trap WP-PRIOR-01: Prior Art Differentiation Gap

| Field | Value |
|-------|-------|
| **Description** | Chapter 8 (Related Work) or Section 8.7 (infrastructure comparison) describes a competitor or related system without explicitly stating how the Lattice Protocol differs from or improves upon it |
| **Example** | "HuggingFace Hub provides a model registry with versioning and collaboration features" — but no follow-up sentence explaining what Lattice Protocol does that HuggingFace Hub doesn't (typed connections, compliance lattice, mathematical guarantees). |
| **Where It Hides** | Descriptions of related work that are descriptive but not contrastive. The "comparison table" (if any) that shows features without explaining why Lattice's approach is architecturally different, not just featurally different. Citations that acknowledge prior work without positioning against it. |
| **Severity Default** | **High** |

**Detection method**: For each system mentioned in Ch8 or Section 8.7, check whether the text includes a differentiation statement. The statement must be architectural (not just "we also do X"), identify what the protocol does that the cited system cannot, and reference the relevant innovation (G1-G6) where applicable.

**Why it happens**: Academic convention is to cite related work respectfully. Patent convention requires clear differentiation. The whitepaper must serve both, which creates tension.

---

### Trap WP-PATENT-01: Patent Language Precision

| Field | Value |
|-------|-------|
| **Description** | A novelty claim lacks the "to the authors' knowledge" hedge, or uses absolute language ("first", "only", "unique") without qualifying scope |
| **Example** | "Lattice Protocol is the first system to combine compliance verification with federated AI workflows" — should be "To the authors' knowledge, no existing system combines..." |
| **Where It Hides** | Abstract (compressed language tends to drop hedges). Contributions list in Ch1. Appendix E invention descriptions. Conclusion claims about protocol uniqueness. Any sentence containing "first", "only", "unique", "novel" as applied to the protocol. |
| **Severity Default** | **High** |

**Detection method**: Grep for absolute novelty markers: "first", "only", "unique", "novel", "no existing", "unlike any". For each hit, check whether it is hedged with "to the authors' knowledge" or equivalent qualifier ("to our knowledge", "in the surveyed literature"). Flag any unhedged absolute novelty claim.

**Why it happens**: The C-018 pattern (summary-confidence-inflation) is the primary cause — summaries strip hedges from properly hedged body claims. Also, patent-oriented writing naturally gravitates toward absolute language.

---

## Cross-Trap Escalation Rules

When multiple traps fire on the same location:

| Combination | Escalation |
|---|---|
| WP-GCLAIM-01 + WP-PATENT-01 | -> Critical (innovation claim both overstated AND unhedged) |
| WP-FIGTEXT-01 + WP-AUDIENCE-01 | -> High (figure misaligns with text AND audience can't resolve the discrepancy) |
| WP-PRIOR-01 + WP-GCLAIM-01 | -> Critical (prior art gap weakens the innovation claim) |
| WP-ARC-01 + WP-AUDIENCE-01 | -> High (narrative gap compounds register mismatch) |
| WP-JARGON-01 + WP-AUDIENCE-01 | -> High (unexplained term in wrong-register context) |

## Trap Graduation

This pack starts as **draft** with 9 initial traps. Graduation criteria:

1. Each trap must fire in >=3 cycles with user acceptance >=80%
2. The pack as a whole must surface issues the `core` pack missed
3. Trap detection must be reproducible across multiple chapter types

Traps that graduate become reusable for future whitepaper revisions and related technical documents.

## Learning Store Integration

Every approved fix from this pack's findings is written to `how/campaigns/campaign_whitepaper_iii_deep_review/iii_corrections_campaign.jsonl` in the standard III learning-store format. Entries with `accepted: true` and frequency >=3 become graduation candidates at DG2.
