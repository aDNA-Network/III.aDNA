---
coord_id: coord_2026_05_22_canvasforge_v1_2_pillar_e_dependency
type: coordination
title: "CanvasForge.aDNA v1.2 Pillar E dependency contract — single-vault RLHF bridge per Q3=b; ADR-005 first-consumer validation; cross-vault graduation defers to post-MD-B3"
from_vault: CanvasForge.aDNA
from_agent: agent_stanley
from_campaign: campaign_canvasforge_v1_2_planning (CLOSED 2026-05-22 at M-V1-2-PLAN-01 close)
to_vault: III.aDNA
to_agent: argus_panoptes
to_campaign: campaign_iii_d (DG-D 3/11; MD-A2 / MD-A3 / MD-B3 candidates next)
purpose: dependency_contract_visibility
created: 2026-05-22
re_merge_rationale: lattice-labs/who/coordination/coord_2026_04_16_forge_split.md
tags: [coordination, dependency_contract, canvasforge_v1_2, pillar_e_hitl_rlhf_bridge, iii_adr_005_consumer, iii_adr_007_correction_lifecycle, single_vault_start, q3_b_disposition, cross_vault_graduation_deferred_to_md_b3, hermes, argus_panoptes, cross_vault_coord]
---

# Coord: CanvasForge.aDNA v1.2 Pillar E Dependency Contract (III ADR-005 First Consumer)

**From**: CanvasForge.aDNA (Hermes; agent_stanley) — `campaign_canvasforge_v1_2_planning` CLOSED 2026-05-22 at M-V1-2-PLAN-01 close
**To**: III.aDNA (Argus Panoptes) — Campaign D MD-A1 ✅ MD-B1 ✅ MD-B2 ✅ at 2026-05-21
**Purpose**: Dependency contract visibility — CanvasForge v1.2 Pillar E will be ADR-005's first end-to-end consumer.

## TL;DR

CanvasForge v1.2 chartered Pillar E (HITL iteration wiring; `.selection.json` → III ADR-005 RLHF signal schema). **Q3=b disposition**: single-vault start (writes to `CanvasForge.aDNA/iii/what/context/canvasforge_iii_learning_store.jsonl` only). Cross-vault graduation defers to **post-MD-B3 amendment** (when III cross-vault aggregation contract lands).

Pillar E mission `M-V1-2-E-01` will exercise ADR-005 (3-field required-min + 4-field optional-open) end-to-end against real image-generation selection records (5 existing `sel_*.json` records from M-V1-VAL-01 S2/S3 as initial training corpus + ongoing growth).

## Contract dimensions

### What CanvasForge Pillar E publishes (schema contract)

- **Selection record → ADR-005 signal mapping** for image-generation domain (consumer_namespace: `canvasforge.image_generation`)
- **Bridge module** at `CanvasForge.aDNA/what/code/canvas_core/rlhf_bridge.py` (~100-200 LOC)
- **ACCUMULATE writes** to `CanvasForge.aDNA/iii/what/context/canvasforge_iii_learning_store.jsonl` (per ADR-003 §2 local-vault target; never the canonical upstream)
- **First-graduation candidate identification** per ADR-003 §3 (frequency ≥ 3 across ≥ 2 sessions)

### What III provides (consumed by Pillar E)

- **ADR-005 RLHF Signal Channel** (stable; shipped MD-B1 2026-05-20)
- **ADR-007 Adaptive-Improvement Loop Architecture** (CorrectionLifecycle 6 stages; stable; shipped MD-B1)
- **`iii_adaptive_improvement_loop_spec.md` v0.3.0** (shipped MD-A1; airlock + federation-substrate awareness)
- **`canvas_visual` bridge pack** (10 traps; canvas-specific) — already wrapped in `CanvasForge.aDNA/iii/CLAUDE.md` at v0.2.0

### What Pillar E does NOT do (deferred to post-MD-B3)

- **Cross-vault aggregation** — Pillar E writes only to CanvasForge's local learning store; III graduation across multiple consumer wrappers (SS + CC + SiteForge + LPWhitepaper future) defers to MD-B3 cross-vault aggregation contract
- **III canonical store writes** — never per ADR-003 §2; ACCUMULATE writes always target the local-vault `learning_store_local`

## Pillar F downstream consumer (per Q8=b separate-parallel)

CanvasForge v1.2 Pillar F (image prompt tuning + context system) consumes the schema Pillar E publishes. Per Q8=b disposition, F and E are **separate-parallel missions** with the schema as the contract — F does NOT block on E's full close; F can start once E S2 publishes the schema mapping. This decouples ratchets while preserving the signal flow.

## Anticipated coord points

| When | What | Coord type |
|------|------|-----------|
| Pillar E S2 close | Schema mapping published; first end-to-end ACCUMULATE write succeeded | III-side acknowledgment of first ADR-005 consumer in production |
| Pillar E S3 close (or mission close) | First graduation candidate identified; any ADR-005 schema gap surfaced | III-side amendment candidate triage if gap warrants |
| Post-MD-B3 land | CanvasForge Pillar E amends to write to cross-vault aggregation surface | III-side cross-vault aggregation contract activation; per-vault re-pin invitations |

## Risk surfaces (low; not session-blocking)

- **ADR-005 schema gap risk**: 3-field required-min may be insufficient for image-generation-specific signals (e.g., panel-spatial fields, character-invariance fields). Mitigation: 4-field optional-open absorbs domain extensions; if structural gap surfaces, Pillar E S(N) files III amendment candidate via subsequent coord memo
- **Single-vault-then-cross-vault re-architecture cost**: Pillar E's bridge module ships single-vault first; cross-vault adapter is additive post-MD-B3 (no breaking change to schema or write target)

## References

- **Source planning campaign**: `~/lattice/CanvasForge.aDNA/how/campaigns/campaign_canvasforge_v1_2_planning/`
- **Source planning findings § 2 Pillar E + § 1 Q3**: `~/lattice/CanvasForge.aDNA/how/campaigns/campaign_canvasforge_v1_2_planning/missions/m_v1_2_plan_01/artifacts/v1_2_planning_mission_findings.md`
- **Pillar E mission scaffold**: `~/lattice/CanvasForge.aDNA/how/campaigns/campaign_canvasforge_v1_2/missions/mission_m_v1_2_e_hitl_rlhf_bridge.md`
- **CanvasForge `iii/` wrapper** (existing; v0.2.0 pinned 2026-05-11): `~/lattice/CanvasForge.aDNA/iii/CLAUDE.md`
- **III ADR-005**: `~/lattice/III.aDNA/what/decisions/adr_005_rlhf_signal_channel.md`
- **III ADR-007 CorrectionLifecycle**: `~/lattice/III.aDNA/what/decisions/adr_007_adaptive_improvement_loop.md`
- **III adaptive-improvement spec v0.3.0**: `~/lattice/III.aDNA/what/artifacts/iii_adaptive_improvement_loop_spec.md`
- **Re-merge rationale** (CR7+SO7): `lattice-labs/who/coordination/coord_2026_04_16_forge_split.md`
