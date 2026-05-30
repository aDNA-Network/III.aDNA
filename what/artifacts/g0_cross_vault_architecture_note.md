---
type: mission_artifact
artifact_class: architecture_note
title: "Campaign G G0 — Cross-vault architecture note: III ↔ SiteForge ISS boundary + sweep + graduation path (Operation Atrium)"
campaign: campaign_g_consolidation
mission: g0_charter_recon
mission_class: campaign_planning_execution
status: draft   # a PROPOSAL, not a ratified ADR
version: "0.1.0"
created: 2026-05-29
updated: 2026-05-29
last_edited_by: agent_argus
boundary_posture: boundary_preserving
evidence_method: inspection_grade
non_runnable: true
governed_by: [adr_002_consumer_federation_contract, adr_003_learning_store_ownership, adr_005_rlhf_signal_channel, adr_008_cross_vault_aggregation, adr_028_iss_architecture, adr_029_iss_standard_touch]
tags: [mission_artifact, campaign_g, operation_atrium, architecture_note, iss_boundary, wrapper_sweep, graduation_path, proposal, g0]
---

# G0 — Cross-Vault Architecture Note (proposal)

> **Deliverable #8 of 8** (card §5.8; paired with the 4 outbound coord drafts). States the **III ↔ SiteForge ISS boundary**, the **ADR-002 §3 sweep plan** for v0.4.1→v0.5.0, and the **ADR-003 graduation path** for any C-029 fire at G3. A **proposal**, not a ratified ADR (consumption-only; ADR-009 REJECTED).

## §1 III ↔ SiteForge ISS boundary statement

Three layers touch an ISS surface; III owns exactly one:

```
ISS surface (authored gate)
├─ L1  SiteForge what/lib/iss/        — library contract: generator, primitives, presets, skins, 288 pytest
│        (PREVENTS mechanical defects at generation time)
├─ L2  SiteForge AD-10 6-axis ranker  — DC/ACT/SQ/TC/CL/RU; ran during SiteForge's OWN D1-D10 build
│        (NOT run at consumer-authoring time; internal to substrate development)
└─ L3  III context_iii_iss_surfaces   — SEMANTIC review of the authored gate's residue   ◄── III owns
         (decision-frame · persona-voice · evidence-lede · RLHF-completeness · option-set ·
          SR-under-glass · confidence-signaling · touch-targets · gate-path hygiene)
```

**The boundary is the same shape as `web_design` ↔ SiteForge:** SiteForge runs/produces the mechanical layer (Lighthouse/axe = ISS library+ranker); III reads the contract surface and attributes the semantic residue. III **never** renders, re-renders, scores-via-runtime, or runs an audit. The `iss_surfaces` pack reviews the **gate output JSON (AD-7) + rendered HTML structure + wrapper config** on disk.

**Federation direction (mirror-image of `iii/`):** the ISS pack pins SiteForge's ISS-lib commit-SHA as its reviewed contract (R1 mitigation — snapshot at G0). A consumer vault then carries up to three orthogonal sibling wrappers:
- `<vault>/siteforge/` — public-site generation,
- `<vault>/iss/` — authors operator-decision gates (federation_ref → SiteForge),
- `<vault>/iii/` — review, whose `packs_used` can add `iss_surfaces` to review the gates the `iss/` wrapper authored.

This closes the loop: the same vault authors gates (`iss/`) and reviews their quality (`iii/` with `iss_surfaces`).

## §2 ADR-002 §3 minor-bump sweep plan (G4)

v0.4.1 → v0.5.0 fires the ADR-002 §3 minor-bump review at all minor-policy consumers. Expected sweep:

| Carrier | Adds `iss_surfaces`? | Adds `learning_store`-T3 re-pin? | Disposition |
|---|---|---|---|
| MoleculeForge `iss/` → does it gain an `iii/`? | candidate (it authors gates) | n/a | operator decides whether ISS adoption + `iii/iss_surfaces` is one decision or two (DP-2) |
| WilhelmAI `iss/` (+ future `iii/`) | candidate | n/a | CF→Future-Cross-Coupling-ISS-RLHF-III names WilhelmAI as most-likely first `iss/`+`iii/` vault |
| ZenZachary `iss/` | candidate (14 gates — richest) | n/a | strongest empirical adopter; recommend invite |
| Existing `iii/` carriers (web_design/learning_store users): VideoForge · CanvasForge · wga · SiteForge · LPWhitepaper | only if they add `iss_surfaces` | re-pin if they carry `learning_store` | additive minor bump backward-compat (F4 LPWhitepaper precedent — defer allowed) |
| lattice-labs/iii | — | — | TRACKED-DEFERRED (workspace genesis-first migration) |

**Key DP-2 question:** is "adopt ISS" (add `iss/`) and "review ISS quality" (add `iss_surfaces` to `iii/`) **one decision or two** for a vault? Recommend **two** — a vault can review its gates' quality via `iii/iss_surfaces` whether or not it has formally adopted `iss/`; and a vault with `iss/` need not immediately review. Expected total sweep: **3-7 wrapper pins** (matching F4's 4-carrier shape).

## §3 ADR-003 graduation path (G3 conditional)

If a consumer vault re-observes an ISS-quality residue from its **local** store and proposes upstream via an **ADR-008 `learning_store_graduation` cross_vault_request** by G3/DP-4:
1. Intake at `who/coordination/.aggregation/graduation_proposals_index.jsonl` (per `skill_aggregation_index_intake.md`).
2. ADR-003 §3.6 ≥2-vault evidence gate evaluated (the 3-vault ISS community can satisfy this once ≥2 carry the same residue).
3. IF gate met → graduation ceremony fires; **canonical jsonl md5 ROTATES** with full provenance in the DG-G scorecard (the sole non-invariant path in Campaign G).
4. IF NOT met → candidates-only continuation (canonical INVARIANT), per Campaign F precedent.

The C-029..C-033 candidates from Campaign F remain available for graduation at their own natural-frequency gate; Campaign G's T4 is the *ISS-residue* graduation path, conditional on organic proposal arrival.

## §4 Two acknowledged carry-forwards (NOT in scope, named for coherence)

- **CF→Framework-Pilot** (adaptation-guide §6): III's own first `iss/` wrapper would graduate the hypothetical III ADR-ratification gate (§1.4) to a live anchor. **Reviewing** ISS quality (Campaign G) ≠ **adopting** ISS (this CF). Out of scope; operator-discretionary future standalone.
- **CF→Future-Cross-Coupling-ISS-RLHF-III**: when a consumer wants ISS gate-output JSON to feed III ACCUMULATE as RLHF signal, the adapter lives at the consumer's wrapper level. WilhelmAI is the most-likely first instance (has `siteforge/`+`iss/`; `iii/` arrives with a learning-loop campaign). Directly serves the operator's "RLHF-useful" framing; Campaign G's RLHF-completeness trap (ISS-Trap 4) makes the *captured* signal richer, which is the precondition for this coupling.

## §5 Outbound coord plan (4 memos — drafts at §6; fire at G0 of the executing campaign)

| Recipient | Purpose | Draft |
|---|---|---|
| **SiteForge.aDNA** (Stanley persona) | ISS pattern-stability confirm + pin SHA + invite review of the ISS-pack draft; surface the 2 adaptation-guide-doc candidates (G-W1 decision_significance activation; G-W2 confidence-capture rule) | `draft_coord_2026_05_29_iii_to_siteforge_iss_pattern_stability.md` |
| **MoleculeForge.aDNA + WilhelmAI.aDNA + ZenZachary.aDNA** (multi-recipient) | §G5 validation invite + opt-in `iss_surfaces` `packs_used` consultation (one decision or two) + RLHF-completeness preview | `draft_coord_2026_05_29_iii_to_iss_adopters_validation_invite.md` |
| **LiteratureForge.aDNA** (Thoth) | Re-affirm Campaign E forward-seed stays gated on forge-BUILD; Campaign G does NOT subsume it | `draft_coord_2026_05_29_iii_to_literatureforge_campaign_e_reaffirm.md` |
| **VisualDNA.aDNA** (Pygmalion) | Acknowledge ISS as a parallel-but-distinct artifact class to visual bundles; bundle-review still track-deferred to v1.0 GA | `draft_coord_2026_05_29_iii_to_visualdna_parallel_artifact_class.md` |

## §6 Cross-references
Boundary substrate: `adr_028`/`adr_029`/`framework_orgvault_orggraph_guide.md`. Sweep: `adr_002_consumer_federation_contract.md` §3; F4 precedent (`f6_dg_f_scorecard.md` §4). Graduation: `adr_003_learning_store_ownership.md` §3/§3.6; `adr_008_cross_vault_aggregation.md`; `skill_aggregation_index_intake.md`. Sibling deliverables: `g0_iss_pack_revision_spec.md` (#4); `g0_audit_ingest_schema_note.md` (#6); charter `campaign_g_consolidation.md`.
