---
type: mission_artifact
artifact_class: pack_revision_spec
title: "Campaign G G0 — ISS-surface pack-revision spec (ANCHOR) (Operation Atrium)"
campaign: campaign_g_consolidation
mission: g0_charter_recon
mission_class: campaign_planning_execution
status: draft
version: "0.1.0"
created: 2026-05-29
updated: 2026-05-29
last_edited_by: agent_argus
boundary_posture: boundary_preserving   # Markdown trap definitions only; NO ISS runtime in III; NO audit runtime
evidence_method: inspection_grade
non_runnable: true
proposed_pack: what/context/core_domain_packs/context_iii_iss_surfaces.md   # created at G1, NOT here
landing_target: G1
final_trap_selection_at: DP-2
governed_by: [adr_028_iss_architecture, adr_029_iss_standard_touch, adr_005_rlhf_signal_channel, adr_007_adaptive_improvement_loop, adr_002_consumer_federation_contract]
tags: [mission_artifact, campaign_g, operation_atrium, iss_surfaces, pack_revision_spec, anchor, traps, g0]
---

# G0 — ISS-Surface Pack-Revision Spec (ANCHOR)

> **Deliverable #4 of 8** (card §5.4) — the **anchor** of Campaign G. Specifies a NEW domain pack `context_iii_iss_surfaces.md` carrying **≥5 net-new traps** for operator-decision-gate interaction quality. This is a **spec / proposal** — the actual pack file is authored at **G1** after charter approval (Campaign F F1 precedent). **No pack edited here.** Final trap selection ratified at **DP-2**. Boundary: III stays semantic — these are Markdown trap definitions reviewing the ISS *contract surface*; no ISS runtime, no audit runtime in III.

## §1 Pack identity (proposed)

- **File:** `what/context/core_domain_packs/context_iii_iss_surfaces.md` (sibling to `web_design` / `canvas_visual` / etc.).
- **Reader profile:** *operator-decision-gate quality reviewer* — evaluates an authored ISS gate as decision architecture + RLHF instrument, the way `web_design`'s reviewer evaluates a site as a web-standards practitioner.
- **Reviews:** the ISS contract surface (gate output JSON per AD-7 + rendered HTML structure + wrapper config) — **never re-renders or re-runs** a gate (mirrors Trap 6's "read, don't re-run" discipline).
- **Boundary (firm):** SiteForge owns L1 library + L2 AD-10 design-ranker; III owns **L3 semantic residue** (coverage map §1). The pack complements AD-10's 6 design axes (DC/ACT/SQ/TC/CL/RU) — it does not re-derive them.

## §2 Trap derivation discipline

Each trap follows the `web_design`/`canvas_visual` field shape — **Description · Example · Where It Hides · Severity Default · Detection method** — and cites (a) a substrate anchor (skill directive / ADR / primitive), (b) an external grounding where one exists, (c) the empirical residue ID (gap ledger) it traps. A trap is *authorable* when it has a substrate anchor even if live multi-vault evidence is thin (the basis on which `web_design` Trap 6/7 landed).

## §3 Candidate traps (9 specified; ≥5 to land at DP-2)

### ISS-Trap 1 — Decision-Frame Ambiguity *(RECOMMEND LAND)*
- **Description:** The gate's ASK is not cleanly decidable — `composite_question` folds in analysis (violates C-DWC-3), options are under-differentiated (paradox of choice), or the decision's weight/consequence is unsignaled (`decision_significance` / `consequence_text` absent on a load-bearing gate).
- **Example:** A phase-exit gate whose question embeds the rationale tail; or a 3-option gate where two options differ only in wording (no real path difference).
- **Where it hides:** `composite_question`; option label sets; gates missing `decision_significance` on irreversible decisions.
- **Severity:** High (a mis-framed gate produces a mis-calibrated RLHF signal).
- **Detection:** Check `composite_question` for an embedded "here's why" tail (→ move to `analysis`); check option set for genuine path-difference; check load-bearing gates for `decision_significance` / `consequence_text`.
- **Anchors:** C-DWC-3, C-D4-1, D9 decision_significance · NN/G *Explicit Decisions* + *Simplicity vs Choice* · EGL-2/5.

### ISS-Trap 2 — Persona/Voice Drift *(RECOMMEND LAND — novel)*
- **Description:** Gate copy (composite_title, card titles, SWOT headers, rationale) drifts from the vault's character-brief voice while the *skin* stays correct. Skin = aesthetics; voice = authoring — orthogonal (AD-4; adaptation-guide §2.4 Hygieia-voice/partner-chrome split). A clinical-register sentence under a warm partner skin reads as incoherent to the audience.
- **Example:** A ZenZachary gate (techno-cyber-monk brief) whose copy reads "clinical assessment required"; correct partner colors, wrong voice.
- **Where it hides:** any prose-bearing JSON field; multi-author vaults; generator-authored default copy.
- **Severity:** Medium (High on partner/audience-facing gates).
- **Detection:** Compare gate prose vocabulary against the vault's `recommended_persona` character brief / CLAUDE.md voice; flag register mismatch.
- **Anchors:** AD-4, adaptation-guide §1.2/§2.2/§2.4 · C-DWC-1 anti-slop · EGL-4 (latent).

### ISS-Trap 3 — Evidence-Panel Buries-the-Lede *(RECOMMEND LAND)*
- **Description:** The recommendation/rationale is not surfaced above the supporting analysis — the operator must dig through SWOT/context to find the agent's call ("sludge"). Or `recommendation.rationale`+`confidence` absent where the decision benefits from auditable reasoning.
- **Example:** A gate with a rich SWOT 2×2 but no top-level `recommendation` block, or rationale buried below collapsed analysis.
- **Where it hides:** section ordering; gates using legacy `verdict` where structured `recommendation` is warranted; `recommendation_full_block_html` unused on a multi-option gate.
- **Severity:** Medium-High.
- **Detection:** Check that `recommendation.rationale`+`key_factors`+`confidence` are present and visually precede deep analysis; flag SWOT-only gates on decisions that warrant a stated call.
- **Anchors:** Recommendation-block discipline (skill L231-327), top-level rec (L321-327) · NN/G *Sludge* · EGL (latent).

### ISS-Trap 4 — RLHF Signal-Capture Incompleteness *(RECOMMEND LAND — strongest evidence)*
- **Description:** The gate output is valid per AD-7 schema but loses training signal: `rlhf_reviewer_persona` NULL though persona is known; `rlhf_modification_delta` absent on an `accept_with_modification`; scalar-only capture where pairwise (IM-B) would be more reliable; confidence-scale polarity unstated.
- **Example:** MoleculeForge `p6_1` — `rlhf_reviewer_persona:null`, `axis_scores:null`, no scale-semantics field.
- **Where it hides:** the `rlhf_*` block of `<gate_id>.output.json`; IM-C scalar gates that could be IM-B pairwise.
- **Severity:** High (directly degrades the "RLHF-useful" objective — the operator's anchor framing).
- **Detection:** Check output JSON: `rlhf_reviewer_persona` populated from top-level `persona`; `rlhf_modification_delta` present when `rlhf_signal_type:accept_with_modification`; flag scalar capture where a pairwise comparison was available; confirm scale-polarity legible.
- **Anchors:** ADR-005 §2 (required-min + optional-open), AD-7 DELTA-1/2/3 · Uni-RLHF (pairwise>scalar) + apxml · EGL-1/3/6.

### ISS-Trap 5 — Option-Set Bias / Leading Framing *(RECOMMEND LAND)*
- **Description:** The option set steers the operator — asymmetric framing, non-verb-first labels that obscure the action, paraphrase drift across fallback tiers (T1/T2/T3 option labels diverge), or a missing "need more info / defer" path that forces a false binary.
- **Example:** `approve/amend/discuss` with no defer path forcing "discuss" to absorb "need more time"; or Tier-2 AUQ labels paraphrased away from Tier-1.
- **Where it hides:** radio option sets; `composite_options`; the 3-tier fallback blocks.
- **Severity:** High ("a neutral presentation does not exist" — bias contaminates the signal).
- **Detection:** Check verb-first option labels (C-D5-1); check cross-tier label parity (C-D8 `_canonical_composite_options` no-paraphrase); check for a defer/escape path on non-trivial gates (`escape_hatch`).
- **Anchors:** C-DWC-2, C-D5-1, C-D8 no-paraphrase, D7-C65 duplicate-label · NN/G *Interface Copy Impacts Decision Making* · EGL-2.

### ISS-Trap 6 — Screen-Reader Semantics Under Glassmorphic Skin *(RECOMMEND LAND if a11y re-inspection confirms)*
- **Description:** Decorative glass layers (PRIM-12/14/15 decoration_svg/bg_grid/top_glow + backdrop-blur) are prioritized while keyboard-focus + SR semantics are left at defaults — confidence radios without `confidence_tooltips` SR-parity legend, focus-visible gaps, decoration not `aria-hidden`.
- **Example:** A glass-heavy gate where confidence radios lack the C-D9 SR legend and decorative SVG isn't hidden from the a11y tree.
- **Where it hides:** template `<style>` decoration; confidence-radio aria; focus-visible on custom interactive elements.
- **Severity:** High (excludes SR/keyboard users from the decision).
- **Detection:** Check `confidence_tooltips` enabled where confidence radios render; check decoration primitives carry `aria-hidden`; check every custom `:hover` has a `:focus-visible` sibling.
- **Anchors:** primitives anti-patterns L33-35, C-D9 confidence_tooltips, C-D6-1 no-hover-only · WCAG 2.2 (a11y-name, focus) · substrate-grounded; empirically-thin (needs G1 a11y pass).

### ISS-Trap 7 — Recommendation Confidence Under-Signaling *(RECOMMEND LAND)*
- **Description:** The agent's confidence is omitted, inflated, or its scale meaning is unsurfaced — operator can't calibrate trust; downstream RLHF can't weight the signal. (Distinct from Trap 4: this is the *confidence sub-signal specifically*; Trap 4 is whole-block completeness.)
- **Example:** A consequential gate with no `recommendation.confidence`; or `confidence:5` everywhere (no honest gradient); or no `confidence_tooltips` so "5" has no stated meaning.
- **Where it hides:** `recommendation.confidence`; multi-decision gates lacking confidence radios (EGL-5).
- **Severity:** Medium-High.
- **Detection:** Check `confidence` present + honest (not uniformly 5) on consequential gates; check scale meaning surfaced (`confidence_tooltips`); flag multi-decision gates with no confidence capture.
- **Anchors:** recommendation.confidence semantics (skill L270/L280), C-D9 confidence_tooltips · EGL-3/5.

### ISS-Trap 8 — Mobile Responsive Touch-Target Discipline *(LAND if a11y re-inspection confirms)*
- **Description:** New interactive elements added to a gate's template `<style>` don't mirror the C-D6-1 contract — sub-44px tap targets, 2-col grids without a ≤640px single-column companion, hover-only affordances, text containers without `overflow-wrap`.
- **Example:** A custom `.axis-row` 2-col grid with no `@media (max-width:640px)` single-col rule; a custom button under 44px at mobile.
- **Where it hides:** template-local `<style>` for vault-specific gate elements (primitives.css covers the standard set; template-local additions are the gap).
- **Severity:** Medium (excludes touch/mobile operators).
- **Detection:** Check template-local interactive elements for ≥44px tap target, single-column companion ≤640px, focus-visible siblings, overflow-wrap.
- **Anchors:** C-D6-1, AD-9 · WCAG 2.2 SC 2.5.8 (24px min; substrate ≥44px exceeds it) · substrate-grounded; empirically-thin.

### ISS-Trap 9 — Gate-Path Discipline Erosion *(DEFER candidate — operational, no erosion observed)*
- **Description:** The AD-6 4-file contract (`<gate_id>.{html,pending,output.json,draft.json}` + `archive/<gate_id>/`) + two-file sentinel discipline isn't honored — orphaned `.pending`, un-archived completed gates, slug-invalid `gate_id` under D8 flags.
- **Example:** A `.pending` left after `.output.json` lands; completed gates never moved to `archive/`.
- **Severity:** Low-Medium (operational hygiene, not decision quality).
- **Detection:** Check sentinel state coherence; check archive housekeeping; check `gate_id` slug-shape under D8.
- **Anchors:** AD-6, skill_watch_iss D7-C69 · no erosion observed (MoleculeForge archive intact) → **first-defer candidate** or land as a lighter operational trap.

## §4 Recommended landing set (DP-2 input)

**≥5 to land: Traps 1, 3, 4, 5, 7** (all live-evidenced + substrate + external). **Trap 2** (persona-voice) is the strongest *conceptual* novelty — recommend land as the signature trap if the voice-vs-skin check is authorable from CLAUDE.md briefs. **Traps 6 + 8** (a11y) land contingent on a G1 a11y re-inspection of a rendered gate. **Trap 9** is the first-defer (no erosion observed). A 5-trap floor lands {1,3,4,5,7}; a 7-trap target adds {2,6}; the full 9 is the ceiling.

## §5 Quality-metric target (ADR-007 §3)
New pack scored at G1 against the six-axis rubric. Target composite **≥4.0**: signal_density 5 (concrete detection cues), actionability 5 (each trap names a fix), coverage_uniformity 4 (even across the 9 axes), source_diversity 4 (decision-UX + a11y-normative + RLHF incl. academic + substrate — **5 reachable** with decision-science academic citations, DP-2 option), cross_topic_coherence 5 (coheres with web_design a11y + introspect calibration), graduation_yield 1 (new pack, no PACK_DELTA yet — measurement artifact per ADR-007 §3.6, not a content gap). Projected: (5+5+4+4+5+1)/6 = **4.00**.

## §6 Boundary assertion
Markdown trap definitions only. The pack *reads* gate output JSON + rendered HTML structure + wrapper config; it never renders, re-renders, or scores a gate via runtime. No ISS generator in III. No audit runtime. No new III ADR (ADR-009 ISS-Surface Artifact Class pre-disposed REJECTED — ISS is a consumed pattern from ADR-028/029, same precedent as `web_design` packing against an external artifact class without a III-side structural ADR).

## §7 Cross-references
`g0_iss_external_sota_dossier.md` (#1) · `g0_iss_internal_coverage_map.md` (#2) · `g0_iss_empirical_gap_ledger.md` (#3) · `g0_cross_vault_architecture_note.md` (#8). Substrate: `skill_create_iss.md`, `adr_028`, `adr_029`, `framework_orgvault_orggraph_guide.md`, `primitives/README.md`. Pattern precedent: `context_iii_domain_packs_web_design.md` (Trap field shape + F2 source_diversity discipline).
