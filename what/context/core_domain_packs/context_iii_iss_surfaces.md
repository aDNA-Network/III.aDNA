---
type: context_guide
topic: iii_domain_packs
subtopic: iss_surfaces
created: 2026-05-29
updated: 2026-05-29
token_estimate: 2700
last_edited_by: agent_argus
tags: [context_guide, iii-review, traps, iss_surfaces, operator_decision_gates, rlhf, decision_ux, campaign_g, operation_atrium]
banner: "who/assets/banners/banner_context_guide.jpg"
icon: layout
fair:
  keywords: [iii-review, iss-surfaces, operator-decision-gates, decision-ux, rlhf, trap-library]
  license: Apache-2.0
boundary_posture: boundary_preserving   # Markdown trap definitions only — reviews the ISS contract surface on disk; NEVER renders, re-renders, or runs a gate; no ISS runtime in III; no audit runtime
authored_by_campaign: campaign_g_consolidation   # G1 (ANCHOR); spec at what/artifacts/g0_iss_pack_revision_spec.md
governed_by: [adr_028_iss_architecture, adr_029_iss_standard_touch, adr_005_rlhf_signal_channel, adr_007_adaptive_improvement_loop, adr_002_consumer_federation_contract]
reviews_contract_surface_at:
  siteforge_iss_snapshot: "903f461 / ^0.3.0"   # R1 pin (Campaign G); pack reviews this pinned ISS contract surface; SiteForge pattern-stability confirm pending (coord 2026-05-29)
  surfaces: ["gate output JSON (AD-7)", "rendered gate HTML structure", "iii/iss wrapper config"]
quality_metric:
  rubric_version: "adr_007_v0"
  scored_at: 2026-05-29
  scored_by: agent_argus
  scoring_mission: campaign_g_consolidation G1
  signal_density: 5
  actionability: 5
  coverage_uniformity: 4
  source_diversity: 4
  cross_topic_coherence: 5
  graduation_yield: 1
  composite: 4.00
  floor_check: triggered
  floor_axes: [graduation_yield]
  notes: "G1 landing score (new pack). Six traps {1,2,3,4,5,7} each carrying description / example / where it hides / severity default / detection method + Anchors line — signal_density + actionability hold at 5. coverage_uniformity 4 (even across the six landed axes; capped at 4 because two adjacent a11y axes — Trap 6 SR-under-glass + Trap 8 touch-targets — are held as candidates, not landed, after the G1 a11y re-inspection found the SiteForge primitives library covers the standard a11y surface on the live ZenZachary corpus; the residue is latent-only). source_diversity 4: corpus spans practitioner (NN/G decision-UX) + normative (WCAG 2.2) + academic (Uni-RLHF) + empirical (g0 gap ledger) + substrate (ADR-028/029, skill_create_iss); capped at 4 not 5 conservatively at first landing — 5 is defensible and reachable with a fuller decision-science academic dossier (parallel to web_design's wdr_external_sota_dossier; DP-2 option, deferred). cross_topic_coherence 5 (coheres with web_design a11y/Trap-3, canvas_visual, and introspect calibration). graduation_yield 1 = the known measurement artifact per ADR-007 §3.6 (new pack carries zero PACK_DELTA_LANDED ceremony records; floor trigger is the measurement artifact, NOT pack content — not a revision candidate). Projected (5+5+4+4+5+1)/6 = 24/6 = 4.00."
---

# III Domain Pack: ISS Surfaces (Operator-Decision-Gate Quality)

Domain-specific trap library for reviewing **ISS (operator-decision-gate) surfaces** — the structured HTML gates agents render to put a decision in front of an operator and capture the response as RLHF signal. Reviews the gate as **decision architecture + RLHF instrument**, the way the [[what/context/core_domain_packs/context_iii_domain_packs_web_design|web_design]] pack reviews a site as a web-standards practitioner.

> **Boundary (firm).** This pack is **Markdown trap definitions only**. It *reads* the ISS contract surface on disk — the gate output JSON (per ADR-028 AD-7), the rendered gate HTML structure, and the `iii/iss` wrapper config — and **never renders, re-renders, or runs a gate**. SiteForge owns the ISS **library/primitives (L1)** and its internal **AD-10 6-axis design ranker (L2)**, run only during substrate build. This pack owns the **L3 semantic residue**: operator-decision-gate quality at consumer-authoring time, which no library lint and no runtime ranker catches. No ISS runtime in III; no audit runtime; no new III ADR (ADR-009 "ISS-Surface Artifact Class" was evaluated and **REJECTED** — ISS is a *consumed* pattern from ADR-028/029, the same precedent on which `web_design` packs against an external artifact class with no III-side structural ADR).

## When to Load

Load alongside `core` when reviewing:
- An authored ISS gate's **output JSON** (`<gate_id>.output.json`) for RLHF-signal quality
- A **rendered gate** (`<gate_id>.html`) for decision-frame, option-set, evidence, and confidence quality
- A consumer vault's `iii/iss` (or `iss/`) **wrapper config** + the gate skeletons it authors
- Any vault standing up operator-decision gates (phase-exit, ADR-ratification, approval, structured-input) via `skill_create_iss.md`

The reviewer evaluates as an **operator-decision-gate quality reviewer**: is the ASK cleanly decidable, is the agent's call surfaced and honestly confidence-weighted, do the options present a neutral and complete choice, and does the captured signal survive as useful RLHF? It complements AD-10's six *design* axes (DC/ACT/SQ/TC/CL/RU) — it does not re-derive them.

## ISS-Surface Traps

### Trap 1: Decision-Frame Ambiguity

| Field | Value |
|-------|-------|
| **Description** | The gate's ASK is not cleanly decidable — `composite_question` folds analysis into the question (violates C-DWC-3), options are under-differentiated (paradox of choice), or the decision's weight is unsignaled (`decision_significance` / `consequence_text` absent on a load-bearing or irreversible gate) |
| **Example** | A phase-exit gate whose question embeds its own rationale tail ("Approve P3→P4 given the ledger work is de-risked and …" — the "given …" belongs in `analysis`); or a 3-option gate where two options differ only in wording, not in the path they commit to |
| **Where It Hides** | `composite_question`; option label sets; load-bearing/irreversible gates that omit `decision_significance` (`ROUTINE` / `LOAD_BEARING` / `IRREVERSIBLE`) and `consequence_text` |
| **Severity Default** | High (a mis-framed gate produces a mis-calibrated RLHF signal — the error propagates downstream) |

**Detection method**: Read `composite_question` for an embedded "here's why" tail — if present, the analysis should move to the `analysis` block and the question should state only the decidable ASK. Check the option set for genuine *path* difference (not paraphrase). Check load-bearing/irreversible gates for `decision_significance` + `consequence_text`; their absence on a consequential decision is the tell.

**Anchors**: C-DWC-3 (no analysis-in-question), C-D4-1, D9 `decision_significance` (`skill_create_iss.md`) · NN/G *Explicit Decisions* + *Simplicity vs. Choice* · gap ledger EGL-2/EGL-5.

### Trap 2: Persona / Voice Drift

| Field | Value |
|-------|-------|
| **Description** | Gate copy (composite_title, card titles, SWOT headers, rationale prose) drifts from the vault's character-brief voice while the *skin* stays correct. Skin = aesthetics; voice = authoring — orthogonal per AD-4 (adaptation-guide §2.4 Hygieia-voice / partner-chrome split). A clinical-register sentence under a warm partner skin reads as incoherent to the audience. The *layout/structural* analog of the web_design Template-Voice trap, applied to a decision surface |
| **Example** | A ZenZachary gate (techno-cyber-monk character brief) whose copy reads "clinical assessment required" — correct partner colors, wrong voice; or a generator-authored default title ("Phase Exit Decision") left un-voiced on a partner-facing gate |
| **Where It Hides** | Any prose-bearing JSON field; multi-author vaults; **generator-authored default copy** that ships un-revised (gates authored by `agent_siteforge_native` then skinned to a partner) |
| **Severity Default** | Medium (High on partner / audience-facing gates) |

**Detection method**: Compare the gate's prose vocabulary against the vault's `recommended_persona` character brief / CLAUDE.md voice. Flag register mismatch — clinical copy under a warm brief, generic template copy where the brief calls for a distinct voice. The skin being correct does **not** clear this; voice and skin are independent (AD-4).

**Anchors**: AD-4 (skin≠voice), adaptation-guide §1.2 / §2.2 / §2.4 · C-DWC-1 (anti-slop) · gap ledger EGL-4 (latent, 1-vault — the *signature conceptual* gap; weakest live evidence, strongest novelty).

### Trap 3: Evidence-Panel Buries-the-Lede

| Field | Value |
|-------|-------|
| **Description** | The recommendation / rationale is not surfaced *above* the supporting analysis — the operator must dig through SWOT or context to find the agent's call ("sludge"). Or `recommendation.rationale` + `confidence` are absent where the decision benefits from auditable reasoning |
| **Example** | A gate with a rich SWOT 2×2 but **no top-level `recommendation` block**; or a rationale buried below a collapsed analysis accordion; or a multi-option gate using the legacy `verdict` field where a structured `recommendation` block is warranted |
| **Where It Hides** | Section ordering; gates using legacy `verdict` where structured `recommendation` fits; `recommendation_full_block_html` left unused on a multi-option gate |
| **Severity Default** | Medium-High |

**Detection method**: Confirm `recommendation.rationale` + `key_factors` + `confidence` are present and visually **precede** deep analysis. Flag SWOT-only gates on decisions that warrant a stated call. The agent's recommendation is the lede; the analysis is support — order accordingly.

**Anchors**: recommendation-block discipline + top-level recommendation (`skill_create_iss.md` recommendation section) · NN/G *Sludge* (cost of effortful interactions) · gap ledger (latent empirically; substrate-strong).

### Trap 4: RLHF Signal-Capture Incompleteness

| Field | Value |
|-------|-------|
| **Description** | The gate output is valid per AD-7 schema but **loses training signal**: `rlhf_reviewer_persona` absent/NULL though the persona is known; `rlhf_modification_delta` missing on an `accept_with_modification`; scalar-only capture where a pairwise comparison (IM-B) would be more reliable; confidence-scale polarity unstated in the output. This is the trap with the **strongest live evidence** and it maps directly to the operator's anchor framing ("make the surfaces RLHF-rich") |
| **Example** | MoleculeForge `p6_1.output.json` — top-level `persona:"franklin"` is known, yet there is **no `rlhf_reviewer_persona` field**; `axis_scores:null` / `rlhf_signal.value.axes:null`; the 1-5 confidence direction is never stated in-output (`confidence_score:5 → rlhf_severity_calibration:1.0` is correct but undecodable downstream) |
| **Where It Hides** | The `rlhf_*` block of `<gate_id>.output.json`; IM-C scalar gates that could have been IM-B pairwise; multi-section gates where per-section signal is flattened |
| **Severity Default** | High (directly degrades the "RLHF-useful" objective — the reason these surfaces exist) |

**Detection method**: Read the output JSON. Confirm `rlhf_reviewer_persona` is populated from the top-level `persona`; confirm `rlhf_modification_delta` is present whenever `rlhf_signal_type:accept_with_modification`; flag scalar capture where a pairwise comparison was available; confirm the confidence-scale polarity is legible in-output (not only inferable from a calibration constant).

**Anchors**: ADR-005 §2 (required-min + optional-open RLHF fields), AD-7 DELTA-1/2/3 · Uni-RLHF (pairwise feedback > scalar reliability) · gap ledger EGL-1 / EGL-3 / EGL-6 (live, MoleculeForge `p6_1`).

### Trap 5: Option-Set Bias / Leading Framing

| Field | Value |
|-------|-------|
| **Description** | The option set steers the operator — asymmetric framing, non-verb-first labels that obscure the action, paraphrase drift across the 3-tier fallback (T1/T2/T3 labels diverge), or a missing "need more info / defer" path that forces a false binary. "A neutral presentation does not exist" — so a *biased* one actively contaminates the captured signal |
| **Example** | `approve / amend / discuss` with no defer path, forcing "discuss" to absorb "I need more time"; or a Tier-2 AUQ option paraphrased away from its Tier-1 wording so the same choice reads differently across renders |
| **Where It Hides** | Radio option sets; `composite_options`; the 3-tier fallback blocks (`_canonical_composite_options`) |
| **Severity Default** | High (bias in the option set propagates straight into the RLHF signal) |

**Detection method**: Check option labels are **verb-first** (C-D5-1) and name the action, not a mood. Check cross-tier label **parity** (C-D8 no-paraphrase — the canonical options must read identically across T1/T2/T3). Check for a defer / escape path (`escape_hatch`) on any non-trivial gate so the operator is never forced into a false binary.

**Anchors**: C-DWC-2, C-D5-1 (verb-first), C-D8 (no-paraphrase canonical options), D7-C65 (duplicate-label) · NN/G *The Impact of Interface Copy on Decision Making* · gap ledger EGL-2.

### Trap 7: Recommendation Confidence Under-Signaling

| Field | Value |
|-------|-------|
| **Description** | The agent's confidence is omitted, inflated, or its scale meaning is unsurfaced — the operator can't calibrate trust and downstream RLHF can't weight the signal. **Distinct from Trap 4**: this is the *confidence sub-signal specifically* (present? honest? scale legible?); Trap 4 is whole-block completeness |
| **Example** | A consequential gate with **no `recommendation.confidence`**; or `confidence:5` on every decision (no honest gradient); or a confidence radio rendered with a "1-5 scale (required)" legend but **no stated polarity** ("5 = highest conviction") and no `confidence_tooltips` — so "5" has no decodable meaning (observed on ZenZachary review gates + MoleculeForge `p6_1`) |
| **Where It Hides** | `recommendation.confidence`; the confidence-radio legend; multi-decision gates lacking confidence capture entirely (EGL-5); rank-only gates that drop confidence without signaling the omission is intentional |
| **Severity Default** | Medium-High |

**Detection method**: Confirm `confidence` is present and **honest** (not uniformly 5) on consequential gates. Confirm the scale **meaning** is surfaced — polarity stated and, where confidence radios render, a `confidence_tooltips` / SR-parity legend present. Flag multi-decision gates with no confidence capture, and rank-only gates whose confidence omission is adaptive-but-unsignaled.

**Anchors**: `recommendation.confidence` semantics (`skill_create_iss.md`), C-D9 `confidence_tooltips` · gap ledger EGL-3 / EGL-5 (live; reinforced by the G1 a11y re-inspection finding that the 1-5 scale renders without polarity semantics).

## Candidate Axes (not landed at G1 — held with honest evidence)

Three further axes from the G0 pack-revision spec (`g0_iss_pack_revision_spec.md` §3) are **on-record candidates**, not landed traps. They are substrate-grounded but empirically-thin; they graduate to full traps when live residue actualizes (the same candidate-discipline `f3` applied to the web_design C-032 motion-trap). DP-2 ratified the 6 landed traps {1,2,3,4,5,7} with 6/8 contingent on a G1 a11y re-inspection.

- **Candidate 6 — Screen-Reader Semantics Under Glassmorphic Skin.** Decorative glass layers prioritized while SR/keyboard semantics stay at defaults. **G1 a11y re-inspection (ZenZachary `p1_1r`/`p1_1a` rendered gates) did NOT confirm live residue**: the SiteForge primitives library covers the standard surface — `:focus-visible` siblings present (≥ `:hover` count), decoration `aria-hidden`, `prefers-reduced-motion` + `max-width:600px` responsive rules, `sr-only` legends present. The residue is **latent** — it actualizes only when a vault adds *template-local* custom interactive elements that bypass the primitives contract. (The one near-residue observed — confidence radios without polarity semantics — is owned by **Trap 7**, not here.) Anchors: primitives anti-patterns, C-D9, C-D6-1 · WCAG 2.2 (a11y-name, focus) · EGL "not yet observed."
- **Candidate 8 — Mobile Responsive Touch-Target Discipline.** Template-local interactive additions that don't mirror the C-D6-1 contract (sub-44px targets, 2-col grids without a ≤640px single-column companion, hover-only affordances). Same re-inspection result: the library-rendered surface honors ≥44px targets + a `max-width:600px` companion; residue is latent to template-local additions. Anchors: C-D6-1, AD-9 · WCAG 2.2 SC 2.5.8 (24px floor; substrate ≥44px exceeds it).
- **Candidate 9 — Gate-Path Discipline Erosion.** The AD-6 4-file contract + two-file sentinel discipline not honored (orphaned `.pending`, un-archived completed gates, slug-invalid `gate_id`). **No erosion observed** (MoleculeForge archive hygiene intact) → first-defer candidate or a future lighter "operational" trap. Anchors: AD-6, `skill_watch_iss` D7-C69.

## Reader Profile

ISS-surface review targets the operator-decision gates produced by `skill_create_iss.md` and the SiteForge `what/lib/iss/` library. The reviewer evaluates as an **operator-decision-gate quality reviewer** who cares about: cleanly decidable framing, a surfaced-and-honestly-confidence-weighted recommendation, a neutral and complete option set, voice coherence with the vault's character brief, and RLHF-signal completeness that survives downstream. The anti-slop doctrine applies to *decision copy* the way web_design applies it to marketing copy.

## Relationship to Other Checks

| Layer | Owner | What it validates |
|---|---|---|
| **L1 — library / primitives lint** | SiteForge `what/lib/iss/` (`validate_authoring`) | verb-first / duplicate-label / title-length; primitives a11y (focus-visible, aria-hidden, 44px, responsive) |
| **L2 — AD-10 6-axis design ranker** | SiteForge (run during substrate build only) | DC / ACT / SQ / TC / CL / RU design quality of the *library*, not the authored instance |
| **L3 — ISS-surface semantic residue** | **III + this pack** | decision-frame, voice, evidence-lede, RLHF-completeness, option-set bias, confidence — the consumer-authoring-time residue no lint/ranker catches |

This pack owns the **residue** the library and ranker provably miss — it **reads** the contract surface (output JSON + rendered HTML + wrapper config) on disk and attributes a quality defect to an authoring decision. The boundary is firm: **library + generator + AD-10 ranker stay in SiteForge; semantic attribution at authoring time is III's.** III never runs the ISS runtime.

## Sources & Citations

The grounding corpus for the traps above (the `source_diversity` axis). This is a **hybrid** pack (genuine external decision-UX + RLHF + a11y literature alongside the ISS substrate), unlike the reflexive `vault_maintenance` / `learning_store` packs — its citation ceiling is higher, scored conservatively at 4 at first landing (5 reachable with a fuller decision-science academic dossier; deferred per the web_design `wdr_external_sota_dossier` precedent).

### Internal contracts (ISS substrate — the pattern this pack reviews)
1. `aDNA.aDNA/how/skills/skill_create_iss.md` (D1–D10 authoring directives; C-DWC-1/2/3, C-D4-1, C-D5-1, C-D6-1, C-D8, C-D9, D9 `decision_significance`) — grounds Traps 1/2/3/5/7.
2. `aDNA.aDNA/what/decisions/adr_028_iss_architecture.md` (AD-1..AD-10; AD-6 file contract, AD-7 output schema, AD-9 responsive, AD-10 design ranker) — grounds Trap 4 + Candidates 8/9.
3. `aDNA.aDNA/what/decisions/adr_029_iss_standard_touch.md` (ST-1..ST-6) — pattern-touch boundary.
4. SiteForge `what/lib/iss/` (primitives / templates / tokens / skins / runtime / adaptation-guides; `framework_orgvault_orggraph_guide.md` III-as-consumer guide) — the L1/L2 layers this pack complements. **Pinned at `903f461` / `^0.3.0`** (R1; confirm pending coord 2026-05-29).

### Internal contracts (III canonical ADRs)
5. `what/decisions/adr_005_rlhf_signal_channel.md` §2 (required-min + optional-open RLHF fields) — grounds Trap 4.
6. `what/decisions/adr_007_adaptive_improvement_loop.md` §3 / §3.6 (six-axis quality rubric + `graduation_yield` measurement-artifact rule) — governs this pack's `quality_metric`.
7. `what/decisions/adr_002_consumer_federation_contract.md` §3 (minor-bump wrapper review that propagates this pack to ISS-adopter consumers at G4) · `adr_003_learning_store_ownership.md` §3 (graduation gate governing future ISS-trap corrections).

### External grounding
8. Nielsen Norman Group — decision UX: *Explicit Decisions*, *Simplicity vs. Choice* (Trap 1), *Sludge* (Trap 3), *The Impact of Interface Copy on Decision Making* (Trap 5). [practitioner]
9. WCAG 2.2 — accessible name / focus / SC 2.5.8 Target Size (Candidates 6/8). [normative]
10. Uni-RLHF and related RLHF-from-human-feedback literature — pairwise feedback reliability > scalar (Trap 4). [academic]

### Empirical residue (first-hand, on-disk)
11. `what/artifacts/g0_iss_empirical_gap_ledger.md` (EGL-1..6 across MoleculeForge `p6_1` output + ZenZachary 14 rendered gates + WilhelmAI config) — the live/latent residue each trap traps. Honest evidence-strength labels preserved (1-vault / latent at G1; multi-vault confirmation accrues as adopters collect outputs).

### Citation-ceiling rationale
`source_diversity = 4` at G1 landing: the corpus spans practitioner + normative + academic + empirical + substrate. Held at 4 (not 5) conservatively at first landing — the F2 / MX-1 discipline of scoring a freshly-landed pack one notch below its defensible ceiling pending a fuller dossier. A decision-science academic dossier (parallel to `wdr_external_sota_dossier.md`) would substantiate 5; deferred as a DP-2 option.

## Related

- [[how/skills/skill_iii_review|III Review Skill]] — the review procedure that consumes this pack
- [[what/context/core_domain_packs/context_iii_domain_packs_web_design|Web Design Pack]] — sibling pack; Trap 3 (semantic a11y) is the cross-reference for ISS Candidates 6/8
- [[what/context/core_domain_packs/context_iii_inspect_procedures|INSPECT Procedures]] — Five Traps + modality procedures
- `what/artifacts/g0_iss_pack_revision_spec.md` — the G0 anchor spec (9 candidate axes; this pack lands {1,2,3,4,5,7})
- `what/artifacts/g0_iss_empirical_gap_ledger.md` · `g0_iss_internal_coverage_map.md` · `g0_iss_external_sota_dossier.md` — G0 recon
- `aDNA.aDNA/how/skills/skill_create_iss.md` — the ISS authoring skill this pack reviews against
