---
type: mission_artifact
artifact_class: external_sota_dossier
title: "Campaign G G0 — ISS-surface external SOTA + canonical-substrate research dossier (Operation Atrium)"
campaign: campaign_g_consolidation
mission: g0_charter_recon
mission_class: campaign_planning_execution
status: draft
version: "0.1.0"
created: 2026-05-29
updated: 2026-05-29
last_edited_by: agent_argus
boundary_posture: boundary_preserving   # frames external UX/RLHF SOTA for III's SEMANTIC review layer; no ISS runtime, no audit runtime
evidence_method: inspection_grade
governed_by: [adr_028_iss_architecture, adr_029_iss_standard_touch, adr_005_rlhf_signal_channel, adr_007_adaptive_improvement_loop]
non_runnable: true
source_diversity_target: 4   # principled-4 ceiling (matches web_design F2); 5 reachable if academic decision-science added at DP-2
tags: [mission_artifact, campaign_g, operation_atrium, iss_surfaces, external_sota, decision_ux, rlhf_capture, accessibility, g0]
---

# G0 — ISS-Surface External SOTA + Canonical-Substrate Dossier

> **Deliverable #1 of 8** (card §5.1). Frames modern operator-decision-gate / interactive-form / RLHF-capture UX best practices **for III's semantic review layer** — what a III ISS-surface pack should *trap*, grounded in both external SOTA and the canonical ISS substrate. Inspection-grade: no live runs. This is a *proposal input* to the Campaign G charter, not a ratified pack.

## §1 Purpose & framing

The operator's anchor framing (2026-05-28): *"III support … for the ISS system so the interaction surfaces our agents share with users are always as high quality / well composed / rich / useful for RLHF as possible."* III today has **zero ISS coverage** — no pack reviews operator-decision-gate quality. The ISS *mechanical* layer (SiteForge library + `skill_create_iss` + ADR-028/029) is mature and enforces a large contract; what's missing is a **semantic review pack** that catches the residue the mechanical contract cannot — the same division of labor `web_design` has with SiteForge's Lighthouse/axe gates.

This dossier maps external SOTA in three domains — **decision UX**, **accessibility**, **RLHF capture** — onto the canonical substrate, so each candidate trap has both an external grounding and a substrate anchor.

## §2 Decision-UX SOTA → III semantic traps

| External finding | Source | Maps to substrate | Candidate trap |
|---|---|---|---|
| **"A neutral presentation of options does not exist"** — option framing/order steers choice; buried key-differences cause wrong selection | NN/G *Design Principles to Support Better Decision Making*; *Interface Copy Impacts Decision Making* | `skill_create_iss` C-DWC-2 (verdict pattern), C-D5-1 (verb-first option labels), C-D8 `composite_options` no-paraphrase | **Option-set bias / leading framing** |
| **Paradox of choice** — fewer, well-differentiated options ease decisions; abundance degrades completion | NN/G *Simplicity Wins over Abundance of Choice* | IM-A/B/C taxonomy (binary/pairwise/scalar); SWOT 2×2 forcing explicit positions | **Decision-frame ambiguity** (too many under-differentiated options; question conflated with analysis — C-DWC-3) |
| **Sludge** — friction/opacity in decision workflows degrades outcomes; surface the basis for the decision | NN/G *Clean the Sludge from Decision-Making Workflows* | Recommendation block (rationale/key_factors/risks/confidence/next_actions); evidence panel layering | **Evidence-panel buries-the-lede** (recommendation/rationale not surfaced above analysis) |
| **Explicit decisions** — name the action the operator authorizes; make consequences legible before commit | NN/G *Explicit Decisions* | C-D4-1 `consequence_text`; D9 `decision_significance` badge + `show_post_submit_explainer` | **Decision-frame ambiguity** (consequence/significance unsignaled) |

## §3 Accessibility SOTA → III semantic traps

| External finding | Source | Maps to substrate | Candidate trap |
|---|---|---|---|
| **WCAG 2.2 SC 2.5.8 Target Size (Minimum)** — interactive targets ≥ 24×24 CSS px or sufficient spacing; dexterity/touch users mis-activate small targets | [W3C WAI Understanding SC 2.5.8](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html) | `skill_create_iss` C-D6-1 (substrate uses ≥44px — the stricter 2.5.5 Enhanced; exceeds the 24px minimum); `@media (max-width:600px)` tap-target block | **Mobile responsive touch-target discipline** (new interactive elements added to a gate's template `<style>` that don't mirror the ≥44px / single-column / no-hover-only contract) |
| **Glassmorphism + decoration vs semantics** — decorative blur/glow layers can crowd out keyboard-focus + SR semantics; confidence radios need SR-parity legends | SiteForge `primitives/README.md` anti-patterns L33-35 ("No keyboard-focus styling beyond defaults"; backdrop-blur fixed); C-D9 `confidence_tooltips` (closes the sighted/SR confidence-radio gap) | PRIM-12/14/15 (decoration_svg / bg_grid / top_glow) | **Screen-reader semantics under glassmorphic skin** (decoration prioritized; focus-visible / aria-label / SR-parity residue) |

## §4 RLHF-capture SOTA → III semantic traps

| External finding | Source | Maps to substrate | Candidate trap |
|---|---|---|---|
| **Humans excel at relative judgments** — pairwise comparison is more reliable for reward-model training than absolute scalar scoring | [Uni-RLHF (arXiv 2402.02423)](https://arxiv.org/pdf/2402.02423); [apxml — Human Preference Data Collection](https://apxml.com/courses/rlhf-reinforcement-learning-human-feedback/chapter-3-reward-modeling-human-preferences/preference-data-collection) | IM-B pairwise mode; `rlhf_signal.signal_shape` (binary/pairwise/scalar/correction/structured) per ADR-028 AD-7 / ADR-005 | **RLHF signal-capture incompleteness** (scalar-only capture where pairwise would yield a stronger training signal) |
| **"Without explicit guidance on what 'better' means, annotators make inconsistent judgments"** — capture the criterion + the modification rationale, not just the disposition | apxml; [AWS — What is RLHF](https://aws.amazon.com/what-is/reinforcement-learning-from-human-feedback/) | ADR-005 §2 required-min (`rlhf_signal_type`, `rlhf_session_id`, `rlhf_captured_at`) + optional-open (`rlhf_modification_delta`, `rlhf_reviewer_persona`, `rlhf_severity_calibration`, `rlhf_cross_session_link`); ADR-028 AD-7 DELTA-3 | **RLHF signal-capture incompleteness** (e.g. `rlhf_reviewer_persona` / `rlhf_modification_delta` absent → downstream loses *which voice* + *why-modified*; confidence-scale polarity unstated) |
| **Minimize annotator cognitive load; user-friendly interface** | apxml; Uni-RLHF | C-D9 onboarding dual-audience (`first_timer_hint`, `confidence_tooltips`, `show_post_submit_explainer`) | **Confidence under-signaling** (no confidence captured / scale meaning not surfaced) |

## §5 Synthesis — 9 candidate axes (each external-grounded + substrate-anchored)

1. **Decision-frame ambiguity** — §2 (NN/G explicit decisions + paradox of choice) × C-DWC-3 / D9 decision_significance.
2. **Persona/voice drift** — voice register vs rendering skin are orthogonal (ADR-028 AD-4; adaptation-guide §2.4 Hygieia-voice / partner-chrome split) — copy can drift from the vault's character brief while the skin stays correct. *(Internal-grounded; external analog: brand-voice consistency.)*
3. **Evidence-panel buries-the-lede** — §2 (NN/G sludge) × SWOT/recommendation stacking discipline.
4. **RLHF signal-capture incompleteness** — §4 × ADR-005/AD-7.
5. **Option-set bias / leading framing** — §2 (NN/G neutral-presentation-myth) × C-DWC-2 / C-D8 no-paraphrase.
6. **Screen-reader semantics under glassmorphic skin** — §3 × primitives anti-patterns / C-D9 confidence_tooltips.
7. **Recommendation confidence under-signaling** — §4 (honest 1-5; scale meaning) × recommendation.confidence / C-D9 confidence_tooltips.
8. **Mobile responsive touch-target discipline** — §3 (WCAG 2.5.8) × C-D6-1 / AD-9.
9. **Gate-path discipline erosion** — AD-6 4-file contract (`<vault>/how/gates/<id>.{html,pending,output.json,draft.json}` + archive) + `skill_watch_iss` sentinel discipline. *(Internal-grounded; the operational-hygiene axis.)*

**Recommendation to charter (DP-2):** land **≥5** of these as net-new traps at G1. The strongest-evidenced (live-residue + external + substrate) cluster: **#4 RLHF-completeness, #5 option-set bias, #3 evidence-lede, #6 SR-under-glass, #7 confidence-signaling** — see the empirical gap ledger (deliverable #3) for which actually fired on live gates.

## §6 Source-diversity note

Three external clusters (decision-UX / accessibility-normative / RLHF — incl. one academic, Uni-RLHF) + the internal canonical substrate (skill + 2 ADRs + adaptation guide). This supports `source_diversity = 4` at parity with `web_design` (F2). Unlike the reflexive packs (`vault_maintenance`, `learning_store`) capped at 4, the ISS pack reviews an **external artifact class** and *could* reach **5** if peer-reviewed decision-science / choice-architecture literature is added — flagged as a DP-2/G1 option, not assumed.

## §7 Cross-references

- Canonical substrate: `aDNA.aDNA/how/skills/skill_create_iss.md` (D1–D10 directives); `adr_028_iss_architecture.md` (AD-4/6/7/9/10); `adr_029_iss_standard_touch.md` (ST-1..6); `SiteForge.aDNA/what/lib/iss/adaptation_guides/framework_orgvault_orggraph_guide.md`; `primitives/README.md`.
- III contracts: `adr_005_rlhf_signal_channel.md`; `adr_007_adaptive_improvement_loop.md` §3.
- Sibling deliverables: `g0_iss_internal_coverage_map.md` (#2); `g0_iss_empirical_gap_ledger.md` (#3); `g0_iss_pack_revision_spec.md` (#4).
- External: NN/G [simplicity-vs-choice](https://www.nngroup.com/articles/simplicity-vs-choice/) · [design-principles](https://www.nngroup.com/articles/design-principles/) · [interface-copy-decision-making](https://www.nngroup.com/articles/interface-copy-decision-making/) · [sludge-decisions](https://www.nngroup.com/articles/sludge-decisions/) · [explicit-decisions](https://www.nngroup.com/videos/explicit-decisions/); [W3C WAI SC 2.5.8](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html); [Uni-RLHF arXiv 2402.02423](https://arxiv.org/pdf/2402.02423); [apxml preference-data-collection](https://apxml.com/courses/rlhf-reinforcement-learning-human-feedback/chapter-3-reward-modeling-human-preferences/preference-data-collection); [AWS RLHF](https://aws.amazon.com/what-is/reinforcement-learning-from-human-feedback/).
