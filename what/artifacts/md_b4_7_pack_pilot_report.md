---
type: artifact
title: MD-B4 7-Pack Pilot Report — Per-Pack Quality Scores + RLHF Signal Volumes + Graduation-Candidate Surface
created: 2026-05-23
updated: 2026-05-23
last_edited_by: agent_argus
mission_origin: campaign_d_federation_adaptive_loop MD-B4
status: ratified
authoring_provenance: MD-B4 single-session close 2026-05-23
governs: per-pack quality scores under ADR-007 §3 six-axis rubric; RLHF signal-volume baseline under ADR-005 schema; graduation-candidate enumeration for MD-B5 intake
tags: [artifact, md_b4, 7_pack_pilot, quality_metric, rlhf_signal_volume, graduation_candidates, mb4_2_finding, mx1_carve_out, template_dh_b_scan]
related_adrs:
  - adr_003_learning_store_ownership      # §3 graduation ceremony + §3.6 ≥50 elevated-scrutiny + ≥2-vault evidence
  - adr_005_rlhf_signal_channel            # 3 required-min fields the RLHF signal-volume table reports on
  - adr_007_adaptive_improvement_loop      # §3 six-axis rubric this report applies; §1 CorrectionLifecycle
  - adr_008_cross_vault_aggregation        # §3 aggregation contract this report's graduation candidates feed
companion_artifacts:
  - what/context/core_domain_packs/context_iii_vault_maintenance.md         # +quality_metric frontmatter block at MD-B4
  - what/context/core_domain_packs/context_iii_domain_packs_web_design.md   # +quality_metric
  - what/context/core_domain_packs/context_iii_introspect_checks.md         # +quality_metric
  - what/context/core_domain_packs/context_iii_inspect_procedures.md        # +quality_metric
  - what/context/core_domain_packs/context_iii_whitepaper_communication.md  # +quality_metric
  - what/context/core_domain_packs/context_iii_canvas_visual.md             # +quality_metric
  - what/context/core_domain_packs/context_iii_learning_store.md            # MD-B1 exemplar (already carried quality_metric pre-MD-B4)
  - how/skills/skill_aggregation_index_intake.md                            # ADR-008 §3.2 populating ceremony
  - who/coordination/.aggregation/graduation_proposals_index.jsonl          # seeded with VFL-001/-002 records
---

# MD-B4 7-Pack Pilot Report

**Mission**: `campaign_d_federation_adaptive_loop` MD-B4 (Track D2, Phase P3)
**Window**: pack lifetime since first load (per adaptive-loop spec §4.3 default)
**Canonical store state at scoring**: 28 entries, 5 graduated (C-001, C-002, C-005, C-027, C-028); md5 `5adb0dfa38d9224649c3b2cba83852ae` invariant pre + mid + post

This report executes the adaptive loop's measurement surface across all 7 core domain packs at `what/context/core_domain_packs/`. Six packs are newly scored against the ADR-007 §3 six-axis rubric; the seventh (`context_iii_learning_store.md`, MD-B1 exemplar at composite 3.83) is re-cited here for completeness. The report also discharges the MD-B4 carry-forward dispositions: a TEMPLATE-DH-B III-relevance scan (concluding **MD-X1 carve-out as TRACKED-DEFERRED no-op for III**), an end-to-end exercise of the ADR-008 §3.2 aggregation-index populating ceremony (seeded with the VFL-001/-002 precedent — 1 vault, gate unmet, no graduation fires), and the surfacing of graduation candidates from the canonical store for MD-B5 intake.

**Zero canonical mutations**. The canonical jsonl is read-only at MD-B4 per ADR-008 §5.3 boundary-discipline + MD-B4 charter invariants.

---

## §1 — TEMPLATE-DH-B III-Relevance Scan + MD-X1 Carve-Out

**Charter directive** (line 150 carry-forward disposition): "Standalone interstitial MD-X1 OR fold into D2 MD-B4 per relevance. If all 5 candidates III-relevant, fold into MD-B4; if mixed, MD-X1 carves out an assessment-only mission."

**Source memo**: `~/aDNA/LatticeLabs.aDNA/who/coordination/coord_2026_05_20_adna_doctrine_promotion_candidates.md` (Berthier; filed at LL.aDNA Phase 4 S22 close; target `aDNA.aDNA`; courtesy memo, no countersign required, no urgency).

**5 candidate files** (all at `~/aDNA/LatticeLabs.aDNA/how/doctrine/`):

| # | File | LL Strength | III hits (`grep -ciE 'III\|iii\b\|introspect\|inspect\|improve\|adaptive.{0,10}loop\|RLHF\|graduation_yield\|domain.pack'`) | III-relevance verdict |
|---|------|-------------|---|---|
| 1 | `war_ontology.md` | medium | 3 | **citation-only** — references `skill_iii_review` as an OBSERVE/ORIENT aid in the OODA cascade (lines 107, 109, 256); III is used by the war framework, not absorbed into it |
| 2 | `doctrine_org_graph_pattern.md` | strong | 0 | **aDNA-only** — defines Org-Graph.aDNA as the 5th pattern category (LIP-0005); no III scope |
| 3 | `doctrine_dg_check.md` | medium | 1 | **aDNA-only** — single "Move III" reference is war/move terminology, not the III framework |
| 4 | `doctrine_audit_input_aar.md` | medium | 6 | **aDNA-only** — all 6 "III" hits are war/Move-III terminology + an `m03_iii_interface_report.md` artifact path; no III-framework absorption needed |
| 5 | `estimation_calibration.md` | strong | 0 | **aDNA-only** — ~40-line universal estimation methodology; zero cross-refs |

**Disposition**: 0 of 5 candidates require III-side absorption. The cleanest possible outcome — even cleaner than charter line 150's "mixed" case. **MD-X1 is carved out as TRACKED-DEFERRED no-op for III**: the absorption work belongs entirely to aDNA.aDNA's standard-evolution flow at its own cadence; no III-side action surface exists. The single citation-pattern in `war_ontology.md` is informational (the war ontology consumes III as a tool) and does not require III-side acknowledgment beyond this scan record.

**Charter row to add to `campaign_d_federation_adaptive_loop.md`**:

```
| MD-X1 | TEMPLATE-DH-B III-relevance assessment | TRACKED-DEFERRED | scan executed inside MD-B4; 0 of 5 candidates require III absorption; aDNA.aDNA promotion remains scope-out of III |
```

---

## §2 — Per-Pack Quality Scoring Table

All scores per ADR-007 §3 six-axis rubric (1–5 integer per axis; composite = arithmetic mean to 2dp; floor rule: any axis ≤2 triggers pack revision flag). Measurement window: pack lifetime since first load (spec §4.3 default).

| # | Pack | sd | act | cu | srcdiv | ctc | grad | **Composite** | Floor | Notes |
|---|------|----|-----|----|--------|-----|------|---------------|-------|-------|
| 1 | `context_iii_vault_maintenance.md` | 4 | 4 | 3 | **2** | 4 | **1** | **3.00** | **TRIGGERED** | source_diversity is real pack-revision finding (no ADR/skill/external cites); graduation_yield is MB4-2 |
| 2 | `context_iii_domain_packs_web_design.md` | 5 | 5 | 5 | 3 | 5 | **1** | **4.00** | TRIGGERED | graduation_yield only (MB4-2); pack quality unaffected |
| 3 | `context_iii_introspect_checks.md` | 5 | 5 | 4 | 4 | 5 | 4 | **4.50** | passed | highest composite; carries C-027 PACK_DELTA_LANDED |
| 4 | `context_iii_inspect_procedures.md` | 5 | 5 | 4 | 4 | 5 | 3 | **4.33** | passed | carries C-028 PACK_DELTA_LANDED (Modality 2 Code Inspect) |
| 5 | `context_iii_whitepaper_communication.md` | 5 | 5 | 5 | 3 | 4 | **1** | **3.83** | TRIGGERED | graduation_yield only (MB4-2); pack is draft + campaign-scoped with its own graduation track |
| 6 | `context_iii_canvas_visual.md` | 5 | 5 | 4 | 4 | 5 | **1** | **4.00** | TRIGGERED | graduation_yield only (MB4-2); pack is **MOST loop-mature** with 3-cycle log + pack-internal CV-PENDING-01 graduation pending M20 |
| 7 | `context_iii_learning_store.md` *(MD-B1 exemplar)* | 4 | 4 | 4 | 3 | 5 | 3 | **3.83** | passed | re-cited from MD-B1 close 2026-05-20 (`scoring_mission: MD-B1`) |

**Distribution**: composite range 3.00 – 4.50; median 4.00; mean 3.93. The MD-B1 exemplar (3.83) sits at the 33rd percentile of the 7-pack set, validating it as a reasonable baseline.

**Axes legend**: sd = signal_density · act = actionability · cu = coverage_uniformity · srcdiv = source_diversity · ctc = cross_topic_coherence · grad = graduation_yield.

---

## §3 — RLHF Signal Volume Table

Per ADR-005 §2 required-minimum schema, RLHF signals are tracked via `rlhf_signal_type` ∈ {`accept` | `reject` | `defer` | `accept_with_modification`} on each correction entry. The canonical store contains **28 entries**; **only 2** (C-027, C-028 — the VFL-001/-002 graduations at MD-B2 2026-05-21) carry `rlhf_signal_type`. The remaining 26 entries (C-001..C-026) pre-date ADR-005 ratification and have no RLHF signal field → bucket `unknown`.

**Per-pack canonical-store RLHF signal volumes** (counted via `graduated_to:` field per spec §4.3 default + cross-referenced against pattern-occurrence in pack body):

| Pack | `accept` | `reject` | `defer` | `accept_with_modification` | `unknown` (pre-ADR-005) | **Pack-relevant total** |
|------|---------|---------|---------|---------------------------|-------------------------|-------------------------|
| `context_iii_vault_maintenance.md` | 0 | 0 | 0 | 0 | 1 *(C-009 staleness)* | **1** |
| `context_iii_domain_packs_web_design.md` | 0 | 0 | 0 | 0 | 4 *(C-012/-013/-015/-016)* | **4** |
| `context_iii_introspect_checks.md` | 1 *(C-027)* | 0 | 0 | 0 | 1 *(C-005 cross-ref)* | **2** |
| `context_iii_inspect_procedures.md` | 1 *(C-028)* | 0 | 0 | 0 | 16 *(C-001..C-008, C-010, C-011, C-014, C-017, C-018, C-019, C-026 — Five Traps cluster)* | **17** |
| `context_iii_whitepaper_communication.md` | 0 | 0 | 0 | 0 | 6 *(C-018, C-020..C-025 tone+confidence)* | **6** |
| `context_iii_canvas_visual.md` | 0 | 0 | 0 | 0 | 0 *(pack-internal CV-PENDING-01 graduation predates ADR-005; not in canonical)* | **0** |
| `context_iii_learning_store.md` | 0 | 0 | 0 | 0 | 1 *(C-001 in body)* | **1** |
| **Canonical total** | **2** | **0** | **0** | **0** | **26** | **28** |

**Key observations**:

1. **94% of canonical entries (26 of 28) lack RLHF signals.** ADR-005 ratified at MD-B1 (2026-05-20); only post-MD-B1 graduations carry the required-minimum signal fields. This is expected behavior and does NOT indicate a contract violation — the schema is forward-additive per ADR-002 §3.

2. **Acceptance dominates the post-ADR-005 sample (2 of 2 = 100% `accept`).** Sample size too small for distribution inference; MD-B5 with broader cross-vault data will produce meaningful rate distributions.

3. **The pack-relevant total column reflects pattern-occurrence-in-pack-body, NOT canonical `graduated_to:` routing** (which is `"core"` for all 5 graduated entries — see §4 and the MB4-2 finding). The `inspect_procedures` total (17) reflects that pack's defining role as the Five Traps reference — most pre-ADR-005 entries thread through it.

4. **`canvas_visual` shows 0 canonical-RLHF presence** despite being the most loop-mature pack (3 cycles, internal CV-PENDING-01 graduated). The pack-internal graduation track is pre-canonical (M20 III graduation pending) — another MB4-2 axis.

---

## §4 — Floor-Rule Outcomes

4 of the 6 newly-scored packs triggered the floor rule. Disposition per the MD-B4 plan (charter line 76 + risk MB4-3):

| Pack | Triggered axes | Real quality issue? | Action |
|------|---------------|---------------------|--------|
| `context_iii_vault_maintenance.md` | source_diversity (2), graduation_yield (1) | **YES on source_diversity** | Open as pack-revision candidate for a future mission (NOT in MD-B4 scope); add ADR-references for trap-inventory provenance + skill cross-refs |
| `context_iii_domain_packs_web_design.md` | graduation_yield (1) | NO — pure MB4-2 | No revision; document |
| `context_iii_whitepaper_communication.md` | graduation_yield (1) | NO — pure MB4-2 | No revision; document |
| `context_iii_canvas_visual.md` | graduation_yield (1) | NO — pure MB4-2 (ironic — pack is most loop-mature) | No revision; document |

**MB4-2 finding ratified** (graduation_yield axis collapses to noise on most packs):

The ADR-007 §3 formula `graduation_yield = (corrections with graduated_to=<pack_name> over period) / (review cycles where pack was loaded over period)` produces score 1 for **5 of 7 packs** (all except `introspect_checks` + `inspect_procedures` + `learning_store`). Root cause: canonical jsonl records `graduated_to: "core"` for all 5 graduated entries — the field does not distinguish pack routing. The actual PACK_DELTA_LANDED stage HAS fired into specific packs (C-027 → `introspect_checks` Check 2c.i; C-028 → `inspect_procedures` Modality 2; C-001 → `learning_store` body example) but this routing is observable only by pattern-occurrence grep, not by the formula's expected field.

**Disposition**: ADR-007 §3 amendment is OUT of MD-B4 scope (consumption-only per MD-A1+MD-A2+MD-A3 precedent). Flag for MD-B5 (cross-vault validation) or DG-D close to consider: (a) extending canonical jsonl `graduated_to:` to carry pack-specific values; (b) amending ADR-007 §3 graduation_yield formula to use pattern-occurrence-in-pack-body; (c) accepting the measurement-issue as a known limitation. This is an architectural decision requiring Stanley signature.

---

## §5 — Graduation-Candidate Surface

Per ADR-003 §3 ceremony criteria (`frequency ≥ 3 AND acceptance ≥ 80% AND not yet graduated`):

| ID | Trap | Pattern | Freq | Accepted | Likely pack target | Status |
|----|------|---------|------|----------|---------------------|--------|
| C-003 | confidence | overstated_validation_scope | 5 | true | inspect_procedures (Confidence trap) | **Candidate (frequency 5; meets ADR-003 §3 standard ceremony gate)** |
| C-007 | boundary | performance_claim_without_benchmark | 5 | true | inspect_procedures (Boundary trap) | **Candidate (frequency 5)** |
| C-009 | staleness | derived_view_drift | 5 | true | vault_maintenance (Staleness trap) | **Candidate (frequency 5)** |
| C-010 | confidence | speculative_mapping_unmarked | 3 | true | inspect_procedures (Confidence trap) | **Candidate (frequency 3 — at floor)** |
| C-018 | confidence | summary_confidence_inflation | 6 | true | whitepaper_communication (already cited as cross-ref) + inspect_procedures | **Candidate (frequency 6; highest non-graduated)** |

5 standard-ceremony candidates surface. **None meets ADR-003 §3.6 elevated-scrutiny threshold** (≥50 cross-fork frequency + ≥2-vault evidence); the §5 candidate list is for standard-ceremony review only.

**MD-B5 intake**: these 5 entries are the candidate set for the next graduation ceremony. MD-B5 (cross-vault validation) should:
1. Cross-check each against consumer-vault local stores for independent observation (the ≥2-vault gate is OFF for standard ceremony per ADR-008 §3.4, but observation contributes to MB4-2 amendment evidence)
2. Decide canonical jsonl format question (does `graduated_to:` get extended? — MB4-2 architectural question above)
3. Stanley + Argus co-ratification per ADR-003 §3 + ADR-007 §1 stage `GRADUATION_RATIFIED`

**No MD-B4 graduation fires**. Canonical store untouched; md5 `5adb0dfa38d9224649c3b2cba83852ae` invariant.

---

## §6 — Measurement Window Note

Per adaptive-loop spec §4.3 default, the measurement window for each pack is **"pack lifetime since first load"**:

| Pack | First load (created date) | Measurement window |
|------|---------------------------|---------------------|
| `context_iii_canvas_visual.md` | 2026-04-11 | ~42 days |
| `context_iii_whitepaper_communication.md` | 2026-04-13 | ~40 days |
| `context_iii_inspect_procedures.md` | 2026-04-03 | ~50 days |
| `context_iii_introspect_checks.md` | 2026-04-03 | ~50 days |
| `context_iii_domain_packs_web_design.md` | 2026-04-04 | ~49 days |
| `context_iii_vault_maintenance.md` | 2026-04-02 | ~51 days |
| `context_iii_learning_store.md` *(MD-B1 exemplar)* | 2026-04-03 | ~50 days |

Window range: 40–51 days; tight enough that pack-to-pack composite comparisons are not confounded by window-length disparity. MD-B5 may adopt a fixed window (e.g., MD-B4 close → MD-B5 close ~14 days) for inter-session score progression tracking.

---

## §7 — Deferrals + Forward-References Resolved

**Resolved at MD-B4 (this report)**:

- `iii_adaptive_improvement_loop_spec.md` §4.5 forward-ref "Scoring remaining 6 canonical packs against 6-axis rubric — pending MD-B4" → **RESOLVED**: all 6 unscored packs now carry `quality_metric:` frontmatter; see §2.
- `iii_adaptive_improvement_loop_spec.md` §7.2 forward-ref "First population of ADR-008 §3 aggregation index — pending MD-B4" → **RESOLVED**: `who/coordination/.aggregation/graduation_proposals_index.jsonl` created + seeded with VFL-001/-002 retroactive records via `skill_aggregation_index_intake.md`; ≥2-vault evidence gate exercised end-to-end (gate unmet — no graduation candidacy escalates).
- `iii_adaptive_improvement_loop_spec.md` §7.3 forward-ref "Per-pack RLHF signal-volume baseline — pending MD-B4" → **RESOLVED**: §3 table.
- ADR-007 §3 forward-ref "MD-B4 propagates [per-pack quality scoring] across remaining 6 [packs]" → **RESOLVED**: §2 table.
- ADR-008 §3.2 forward-ref "the populating tool is deferred to MD-B4" → **RESOLVED**: `how/skills/skill_aggregation_index_intake.md` (manual ceremony per operator-locked scope decision; not a Python script per MD-A2/MD-B1/MD-B3 spec-only precedent).
- Lattice yaml 1.2.3 elevated_scrutiny note "the aggregator implementation is a substrate enhancement out of scope at MD-B2" + 1.2.4 cross_vault_proposal_intake_policy "the index-populating tool is deferred to MD-B4 (cross-fork aggregation execution)" → **RESOLVED**: ceremony documented in skill; lattice yaml patch-bumped 1.2.4 → 1.2.5 to register the skill + index path.

**Deferred forward (MD-B5 / MD-B6 / DG-D / future)**:

- **MB4-2 architectural decision**: canonical jsonl `graduated_to:` field semantics (currently "core" for all graduations) — Stanley signature required; flag at MD-B5 intake.
- **ADR-007 §3 graduation_yield formula amendment**: candidate for amendment if (a) above is decided in favor of pack-specific routing.
- **Pack revision for `context_iii_vault_maintenance.md`**: source_diversity = 2 floor trigger; open as standalone mission post-DG-D (NOT in Campaign D scope — pack content amendment requires its own session).
- **Cross-vault aggregation execution**: the index is seeded with 1-vault VFL precedent at MD-B4. The first ≥2-vault candidate surfaces only when a second consumer files a `learning_store_graduation` proposal for an existing pattern. Defers to organic consumer activity per ADR-008 §5 boundary discipline.
- **Cross-vault audit-log federation**: airlock substrate impl v0.3 §8 deferred item; multi-vault audit federation not in MD-B4 scope.
- **Pub/sub LedgerObservation upgrade** (LN Carry 3 EP-1 Phase 5): airlock impl-doc §6.3 `assumes_draft` workaround stays in force at MD-B4; native enum path lands at LN Phase 5.

---

## Authoring provenance + verification

- **Plan**: `/Users/stanley/.claude/plans/please-read-the-claude-md-pure-patterson.md` (ExitPlanMode 2026-05-23)
- **Session**: `how/sessions/active/session_stanley_20260523_iii_adna_md_b4_7_pack_pilot.md`
- **Pre-flight md5**: `5adb0dfa38d9224649c3b2cba83852ae` ✅
- **Post-flight md5**: `5adb0dfa38d9224649c3b2cba83852ae` ✅ (verified at §11 of session activity log)
- **Consumer wrappers**: all 6 diff-empty ✅ (UNTOUCHED at MD-B4)
- **New III ADRs at MD-B4**: 0 (consumption-only per MD-A1+MD-A2+MD-A3 precedent)
- **Git tag**: no bump at MD-B4 (deferred to DG-D close per Campaign B+C+MD-B1+MD-B2+MD-A1+MD-A2+MD-A3+MD-A4+MD-B3 precedent)

*This report is the consumer-facing reference for the MD-B4 pilot outcome. Substrate detail (skill procedure, index schema, lattice yaml node) lives in the companion artifacts named in frontmatter.*
