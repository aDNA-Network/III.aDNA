---
type: mission_artifact
artifact_class: pack_revision_spec
title: "Campaign G G0 — learning_store pack-revision spec (T3, MX-1 recipe replay) (Operation Atrium)"
campaign: campaign_g_consolidation
mission: g0_charter_recon
mission_class: campaign_planning_execution
status: draft
version: "0.1.0"
created: 2026-05-29
updated: 2026-05-29
last_edited_by: agent_argus
boundary_posture: boundary_preserving   # citation-hardening only; no trap redefinition; canonical jsonl INVARIANT
evidence_method: inspection_grade
non_runnable: true
target_pack: what/context/core_domain_packs/context_iii_learning_store.md
landing_target: G2
governed_by: [adr_007_adaptive_improvement_loop, adr_003_learning_store_ownership, adr_005_rlhf_signal_channel, adr_008_cross_vault_aggregation]
tags: [mission_artifact, campaign_g, operation_atrium, learning_store, pack_revision_spec, t3, citation_hardening, rebaseline, g0]
---

# G0 — learning_store Pack-Revision Spec (T3)

> **Deliverable #5 of 8** (card §5.5). MX-1 citation-recipe replay onto `context_iii_learning_store.md`. **Spec only** — the pack edit lands at **G2** after charter approval. **Carries a corrected re-baseline** that materially right-sizes T3.

## §1 Corrected baseline (THE finding — flag to operator + charter)

The planning card + prior-session AAR carried T3's premise as **"composite 3.50 / source_diversity 2, the next-weakest pack."** Direct inspection of the pack frontmatter (2026-05-29) shows otherwise:

| Axis | Card's claimed value | **Actual frontmatter value** |
|---|---|---|
| signal_density | — | 4 |
| actionability | — | 4 |
| coverage_uniformity | — | 4 |
| **source_diversity** | **2** | **3** |
| cross_topic_coherence | — | 5 |
| graduation_yield | — | 3 |
| **composite** | **3.50** | **3.83** |
| floor_check | (implied fail) | **passed** |

The "3.50 / sd 2" appears to be a **conflation of `vault_maintenance`'s pre-MX-1 numbers** (3.00 / sd 2, the pack MX-1 actually deepened). `learning_store` is **not the weakest pack** (post-MX-1 `vault_maintenance` is 3.33; `learning_store` is mid-pack at 3.83), and its `source_diversity` **already passes the floor at 3**. This is exactly the empirical-drift the prior-session AAR Change (b) warned about — surfaced, not silently carried.

## §2 Re-scoped T3 value proposition

T3 is therefore **not floor-remediation** (no floor breach exists). Its real, smaller-but-genuine value: `learning_store` cites its sources **inline/embedded** (~21 cross-refs scattered through the body) with **no structured `§Sources & Citations` section** — unlike `vault_maintenance` post-MX-1. T3 formalizes those embedded citations into the MX-1 5-subsection structure, which:

- Lifts **source_diversity 3 → 4** (principled-4 ceiling; `learning_store` is **reflexive** like `vault_maintenance` — its subjects ARE III's own ADRs/skills/lifecycle).
- Lifts **composite 3.83 → ~4.00** projected: (4+4+4+**4**+5+3)/6 = 24/6 = **4.00**.
- Holds all other axes (citation-add only — no trap/detection redefinition; matches MX-1 scope-discipline).

**Operator decision surfaced (DP-2/charter):** T3 is now *polish*, not *remediation*. Options: (a) keep T3 as specified (formalize §Sources, 3→4, ~+0.17); (b) **first-defer T3** alongside T4 if multi-track pressure (per card R5) — `learning_store` already passes floor; (c) re-point T3 at the genuinely-weakest pack if one is preferred. Recommend **(a)** — it's a clean, low-risk, real improvement and closes the "embedded-not-structured citations" gap; but the charter should record that T3's stakes are lower than the card implied.

## §3 The MX-1 recipe to replay

`context_iii_vault_maintenance.md` §Sources & Citations (MX-1 v0.4.1) structures internal grounding as:

1. **### Internal contracts (III canonical ADRs)** — the ADRs the pack enforces, each tagged with the trap/section it grounds.
2. **### Internal procedures (III canonical skills)** — the skills the pack operationalizes.
3. **### Cross-campaign precedent** — AARs/scorecards the pack inherits discipline from.
4. **### Workspace-level grounding** — workspace CLAUDE.md / standing rules.
5. **### Citation ceiling rationale (principled-4)** — explicit statement that a reflexive pack caps at 4 (academic literature at wrong abstraction layer; padding would *reduce* signal density).

## §4 learning_store citation plan (≥4 internal source clusters)

| Cluster | Sources | Grounds (load-bearing application point) |
|---|---|---|
| **Internal contracts (ADRs)** | ADR-003 §3 (graduation ceremony + §3.6 ≥2-vault elevated scrutiny); ADR-005 §2/§3 (RLHF required-min + optional-open + consumer namespace); ADR-007 §1 (CorrectionLifecycle 6-stage state machine) + §3/§3.6 (six-axis rubric + graduation_yield amendment); ADR-008 (cross-vault aggregation transport) | §Architecture, §Corrections JSONL Schema (rlhf_* rows), §Lifecycle Protocol steps 1-4, §3.5 quality-metric |
| **Internal procedures (skills)** | `skill_iii_review.md` (Step 3b ACCUMULATE — read/write edge); `skill_session_close_ceremony.md` (cascade discipline); `skill_aggregation_index_intake.md` (ADR-008 §3.2 intake ceremony) | §Lifecycle Injection/Accumulation/Graduation |
| **Cross-campaign precedent** | MD-B1 (first per-pack score — this pack is the exemplar); MD-B2 (canonical md5 rotation); MD-B4 7-pack pilot (graduation_yield formula finding); Campaign D MD-B6 DG-D scorecard | §3.5 quality-metric provenance; floor-rule application |
| **The canonical artifact itself** | `iii_corrections_canonical.jsonl` (28 entries; md5 `5adb0dfa…`); `iii_adaptive_improvement_loop_spec.md` §5 (state machine) | §Corrections JSONL Schema, §Lifecycle |
| **Citation ceiling rationale (principled-4)** | reflexive-pack statement mirroring MX-1 / web_design F2 | §Sources preamble |

5 clusters → comfortably supports `source_diversity = 4` (the principled-4 ceiling for a reflexive pack).

## §5 Hard invariants (T3 at G2)
- **Canonical jsonl md5 INVARIANT** — T3 is documentation revision; zero graduation; no `rlhf_*` schema change.
- Trap/detection content HELD (citation-add only).
- Pack version: patch or minor per G2 disposition; lattice yaml unchanged unless oracle re-registration (G2 decision).
- No new III ADR.

## §6 Cross-references
Target: `context_iii_learning_store.md` (frontmatter composite 3.83 / sd 3). Recipe: `context_iii_vault_maintenance.md` §Sources & Citations (MX-1) + `what/artifacts/mx1_validation_self_review.md`. Contracts: ADR-003/005/007/008. Sibling: `g0_iss_pack_revision_spec.md` (#4 — the anchor; T1).
