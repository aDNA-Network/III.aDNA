---
type: mission_artifact
artifact_class: coverage_map
title: "Campaign G G0 — Internal lattice ISS-coverage map (Operation Atrium)"
campaign: campaign_g_consolidation
mission: g0_charter_recon
mission_class: campaign_planning_execution
status: draft
version: "0.1.0"
created: 2026-05-29
updated: 2026-05-29
last_edited_by: agent_argus
boundary_posture: boundary_preserving
evidence_method: inspection_grade
non_runnable: true
governed_by: [adr_002_consumer_federation_contract, adr_028_iss_architecture, adr_029_iss_standard_touch, adr_007_adaptive_improvement_loop]
tags: [mission_artifact, campaign_g, operation_atrium, iss_surfaces, coverage_map, reuse_before_invent, g0]
---

# G0 — Internal Lattice ISS-Coverage Map

> **Deliverable #2 of 8** (card §5.2). Reuse-before-invent: inventory what III's existing packs + the ISS substrate ALREADY cover, so the new `iss_surfaces` pack traps only the genuine **III-side residue** and duplicates nothing. Inspection-grade.

## §1 The three review layers that touch ISS surfaces

An ISS surface (a rendered operator-decision gate) is reviewed — or could be — by three distinct layers. The new III pack is the *third* and must not re-derive the first two.

| Layer | Owner | What it covers | Mechanism |
|---|---|---|---|
| **L1 — mechanical library contract** | SiteForge `what/lib/iss/` (generator, primitives, presets, skins, 288 pytest) | Round-trip correctness, single-file output, token discipline, preset flag-wiring, byte-stable render, `validate_authoring` lints (verb-first, duplicate-label, title length) | Runtime + tests; **prevents** violations at generation time |
| **L2 — SiteForge's own 6-axis ISS-design ranker** | SiteForge `campaign_siteforge_iss` P3.1 domain pack (ADR-028 **AD-10**: DC/ACT/SQ/TC/CL/RU) | Surface *design* quality during SiteForge's own D1-D10 build; ran at every decadal AAR; converged 4.00→4.95 | SiteForge-internal review during substrate development — **not** a III.aDNA pack, **not** run at consumer-authoring time |
| **L3 — III semantic review (THE GAP)** | *None today* → Campaign G `iss_surfaces` pack | Semantic/composition residue at **consumer-authoring time**: decision-frame, persona-voice drift, evidence-lede, RLHF-completeness, option-set bias, SR-under-glass, confidence-signaling, touch-targets, gate-path hygiene | Agentic III review of a consumer vault's authored gates |

**Boundary statement (load-bearing for the charter + architecture note):** L1 prevents mechanical defects; L2 was SiteForge's internal design-quality gate *while building the substrate* (it does not run when MoleculeForge/WilhelmAI/ZenZachary author a gate); L3 is the consumer-facing semantic layer III owns. The `iss_surfaces` pack must **complement, not duplicate, AD-10's 6 axes** — it traps the semantic residue an authored gate exhibits, not the substrate's design coherence.

## §2 III adjacent-pack adjacency (what NOT to duplicate)

| III pack | Composite | Artifact class it reviews | Relation to `iss_surfaces` |
|---|---|---|---|
| `context_iii_canvas_visual.md` | 4.00 | **Rendered pixels** — decks, comics, canvas HTML renders, Playwright screenshots | **Distinct.** Reviews static rendered output as an image. ISS surfaces are *interactive decision* surfaces — reviewed as decision architecture + captured JSON, not as pixels. A future `visualdna_bundles` pack is the pixel-of-a-bundle analog; `iss_surfaces` is the interaction analog. No overlap. |
| `context_iii_domain_packs_web_design.md` | 4.17 | **Marketing/product websites** (Astro/CSS/schema) — hierarchy, voice, a11y semantics, token drift, CWV-attribution, AI-slop | **Adjacent but distinct surface class.** Shares the *a11y-semantics* and *token-discipline* vocabulary, but a marketing page ≠ an operator-decision gate. Trap 3 (semantic a11y / SC 2.5.8) and Trap 5 (token drift) are the closest neighbors — the ISS SR-under-glass + touch-target traps are gate-specialized cousins, not copies. |
| `context_iii_introspect_checks.md` | 4.50 | **Structural reasoning** — 7 calibration checks (confidence gradient, analogy-vs-argument, structural completeness, meta-patterns, argumentation, denominator, corrections-match) | **Applies structurally, cross-pack.** INTROSPECT runs *on top of* any INSPECT pass, including an ISS review. The confidence-gradient check (2a) and argumentation-structure check (2e) are natural calibrators for the ISS decision-frame + confidence-signaling traps. Reused, not duplicated. |
| `context_iii_learning_store.md` | 3.83 | The corrections substrate + RLHF schema (ADR-005 fields) | **Schema source, not a reviewer.** Its `rlhf_*` field documentation (L88-95) is the *contract* the RLHF-completeness trap checks an ISS gate output against. T3 deepens this pack in parallel (deliverable #5). |
| `context_iii_vault_maintenance.md` | 3.33 | Vault staleness/cascade discipline (reflexive) | **Distinct.** Operational hygiene of the vault, not gate quality. The MX-1 §Sources recipe (deliverable #5) is the citation pattern T3 replays — methodological reuse only. |

## §3 What's library-only (no semantic reviewer) — the residue surface

The substrate **enforces** (L1) a large contract: provenance footer, first-timer hint, post-submit explainer, color semantics, persona cascade, confidence-dot rendering *when present*, recommendation-block rendering *when data present*, verb-first/duplicate-label lints. It does **not** judge whether the authored *content* is good:

- Whether the **decision frame** is genuinely decidable (options differentiated; question separate from analysis; consequence/significance signaled).
- Whether the **copy voice** matches the vault's character brief (skin = aesthetics; voice = authoring — orthogonal, per adaptation-guide §2.4).
- Whether the **evidence panel** surfaces the lede or buries it.
- Whether **RLHF capture** is complete for the training loop (reviewer-persona, modification-delta, scale-polarity).
- Whether the **option set** is leading / asymmetric.
- Whether **decoration-under-glass** crowded out SR semantics / focus-visible.
- Whether **confidence** is honestly captured + its scale meaning surfaced.
- Whether **new interactive elements** added to a gate's template mirror the ≥44px / single-column / no-hover-only contract.
- Whether the **gate-path 4-file discipline** (AD-6) + sentinel/archive hygiene held.

This is exactly L3 — the `iss_surfaces` pack target.

## §4 Empirical adopter landscape (corrected against the filesystem)

The planning card's "definitive 2 live wrappers" **undercounts**, and ADR-029 ST-3's "3 live (incl. RareHarness)" is **stale**. Verified by directory inspection 2026-05-29:

| Wrapper | State | Pin | Persona / preset | Gates on disk |
|---|---|---|---|---|
| `MoleculeForge.aDNA/iss/` | **live** | SiteForge `903f461` (D10) | franklin / gate-by-gate | 1 completed `.output.json` (p6_1 phase_exit) + ~9 archived/demo |
| `WilhelmAI.aDNA/iss/` | **live** | `903f461` (D10) | partner / `partner_onboarding` | 0 (config only) |
| `ZenZachary.aDNA/iss/` | **live (richest)** | SiteForge P5.M3 `^0.3.0` | partner / `partner_onboarding` | **14 rendered `.html`** (P1 brand-genesis; 0 `.output.json` yet) |
| `RareHarnessOld.aDNA/iss/` | **legacy** | (P4.M1) | franklin clinician / `clinician_evaluation` | the orphaned P4.M1 "RareHarness" wrapper — vault renamed 2026-05-25 |
| `RareHarness.aDNA` (current/Neo) | **none** | — | — | no `iss/` directory (provisioning pending) |

**Charter implication (DP-2):** the card scopes the empirical gap ledger to MoleculeForge + WilhelmAI. But **ZenZachary is where the rendered-gate residue actually lives** (14 gates vs MoleculeForge's 1). Recommend the §G5 validation + §O3 gap-ledger treat **ZenZachary as a primary source** (rendered-gate residue) and **MoleculeForge as the output-JSON exemplar** (only completed `.output.json`). WilhelmAI stays a config exemplar. This is surfaced for operator ratification — it's a scope refinement, not a boundary change.

## §5 Conclusion

The `iss_surfaces` pack occupies **L3** — a genuinely empty slot. It duplicates neither SiteForge's L1 library contract nor AD-10's L2 design-ranker nor any of III's 5 existing packs. Its closest neighbors (`web_design` Trap 3/5, `introspect_checks` 2a/2e) are reused as adjacencies, not copied. The residue surface in §3 is real and unowned.

## §6 Cross-references

- `g0_iss_external_sota_dossier.md` (#1) · `g0_iss_empirical_gap_ledger.md` (#3) · `g0_iss_pack_revision_spec.md` (#4).
- `adr_028_iss_architecture.md` AD-10 (6-axis ranker) · `adr_029_iss_standard_touch.md` ST-3 (wrapper contract; the stale RareHarness row) · `adr_002_consumer_federation_contract.md` (`packs_used` adoption).
- III packs: `context_iii_canvas_visual.md` · `context_iii_domain_packs_web_design.md` · `context_iii_introspect_checks.md` · `context_iii_learning_store.md` · `context_iii_vault_maintenance.md`.
