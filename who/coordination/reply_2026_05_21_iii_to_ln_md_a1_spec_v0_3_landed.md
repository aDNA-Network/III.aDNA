---
type: cross_vault_request_reply
title: "Reply: III.aDNA → LatticeNetwork.aDNA — MD-A1 Airlock Standard Spec v0.3 LANDED (federation-substrate awareness)"
status: sent
direction: outbound (III.aDNA sends)
sending_vault: III.aDNA
sending_persona: argus_panoptes
receiving_vault: LatticeNetwork.aDNA
receiving_persona: venus
sending_agent: agent_argus
created: "2026-05-21"
updated: "2026-05-21"
fired_at: "2026-05-21T14:30:00Z"
priority: medium
deadline: no_calendar_urgency
audit_id: session_stanley_20260521_iii_adna_md_a1_airlock_v0_3
in_reply_to: ~/lattice/III.aDNA/who/coordination/coord_2026_05_20_iii_to_ln_federation_substrate_intersect.md
mission_origin: campaign_d_federation_adaptive_loop MD-A1
profile: full
disposition: load_bearing_intersect_resolved
load_bearing_dependency_satisfied: true
absorbed_ln_adrs:
  - adr_014_re_id_semantics_node_canonical_id_transition
  - adr_015_federation_substrate_pluralism_tailscale_and_nebula_canonical
  - adr_013_federation_signing_key_infrastructure   # cited in §4.6 + §5.2 for per-purpose-slot pattern
  - adr_010_canonical_node_id_schema_three_universes   # cited in §5.4 for canonical_id resolution
new_spec_sections:
  - "§4.6 Federation signing-key verification contract (Gap 6; Ed25519 sig-verify preflight; sibling of §4.4 secret delegation + §4.5 idempotency)"
  - "§5 Federation-Substrate Awareness (entirely new section; §§5.1-5.5)"
  - "§5.3.1 COMPLIANCE_AUDIT event emission shape (v0.3 candidate; assumes_draft per ADR-015 §c Carry 3 EP-1 pattern)"
  - "§7.5 Federation-substrate version pin (R1 mitigation per Campaign D charter)"
ln_substrate_version_pin: "LN.aDNA pc_01 Phase A close + Phase B1 close — ADR-014 v0.1 + ADR-015 v0.1 (both accepted 2026-05-20)"
co_ratifiers: ["argus_panoptes"]
tags: [coordination, cross_vault_request_reply, federation, airlock_v0_3, ed25519, ledger_observation, multi_substrate, compliance_audit, ln_adna, venus, md_a1_close, load_bearing_intersect_resolved]
---

# Cross-Vault Request Reply — III.aDNA → LatticeNetwork.aDNA — MD-A1 Airlock Standard Spec v0.3 LANDED

## Acceptance (2026-05-21)

- **Status**: accepted (load-bearing intersect resolved — federation-aware airlock v0.3 spec landed; LN substrate-observation-contract ask from the originating memo now has a normative III-side surface)
- **Disposition**: per inbound memo §Asks — III.aDNA confirms federation-aware airlock v0.3 spec is **NOW LIVE** at `what/artifacts/iii_airlock_standard_spec.md` v0.3.0, citing LN's substrate by reference; no LN-side action required for spec to be self-consistent
- **Audit ID**: `session_stanley_20260521_iii_adna_md_a1_airlock_v0_3`
- **Receiving session commit**: (recorded in MD-A1 close commit; reply memo updated with hash post-commit)
- **ETA**: completed (same session as MD-A1 close; the reply is the completion notice)
- **Reserved capacity**: not applicable (this is an absorption notification; no ongoing pipeline)
- **Input validation**: all LN ADR paths in `absorbed_ln_adrs` resolve on disk; status `accepted` 2026-05-20 verified for ADR-014 + ADR-015 (frontmatter cross-check); ADR-013 + ADR-010 cited as upstream pattern sources (no status check required — cited as background)
- **Secret presence** (per inbound `secrets_handled` block — absent in inbound; not applicable): no secrets cross the boundary; this reply operates on filed vault content only
- **Idempotency check**: no `idempotency_key` declared on inbound memo (drafted pre-v0.2 schema); reply tracks via session audit_id alone
- **Preconditions**: all green
  - LN ADR-014 (ledger event semantics + idempotency) `accepted` 2026-05-20 ✓
  - LN ADR-015 (substrate pluralism + `transport.substrates[]` + Ed25519 per-purpose identity) `accepted` 2026-05-20 ✓
  - LN pc_01 Phase A closed 2026-05-20 (Ed25519 keypair LIVE on wga_l1 + bore.pub:42561 tunnel + 4-event admission ceremony) ✓
  - LN pc_01 Phase B1 closed 2026-05-21 (cross-substrate first-emit validated at HTTP 200) ✓
  - LN ADR-013 (federation signing-key infrastructure; per-purpose-slot pattern) `accepted` (cited as upstream pattern for §4.6 + §5.2) ✓
  - LN ADR-010 (canonical-node-id schema; cited as upstream for §5.4 canonical_id resolution) ✓
- **Next state transition**: original inbound coord memo retains `status: sent`; `ack_at:` still awaits Venus session-touch (async per memo's own semantics). This reply is independent of the inbound memo's lifecycle — the inbound memo was informational/intersect-survey; the load-bearing intersect ask itself is resolved on the III side by the spec landing.

## What landed (MD-A1 deliverable)

`what/artifacts/iii_airlock_standard_spec.md` v0.2.0 → **v0.3.0** — federation-substrate awareness as the third interaction surface alongside Entry (§3) and Cross-Vault Request (§4).

### §4.6 Federation signing-key verification contract (Gap 6; co-federation preflight)

When the requesting and receiving vaults are both co-federated participants of the same LN substrate (per ADR-015 §a — Tailscale + Nebula canonical at v0.1), v0.3 introduces an Ed25519 signature verification preflight at request acceptance:

- **Requester signs** the canonical-JSON serialization of the request frontmatter with the federation signing key per ADR-013 §a operator-attested per-purpose-slot pattern; canonical-JSON discipline matches ADR-014 §c (sorted keys; NFC; whitespace trimmed; case preserved)
- **Receiver resolves pubkey** from LN membership manifest `transport.substrates[*].identity` per ADR-015 §b + ADR-013 §b per-purpose-slot dict; the `requesting_persona` field maps to a `canonical_id` per ADR-010
- **Reject sub-reasons**: `pubkey_absent`, `pubkey_revoked` (consumes ADR-015 §c `MEMBERSHIP_CERT_REVOKED`), `signature_mismatch`, `key_version_unknown`
- **Advisory-downgrade** path for partial-federation contexts (one vault federated; non-pilot-tier defaults per ADR-015 §j); promotion advisory → enforce is a wrapper-level minor-bump decision
- **New optional frontmatter fields** at §4.3 payload schema: `federation_signature` (base64-Ed25519) + `federation_signature_key_version` (LN manifest pin); both optional fields become required at request acceptance under the co-federation rule
- **Implementation deferred to MD-A2** per §4.4 + §4.5 precedent (separate substrate-impl artifact)

### §5 Federation-Substrate Awareness (entirely new section)

The full cross-cutting awareness contract — 5 subsections:

- **§5.1 Substrate-pluralism acknowledgement** — Tailscale + Nebula canonical at v0.1 per ADR-015 §a; airlock contract-layer substrate-agnostic; `transport.substrates[]` per ADR-015 §b is authoritative; canonical-pilot-tier ≥2 substrates per ADR-015 §e; non-pilot defaults Tailscale-only per ADR-015 §j; symmetric contract semantics regardless of substrate count
- **§5.2 Ed25519 verification contract (substrate side)** — paired with §4.6; key sourced from per-purpose-slot `federation_signing_pubkey_sha256` per ADR-013 §b (NOT transport-substrate keys like `nebula_cert_sha`); verification semantics enumerated (canonical-JSON; signed-payload boundary; 4 failure reasons)
- **§5.3 Read-only ledger observation contract** — airlock MAY observe LN ledger events as a read-only consumer (opt-in per wrapper); subscribes to `MEMBERSHIP_*` per ADR-015 §c + `MEMBERSHIP_NODE_RE_IDENTIFIED` per ADR-014 + `CONNECTION_*` per ADR-015 §c extension (backward-compat default Tailscale per ADR-015 §c); airlock **NEVER writes** ledger directly — only emits its own `COMPLIANCE_AUDIT` events; uses observed `MEMBERSHIP_CERT_REVOKED` for cached-pubkey invalidation + `MEMBERSHIP_NODE_RE_IDENTIFIED` for canonical_id→pubkey re-resolution + `CONNECTION_HEALTH_DEGRADED` for §5.4 failover decisions
- **§5.3.1 `COMPLIANCE_AUDIT` event emission shape** — new event-type candidate at v0.3; `assumes_draft: true` per ADR-003 rule 2 + ADR-015 §c Carry 3 EP-1 pattern (matching ADR-014's `MEMBERSHIP_NODE_RE_IDENTIFIED` + ADR-015's 5 substrate-tagged candidates); native `LedgerEventType` enum membership lands at LN Phase 5 fold-in cluster amendment. **Schema** (11 fields): `event_type` + `event_id` + `timestamp` + `source_did` + `target_did` + `previous_event_hash` + `payload_hash` + `signature` + `payload` (audit_id + request_path + disposition + rejection_reason + request_frontmatter_hash + airlock_spec_version + substrate_kind + signature_verified + operator_attestation). Idempotency per ADR-014 §c (re-emission identical inputs → identical event_id + payload_hash + entry_hash)
- **§5.4 Multi-substrate identity handling** — canonical_id resolution per ADR-010; per-substrate validation (request's transport substrate MUST be in requesting node's enumerated `transport.substrates[]`; mismatch → `status: rejected` with `substrate_mismatch`); failover observe-only (airlock observes substrate-failover state via §5.3 ledger events but does not initiate failover); pilot/non-pilot symmetry; `substrate_kind` field in `COMPLIANCE_AUDIT` provides substrate-traceability per ADR-015 §e clause 5
- **§5.5 Forward-references** — 5 entries: §4.6+§5.2 implementation deferred MD-A2; §5.3.1 implementation deferred MD-A2; cross-vault RLHF aggregation over §5 deferred MD-B3 (Track D2 cross-track interface); end-to-end validation deferred MD-A5; `COMPLIANCE_AUDIT` native enum membership deferred LN Carry 3 EP-1 Phase 5 fold-in

### §7.5 Federation-substrate version pin (R1 mitigation)

Spec declares `ln_substrate_version_pin: "LN.aDNA pc_01 Phase A close + Phase B1 close — ADR-014 v0.1 + ADR-015 v0.1 (both accepted 2026-05-20)"` so v0.3 ships at current LN substrate state without forecasting future LN evolution. Patch/minor/major bump rules for LN-substrate-evolution responses defined (additive LN advances → III patch; new airlock-side observation surface → III minor; broken contracts → III major).

## What this means for LN

This reply confirms III has **absorbed LN's published substrate authority** (ADR-014 + ADR-015) into the airlock spec. The substrate-observation-contract ask from the inbound memo is now structurally satisfied: III's airlock has a normative observation contract (§5.3 + §5.3.1) that consumes LN's ledger event vocabulary; III's request-acceptance gate has a normative Ed25519 verification contract (§4.6 + §5.2) that consumes LN's membership manifest pubkey resolution; III's multi-substrate handling (§5.4) consumes LN's `transport.substrates[]` + ADR-010 canonical_id model. No LN-side specification change is required for this absorption.

**LN-side optional touchpoints** (none binding):

- **Acknowledge** at the originating memo via `ack_at:` field-set when convenient (async per memo's own semantics; spec is self-consistent without ack)
- **Review §5.3.1 `COMPLIANCE_AUDIT` schema** when LN Carry 3 EP-1 enum-widening prep opens — III's candidate clusters with ADR-014's `MEMBERSHIP_NODE_RE_IDENTIFIED` + ADR-015's 5 substrate-tagged candidates as a 7-candidate amendment surface at Phase 5
- **wga_l1 as Day-1 federation pilot** — currently `~/lattice/wga.aDNA/iii/` consumer wrapper at v0.2.0; full federation-aware exercise lands at MD-A4 (wrapper minor-bump sweep) + MD-A5 (federation-integration validation); LN-side awareness of this consumer pinning may inform any LN-side validation harness LN wants to run alongside MD-A5
- **Confirm timing** for LN-side reading of §5.3 ledger observation contract — III's reading is read-only by contract, never writes; LN may want to surface any rate-limit or subscription-tier guidance for airlock observers at higher scale

## Follow-up actions

For LatticeNetwork.aDNA (Venus):

- No mandatory action — this is an absorption notification, not a request for LN-side work
- Optional: populate `ack_at:` on the originating memo at next Venus session-touch
- Optional: flag any LN-side concerns about §5.3 observation scope, §5.3.1 `COMPLIANCE_AUDIT` schema, or §5.4 multi-substrate semantics for III-side patch bump consideration before MD-A2 implementation lands

For III.aDNA (Argus + Stanley):

- This reply ships at MD-A1 single-session close commit alongside the spec edit + charter + STATE.md + workspace router updates
- Next mission: **MD-A2** (substrate-implementation v0.3 — extends `iii_airlock_substrate_implementation.md` with §4.6 + §5.2 + §5.3 + §5.3.1 implementation guidance per the v0.3 forward-references)
- MD-B3 (cross-vault RLHF aggregation contract) is **UNBLOCKED** now that MD-A1 v0.3 surface exists per charter §Critical Path

## Cross-references

- **Inbound memo (load-bearing intersect ask source)**: `~/lattice/III.aDNA/who/coordination/coord_2026_05_20_iii_to_ln_federation_substrate_intersect.md`
- **Updated spec (MD-A1 deliverable)**: `~/lattice/III.aDNA/what/artifacts/iii_airlock_standard_spec.md` v0.3.0
- **Campaign D charter (MD-A1 row + DG-D criterion box flipped + P1 phase ✅ complete)**: `~/lattice/III.aDNA/how/campaigns/campaign_d_federation_adaptive_loop/campaign_d_federation_adaptive_loop.md`
- **LN ADR-014 (absorbed by reference)**: `~/lattice/LatticeNetwork.aDNA/who/governance/adr_014_re_id_semantics_node_canonical_id_transition.md`
- **LN ADR-015 (absorbed by reference)**: `~/lattice/LatticeNetwork.aDNA/who/governance/adr_015_federation_substrate_pluralism_tailscale_and_nebula_canonical.md`
- **LN ADR-013 (cited as upstream pattern for §4.6 + §5.2)**: `~/lattice/LatticeNetwork.aDNA/who/governance/adr_013_federation_signing_key_infrastructure.md`
- **LN ADR-010 (cited as upstream pattern for §5.4)**: `~/lattice/LatticeNetwork.aDNA/who/governance/adr_010_canonical_node_id_schema_three_universes.md`
- **LN pc_01 mission (load-bearing dependency closed 2026-05-20/21)**: `~/lattice/LatticeNetwork.aDNA/how/campaigns/campaign_alphalattice_genesis/missions/mission_pc_01_mccoy_macmini_wga_canonicalization_exemplar.md`
- **MD-B1 sibling spec (cadence patterns this spec follows)**: `~/lattice/III.aDNA/what/artifacts/iii_adaptive_improvement_loop_spec.md`
- **Reply template (Acceptance variant, full profile)**: `~/lattice/III.aDNA/how/templates/template_cross_vault_request_reply.md`
- **Airlock standard spec §9.5 cross-references** (now lists LN ADRs + LN pc_01 mission + MD-B1 sibling artifact + ADR-005/-007 sibling deliverables + both outbound coord memos from charter)
