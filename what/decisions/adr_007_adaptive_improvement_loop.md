---
type: decision
adr_id: "007"
title: Adaptive-Improvement Loop Architecture — named stages, lifecycle state machine, per-pack quality metric
status: proposed
created: 2026-05-20
updated: 2026-05-25
last_edited_by: agent_argus
signed_by: Stanley (Chief Steward) — ratified at MD-B1 close 2026-05-20
supersedes:
superseded_by:
amendments:
  - { date: 2026-05-25, mission: MD-B6, summary: "§3.6 pack-credit attribution procedure clarification (MB4-2 ratification — non-breaking READ-side rule)" }
tags: [adr, adaptive_loop, architecture, lifecycle_state_machine, per_pack_quality, iii, v0_3]
related_adrs:
  - adr_001_module_architecture           # 8-module composition this ADR coordinates over
  - adr_002_consumer_federation_contract  # per-vault wrappers consume the loop
  - adr_003_learning_store_ownership      # §3 ceremony is one stage of the lifecycle
  - adr_005_rlhf_signal_channel           # co-ratified signal schema riding on this lifecycle
mission_origin: campaign_d_federation_adaptive_loop MD-B1
governs: stage IDs of the III adaptive-improvement loop; per-pattern CorrectionLifecycle state machine; per-pack quality metric (6-axis)
---

# ADR-007: Adaptive-Improvement Loop Architecture

## Status

Accepted — ratified by Stanley at MD-B1 close 2026-05-20. Amended at MD-B6 close 2026-05-25 (in-place §3.6 procedure clarification for pack-credit attribution; non-breaking; READ-side rule over the existing schema).

## Context

The III adaptive-improvement loop is, today, **operationally complete but architecturally unnamed**. The mechanics already work:

- `module_iii_accumulate` (`what/modules/module_iii_accumulate.md` line 72) emits a `graduation_signals` output whenever a correction crosses `frequency ≥ 3` AND `acceptance_rate ≥ 80%`.
- `module_iii_introspect` Check 2g `corrections_pattern_match` (`what/modules/module_iii_introspect.md` line 75) closes the read-side loop — it scans merged findings against the canonical + local corrections store and surfaces graduation candidates back into the current review cycle.
- `module_iii_improve` (`what/modules/module_iii_improve.md` line 61) accepts an optional `learning_store_acceptance_rates` input to calibrate severity using historical accept-rate distribution.
- The lattice `lattice_iii_verification_oracle.lattice.yaml` v1.2.1 wires `human_review → accumulate → corrections_store → introspect` (read-only feedback edge).

But nothing in the vault calls this *the loop*. There is no file that says "the III adaptive-improvement loop is version X.Y.Z; it has these stages; these are the transitions; this is what closes a cycle." Three concrete consequences of that gap:

1. **Charter D2's "≥50 corrections threshold" (line 74) is ill-defined.** Threshold counted against what state? `frequency ≥ 3` candidates are one thing; graduated entries are another; pruned entries are a third. Without named stages MD-B2 cannot canonicalize the threshold meaningfully.

2. **Cross-vault aggregation (MD-B3) has no architecture to attach to.** "Riding on the v0.3 airlock" is a transport claim; what is being transported needs an architectural anchor. Stages provide the join points.

3. **Per-pack quality is unmeasured.** Each of the 7 canonical domain packs at `what/context/core_domain_packs/` produces corrections at different rates with different acceptance distributions; today no pack carries a quality score, so MD-B4's planned "7-pack pilot" (charter line 99) has no scoring scaffold. The `skill_context_quality_audit.md` 5-axis rubric (lines 61–66) lives nearby but has never been applied to III canonical packs.

ADR-007 names the loop, defines its lifecycle state machine, and adopts a per-pack quality metric — the architectural anchor that ADR-005 (signal schema) and the MD-B1 spec (consumer-facing reference) sit on top of.

## Decision

### 1. The Adaptive-Improvement Loop — six progression stages, two terminal-non-graduated states

The loop is named **III Adaptive-Improvement Loop**, version `0.3.0` (matching the MD-B1 spec). It is per-pattern (a "pattern" is a single correction-entry equivalence class identified by `pattern: snake_case_id`).

**`CorrectionLifecycle` — six progression stages**:

```
SIGNAL_CAPTURED         # first ACCUMULATE write for this pattern; rlhf_signal_type set; frequency=1
    ↓
CORRECTION_TRACKED      # second-or-later observation; frequency >= 2; not yet a candidate
    ↓
GRADUATION_CANDIDATE    # frequency >= 3 AND acceptance_rate >= 80%; ACCUMULATE emits graduation_signals
    ↓
GRADUATION_PROPOSED     # candidate filed via cross-vault request memo per ADR-003 §3 (VFL-001 precedent)
    ↓
GRADUATION_RATIFIED     # Argus + Stanley co-ratification fired; canonical jsonl append + md5 rotation
    ↓
PACK_DELTA_LANDED       # graduated correction translated into static trap inside relevant domain pack per ADR-003 §6
```

**Two terminal-non-graduated states**:

- `PRUNED` — `acceptance_rate < 50%` over `≥ 5` observations (`context_iii_learning_store.md` lifecycle protocol line 108). Pattern is retired; entry retained for audit but not loaded into future review cycles.
- `ARCHIVED` — superseded by a graduated entry (e.g., a finer-grained sub-pattern absorbed into its parent at GRADUATION_RATIFIED). Preserved in `iii_corrections_archive.jsonl` (future artifact; MD-B2 may author).

Every entry in the canonical or any consumer-fork learning store has an implied CorrectionLifecycle state inferable from existing fields:

| Field combination | Implied CorrectionLifecycle state |
|--------------------|----------------------------------|
| `frequency: 1`, no `graduation_candidate` | `SIGNAL_CAPTURED` |
| `frequency >= 2`, `graduation_candidate` absent or false, `graduated: false` | `CORRECTION_TRACKED` |
| `graduation_candidate: true`, `graduated: false`, no graduation memo on file | `GRADUATION_CANDIDATE` |
| `graduation_candidate: true`, `graduated: false`, graduation memo on file | `GRADUATION_PROPOSED` |
| `graduated: true`, `graduated_to: null` or `<pack_name>` not yet present | `GRADUATION_RATIFIED` |
| `graduated: true`, `graduated_to: <pack_name>` present + trap appears in pack | `PACK_DELTA_LANDED` |
| `acceptance_rate < 0.5` over `frequency >= 5`, no graduation memo | `PRUNED` |
| superseded_by populated | `ARCHIVED` |

The state machine is **inference-from-observable-fields**, not a separate `lifecycle_state:` field on each entry. This preserves the ADR-003 §4 schema unchanged and the canonical jsonl md5 invariant.

### 2. CorrectionLifecycle is parallel to ReviewState — NOT extending it

`ReviewState` (`what/lattices/lattice_iii_verification_oracle.lattice.yaml` lines 240–245; `context_iii_learning_store.md` line 147) is the **per-artifact** state machine: `UNREVIEWED < INSPECTED < INTROSPECTED < IMPROVED`. It tracks one document/codebase/canvas through one review cycle.

`CorrectionLifecycle` is the **per-pattern** state machine: it tracks one correction equivalence class across many review cycles. A single artifact's IMPROVED state may produce signals that move multiple patterns through CorrectionLifecycle stages.

The two state machines never coincide; do not introduce new ReviewState values for lifecycle phases or vice versa.

### 3. Per-pack quality metric — six-axis rubric

Each canonical domain pack at `what/context/core_domain_packs/` SHOULD carry a quality score in its frontmatter. The rubric is **five axes adopted verbatim** from `how/skills/skill_context_quality_audit.md` lines 61–66, plus **one new III-specific axis**:

| Axis | Source | Definition (paraphrased) |
|------|--------|-------------------------|
| `signal_density` | `skill_context_quality_audit.md` | Ratio of decision-relevant content to filler. |
| `actionability` | same | Fraction of pack content that is executable guidance vs. background prose. |
| `coverage_uniformity` | same | Distribution of detail across the pack's named sections (no single section dominates or starves). |
| `source_diversity` | same | Variety of provenance sources cited within the pack. |
| `cross_topic_coherence` | same | How well pack sections relate to each other / share vocabulary. |
| `graduation_yield` | **new at ADR-007** | Count of corrections graduated from review cycles applying this pack, normalized by review-cycle count. Specifically: `(corrections with graduated_to=<pack_name> over period) / (review cycles where pack was loaded over period)`. |

Each axis scored on the 1–5 integer scale from `skill_context_quality_audit.md` lines 50–60. Composite quality = arithmetic mean of all six axes. **Floor rule retained**: any axis scoring ≤2 triggers pack revision regardless of composite.

The `graduation_yield` axis is the III-loop-specific diagnostic the existing rubric lacks. A pack with `signal_density: 5` but `graduation_yield: 1` is producing findings that never accumulate to graduation — exactly the case MD-B4's "7-pack pilot" needs to surface.

#### 3.6 Procedure clarification — pack-credit attribution (MB4-2 ratification, MD-B6 2026-05-25)

The `graduation_yield` formula above (`(corrections with graduated_to=<pack_name> over period) / (review cycles where pack was loaded over period)`) **MUST be read** with the following procedural rule:

- **`graduated_to:` is canonical-schema-wide**, not consumer-routing metadata. In practice, every graduated correction in the canonical jsonl carries `graduated_to: "core"` (the value names the destination *store* — the canonical core domain packs — not the specific pack file that received the delta). MD-B5 cross-consumer evidence (`what/artifacts/md_b5_consumer_vault_validation.md` §8) confirmed this is recorded identically at BOTH consumer-local AND canonical layers, ruling out the alternative interpretation that consumers might use the field for pack-specific routing.
- **Pack-credit therefore attaches via the PACK_DELTA_LANDED stage marker** (per §1 lifecycle), NOT by floor-matching `graduated_to` against pack names. Specifically: the destination pack is the file the graduation memo's `pack_delta_target` (or equivalent provenance field in the graduation ceremony record) names. For the two MD-B2 graduations: VFL-001 → `context_iii_introspect_checks.md` Check 2c.i; VFL-002 → `context_iii_inspect_procedures.md` Modality 2 Code Inspect Static trap. Both are observable from the MD-B2 close-record + the canonical entries' `source_review` provenance, not from `graduated_to:`.
- **Effect on the rubric**: when applying §3 to a future pack-quality assessment, `graduation_yield` is computed by counting corrections whose PACK_DELTA_LANDED transition landed at the assessed pack's file (per ceremony record), divided by review cycles where the pack was loaded. Packs that have received zero PACK_DELTA_LANDED entries continue to score 1 (the floor); packs that received deltas score upward attributably.

This is a **non-breaking READ-side rule**: existing data (28 canonical entries + 4 VideoForge VFL entries + 4 CanvasForge legacy entries) does NOT need rewriting. No write-side migration. The 7-pack pilot scores from MD-B4 (`what/artifacts/md_b4_7_pack_pilot_report.md`) remain valid; the next pilot's `graduation_yield` numbers, computed per §3.6, will not collapse to noise on packs that actually received deltas.

**Bounded forward**: future RLHF-pack-routing schema work (v0.4+ territory only) may revisit whether `graduated_to:` should ever become pack-specific. §3.6's clarification holds for the v0.3 release line and is the load-bearing procedure for any v0.3 pack-quality re-assessment.

### 4. Per-pack quality and ≥50 corrections threshold are SEPARATE, complementary signals

The Campaign D charter line 74 mentions a "≥50 corrections threshold canonicalized per ADR-003 amendment" as MD-B2 scope. This threshold is **distinct from** per-pack quality. They MUST NOT be merged:

- Per-pack quality (this §3) is a **rubric-driven** assessment of the pack's content quality, scored manually-or-tooling-assisted on the 6-axis scale.
- ≥50 corrections threshold (MD-B2) is a **frequency-distribution-driven** graduation discipline: when a pattern accumulates ≥50 observations across vaults (not 3 across 2 sessions per ADR-003 §3), it auto-enters a stricter graduation queue.

A pack can score high quality with low aggregate corrections (mature, stable pack); a pack can score low quality with high aggregate corrections (frequent but un-graduatable noise); both signals coexist independently. MD-B2 amends ADR-003 to introduce the ≥50 threshold; ADR-007 does NOT speak for that threshold's semantics — only confirms it is a separate signal from per-pack quality.

### 5. Spec / lattice / ADR role split

Three artifacts together govern the loop; each owns a distinct surface:

| Artifact | Owns | Updates via |
|----------|------|-------------|
| `what/artifacts/iii_adaptive_improvement_loop_spec.md` (the MD-B1 spec) | **Consumer-facing reference**: stage names, transition rules, worked examples, verification walk-throughs | Minor version bumps tracked at spec frontmatter; consumer wrappers minor-bump review per ADR-002 §3 |
| `what/lattices/lattice_iii_verification_oracle.lattice.yaml` | **Runtime substrate**: node + edge definitions; how modules execute the loop in practice | Patch-bumps for additive cross-references (e.g., 1.2.1 → 1.2.2 at MD-B1); minor-bumps for new nodes/edges |
| ADR-005 (signal channel) + ADR-007 (this — architecture) | **Governance contracts**: the decisions whose violation is a design regression | Amendments via the amendment-history table; ratification requires Stanley signature |

The consumer-facing spec (`iii_adaptive_improvement_loop_spec.md`) **defers** schema-channel decisions to ADR-005 and architectural decisions to this ADR. The lattice yaml is read-only by consumers (its substrate; not their contract). ADRs are the audit trail for why the spec + lattice look the way they do.

## Consequences

### Positive
- MD-B2 has six named stages to thread VFL-001/002 graduation through; "GRADUATION_PROPOSED → GRADUATION_RATIFIED → PACK_DELTA_LANDED" describes the in-flight steps.
- MD-B3 cross-vault RLHF aggregation has stable architectural surface (SIGNAL_CAPTURED + CORRECTION_TRACKED are the cross-vault visible stages).
- MD-B4's 7-pack pilot has a 6-axis rubric to score against (`graduation_yield` provides the III-loop-specific diagnostic existing rubric lacked).
- `module_iii_introspect` Check 2g semantics now cite ADR-007 §1 (CorrectionLifecycle stage 3 transition). The mechanical behaviour does not change; the documentation gains a stable reference.
- The loop is reviewable as a versioned object — `iii_adaptive_improvement_loop_spec.md` v0.3.0 + ADR-007 v0 → v0.4.0 (future) follows the same minor-bump-review discipline ADR-002 §3 applies to wrappers.

### Negative
- Inference-from-fields state derivation (§1's table) is a parser convention, not a structural enforcement. A malformed entry can be in an ambiguous state; this is a deliberate trade-off to preserve md5 invariance over schema rigidity.
- Six axes (vs five for adna_core packs) means III canonical packs and adna_core packs are NOT directly cross-comparable on composite scores. Trade-off: III-loop-specific diagnostic worth the local-only divergence.
- Two ADRs (005 + 007) co-ratified — amendment ceremony for cross-ADR changes (e.g., adding new required-min fields to ADR-005 that also imply a CorrectionLifecycle stage change in ADR-007) requires coordinated edits.

### Neutral
- Module yaml inputs/outputs unchanged; ADR-007 documents the existing semantics without rewiring lattice edges (patch-level lattice bump at MD-B1).
- Per-pack quality scoring is optional at MD-B1 close (exemplar applied to one pack); MD-B4 propagates across remaining 6.

## Amendment History

| Date | Mission | Amendment summary |
|------|---------|-------------------|
| 2026-05-20 | MD-B1 | Initial ratification at MD-B1 close (commit `b1f1bc4`). Stanley signed. |
| 2026-05-25 | MD-B6 | §3.6 in-place procedure clarification (MB4-2 ratification): `graduation_yield` axis credits packs via the PACK_DELTA_LANDED stage marker / graduation-memo `pack_delta_target` cross-reference, NOT by floor-matching `graduated_to:` against pack names (which is canonical-schema-wide, not consumer-routing metadata, per MD-B5 §8 cross-consumer evidence). Non-breaking; READ-side rule over existing schema; no data rewrite; no version bump per MD-A1+A2+A3+A4+A5+B3+B4+B5 consumption-only precedent. |
