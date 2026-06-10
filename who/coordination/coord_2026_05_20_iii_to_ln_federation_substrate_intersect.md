---
type: coord_memo
direction: outbound
from: III.aDNA (Argus Panoptes)
to: LatticeNetwork.aDNA (Venus)
status: sent   # fired at Campaign D charter ratification 2026-05-20 per session_stanley_20260520_iii_adna_campaign_d_charter close commit; draft → sent direct (no `ready` intermediate per operator pre-commit "fire as single coherent signal at Campaign D charter ratification")
created: 2026-05-20
updated: 2026-05-20
last_edited_by: agent_argus
purpose: federation_substrate_intersect_with_iii_airlock_v0_3_candidate
source: what/artifacts/mc4_5_alignment_recon_dossier.md §4.4 draft 2
materialized_at: 2026-05-20T~21:30Z   # promoted from dossier text to standalone file at MC-4.5-follow-up commit
fired_at: 2026-05-21T00:50Z   # Campaign D charter ratification close commit (operator-frame 2026-05-20)
ack_at:     # populated when LatticeNetwork.aDNA Venus acknowledges
tags: [coord_memo, outbound, draft, mc4_5_follow_up, federation, airlock_v0_3, cross_vault, ln_adna, venus, campaign_d_candidate_d1, load_bearing]
---

# III → LN: federation-substrate intersect with III airlock v0.3+ candidate

> **Materialized 2026-05-20** from `what/artifacts/mc4_5_alignment_recon_dossier.md` §4.4 draft 2 at MC-4.5-follow-up commit. **Status: sent 2026-05-21T00:50Z** (operator-frame 2026-05-20) — fired at Campaign D charter ratification per operator pre-commit ("fire as single coherent signal at Campaign D charter ratification"); see `how/campaigns/campaign_d_federation_adaptive_loop/campaign_d_federation_adaptive_loop.md` §Inbound Signals + §Outbound coord fired at charter. **Load-bearing intersect**: this memo's substrate-observation-contract ask becomes the active dependency for Campaign D Track D1 (MD-A1 spec v0.3 + MD-A2 implementation v0.3).

## Context

III.aDNA Campaign C closed Phase 2 on 2026-05-13 (disk) / 2026-05-20 (git retroactive `c8ce621`); MC-4.5 alignment recon followed at `8235837` on 2026-05-20. MC-4.5 §3.4 surveyed LatticeNetwork.aDNA pc_01 Phase A close (S20-S23 2026-05-20) — wga_l1 4-event admission ceremony complete + Ed25519 federation signing keypair LIVE + first cross-machine ledger emit (COMPLIANCE_AUDIT) + durable bore.pub:42561 tunnel + healthcheck + chain_verify watchdogs + Tailscale enrollment. **The federation substrate that pc_01 Phase A LIVE-deployed is LOAD-BEARING for III's airlock v0.3+ federation-aware extension.** Findings + non-binding asks below.

## Findings

1. **III.aDNA Campaign C P2 closed**; MC-5 = BASELINE (~0.75 sess: re-exercise VideoForge → CanvasForge against v0.2 schema + DG-C gate). v0.2.0 commit `246124d` + annotated tag `5cd210e` remain the production pin. Substrate enforcement guidance (`what/artifacts/iii_airlock_substrate_implementation.md` 490 lines) LIVE for secrets_handled preflight + idempotency_key dedup.
2. **LN pc_01 Phase A's federation substrate is LOAD-BEARING for III airlock v0.3+**:
   - **Ed25519 federation signing keypair LIVE** — III's airlock §4.4 secrets_handled preflight could integrate with signing-key check at v0.3+; cryptographic identity substrate now exists across L1 nodes.
   - **First cross-machine ledger emit (COMPLIANCE_AUDIT)** — III's airlock cross-vault-request pattern (spec §4) could observe and integrate with ledger events at v0.3+; ledger schema becomes a candidate contract for III's audit-log JSONL pattern.
   - **wga_l1 4-event admission ceremony** — analogous to III's airlock entry surface (`how/airlock/AIRLOCK.md` Path A-E); cross-pollination opportunity for v0.3+ "node admission via airlock" pattern.
3. **wga_l1 "premier test/pilot node" status** (operator-declared 2026-05-20 at pc_01 Phase A close) makes `wga.aDNA/iii/` a strong candidate first-tester for federation-aware airlock features. The wga wrapper is at v0.2.0 (current canonical pin); ready to absorb v0.3+ features.
4. **III Campaign D candidate sketches** in MC-4.5 dossier §5.2 — Campaign D candidate D1 (federation-aware airlock v0.3) is the primary intersect with LN.aDNA's pc_01 substrate. Plausible 4-5 mission cadence: MD-1 spec extension (depends on LN.aDNA Phase 3+ stability) → MD-2 implementation guidance → MD-3 6-wrapper minor-bump cadence → MD-4 integration validation → MD-5 DG-D close + v0.3.0 tag. Day-1 consumers: wga (federation pilot per pc_01 §3.4) + VideoForge (existing canonical worked example) + CanvasForge.

## Asks (none binding — purely informational; no action required by Venus)

- **Acknowledge** intersect; flag LN.aDNA's preferred timing for III v0.3 federation-aware planning. Plausible windows: (a) after LN Phase 3 (skeleton) close to stabilize the substrate before III v0.3 spec extension; (b) parallel to LN Phase 4+ if LN bandwidth permits; (c) defer until LN Phase 6 cutover finalizes federation roles.
- **Confirm** whether ledger event substrate would tolerate III airlock observers at v0.3+ (read-only ledger introspection from airlock contracts; no ledger writes from III).
- **Share** any LN-side specs that III v0.3 should mirror: e.g., ledger event JSON schema (COMPLIANCE_AUDIT + future event_types); Ed25519 keypair-rotation contract; admission-ceremony 4-event protocol formalization.
- **Indicate** whether LN.aDNA wants to be invited as a Campaign D Day-1 consumer (D1 federation-aware airlock v0.3) when Campaign D planning opens post-DG-C.

## Cross-references

- III.aDNA MC-4.5 dossier (full landscape survey; §3.4 LN signals; §5.2 D1 candidate sketch): `~/aDNA/III.aDNA/what/artifacts/mc4_5_alignment_recon_dossier.md`
- III.aDNA STATE.md (current state with MC-4.5 close bullet): `~/aDNA/III.aDNA/STATE.md`
- AIRLOCK standard spec v0.2.0 (extension point: §4 cross-vault request patterns; v0.3+ would extend §4.4 secrets_handled to include signing-key verification): `~/aDNA/III.aDNA/what/artifacts/iii_airlock_standard_spec.md`
- Substrate implementation guidance (§2 secrets preflight + §3 idempotency dedup; v0.3+ would add federation-substrate observation): `~/aDNA/III.aDNA/what/artifacts/iii_airlock_substrate_implementation.md`
- LN.aDNA pc_01 Phase A artifacts (read-only context): `~/aDNA/LatticeNetwork.aDNA/how/campaigns/campaign_alphalattice_genesis/missions/pc_01*`
- LN.aDNA AIRLOCK.md stub (currently inactive; v0.1.0 pinned to III ~0.2.0): `~/aDNA/LatticeNetwork.aDNA/how/airlock/AIRLOCK.md`
- wga.aDNA `iii/` consumer wrapper (Day-1 federation pilot candidate): `~/aDNA/wga.aDNA/iii/CLAUDE.md`

## Fire policy

This memo is **draft** per MC-4.5 D3=B Rosetta-default. Operator approves fire at one of three moments:
- MC-5 plan ratification (if scope expands to include LN coord — unlikely given BASELINE recommendation)
- Campaign D planning session (most likely; D1 federation-aware airlock v0.3 opens with this memo firing as Day-1 coord)
- Earlier on operator initiative (e.g., if LN.aDNA's pc_01 Phase B-G work creates a hard intersect)

When fired, `status: draft → ready` (operator review pass) → `status: ready → sent` (commit + LN inbox routing). On LN acknowledgement, `ack_at:` populated.

## Forward signal

Campaign D candidate D1 (federation-aware airlock v0.3) carries this memo's intent forward. The 4-5 mission cadence at MC-4.5 §5.2 names LN.aDNA pc_01 Phase A federation substrate as the load-bearing prerequisite for v0.3 spec extension. Operator-discretionary timing.

---

## Firing record

**Fired at Campaign D charter ratification 2026-05-21T00:50Z** (operator-frame 2026-05-20; commit `67a0936`). Operator pre-committed `draft → sent` direct (no `ready` intermediate) at AskUserQuestion gate in this charter session. Campaign D MD-A1 (v0.3 airlock spec extension) opens as the primary downstream consumer of this memo's load-bearing intersect — LN substrate-observation contract specification is the cross-vault dependency MD-A1 must reconcile. See `~/aDNA/III.aDNA/how/campaigns/campaign_d_federation_adaptive_loop/campaign_d_federation_adaptive_loop.md` §Inbound Signals + §Risk Register R1 (LN substrate maturity timing). `ack_at:` awaits LN Venus session-touch.
