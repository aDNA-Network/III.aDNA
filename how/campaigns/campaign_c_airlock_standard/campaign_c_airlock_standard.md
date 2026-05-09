---
type: campaign
campaign_id: campaign_c_airlock_standard
title: "Campaign C: Airlock Standard v0.2 + Cross-Vault Request Patterns"
status: in_flight
created: 2026-05-08
updated: 2026-05-08
last_edited_by: agent_stanley
phase: P1
predecessor: campaign_a_iii_genesis
parallel_eligible_with: campaign_b_iii_federation
inbound_proposal: who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md
inbound_findings_count: 5
tags: [campaign, airlock, standard, v0_2, cross_vault_request, videoforge_findings]
---

# Campaign C: Airlock Standard v0.2 + Cross-Vault Request Patterns

## Mission

Formalize the III.aDNA airlock pattern as a versioned standard. v0.1.0 (the reference implementation in `how/airlock/AIRLOCK.md`, shipped at MA-4) covers **inbound entry paths** — an external agent enters a vault and runs the loop. v0.2 closes the **cross-vault request** gap surfaced by VideoForge.aDNA when it commissioned the Carly + Herb deck from CanvasForge.aDNA: bidirectional, ephemeral two-agent handshakes for "I need you to do X for me" workflows. Author the canonical standard spec at `what/artifacts/iii_airlock_standard_spec.md`, define the request payload YAML schema, bump AIRLOCK.md to v0.2, define substrate-enforcement preflights, and validate the new standard against the same VideoForge → CanvasForge real-world commission.

## Predecessor

Campaign A: Genesis & Foundation — DG-A CLOSED 2026-05-08 9/9. AIRLOCK.md v0.1.0 reference implementation shipped at MA-4 (commit `1628793`). The v0.1.0 anti-regression section "What v0.1.0 does NOT cover" explicitly defers cross-vault request patterns to this campaign.

## Parallel Eligibility

Campaign B (Federation) and Campaign C (Airlock Standard v0.2) are **independently sequencable**. C is lower-priority since v0.1.0 is shipped and no consumer is blocked. B has a critical-path retiring move (MB-1) that takes precedence. C may run interleaved with B once MB-1 closes, or sequentially after Campaign B.

## Inbound Proposal

`who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md` (committed `1bf3074`). VideoForge.aDNA (Iris) exercised AIRLOCK.md v0.1.0 against a real cross-forge commission and surfaced **5 specific gaps**. The proposal lands as a coord memo because III.aDNA does not yet have a `proposed/` channel (per STATE.md MA-0 close 2026-05-07; ADR-005-style RLHF channel not wired). Findings:

1. **Gap 1 — No request-initiation ceremony.** Coord-memo fallback works ad-hoc but has no schema, acceptance protocol, or rejection protocol.
2. **Gap 2 — No handshake.** Wrappers are unidirectional pull-based; no acceptance/ETA/back-pressure surface.
3. **Gap 3 — No request payload schema.** Every memo invents its own structure.
4. **Gap 4 — No cross-vault secret delegation rules.** No handling when receiving vault's pipeline needs a secret the requesting vault holds (or vice versa).
5. **Gap 5 — No idempotency contract for duplicate requests.** Same request filed twice → both processed independently.

The VideoForge → CanvasForge memo at `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` is the worked example v0.2 must accommodate.

## DG-C Criteria

- [x] **MC-1**: `what/artifacts/iii_airlock_standard_spec.md` authored — codifies entry paths (existing v0.1.0 content) + cross-vault request patterns (VideoForge findings); status `reference_implementation`; version `0.2.0` ✅ **2026-05-08**
- [ ] **MC-2**: Request payload YAML schema canonicalized at `what/artifacts/iii_airlock_request_schema.yaml` (or chosen path); validated against the VideoForge → CanvasForge worked example
- [ ] **MC-3**: `how/airlock/AIRLOCK.md` bumped v0.1.0 → v0.2.0; reply-comment template authored at `how/templates/template_cross_vault_request_reply.md` (handshake — Gap 2)
- [ ] **MC-4**: Substrate-enforcement preflight rules documented for `secrets_handled` (Gap 4) + `idempotency_key` contract (Gap 5); receiver-side dedup logic specified
- [ ] **MC-5**: VideoForge → CanvasForge request re-exercised against v0.2 schema; deltas captured vs the v0.1.0 ad-hoc execution; zero regression confirmed
- [ ] Annotated git tag `v0.2.0` cut at MC-3 closure (or MC-5 if waiting for validation)
- [ ] VideoForge proposal status flipped `open` → `accepted` → `absorbed` → `closed` in `coord_2026_05_08_airlock_v0_2_videoforge_findings.md`

## Mission Table

| Mission | Objective | Sessions | Status |
|---------|-----------|----------|--------|
| **MC-1** | Author `iii_airlock_standard_spec.md` (entry paths + cross-vault request patterns) | 1 | ✅ **COMPLETE 2026-05-08** |
| **MC-2** | Request payload YAML schema; canonical location; worked-example validation | 0.5 | pending |
| **MC-3** | AIRLOCK.md v0.1.0 → v0.2.0; handshake reply-comment template | 1 | pending |
| **MC-4** | Substrate enforcement: `secrets_handled` preflight (Gap 4) + `idempotency_key` (Gap 5) | 1 | pending |
| **MC-5** | Re-exercise VideoForge → CanvasForge against v0.2; capture deltas; flip proposal status | 0.5 | pending |

Sequence is the VideoForge proposal's "Suggested v0.2 Mission Sequencing" verbatim — Argus may reorder during execution; current order is recommendation, not contract.

## Critical Path

MC-1 (spec) → MC-2 (schema) → MC-3 (AIRLOCK.md v0.2 + reply-comment template) — these three are sequential. MC-4 (substrate enforcement) is gate-eligible but lower-bandwidth and could run parallel to MC-3. MC-5 (validation) is sequential after MC-3 + MC-4.

→ **DG-C gate** at MC-5 closure.

## Risk Register

| # | Risk | Sev | Mitigation |
|---|------|-----|------------|
| R1 | v0.2 schema misses a real-world pattern not yet exercised | MED | MC-5 validation against real worked example; future findings (Gap 6+) absorbed in v0.3 cycle |
| R2 | Consumer wrappers (Campaign B) ship before v0.2 → out-of-date AIRLOCK.md sections in consumer wrappers | LOW | Campaign B wrappers point at AIRLOCK.md by version pin; AIRLOCK.md bump triggers minor-bump review per ADR-002 §3 |
| R3 | Substrate-enforcement spec (MC-4) lacks an executable runtime to validate | MED | Spec is documentation-grade in v0.2; first executable enforcement lands in a future Platform.aDNA (e.g., RareHarness) integration |
| R4 | Schema bloat — request payload too heavyweight for trivial 2-forge requests | MED | YAML schema marks Gap 2/4/5 fields as optional; required minimum is Gap 3 core (type + spec_path + output_sink); MC-1 spec calls out lightweight vs full profiles |

## Phase Plan

| Phase | Name | Missions | Status |
|-------|------|----------|--------|
| **P1** | Standard authoring | MC-1, MC-2 | **IN-FLIGHT** — MC-1 ✅ 2026-05-08; MC-2 pending |
| **P2** | Reference implementation + substrate | MC-3, MC-4 | pending |
| **P3** | Validation + ship | MC-5, DG-C gate, v0.2.0 tag | pending |

## What v0.1.0 → v0.2 Promises

The MA-4 AIRLOCK.md v0.1.0 anti-regression section ("What v0.1.0 does NOT cover") makes three commitments to v0.2 that this campaign must honor:

1. **Cross-vault request patterns will land in v0.2 / Campaign C MC-1.** ✅ **DELIVERED 2026-05-08** — `what/artifacts/iii_airlock_standard_spec.md` v0.2.0 §4 codifies cross-vault request schema (initiation, handshake, payload, secrets, idempotency) absorbing all 5 VF gaps.
2. **Until v0.2 ships, cross-vault requests use the coord-memo fallback** with the canonical worked example pointed at the CanvasForge memo. ⏳ Holds until MC-3 ship (AIRLOCK.md v0.2.0).
3. **The VideoForge proposal will be cited and acknowledged in v0.2 spec authoring.** ✅ **DELIVERED 2026-05-08** — spec §8.2 + §8.3 acknowledge proposal in full; all 5 gaps absorbed (no gap deferred); proposal status flipped `open → accepted` at MC-1 close.

## AAR
<!-- Template: how/templates/template_aar_lightweight.md — populate at DG-C closure -->

(pending)

## Cross-References

- Inbound proposal: `~/lattice/III.aDNA/who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md`
- v0.1.0 reference implementation: `~/lattice/III.aDNA/how/airlock/AIRLOCK.md`
- Worked example (cross-forge request): `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md`
- Predecessor campaign: `~/lattice/III.aDNA/how/campaigns/campaign_a_iii_genesis/campaign_a_iii_genesis.md`
- Sibling campaign (parallel-eligible): `~/lattice/III.aDNA/how/campaigns/campaign_b_iii_federation/campaign_b_iii_federation.md`
- VideoForge ADR 003 (cross-graph entry contract; pattern source): `~/lattice/VideoForge.aDNA/what/decisions/adr_003_cross_graph_entry_contract.md`
- VideoForge ADR 005 (RLHF channel; pattern source for `proposed/` mechanic): `~/lattice/VideoForge.aDNA/what/decisions/adr_005_context_learning_rlhf.md`
- Forge canonical spec (entry-time pattern, federation-bound): `~/lattice/SiteForge.aDNA/what/artifacts/sf_forge_pattern_spec.md`
