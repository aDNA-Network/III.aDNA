---
type: campaign
campaign_id: campaign_d_federation_adaptive_loop
title: "Campaign D: Federation-Aware Airlock v0.3 + RLHF/Adaptive-Improvement Loop"
status: open
created: 2026-05-20
updated: 2026-05-21
last_edited_by: agent_argus
phase: P1_spec_complete_P2_substrate_partial
chartered_at: 2026-05-20
predecessor: campaign_c_airlock_standard
parallel_eligible_with:
  - carry_forward_lattice_labs_iii_minor_bump   # folded into D1 MD-A4 by default
  - carry_forward_vfl_graduation                # folded into D2 MD-B2 by default
  - carry_forward_template_dh_b_assessment      # standalone OR fold into D2 MD-B4 per relevance
inbound_signal_paths:
  - what/artifacts/mc4_5_alignment_recon_dossier.md   # §5.2 D1/D2/D3 candidate sketches
  - what/artifacts/mc5_validation_videoforge_canvasforge_v0_2.md   # §7 9-item deferred-to-Campaign-D
  - how/campaigns/campaign_c_airlock_standard/campaign_c_airlock_standard.md   # AAR Follow-up #6
  - who/coordination/coord_2026_05_20_iii_to_ll_airlock_status_post_mc4_5.md   # outbound draft → fired this session
  - who/coordination/coord_2026_05_20_iii_to_ln_federation_substrate_intersect.md   # outbound draft → fired this session
outbound_coord_fired_at_charter:
  - who/coordination/coord_2026_05_20_iii_to_ll_airlock_status_post_mc4_5.md
  - who/coordination/coord_2026_05_20_iii_to_ln_federation_substrate_intersect.md
load_bearing_dependency: "LatticeNetwork.aDNA pc_01 Phase A federation substrate (Ed25519 keypair LIVE + first cross-machine COMPLIANCE_AUDIT ledger emit + wga_l1 4-event admission ceremony + durable bore.pub:42561 tunnel; landed 2026-05-20) + Phase B1 dual-substrate ratification ADR-014/-015 (Tailscale + Nebula pluralism)"
parallel_tracks: 2
target_version: 0.3.0
tags: [campaign, federation_aware_airlock, v0_3, rlhf, adaptive_improvement_loop, modular_agentic, parallel_tracks, ln_substrate_load_bearing, post_dg_c, open]
---

# Campaign D: Federation-Aware Airlock v0.3 + RLHF/Adaptive-Improvement Loop

## Mission

Evolve III from v0.2 (cross-vault request patterns shipped at DG-C 2026-05-20) to v0.3, advancing two parallel-eligible tracks that together realize the operator's **"modular agentic + adaptive + RLHF + improvement"** framing:

- **Track D1 — Federation-Aware Airlock v0.3**: extend the v0.2 airlock standard with federation-substrate awareness (Ed25519 signing-key verification at request acceptance + ledger event observation contract + multi-substrate-network-identity handling for Tailscale + Nebula). LOAD-BEARING on `LatticeNetwork.aDNA` pc_01 Phase A federation substrate (landed 2026-05-20) + Phase B1 dual-substrate ratification (ADR-014/-015). The previously-sketched **D3 AIRLOCK activation kit** (from MC-4.5's 9-stub ecosystem discovery) folds into D1 as the **MD-A3** early mission, packaging the recipe + skill + verification harness consumer vaults need to move airlock stubs from inactive to active.

- **Track D2 — RLHF + Adaptive-Improvement Loop**: codify the modular adaptive improvement loop across III's existing substrate — the 8 composable modules at `what/modules/`, the 7 core domain packs at `what/context/core_domain_packs/`, and the 26-entry graduated learning store (`iii_corrections_canonical.jsonl`, md5 `dde2cbd88c0b45956fb22285a2a0f856`; 5 already graduated to packs through DG-B). Defines per-pack quality measurement, per-finding RLHF signal capture, graduation discipline at scale (≥50 corrections threshold), and cross-vault RLHF aggregation contract (which intersects with D1 via the airlock).

**D1 and D2 are parallel-eligible.** Both depend on the same v0.2 substrate (no cross-track ordering constraint at P1 spec authoring). The cross-track **D1+D2 interface** lands in MD-B3 (cross-vault RLHF aggregation contract) — that aggregation rides on the v0.3 airlock; cadence will likely co-close at DG-D.

## Predecessor

**Campaign C: Airlock Standard v0.2 + Cross-Vault Request Patterns** — DG-C CLOSED 2026-05-20 9/9 (charter `status: completed`; AAR populated inline; production pin `v0.2.0` commit `246124d4176a564df0df2823d6d3bbeba51f9d0a`, annotated tag object `5cd210e81ed20540588ee934e88f5f6fb6f7c17a`). 6 consumer wrappers live (5 at v0.2.0; lattice-labs/iii at v0.1.0 stays tracked-deferred). Canonical learning store md5 `dde2cbd88c0b45956fb22285a2a0f856` invariant. Substrate-implementation guidance at `what/artifacts/iii_airlock_substrate_implementation.md` (490 lines; secrets_handled preflight + idempotency_key dedup). MC-4.5 alignment recon at `what/artifacts/mc4_5_alignment_recon_dossier.md` surfaced the 3 Campaign D candidates ratified by this charter (D1, D2, D3) and surveyed the LN.aDNA pc_01 Phase A federation substrate that D1 absorbs as load-bearing dependency.

## Parallel Eligibility

D1 and D2 are **independently sequencable** with no cross-track ordering constraint at P1 spec authoring — both depend on the same v0.2 substrate (airlock standard spec + module/lattice canon + canonical learning store). The only cross-track contract surface is MD-B3 (cross-vault RLHF aggregation rides on the v0.3 airlock from MD-A1+MD-A2); mitigate via shared sub-mission or co-ratified ADR at MD-B3 opening.

Sibling carry-forwards may interleave at any time without blocking the main tracks:
- `lattice-labs/iii` v0.1.0 → v0.3.0 minor-bump review (folded into D1 MD-A4 wrapper sweep by default)
- VFL-001 + VFL-002 graduation ceremony per `who/coordination/coord_2026_05_12_vfl_graduation_proposals.md` (folded into D2 MD-B2 graduation-at-scale by default)
- 5 TEMPLATE-DH-B promotion-candidate assessment from LL.aDNA Phase 4 S22 (standalone interstitial OR fold into D2 MD-B4 per relevance assessment at intake)

## Inbound Signals

| Source | Path | Content |
|--------|------|---------|
| MC-4.5 dossier §5.2 | `what/artifacts/mc4_5_alignment_recon_dossier.md` | 3-candidate Campaign D sketch (D1 federation-aware airlock v0.3 + D2 RLHF/adaptive loop + D3 activation kit); D1's load-bearing prerequisite (LN pc_01 Phase A substrate); D2's framing source ("modular agentic + adaptive + RLHF + improvement") |
| MC-5 §7 | `what/artifacts/mc5_validation_videoforge_canvasforge_v0_2.md` | 9-item enumerated deferred-to-Campaign-D list (includes both D1 and D2 themes plus all carry-forwards) |
| DG-C AAR Follow-up #6 | `how/campaigns/campaign_c_airlock_standard/campaign_c_airlock_standard.md` lines 113 | Operator-framed Campaign D charter scope; first formal statement of D1+D2+D3 trio |
| Outbound coord — LL Berthier | `who/coordination/coord_2026_05_20_iii_to_ll_airlock_status_post_mc4_5.md` | Fired this charter session (status: draft → sent); 4 informational asks; flags LL.aDNA as plausible D1+D3 Day-1 consumer |
| Outbound coord — LN Venus | `who/coordination/coord_2026_05_20_iii_to_ln_federation_substrate_intersect.md` | Fired this charter session (status: draft → sent); 4 asks include load-bearing substrate-observation contract intersect; flags wga_l1 as Day-1 federation pilot |

## DG-D Criteria (draft — refined as missions land)

- [x] **MD-A1**: Airlock standard spec v0.3 extension authored at `what/artifacts/iii_airlock_standard_spec.md` (federation-substrate aware; Ed25519 signing-key verification contract at request acceptance per §4.6 new sibling subsection; ledger event observation contract per new §5; multi-substrate-network-identity handling per ADR-014/-015 Tailscale + Nebula pluralism) — ✅ **CLOSED 2026-05-21** (`what/artifacts/iii_airlock_standard_spec.md` v0.2.0 → v0.3.0; +§4.6 Federation signing-key verification contract; +§5 Federation-Substrate Awareness with §§5.1-5.5 substrate-pluralism + Ed25519 verification + ledger observation + `COMPLIANCE_AUDIT` event emission shape + multi-substrate identity; +§7.5 federation-substrate version pin; LN ADR-014 + ADR-015 absorbed by reference; ZERO new III ADR per charter — operational extension of existing §4 pattern + cross-cutting awareness section; old §5-§8 renumbered §6-§9; spec grows 428 → ~620 lines)
- [x] **MD-A2**: Implementation guidance v0.3 at `what/artifacts/iii_airlock_substrate_implementation.md` (substrate enforcement extended — sig-verify preflight per §2 extension; ledger-observation hooks per new §X; performance budget for federation-aware preflights) — ✅ **CLOSED 2026-05-21** (impl-doc v0.2.0 → v0.3.0; +§4 Federation Signing-Key Verification (Ed25519) Preflight 9 sub-sections resolves spec §4.6 + §5.2; +§5 Ledger Observation Client 7 sub-sections resolves spec §5.3 with polling-at-60s recommendation; +§6 COMPLIANCE_AUDIT Event Emission 7 sub-sections resolves spec §5.3.1 with assumes_draft direct-JSON-write workaround; old §4 Reply-Comment Template → §7 + Ed25519 rejection block; old §5 Cross-References → §8 + 4 LN ADRs; impl-doc 490 → 1347 lines; spec patch-only 7 mechanical flips RESOLVED at MD-A2; reply-comment template line 60 enum extended with `signature_verification_failed:<5-subreason>`; ZERO new III ADR per MD-A1 consumption-only precedent; canonical jsonl md5 invariant; lattice 1.2.3 unchanged; 6 wrappers untouched)
- [ ] **MD-A3**: AIRLOCK activation kit shipped (D3 absorbed): activation skill + recipe + verification harness packaged for downstream 9-stub ecosystem; moves an `AIRLOCK.md` from inactive stub → active reference instance on a consumer vault's command
- [ ] **MD-A4**: 6 consumer wrappers minor-bump reviewed to v0.3.0 (lattice-labs/iii v0.1.0→v0.3.0 carry-forward included; SiteForge + VideoForge + CanvasForge + wga + LPWhitepaper v0.2.0→v0.3.0)
- [ ] **MD-A5**: Federation-integration validation (re-exercise VideoForge → CanvasForge request against v0.3 schema with federation substrate active; observability check against LN.aDNA ledger; zero regression confirmed against v0.2 baseline)
- [x] **MD-B1**: Adaptive-improvement loop spec + RLHF signal schema + ADR-007-class decision authored (per-pack quality metric definition; per-finding RLHF signal capture schema; loop closure protocol — how a signal becomes a correction, becomes a graduation candidate, becomes a graduated pack delta) — ✅ **CLOSED 2026-05-20** (commit `b1f1bc4`; `what/artifacts/iii_adaptive_improvement_loop_spec.md` v0.3.0 + ADR-005 RLHF Signal Channel + ADR-007 Adaptive-Improvement Loop Architecture)
- [x] **MD-B2**: Graduation discipline at scale (≥50 corrections threshold canonicalized per ADR-003 amendment; VFL-001 + VFL-002 graduation ceremony fired per ADR-003 §3 + ADR-005; canonical learning store md5 changes from `dde2cbd88c0b45956fb22285a2a0f856` to new post-graduation hash with full provenance) — ✅ **CLOSED MD-B2 2026-05-21** (ADR-003 §3.6 added — elevated-scrutiny queue requiring standard §3 + ≥2-vault evidence; canonical C-027 + C-028 appended; md5 rotated to `5adb0dfa38d9224649c3b2cba83852ae`; PACK_DELTA_LANDED for both per operator gate — VFL-001 → `context_iii_introspect_checks.md` Check 2c.i; VFL-002 → `context_iii_inspect_procedures.md` Modality 2 Code Inspect Static trap; VideoForge local store updated; inbound coord memo `open → accepted → absorbed → closed`; reply memo fired; spec forward-refs §3.4 / §7.2 / §7.3 resolved)
- [ ] **MD-B3**: Cross-vault RLHF aggregation contract (uses v0.3 airlock from MD-A1; defines how local downstream `*_iii_learning_store.jsonl` entries can be proposed for upstream canonical graduation across vault boundaries; intersects D1 — co-ratified ADR or shared sub-mission MD-AB-interface)
- [ ] **MD-B4**: 7-domain-pack pilot — execute the adaptive loop across all 7 core packs over a measurement window; produce per-pack quality scores + RLHF signal volumes; surface candidates for next graduation cycle
- [ ] **MD-B5**: Validation across ≥3 consumer vaults (LPWhitepaper natural Day-1 as first 6/7-pack wrapper; plus VideoForge + CanvasForge; or substitute wga_l1 as federation pilot per LN coord intersect)
- [ ] **MD-B6**: DG-D gate + AAR populated inline per Campaign B+C precedent + annotated `v0.3.0` git tag cut (or `v1.0.0` if RLHF loop + federation airlock together represent a major-version milestone; decision deferred to DG-D plan ratification)

## Mission Table (sketched per MC-4.5 §5.2 cadence; refined when each mission opens)

### Track D1 — Federation-Aware Airlock v0.3 (~4-5 missions)

| Mission | Objective | Sessions | Status |
|---------|-----------|----------|--------|
| **MD-A1** | Airlock standard spec v0.3 extension (federation-substrate aware; Ed25519 sig-verify + ledger observation contract + Tailscale+Nebula multi-substrate handling) | 1 | ✅ **COMPLETE 2026-05-21** (single-session close; spec v0.2.0 → v0.3.0 at `what/artifacts/iii_airlock_standard_spec.md`; +§4.6 +§5; LN ADR-014/-015 absorbed by reference; no new III ADR per consumption-only disposition) |
| **MD-A2** | Implementation guidance v0.3 (substrate-enforcement extensions per spec v0.3) | 1 | ✅ **COMPLETE 2026-05-21** (single-session close; impl-doc v0.2.0 → v0.3.0 at `what/artifacts/iii_airlock_substrate_implementation.md`; +§4 Ed25519 + §5 ledger observation polling-at-60s + §6 COMPLIANCE_AUDIT assumes_draft workaround; spec patch-only 7 mechanical flips; reply-comment template line 60 enum extended; ZERO new III ADR per MD-A1 precedent) |
| **MD-A3** | AIRLOCK activation kit (D3 absorbed — recipe + skill + verification harness; 9-stub ecosystem activation enablement) | 0.5-1 | pending |
| **MD-A4** | 6 consumer wrappers minor-bump → v0.3.0 (lattice-labs/iii v0.1.0→v0.3.0 carry-forward absorbed) | 1 | pending |
| **MD-A5** | Federation-integration validation (re-exercise + ledger observability) | 0.5 | pending |

### Track D2 — RLHF + Adaptive-Improvement Loop (~5-7 missions)

| Mission | Objective | Sessions | Status |
|---------|-----------|----------|--------|
| **MD-B1** | Adaptive-improvement loop spec + RLHF signal schema + ADR-007-class decision | 1 | ✅ **COMPLETE 2026-05-20** (commit `b1f1bc4`) |
| **MD-B2** | Graduation discipline at scale (≥50 corrections; VFL-001+VFL-002 ceremony absorbed) | 1 | ✅ **COMPLETE 2026-05-21** (single-session close; ADR-003 §3.6 elevated-scrutiny queue + canonical C-027/C-028 + md5 rotated to `5adb0dfa38d9224649c3b2cba83852ae` + PACK_DELTA_LANDED for both VFL entries per operator gate) |
| **MD-B3** | Cross-vault RLHF aggregation contract (intersects D1; co-ratified ADR or shared sub-mission) | 1 | pending |
| **MD-B4** | 7-pack pilot — execute loop across all 7 core packs; per-pack quality scores | 1-2 | pending |
| **MD-B5** | Validation across ≥3 consumer vaults (LPWhitepaper + 2 others) | 0.5-1 | pending |
| **MD-B6** | DG-D gate + AAR + annotated v0.3.0 (or v1.0.0) tag | 0.5 | pending |

### Standalone (carry-forward; interleavable)

| Mission | Objective | Sessions | Status |
|---------|-----------|----------|--------|
| MD-X1 (optional) | 5 TEMPLATE-DH-B promotion-candidate assessment from LL.aDNA Phase 4 S22 — III-relevance scan + absorption decisions | 0.5 | pending (standalone OR fold into MD-B4) |

Mission ordering within each track is recommendation, not contract. First-mission selection at P1 (MD-A1 spec authoring vs MD-B1 spec authoring) is operator-discretionary at next session open. Co-closes possible at MD-A5 + MD-B6 → DG-D combined commit per MC-5+DG-C precedent.

## Critical Path

**Track D1**: MD-A1 (spec v0.3) → MD-A2 (implementation v0.3) → MD-A3 (activation kit) → MD-A4 (wrapper minor-bump sweep) → MD-A5 (validation) → DG-D close. Sequential within track.

**Track D2**: MD-B1 (loop spec + RLHF schema) → MD-B2 (graduation at scale) → MD-B3 (cross-vault aggregation — intersects D1) → MD-B4 (7-pack pilot) → MD-B5 (≥3-vault validation) → MD-B6 DG-D close + v0.3.0 tag. Sequential within track; MD-B3 cannot ship before MD-A1 v0.3 spec exists.

**Cross-track interface**: MD-B3 depends on MD-A1+MD-A2 substrate (the v0.3 airlock surface). Otherwise tracks are independent.

→ **DG-D gate** at MD-A5 + MD-B6 co-close (combined-gate variant per MC-5+DG-C precedent `1ea1c4d`).

## Risk Register

| # | Risk | Sev | Mitigation |
|---|------|-----|------------|
| R1 | LN.aDNA pc_01 substrate maturity timing (D1 v0.3 spec depends on LN Phase 3+ stability — substrate may evolve during D1 authoring) | HIGH | MD-A1 declares LN-substrate-version pin in spec frontmatter; v0.3 spec versions independently from LN substrate version; rev MD-A1 spec at LN Phase 3+ ratification if needed; fired LN coord memo solicits LN's preferred timing window |
| R2 | RLHF signal schema bloat (D2; per-finding capture schema accumulates fields) | MED | Additive-only contract per ADR-002 precedent; minimum-required-fields baseline + open-extension at consumer side mirroring spec §4.3 cross-vault request schema design |
| R3 | Cross-track contract drift (D1 federation-substrate observation vs D2 cross-vault RLHF aggregation both ride on airlock — schema collisions possible) | MED | Shared sub-mission MD-AB-interface OR co-ratified ADR at MD-B3 opening; explicit cross-track review at MD-B3 plan ratification |
| R4 | Wrapper-fatigue (6 consumer wrappers each get minor-bump review at MD-A4 AND retest at MD-B5) | LOW | Stagger reviews — MD-A4 sweeps wrappers for v0.3 airlock pin; MD-B5 spot-checks RLHF interaction on subset (LPWhitepaper + 2 others, not all 6); precedent: Campaign B P2 shipped 5 wrappers in 2 days without rollbacks |
| R5 | Graduation-ceremony backlog starvation (VFL-001/002 + 5 TEMPLATE-DH-B candidates queued pre-charter; risk they get deprioritized if D1+D2 sprint long) | MED | Folded into D2 MD-B2 + MD-B4 by default; if MD-B2 slips past 1 month from charter, file as standalone interstitial per closure-skill-interstitial precedent |
| R6 | First-canonical-use of `skill_session_close_ceremony.md` reveals procedural gap (MD-A1 close discovers a step the skill missed) | LOW | This charter session is itself the first post-adoption use; if gap surfaces, file as v0.3-eligible skill patch at backlog (`idea_skill_session_close_ceremony.md` already adopted, can amend); already-running closure-skill exemplars provide partial validation (`8235837`, `1ea1c4d`, `c4a5f77`) |

## Phase Plan

| Phase | Name | Missions | Status |
|-------|------|----------|--------|
| **P0** | Charter ratification (this session) — scope locked, 2 coord memos fired, STATE+MANIFEST+router updated, single-commit close per session-close ceremony (first post-adoption canonical application) | (charter only) | ✅ **CLOSED 2026-05-20** (this session) |
| **P1** | Spec authoring (parallel-eligible) — MD-A1 v0.3 airlock spec + MD-B1 adaptive-loop spec + RLHF signal schema | MD-A1, MD-B1 | ✅ **COMPLETE** — MD-B1 ✅ 2026-05-20 (`b1f1bc4`); MD-A1 ✅ 2026-05-21 (this commit) |
| **P2** | Substrate + implementation — MD-A2 v0.3 implementation guidance + MD-A3 activation kit + MD-B2 graduation-at-scale (VFL ceremony fires) | MD-A2, MD-A3, MD-B2 | **partial** — MD-B2 ✅ 2026-05-21 + MD-A2 ✅ 2026-05-21 (both single-session closes); MD-A3 pending |
| **P3** | Integration + pilot — MD-A4 6-wrapper minor-bump sweep + MD-B3 cross-vault RLHF aggregation contract (cross-track interface) + MD-B4 7-pack pilot | MD-A4, MD-B3, MD-B4 | pending |
| **P4** | Validation + DG-D — MD-A5 federation-integration validation + MD-B5 ≥3-vault validation + MD-B6 DG-D gate + annotated v0.3.0 (or v1.0.0) tag | MD-A5, MD-B5, MD-B6 | pending |

## Carry-Forward Disposition

From DG-C AAR Follow-up + MC-5 §7 + MC-4.5 §5.1:

| Carry-forward | Source | Default disposition | Rationale |
|---------------|--------|---------------------|-----------|
| `lattice-labs/iii` v0.1.0 → v0.3.0 minor-bump review | ADR-002 §3 + DG-C AAR Follow-up (2) | **Fold into D1 MD-A4** (wrapper minor-bump sweep) | All 6 consumer wrappers get reviewed at v0.3 cut; lattice-labs/iii joins the sweep rather than being a standalone mission |
| VFL-001 + VFL-002 graduation ceremony | `who/coordination/coord_2026_05_12_vfl_graduation_proposals.md`; DG-C AAR Follow-up (3) | **Fold into D2 MD-B2** (graduation discipline at scale) | Graduation ceremony naturally co-fires with the canonicalization of the ≥50-corrections threshold; both promoted in same ceremony |
| 5 TEMPLATE-DH-B promotion-candidate assessment | LL.aDNA Phase 4 S22; MC-4.5 §3.5; DG-C AAR Follow-up (5) | **Standalone interstitial MD-X1 OR fold into D2 MD-B4** (per relevance) | III-relevance scan deferred to MD-B4 intake; if all 5 candidates III-relevant, fold into MD-B4; if mixed, MD-X1 carves out an assessment-only mission |
| Closure-protocol-drift mitigation | MC-4 lesson via MC-4.5 §7 AAR | **ALREADY SHIPPED 2026-05-20** (`how/skills/skill_session_close_ceremony.md`; carry-forward #1 closed at `c4a5f77`) | No further work; this charter session is the first post-adoption canonical application |

## AAR
<!-- Template: how/templates/template_aar_lightweight.md — campaign-level (multi-line per field per Campaign A + Campaign B + Campaign C precedent; populated at DG-D close, not at P0 charter ratification) -->

(pending; populated at DG-D close)

## Cross-References

- Predecessor campaign: `~/lattice/III.aDNA/how/campaigns/campaign_c_airlock_standard/campaign_c_airlock_standard.md` (DG-C CLOSED 2026-05-20 9/9)
- MC-4.5 alignment recon dossier (§5.2 D1/D2/D3 candidate sketches; §3.4 LN substrate landscape; §3.5 TEMPLATE-DH-B sample assessment): `~/lattice/III.aDNA/what/artifacts/mc4_5_alignment_recon_dossier.md`
- MC-5 validation artifact (§7 9-item deferred-to-Campaign-D enumeration): `~/lattice/III.aDNA/what/artifacts/mc5_validation_videoforge_canvasforge_v0_2.md`
- Airlock standard spec v0.2.0 (extension point for MD-A1): `~/lattice/III.aDNA/what/artifacts/iii_airlock_standard_spec.md`
- Substrate implementation guidance v0.3.0 (closed at MD-A2 2026-05-21): `~/lattice/III.aDNA/what/artifacts/iii_airlock_substrate_implementation.md`
- AIRLOCK.md v0.2.0 (extension point for MD-A1 + activation kit MD-A3): `~/lattice/III.aDNA/how/airlock/AIRLOCK.md`
- Canonical learning store (extension/graduation target for D2): `~/lattice/III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl` (md5 `dde2cbd88c0b45956fb22285a2a0f856` invariant at charter)
- ADR-002 (consumer federation contract; §3 minor-bump review policy): `~/lattice/III.aDNA/what/decisions/adr_002_consumer_federation_contract.md`
- ADR-003 (corrections graduation protocol; §3 ceremony for VFL graduation): `~/lattice/III.aDNA/what/decisions/adr_003_corrections_graduation.md`
- ADR-005 (RLHF Signal Channel; authored at MD-B1 close 2026-05-20 commit `b1f1bc4`): `~/lattice/III.aDNA/what/decisions/adr_005_rlhf_signal_channel.md`
- ADR-007 (Adaptive-Improvement Loop Architecture; authored at MD-B1 close 2026-05-20 commit `b1f1bc4`): `~/lattice/III.aDNA/what/decisions/adr_007_adaptive_improvement_loop.md`
- Session-close ceremony skill (first post-adoption application at this charter session): `~/lattice/III.aDNA/how/skills/skill_session_close_ceremony.md`
- VFL graduation proposal: `~/lattice/III.aDNA/who/coordination/coord_2026_05_12_vfl_graduation_proposals.md`
- LN.aDNA pc_01 Phase A artifacts (D1 load-bearing dependency; read-only context): `~/lattice/LatticeNetwork.aDNA/how/campaigns/campaign_alphalattice_genesis/missions/pc_01*`
- LL.aDNA Phase 3+4 (TEMPLATE-DH-B candidate source): `~/lattice/LatticeLabs.aDNA/how/campaigns/campaign_latticelabs_genesis/`
- Outbound coord memo — III → LL Berthier (fired at this charter ratification): `~/lattice/III.aDNA/who/coordination/coord_2026_05_20_iii_to_ll_airlock_status_post_mc4_5.md`
- Outbound coord memo — III → LN Venus (fired at this charter ratification): `~/lattice/III.aDNA/who/coordination/coord_2026_05_20_iii_to_ln_federation_substrate_intersect.md`
