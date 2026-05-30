---
type: mission_artifact
artifact_class: empirical_gap_ledger
title: "Campaign G G0 — Empirical ISS-surface gap ledger across live adopters (Operation Atrium)"
campaign: campaign_g_consolidation
mission: g0_charter_recon
mission_class: campaign_planning_execution
status: draft
version: "0.1.0"
created: 2026-05-29
updated: 2026-05-29
last_edited_by: agent_argus
boundary_posture: boundary_preserving
evidence_method: inspection_grade   # read saved gate outputs + rendered HTML + wrapper configs on disk; NO live gate runs
non_runnable: true
adopters_inspected: [MoleculeForge.aDNA, WilhelmAI.aDNA, ZenZachary.aDNA]
classification: [library_enforced, adaptation_guide_warned, neither_caught]
tags: [mission_artifact, campaign_g, operation_atrium, iss_surfaces, gap_ledger, empirical, g0]
---

# G0 — Empirical ISS-Surface Gap Ledger

> **Deliverable #3 of 8** (card §5.3). Inspection-grade review of live ISS adopters' rendered surfaces + saved gate outputs. Each observed residue is classified **library-enforced** (L1 prevents it) / **adaptation-guide-warned** (a guide or preset would flag it) / **neither-caught** (the L3 residue — the `iss_surfaces` pack target). Evidence-discipline mirrors `f3_correction_candidates.md`: **honest strength labels** (live / latent / 1-vault); no claim inflated. Findings are *candidate observations*, not graduated corrections — the canonical jsonl is INVARIANT.

## §1 Method + corpus

Read on disk (no runs): the 3 live wrapper `iss/CLAUDE.md` configs; MoleculeForge's `how/gates/*.output.json` (1 completed: `p6_1`); ZenZachary's 14 rendered `how/gates/*.html` (sampled 3 of the richest); WilhelmAI (config-only, 0 gates). **ZenZachary is the richest rendered-gate corpus** (correcting the card's MoleculeForge+WilhelmAI scope — see coverage-map §4). Findings drawn primarily from an Explore-agent pass that read CSS-comment + populated-field evidence; claims below carry the strength at which they were observed.

## §2 Per-adopter snapshot

| Adopter | Pin | Persona/preset | Completed outputs | Rendered gates |
|---|---|---|---|---|
| MoleculeForge | `903f461` | franklin / gate-by-gate | **1** (`p6_1` phase_exit, IM-B) | + ~9 archived/demo |
| ZenZachary | `^0.3.0` | partner / `partner_onboarding` | 0 | **14** (P1 brand-genesis) |
| WilhelmAI | `903f461` | partner / `partner_onboarding` | 0 | 0 |

## §3 Residue ledger (classified)

### L1 — library-enforced (NOT trap targets; substrate already prevents)
Provenance footer, first-timer hint, post-submit explainer (all preset-wired on partner gates); decision-outcome color semantics; persona cascade; confidence-dot rendering *when present*; recommendation-block rendering *when `data.recommendation` present*; verb-first / duplicate-label / title-length `validate_authoring` lints. **No trap needed.**

### L2 — adaptation-guide-warned (candidate guide-doc fixes, NOT III traps)
- **G-W1 — decision_significance activation undocumented.** The badge is opt-in; no adaptation-guide passage tells a vault author *when* to set `ROUTINE | LOAD_BEARING | IRREVERSIBLE`. → SiteForge adaptation-guide candidate (coord memo to SiteForge), not a III trap.
- **G-W2 — confidence-capture design rule implicit.** No guide states "rank-only gates skip confidence; multi-decision gates require it." → adaptation-guide candidate.

### L3 — neither-caught (THE TRAP TARGETS)

| ID | Residue | Live evidence | Strength | Maps to axis |
|---|---|---|---|---|
| **EGL-1** | **RLHF capture incomplete** — `rlhf_reviewer_persona` NULL though top-level `persona:"franklin"` known; `rlhf_modification_delta` present but `axis_scores` null; no in-output confidence-scale polarity field | MoleculeForge `p6_1` output.json | **live, 1-vault** | #4 RLHF-completeness |
| **EGL-2** | **Semantic outcome labels unexplained** — `approve / amend / discuss` rendered without prose decoding for a first-timer; post-submit explainer covers round-trip mechanics, not option *meaning* | ZenZachary `p1_1r`, `p1_1a` | **live, 1-vault (partner-facing)** | #1 decision-frame ambiguity / #5 option-set |
| **EGL-3** | **Confidence-scale polarity not surfaced** — `confidence_score:5 → rlhf_severity_calibration:1.0` correct, but the 1-5 direction ("5 = highest conviction") never stated in gate or output; downstream RLHF could invert | MoleculeForge `p6_1`; ZenZachary review gates | **live, 1-vault + latent** | #7 confidence under-signaling |
| **EGL-4** | **Persona-voice drift risk** — gate copy authored by `agent_siteforge_native` (generator voice), skinned partner; nothing checks the copy echoes the vault's character brief (ZenZachary techno-cyber-monk) vs a generic/clinical register | ZenZachary (latent; not yet actualized in sample) | **latent, 1-vault** | #2 persona/voice drift |
| **EGL-5** | **Confidence omitted on simpler gates** — rank-only gate (`p1_1a`) carries no confidence radios; multi-section gate (`p1_1r`) does; adaptive but unsignaled | ZenZachary | **live, 1-vault** | #7 confidence / #1 frame |
| **EGL-6** | **Axis-ranker optionality unsignaled** — `axis_scores` nullable; gate doesn't tell the operator whether 6-axis scoring is required or bonus | ZenZachary `p1_1r` | **live, 1-vault** | #4 RLHF / #1 frame |

**Not yet observed (honest absence):** SR-under-glass (#6) and touch-target (#8) residues were *not* directly caught in this pass — they are substrate-evidenced (primitives anti-patterns L33-35; C-D6-1) but need a focused a11y re-inspection of a rendered gate at G1/G3. Gate-path discipline (#9) showed no erosion (MoleculeForge archive hygiene intact). These are carried as **substrate-grounded but empirically-thin** — exactly the candidate-bar honesty `f3` applied (cf. C-032 motion-trap which stayed a non-landed candidate on thin evidence).

## §4 Evidence-discipline note

Every L3 row is **1-vault or latent** at this pass — none has the ≥2-vault evidence the candidate bar wants (cf. `f3` C-030..C-033). That is *expected* at planning recon: the ISS-adopter community is 3 vaults, only 1 with completed outputs. The traps these motivate are **substrate-grounded** (each cites a real directive/ADR/anti-pattern), so they are authorable from the contract surface even where live multi-vault evidence is thin — the same basis on which `web_design` Trap 6/7 landed from SiteForge-contract + single-site evidence. The empirical ledger *prioritizes* the trap set; the *substrate* justifies each trap. Multi-vault confirmation accrues as ZenZachary's 14 gates collect `.output.json` responses and MoleculeForge/WilhelmAI add gates.

## §5 Prioritization for the pack-revision spec (#4)

Rank by (live-evidence × substrate-strength × external-grounding):
1. **#4 RLHF-completeness** (EGL-1/3/6; live; ADR-005+AD-7; Uni-RLHF) — strongest.
2. **#5 option-set bias / #1 decision-frame** (EGL-2/5; live; NN/G + C-DWC-2/3).
3. **#7 confidence under-signaling** (EGL-3/5; live; C-D9 confidence_tooltips).
4. **#3 evidence-lede** (substrate + NN/G sludge; latent empirically).
5. **#6 SR-under-glass / #8 touch-targets** (substrate-strong, empirically-thin — land if a11y re-inspection confirms at G1).
6. **#2 persona/voice-drift** (latent; novel — strongest *conceptual* gap, weakest live evidence).
7. **#9 gate-path hygiene** (no erosion observed — candidate for a lighter "operational" trap or defer).

→ **≥5 recommended to land: #4, #5/#1, #7, #3, #6.** Final selection at DP-2.

## §6 Cross-references
`g0_iss_external_sota_dossier.md` (#1) · `g0_iss_internal_coverage_map.md` (#2 — adopter landscape §4) · `g0_iss_pack_revision_spec.md` (#4). Adopter configs: `MoleculeForge.aDNA/iss/CLAUDE.md` · `WilhelmAI.aDNA/iss/CLAUDE.md` · `ZenZachary.aDNA/iss/CLAUDE.md`. Evidence-discipline precedent: `what/artifacts/f3_correction_candidates.md`.
