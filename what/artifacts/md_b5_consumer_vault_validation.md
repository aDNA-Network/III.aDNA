---
type: mission_artifact
title: "MD-B5 Validation — RLHF Signal Channel + Adaptive-Improvement Loop Re-Exercised Across 3 Stable v0.3 Consumer Vaults (SiteForge + VideoForge + CanvasForge)"
campaign: campaign_d_federation_adaptive_loop
mission: MD-B5
mission_class: validation
status: complete
version: "0.3.0"   # matches adaptive-loop spec / ADR-005/-007/-008 versions under test; MD-B5 is validation evidence, not new contract
created: 2026-05-25
updated: 2026-05-25
last_edited_by: agent_argus
authoring_mission: campaign_d_federation_adaptive_loop MD-B5
governed_by:
  - what/artifacts/iii_adaptive_improvement_loop_spec.md   # v0.3.0; §8 verification methodology re-exercised here across 3 consumers
  - what/decisions/adr_005_rlhf_signal_channel.md          # §2 required-min schema + §3 consumer namespace
  - what/decisions/adr_007_adaptive_improvement_loop.md    # §1 CorrectionLifecycle + §3 six-axis quality rubric
  - what/decisions/adr_008_cross_vault_aggregation.md      # §2 boundary-crossing field set + §3 ≥2-vault evidence gate
  - what/decisions/adr_003_corrections_graduation.md       # §3.6 elevated-scrutiny queue + §4 entry schema
predecessor_validation: what/artifacts/md_a5_federation_integration_validation.md   # Track-D1 sibling; MD-B5 is the Track-D2 analog (three-sub-method model reused)
consumers_validated: [SiteForge.aDNA/iii, VideoForge.aDNA/iii, CanvasForge.aDNA/iii]
local_stores_walked:
  - ~/lattice/SiteForge.aDNA/iii/what/context/siteforge_iii_learning_store.jsonl   # 0 entries (empty; loop ready, not yet exercised)
  - ~/lattice/VideoForge.aDNA/iii/what/context/videoforge_iii_learning_store.jsonl  # 4 entries (VFL-001..004; 2 graduated → C-027/C-028)
  - ~/lattice/CanvasForge.aDNA/iii/what/context/canvasforge_iii_learning_store.jsonl  # 4 entries (legacy schema)
aggregation_index_walked: who/coordination/.aggregation/graduation_proposals_index.jsonl   # 2 seed records; 1 distinct originating vault
documentation_grade: true
non_runnable: true   # validation by inspection + schema walk + lifecycle inference; no executable validator runs at MD-B5
zero_regression_confirmed: true
closes: "Track D2 validation half (MD-B5); the DG-D gate + AAR + annotated tag remain MD-B6 per operator decision 2026-05-25 (solo close, not co-closed with MD-B6)"
mb4_2_disposition: recommend_defer   # cross-consumer evidence assembled + non-breaking ADR-007 §3 procedure clarification recommended; ratification deferred to MD-B6/DG-D
new_iii_adrs_authored: []
canonical_jsonl_touched: false
consumer_wrappers_touched: []
tags: [mission_artifact, md_b5, validation, campaign_d, rlhf_signal_channel, adaptive_improvement_loop, three_consumer_validation, correction_lifecycle, cross_vault_boundary, two_vault_evidence_gate, mb4_2_graduation_yield, zero_regression, track_d2_validation, dg_d_input]
---

# MD-B5 Validation — RLHF + Adaptive-Improvement Loop Across 3 Stable v0.3 Consumer Vaults

## §1 Purpose

MD-B5 is the **Track-D2 analog of MD-A5**: a documentation-grade validation by inspection that confirms the RLHF signal channel (ADR-005) + adaptive-improvement loop (MD-B1 spec + ADR-007) + cross-vault aggregation contract (ADR-008) hold across **real consumer wrappers**, with **zero regression**. Where MD-A5 closed Track D1 by re-exercising the federation-aware airlock against the VideoForge → CanvasForge worked example, MD-B5 closes the **Track-D2 validation half** by re-exercising the adaptive-loop spec **§8 verification** (§8.1 schema validation + §8.2/§5.2 loop-closure + §8.3 per-pack quality) — but extended from MD-B1's single-vault (canonical-store) walk to the **federated 3-consumer surface**.

Per the MD-X2 reframe (charter lines 100, 112, 143), the ≥3 consumers are the **3 stable v0.3 consumers** — **SiteForge + VideoForge + CanvasForge** (all pinned at v0.3.0 / `a309ad4` since the MD-A4 sweep). LPWhitepaper is deferred to its post-LiteratureForge-migration chain (it is becoming a `LiteratureForge.aDNA` consumer per the MD-X2 cross-vault note §5). The **MD-B6 DG-D gate + AAR + annotated tag** remain a separate, human-gated session per operator decision at MD-B5 plan ratification (solo close, not co-closed).

Three sub-methods compose this validation (mirroring MD-A5 §1):

1. **Wrapper-pin baseline re-walked** (§3): confirm all 3 consumers still parse against the v0.3 federation contract at the MD-A4 pin — `version 0.3.0` / `pinned_at_commit a309ad4` / `lattice_version 1.2.3` / Layer-1 (no `AIRLOCK.md` activation). Re-uses the MD-A4 sweep as the established trace; MD-B5 confirms no consumer regressed.
2. **RLHF/loop deltas verified against live store evidence** (§4–§8): walk each consumer's local learning store against the ADR-005 §2 required-min schema (+ §3.5 backward-compat), infer each entry's CorrectionLifecycle state per spec §5.2, verify the ADR-008 §2 cross-vault boundary against canonical C-027/C-028, and exercise the ADR-008 §3 / ADR-003 §3.6 ≥2-vault evidence gate against the seeded aggregation index.
3. **Zero-regression assertion** (§9): table-form check of the loop's core invariants (canonical md5 invariance, boundary discipline, lifecycle-inference soundness, gate protective behavior); each marked ✅ still-satisfied across the 3-consumer surface, with every surfaced finding documented as a **non-blocking recommendation** that regresses no v0.2/v0.3 contract.

**Validation grade**: documentation-grade by inspection. No writes to any consumer's learning store or to the canonical store are performed (the validation itself proves nothing is rewritten — canonical md5 `5adb0dfa38d9224649c3b2cba83852ae` verified invariant pre + post). The first executable validator runtime lands when a federation-issuing consumer files a live cross-vault graduation proposal end-to-end through the v0.3 airlock.

**Zero-regression definition**: the RLHF/adaptive-loop substrate landed at MD-B1 (spec) + MD-B2 (VFL graduation; canonical md5 rotation `dde2cbd8…` → `5adb0dfa…`) + MD-B3 (ADR-008) + MD-B4 (7-pack pilot + aggregation index seed) remains valid evidence across the 3 live consumer wrappers; no consumer configuration forces any retroactive canonical edit, schema break, or boundary violation. **Zero regression confirmed** at §9.

## §2 Inputs

| Input | Path | Role |
|---|---|---|
| SiteForge wrapper | `~/lattice/SiteForge.aDNA/iii/CLAUDE.md` (+ `Integration Notes` RLHF Alignment §) | Consumer #1; v0.3.0 pin + RLHF opt-in policy (signal flow defined at `campaign_siteforge_iss` P5.5) |
| VideoForge wrapper | `~/lattice/VideoForge.aDNA/iii/CLAUDE.md` | Consumer #2; v0.3.0 pin + ADR-006 bridge_pack; the one vault with graduations landed (C-027/C-028) |
| CanvasForge wrapper | `~/lattice/CanvasForge.aDNA/iii/CLAUDE.md` | Consumer #3; v0.3.0 pin + bridge_pack + local_skill |
| SiteForge local store | `…/iii/what/context/siteforge_iii_learning_store.jsonl` | 0 entries (empty; loop ready, not yet exercised) |
| VideoForge local store | `…/iii/what/context/videoforge_iii_learning_store.jsonl` | 4 entries (VFL-001..004); VFL-001/-002 carry the 3 ADR-005 §2 required fields + 5 `rlhf_consumer_namespace.videoforge.*` fields each |
| CanvasForge local store | `…/iii/what/context/canvasforge_iii_learning_store.jsonl` | 4 entries in a bespoke legacy schema (`id/type/title/signal/origin/status/date`) |
| Canonical learning store | `what/context/core_domain_packs/iii_corrections_canonical.jsonl` (md5 `5adb0dfa38d9224649c3b2cba83852ae`) | C-027/C-028 boundary-discipline target; 28 entries / 5 graduated |
| Aggregation index | `who/coordination/.aggregation/graduation_proposals_index.jsonl` | 2 seed records (VFL-001/-002 → C-027/C-028); both `originating_vault: VideoForge.aDNA` |
| Adaptive-loop spec | `what/artifacts/iii_adaptive_improvement_loop_spec.md` v0.3.0 | §3 RLHF schema + §4 quality metric + §5 loop-closure + §8 verification methodology (re-exercised here) |
| ADR-005 / -007 / -008 / -003 | `what/decisions/adr_00{5,7,8,3}_*.md` | Normative authority for schema / lifecycle / boundary / graduation gate |
| MD-A5 sibling validation | `what/artifacts/md_a5_federation_integration_validation.md` | Track-D1 precedent; three-sub-method structural template |

## §3 Wrapper-Pin Baseline Re-Walked (3 consumers)

All three stable consumers conform to the MD-A4 v0.3 pin. No consumer regressed; no consumer activated an airlock (Layer-1 only per ADR-008 operator-discretionary cadence).

| Consumer | `version` | `pinned_at_commit` | `pinned_at` | `lattice_version` | `AIRLOCK.md` (Layer 2) | local store |
|---|---|---|---|---|---|---|
| SiteForge.aDNA/iii | 0.3.0 ✅ | a309ad4 ✅ | 2026-05-22 ✅ | 1.2.3 ✅ | none (Layer-1) ✅ | empty (0) |
| VideoForge.aDNA/iii | 0.3.0 ✅ | a309ad4 ✅ | 2026-05-22 ✅ | 1.2.3 ✅ | none (Layer-1) ✅ | 4 entries |
| CanvasForge.aDNA/iii | 0.3.0 ✅ | a309ad4 ✅ | 2026-05-22 ✅ | 1.2.3 ✅ | none (Layer-1) ✅ | 4 entries |

**Findings**: 3/3 pins conform; 0 airlock activations (matches MD-A4 close + MD-A5 §3). One **non-blocking prose-drift observation (F-PROSE)**: SiteForge `iii/CLAUDE.md` narrative body still reads "pins the upstream version at `v0.2.0`" (a pre-MD-A4 sentence) while its `federation_ref:` YAML and `Integration Notes` correctly read `0.3.0`. Consumer-side documentation nit only — the machine-read `federation_ref` is authoritative and correct; **flag, do not fix** (wrappers untouched at MD-B5). Recommend the SiteForge owner refresh the prose at its next wrapper-review cadence.

## §4 Per-Consumer Schema Validation (spec §8.1 analog)

Each local store's entries are checked against the ADR-005 §2 required-minimum (`rlhf_signal_type`, `rlhf_session_id`, `rlhf_captured_at`) and §3.5 backward-compatibility (optional `rlhf_*` absent ⇒ parser treats as `unknown`; `accepted` back-derives `rlhf_signal_type`).

| Consumer | Entries | Schema basis | Conformance |
|---|---|---|---|
| SiteForge | 0 | — | **vacuously conforms** ✅ — empty store per ADR-003 §2; ACCUMULATE writes target it; RLHF opt-in surface defined at `campaign_siteforge_iss` P5.5 (not yet active) |
| VideoForge | 4 | ADR-003 §4 + ADR-005 §2 | **conforms** ✅ — VFL-001/-002 carry all 3 required-min fields (`rlhf_signal_type: accept`; `rlhf_session_id: session_stanley_20260521_iii_adna_md_b2_vfl_graduation`; `rlhf_captured_at: 2026-05-21T08:07:12Z`). VFL-003/-004 (`frequency: 1`, no `rlhf_*`) are **valid by §3.5** (absence ⇒ `unknown`; `accepted: true` back-derives `accept`) — minor finding **F4** |
| CanvasForge | 4 | bespoke legacy | **non-conformant (non-blocking)** — entries use `id/type/title/signal/origin/status/date`, divergent from both ADR-003 §4 and ADR-005 §2; carry no `rlhf_*` fields and no `frequency`/`accepted`/`graduation_candidate` — finding **F3** |

**F3 (CanvasForge legacy schema)**: the CanvasForge local store predates ADR-003 §4 / ADR-005 alignment and uses a free-form accumulation schema. It is **downstream-isolated** — the wrapper routes ACCUMULATE locally per ADR-003 §1, the canonical store is unaffected, and the three CanvasForge downstream consumers do not read it (per the wrapper's downstream-safety note). Non-blocking. **Recommendation**: migrate the CanvasForge local store to the ADR-003 §4 + ADR-005 §2 shape at CanvasForge's discretion (separate consumer-side mission); MD-B5 flags, does not fix.

## §5 Per-Consumer Loop-Closure / CorrectionLifecycle Inference (spec §8.2 / §5.2 analog)

State inferred per the spec §5.2 table (inference-from-fields; no stored `lifecycle_state:`).

| Consumer | Entry | Observable fields | Inferred state |
|---|---|---|---|
| VideoForge | VFL-001 | `graduated: true`, `graduated_to: "core"`, trap landed in `context_iii_introspect_checks.md` Check 2c.i (per MD-B2) | **PACK_DELTA_LANDED** ✅ (see §8 caveat on `"core"`) |
| VideoForge | VFL-002 | `graduated: true`, `graduated_to: "core"`, trap landed in `context_iii_inspect_procedures.md` (per MD-B2) | **PACK_DELTA_LANDED** ✅ (see §8 caveat) |
| VideoForge | VFL-003 | `frequency: 1`, `graduated: false`, no candidate flag | **SIGNAL_CAPTURED** ✅ |
| VideoForge | VFL-004 | `frequency: 1`, `graduated: false`, no candidate flag | **SIGNAL_CAPTURED** ✅ |
| CanvasForge | C-NEW-…-A..D (×4) | `status: "accumulating"`; legacy schema lacks `frequency`/`graduation_candidate` | **inference-incomplete** — maps loosely to a pre-graduation (SIGNAL_CAPTURED-equivalent) state via `status: "accumulating"`, but the §5.2 table cannot be applied cleanly (facet of F3) |
| SiteForge | — | empty store | no entries to trace (loop ready) |

**Findings**: VideoForge's 4 entries each map to exactly one CorrectionLifecycle state via §5.2 — the loop-closure inference is **sound end-to-end** at the one vault that has exercised it (2 entries reached PACK_DELTA_LANDED; 2 sit at SIGNAL_CAPTURED). CanvasForge's legacy entries cannot be cleanly inferred (F3 facet) — non-blocking; resolved by the F3 schema-migration recommendation.

## §6 Cross-Vault Boundary Discipline Check (ADR-008 §2)

ADR-008 §2: the canonical-eligible *projection* of a local entry crosses the boundary (`pattern`, `trap`, `description`, `example`, `frequency`, `accepted`, `source_*`, the 3 ADR-005 §2 required fields, `originating_vault`); **every `rlhf_consumer_namespace.<vault>.*` field and the consumer-local id stay vault-local** (ADR-005 §3 rule 5).

**Verified against canonical C-027/C-028** (graduated from VideoForge VFL-001/-002 at MD-B2):

- VideoForge VFL-001/-002 each carry **5** `rlhf_consumer_namespace.videoforge.*` fields (`last_updated`, `graduation_proposal_filed`, `graduation_proposal_path`, `graduated_to_canonical_id`, `graduation_note`).
- Canonical C-027/C-028 carry **zero** keys containing `consumer_namespace` (programmatically verified: `keys_with_consumer_namespace = []`). **Boundary discipline HELD** — finding **F1** (a zero-regression positive). The canonical entries are the local entries stripped of their consumer namespace, exactly as ADR-008 §2 prescribes.

**F2 (originating_vault absent on canonical)**: C-027/C-028 carry `originating_vault: null`. These graduated at **MD-B2 (2026-05-21)**, *before* ADR-008 was ratified (MD-B3, 2026-05-23), so the §2 provenance field that ADR-008 now requires was not populated. **Non-blocking** (additive field; backward-compatible per ADR-002 precedent). The provenance is fully recoverable from the aggregation-index seed records (both name `originating_vault: VideoForge.aDNA`) and the VFL graduation memo. **Flag, do NOT fix** — backfilling `originating_vault` on C-027/C-028 would rotate the canonical md5, which MD-B5 holds invariant. **Recommendation** → MD-B6 disposition: either (a) backfill at a future graduation ceremony that already rotates the md5, or (b) record a temporal-scope note that ADR-008 §2 provenance applies to graduations ratified on/after 2026-05-23.

## §7 ≥2-Vault Evidence Gate Behavior (ADR-008 §3 / ADR-003 §3.6)

The aggregation index (`who/coordination/.aggregation/graduation_proposals_index.jsonl`) holds **2 records**, both `originating_vault: VideoForge.aDNA`, both `idempotency_key: vfl_graduation_proposals_m_3_04_close`:

| pattern_key | originating_vault | frequency | status |
|---|---|---|---|
| `forward_link:producer_consumer_pair_unwired` | VideoForge.aDNA | 3 | ratified_C-027 |
| `meta_pattern:spec_verbatim_port_to_code` | VideoForge.aDNA | 3 | ratified_C-028 |

**Cross-fork frequency = sum across distinct originating vaults = 1 distinct vault** for each pattern. Per ADR-008 §3 / ADR-003 §3.6, the **≥2-vault independent-evidence gate is UNMET** for both patterns — and this is the **designed protective behavior**: a pattern from a single vault does not clear elevated scrutiny regardless of frequency. Confirmed across the real 3-consumer surface:

- SiteForge has filed **zero** graduation proposals (empty store).
- CanvasForge has filed **zero** graduation proposals (legacy entries, none promoted).
- → No pattern has a *second* independent originating vault. The gate correctly returns UNMET. **Finding F7** — gate exercised end-to-end from the receiver side; protective behavior confirmed.

(Note on idempotency: both seed records share one `idempotency_key`, so per ADR-008 §4 a same-vault re-file would not double-count — consistent with the single-ceremony origin of VFL-001/-002.)

## §8 MB4-2 — `graduation_yield` Axis: Cross-Consumer Evidence + Recommendation (deferred to MD-B6)

**The open item** (flagged at MD-B4 for DG-D/MD-B5): the ADR-007 §3 `graduation_yield` axis — `(corrections with graduated_to=<pack_name>) / (review cycles where pack was loaded)` — collapses to the floor rule on 5 of 7 packs because canonical entries record `graduated_to: "core"` (a generic class) rather than the specific pack the delta landed in.

**Cross-consumer evidence assembled at MD-B5** (the new data the consumer surface provides):

- VideoForge **local** VFL-001/-002 both record `graduated_to: "core"` — *and* the actual pack deltas DID land in specific packs (VFL-001 → `context_iii_introspect_checks.md` Check 2c.i; VFL-002 → `context_iii_inspect_procedures.md`, per MD-B2).
- Canonical **C-027/C-028** likewise record `graduated_to: "core"`.
- → The `"core"` value is recorded **identically at both the consumer-local layer and the canonical layer**. This confirms MB4-2 is a **canonical-schema-wide convention**, **not a consumer-specific defect**. No consumer is routing `graduated_to` pack-specifically; the data needed for a pack-specific yield denominator lives only in the graduation memo / PACK_DELTA record, not in the `graduated_to` field.

**Recommendation (RECOMMEND + DEFER per operator decision 2026-05-25)** — a **non-breaking ADR-007 §3 procedure clarification** (not an ADR-003 §4 schema break):

> Amend the `graduation_yield` scoring procedure so a `graduated_to: "core"` entry is **credited to the pack where its PACK_DELTA actually landed**, cross-referenced from the graduation memo / commit (the `who/coordination/` graduation record names the target pack), instead of triggering the floor rule on the generic `"core"` class. This leaves the ADR-003 §4 entry schema unchanged (no canonical md5 churn) and removes the false floor-rule trigger on the 5 affected packs.

**Disposition**: MD-B5 assembles the evidence and records the recommendation only. **Ratification is deferred to MD-B6 / DG-D** — MD-B5 authors **zero ADR changes** (consumption/validation-only, per the MD-A1..MD-A5 + MD-B3 + MD-B4 precedent). The ADR-007 §3 amendment is a candidate for the DG-D plan-ratification slate.

## §9 Zero-Regression Assertion

| # | Loop invariant (post-MD-B4 baseline) | Status across 3-consumer surface under MD-B5 |
|---|---|---|
| 1 | Canonical jsonl md5 `5adb0dfa38d9224649c3b2cba83852ae` invariant | ✅ verified pre + post (no learning-store touch; no graduation fired) |
| 2 | Boundary discipline — no `rlhf_consumer_namespace.*` in canonical | ✅ HELD (C-027/C-028 carry zero consumer-namespace keys; §6 F1) |
| 3 | RLHF required-min schema (ADR-005 §2) at exercised entries | ✅ VideoForge VFL-001/-002 carry all 3 fields; §3.5 back-compat covers VFL-003/-004 |
| 4 | CorrectionLifecycle inference (spec §5.2) sound | ✅ all 4 VideoForge entries map 1:1 to a state (CanvasForge legacy = F3 non-blocking) |
| 5 | ≥2-vault evidence gate (ADR-008 §3 / ADR-003 §3.6) protective | ✅ UNMET as designed (1 distinct vault); §7 F7 |
| 6 | Idempotency (ADR-008 §4) — same-vault re-file no double-count | ✅ shared idempotency_key on the 2 seed records |
| 7 | Consumer pins at MD-A4 v0.3 surface | ✅ 3/3 at v0.3.0 / a309ad4 / 1.2.3 / Layer-1 (§3) |
| 8 | No new III ADR; lattice yaml 1.2.5; 6 wrappers; no git tag | ✅ all unchanged at MD-B5 |

**Closing assertion**: *Zero regression confirmed across the 3 stable v0.3 consumer vaults. The RLHF + adaptive-improvement loop's core invariants hold under the federated consumer surface; every surfaced finding (F2 originating_vault, F3 CanvasForge legacy schema, F4 VFL-003/-004 absence-validity, F-PROSE SiteForge prose) is a non-blocking recommendation that regresses no v0.2/v0.3 contract. MB4-2 is confirmed canonical-schema-wide and dispositioned as a deferred non-breaking ADR-007 §3 procedure recommendation.*

## §10 Deferred / Out of Scope

| Item | Why deferred | When/where |
|---|---|---|
| MD-B6 DG-D gate + AAR + annotated v0.3.0/v1.0.0 tag | Operator chose solo MD-B5 close; gate is a deliberate human-gated session (Standing Rule 7) | MD-B6 (next session) |
| MB4-2 ADR-007 §3 amendment ratification | Validation-only mission; recommend + defer per operator | MD-B6 / DG-D plan-ratification slate |
| F2 — backfill `originating_vault` on C-027/C-028 | Backfill rotates canonical md5 (held invariant at MD-B5) | A future graduation ceremony that already rotates md5, OR a temporal-scope note at MD-B6 |
| F3 — CanvasForge local-store schema migration | Consumer-side; downstream-isolated; non-blocking | CanvasForge discretion (separate consumer mission) |
| F-PROSE — SiteForge "v0.2.0" prose drift | Wrappers untouched at MD-B5 (validation by inspection) | SiteForge next wrapper-review cadence |
| SiteForge RLHF signal opt-in (gate JSON → ACCUMULATE) | Defined at `campaign_siteforge_iss` P5.5; not yet active | SiteForge campaign cadence |
| LPWhitepaper validation | Becomes a LiteratureForge.aDNA consumer; config about to change | Post-LiteratureForge-migration chain (MD-X2 cross-vault note §5) |
| Second-originating-vault aggregation | No consumer beyond VideoForge has filed a proposal | Future cycle; exercises ≥2-vault gate from the satisfying direction |

## §11 Cross-References

- Sibling Track-D1 validation (structural template): `what/artifacts/md_a5_federation_integration_validation.md`
- Adaptive-improvement loop spec (methodology re-exercised): `what/artifacts/iii_adaptive_improvement_loop_spec.md` v0.3.0 §8 / §5.2 / §3.5
- ADR-005 RLHF Signal Channel: `what/decisions/adr_005_rlhf_signal_channel.md` §2 + §3
- ADR-007 Adaptive-Improvement Loop: `what/decisions/adr_007_adaptive_improvement_loop.md` §1 (CorrectionLifecycle) + §3 (six-axis rubric; MB4-2 amendment candidate)
- ADR-008 Cross-Vault RLHF Aggregation Contract: `what/decisions/adr_008_cross_vault_aggregation.md` §2 + §3 + §4
- ADR-003 Corrections Graduation: `what/decisions/adr_003_corrections_graduation.md` §3.6 + §4
- MD-B4 7-pack pilot report (MB4-2 origin): `what/artifacts/md_b4_7_pack_pilot_report.md`
- Aggregation index: `who/coordination/.aggregation/graduation_proposals_index.jsonl`
- VFL graduation memo (the one real precedent): `who/coordination/coord_2026_05_12_vfl_graduation_proposals.md`
- Reply memo (VFL ratified): `who/coordination/reply_2026_05_21_iii_to_videoforge_vfl_graduation_ratified.md`
- Consumer wrappers: `~/lattice/{SiteForge,VideoForge,CanvasForge}.aDNA/iii/CLAUDE.md`
- Canonical learning store (md5 `5adb0dfa38d9224649c3b2cba83852ae`): `what/context/core_domain_packs/iii_corrections_canonical.jsonl`
- Campaign charter: `how/campaigns/campaign_d_federation_adaptive_loop/campaign_d_federation_adaptive_loop.md`
