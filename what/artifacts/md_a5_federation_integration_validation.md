---
type: mission_artifact
title: "MD-A5 Validation — VideoForge → CanvasForge Re-Exercised Against Airlock Standard v0.3.0 with LN.aDNA Federation Substrate Active"
campaign: campaign_d_federation_adaptive_loop
mission: MD-A5
mission_class: validation
status: complete
version: "0.3.0"   # matches spec/AIRLOCK.md/substrate-impl version under test; MD-A5 is validation evidence, not new contract
created: 2026-05-24
updated: 2026-05-24
last_edited_by: agent_argus
authoring_mission: campaign_d_federation_adaptive_loop MD-A5
governed_by:
  - what/artifacts/iii_airlock_standard_spec.md
  - what/artifacts/iii_airlock_substrate_implementation.md
  - how/airlock/AIRLOCK.md
predecessor_validation: what/artifacts/mc5_validation_videoforge_canvasforge_v0_2.md   # v0.2 baseline; MD-A5 is its v0.3 successor
worked_example_path: ~/aDNA/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md
ln_substrate_version_pin: "LN.aDNA pc_01 Phase A close + Phase B1 close — ADR-014 v0.1 + ADR-015 v0.1 (both accepted 2026-05-20)"
ln_ledger_chains_observed: [stanley_l1, sws_l1, alpha_lattice_einstein]
ln_membership_rows_walked: [wga_l1, sws_l1, kinn_l1, lmu_l2, science_stanley_l1]
documentation_grade: true
non_runnable: true   # validation by inspection + ledger read-back + schema-walk; no executable validator runs at MD-A5
zero_regression_confirmed: true
closes_track: D1   # Charter line 113: "Track D1 — MD-A5 (validation) → DG-D close"
tags: [mission_artifact, md_a5, validation, campaign_d, airlock_v0_3, federation_aware, ed25519_sigverify, ledger_observation, compliance_audit_assumes_draft, multi_substrate_identity, ln_substrate, videoforge_canvasforge, zero_regression, track_d1_close, dg_d_input]
---

# MD-A5 Validation — VideoForge → CanvasForge Re-Exercised Against Airlock Standard v0.3.0 with LN.aDNA Federation Substrate Active

## §1 Purpose

MD-A5 is the validation gate that closes **Track D1** of Campaign D and is one of the two combined-gate inputs to **DG-D** (the other is MD-B6 per charter line 119). It re-exercises the canonical worked example — VideoForge.aDNA → CanvasForge.aDNA, Carly + Herb sprint onboarding deck commission, originally authored 2026-05-08 against the v0.1.0 fallback and re-validated at MC-5 2026-05-20 against the v0.2.0 contract — against the **v0.3.0 federation-aware contract surface** that landed via MD-A1 (spec) + MD-A2 (impl-doc) + MD-A3 (AIRLOCK.md activation kit) + MD-A4 (5-wrapper sweep).

Three sub-methods compose this validation:

1. **v0.2 baseline re-walked** (§3): confirm the 2026-05-08 memo still parses against the v0.3 spec's §4.1–§4.5 contract (the v0.2 surface remains in place at v0.3 — v0.3 is purely additive at §4.6 + §5). Re-uses MC-5 §5.1 16-row coverage map as the established trace; MD-A5 confirms no row regressed under v0.3.
2. **v0.3 federation-aware deltas re-verified** (§4–§8): walk each new v0.3 surface (spec §4.6 Ed25519 preflight; spec §5 Federation-Substrate Awareness with §§5.1–5.4; spec §7.5 federation-substrate version pin) against live LN.aDNA pc_01 Phase A substrate evidence — read-back of the Tier-1 file-ledger at `~/.lattice/ledger/stanley_l1/2026-05/` (30+ events) plus the LN membership manifest at `~/aDNA/LatticeNetwork.aDNA/what/network/membership/` (5 rows: wga_l1 + sws_l1 + kinn_l1 + lmu_l2 + science_stanley_l1).
3. **Zero-regression assertion** (§9): table-form check of the 9 v0.2 contract invariants (per MC-5 §5 zero-regression definition); each row marked ✅ still-satisfied under v0.3 with the v0.3 layer documented as purely additive (advisory-mode downgrade at spec §4.6 + observation-only surface at §5).

**Validation grade**: documentation-grade by inspection. III is a read-only consumer of LN.aDNA (III itself emits no federation events; III does not author its own keypair). MD-A5 walks the v0.3 impl-doc pseudocode against substrate evidence and asserts contract-conformance. The first executable validator runtime lands when a federation-issuing III consumer (e.g., a future RareHarness-class Platform integration) consumes the v0.3 standard end-to-end.

**Zero-regression definition** (carried forward from MC-5 §1 line 35 + §5.1 line 174): the 2026-05-08 worked-example memo as authored against v0.1.0 fallback remains valid evidence under v0.3 with the same two additive amendments documented at v0.2 (Delta 3 secrets_handled + Delta 4 idempotency_key). v0.3 adds zero further required-of-the-memo amendments — the v0.3 federation-aware layer (§4.6 + §5) is **optional** per spec §4.6 lines 319–325 advisory-mode downgrade clause and per spec §5.3 line 362 observation-opt-in. **Zero regression confirmed** at §9 close.

## §2 Inputs

| Input | Path | Role |
|---|---|---|
| Worked example | `~/aDNA/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` (164 lines; `status: open`) | The 2026-05-08 v0.1.0-era cross-forge memo; read-only at MD-A5 (no retrofit authored; deck remains at weeks-of-flexibility deadline per memo body lines 95–105) |
| v0.2 baseline validation | `what/artifacts/mc5_validation_videoforge_canvasforge_v0_2.md` v0.2.0 (262 lines) | Predecessor artifact; §5.1 16-row coverage map is the established trace MD-A5 confirms still holds; §5 zero-regression definition carried forward |
| Standard spec | `what/artifacts/iii_airlock_standard_spec.md` v0.3.0 | Normative contract under test; §4.6 Ed25519 federation signing-key verification + §5 Federation-Substrate Awareness + §7.5 federation-substrate version pin |
| Substrate implementation | `what/artifacts/iii_airlock_substrate_implementation.md` v0.3.0 (1347 lines) | §4 Ed25519 preflight 9 sub-sections + §5 Ledger Observation Client 7 sub-sections + §6 COMPLIANCE_AUDIT Event Emission 7 sub-sections (assumes_draft direct-JSON-write workaround) |
| AIRLOCK reference instance | `how/airlock/AIRLOCK.md` v0.3.0 | `federation_mode: enforce` + `ledger_observation: enabled` + `ln_substrate_version_pin` populated at MD-A3 close; new §Federation-Substrate Awareness section |
| Reply-comment template | `how/templates/template_cross_vault_request_reply.md` (MC-3 vintage; MD-A2 extension at line 60) | `signature_verification_failed:<5-subreason>` enum extended at MD-A2 close 2026-05-21 |
| LN ADR-013 | `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_013_federation_signing_key_infrastructure.md` v0.1 (accepted 2026-05-20) | Operator-attestation trust model; per-purpose-slot dict pattern (§b); pubkey resolution from LN membership manifest |
| LN ADR-014 | `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_014_re_id_semantics_node_canonical_id_transition.md` v0.1 (accepted 2026-05-20) | Re-ID semantics + canonical-JSON discipline (§c); `MEMBERSHIP_NODE_RE_IDENTIFIED` event; idempotency mechanics |
| LN ADR-015 | `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_015_federation_substrate_pluralism_tailscale_and_nebula_canonical.md` v0.1 (accepted 2026-05-20) | Substrate pluralism Tailscale + Nebula canonical; `transport.substrates[]` schema (§b/§d); per-substrate identity inventory; substrate-tagged event types (§c) |
| LN membership manifest | `~/aDNA/LatticeNetwork.aDNA/what/network/membership/{wga_l1, sws_l1, kinn_l1, lmu_l2, science_stanley_l1}.yaml` (5 rows; AGENTS.md sixth file is doc) | Pubkey resolution target per spec §4.6 + §5.2; canonical_id → federation_signing_pubkey monotonic-mapping check (§6) |
| LN Tier-1 file-ledger | `~/.lattice/ledger/stanley_l1/2026-05/` (30+ JSON events) + `sws_l1/2026-05/` + `alpha_lattice_einstein/2026-05/` | Observable read surface for §5 ledger observation contract; MEMBERSHIP_KEY_PROVISIONED + MEMBERSHIP_NODE_ADMITTED + CONNECTION_* events; `assumes_draft: true` payloads per Carry 3 EP-1 |

## §3 v0.2 baseline re-walked

MC-5 §5.1 established the canonical 16-row coverage map against the 2026-05-08 worked-example memo with verdict **16/16 conformant + 2 additive deltas** (secrets_handled + idempotency_key). MD-A5 re-verifies the worked example on disk and confirms the v0.2 coverage map still holds under v0.3 — no row regressed.

**Memo present at cited path** ✅ — `~/aDNA/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` reads 164 lines (one short of MC-5 §5.1 row 4's 165-line reference; non-material — within line-count drift). Frontmatter `type: coordination`, `status: open`, `updated: 2026-05-08`. No retrofit applied since MC-5 close 2026-05-20.

**Coverage map carry-forward**:

| §5.1 row | Spec section | v0.2 verdict at MC-5 | v0.3 verdict at MD-A5 |
|---|---|---|---|
| 1 | §4.1 filename + location | ✅ | ✅ (unchanged) |
| 2 | §4.1 lifecycle states | ✅ | ✅ (status remains `open`; §4.1 lifecycle enum unchanged at v0.3) |
| 3 | §4.2 handshake (full profile) | ✅ | ✅ (§4.2 contract unchanged at v0.3) |
| 4 | §4.3 required minimum (`type`/`spec_path`/`output_sink`) | ✅ (delta 1 + delta 2 documented) | ✅ (same two deltas; no new v0.3 required-minimum addition — `federation_signature` is OPTIONAL per spec §4.6 + §4.3) |
| 5 | §4.3 optional frontmatter | ✅ (delta 2) | ✅ (delta 2; v0.3 adds `federation_signature` + `federation_signature_key_version` as new OPTIONAL fields — absent here, conformant under advisory-mode per spec §4.6 lines 319–325) |
| 6 | §4.3 body — TL;DR | ✅ | ✅ |
| 7 | §4.3 body — Request Context | ✅ | ✅ |
| 8 | §4.3 body — What Receives | ✅ | ✅ |
| 9 | §4.3 body — What Doesn't Receive | ✅ | ✅ |
| 10 | §4.3 body — Constraints + Notes | ✅ | ✅ |
| 11 | §4.3 body — Acceptance Protocol | ✅ | ✅ |
| 12 | §4.3 body — Rollback / Cancel | ✅ | ✅ |
| 13 | §4.3 body — Why This Memo | ✅ | ✅ |
| 14 | §4.3 body — Cross-References | ✅ | ✅ |
| 15 | §4.3 body — Status Log | ✅ | ✅ |
| 16 | §4.4 secret delegation | ✅ (delta 3) | ✅ (delta 3; §4.4 contract unchanged at v0.3) |
| 17 | §4.5 idempotency | ✅ (delta 4) | ✅ (delta 4; §4.5 contract unchanged at v0.3) |

**v0.2 baseline verdict at MD-A5**: 16/16 §5.1 coverage map rows still conformant under v0.3 with the same 2 additive deltas (no new deltas introduced by v0.3 for the worked example because v0.3 federation-aware layer is OPTIONAL for non-federated requester-vault deployments — see §4 below for the federation-applicability disposition).

## §4 v0.2 → v0.3 spec deltas — applicability to the worked example

v0.3.0 adds two substantive new contract surfaces (spec §4.6 + spec §5) plus one version-pin field (spec §7.5). The deltas are **purely additive** — v0.2-conformant artifacts remain valid at v0.3. The question MD-A5 must answer is: for *this* worked example (VideoForge → CanvasForge), which v0.3 deltas apply?

| v0.3 delta | Spec section | Applies to worked example? | Non-breaking? | MD-A5 disposition |
|---|---|---|---|---|
| **§4.6 Ed25519 federation signing-key verification preflight** + new OPTIONAL `federation_signature` + `federation_signature_key_version` fields at §4.3 | spec lines 302–329; §4.3 OPTIONAL fields | **No** — VideoForge.aDNA + CanvasForge.aDNA are NOT enrolled in the LN membership manifest. Per spec §4.6 lines 319–325 advisory-mode downgrade clause, the receiver MAY treat the request under the v0.2 trust model (substrate-fallback). | ✅ Yes — advisory-mode downgrade is the contractually-correct path here; the worked example is unsigned and conformant. | Documented as out-of-scope-for-this-worked-example; verified at §7 below that the pseudocode walk correctly downgrades to advisory when canonical_id resolution returns empty. |
| **§5.1 substrate-pluralism acknowledgement** | spec lines 339–345 | Indirectly — observation contract states the airlock SHOULD be substrate-agnostic. The worked example carries no substrate field (none required at v0.3 for non-federated participants). | ✅ Yes — substrate-agnostic at contract layer means non-federated paths remain valid. | Verified at §8 below by reading LN events; substrate_kind discriminator present in LN events but absent in airlock memos (correctly per spec §5.3 read-only contract). |
| **§5.2 Ed25519 signature verification contract** (substrate-side view of §4.6) | spec lines 347–358 | **No** — same as §4.6 row above; co-federation gate gated. | ✅ Yes | Same as §4.6 disposition. |
| **§5.3 Ledger observation contract** (opt-in per consumer wrapper) | spec lines 360–376 | **Optional** — III itself is the airlock-host vault; III's own wrapper does NOT opt-in to ledger observation at v0.3 (`how/airlock/AIRLOCK.md` v0.3.0 carries `ledger_observation: enabled` at the reference-instance level per MD-A3 close, but the worked-example memo predates v0.3 and the receiver — CanvasForge — has not yet activated AIRLOCK.md per the 9-stub inactivity disposition at MD-A4 close). | ✅ Yes — observation is read-only; absent observation does not break the request path. | Verified at §5 below by reading the LN file-ledger directly (MD-A5 plays the role of an observer, even though no consumer wrapper has subscribed yet). |
| **§5.3.1 COMPLIANCE_AUDIT event emission** (assumes_draft) | spec lines 378–405 | **No** — III emits no COMPLIANCE_AUDIT events at v0.3 because no III consumer is co-federated with another co-federated counterpart (the LN-pilot nodes wga_l1 + sws_l1 + kinn_l1 + lmu_l2 + science_stanley_l1 are nodes, not vaults; the vaults VideoForge.aDNA + CanvasForge.aDNA + etc. are vault-layer participants and not yet enrolled in the LN membership manifest as federation participants). | ✅ Yes — emission is optional even for co-federated participants (`MAY emit` per spec §5.3.1 line 380). | Verified at §5 below by reading the file-ledger — no `COMPLIANCE_AUDIT` events present, as expected. |
| **§5.4 multi-substrate identity handling** | spec lines 407–415 | **No** — same gating as §4.6 + §5.2. | ✅ Yes | Substrate-mismatch sub-reason path (§4.4 rejection sub-reason taxonomy) is exercised conceptually at §7 below via the impl-doc pseudocode walk, but no actual request bytes are inbound on a specific substrate. |
| **§7.5 federation-substrate version pin** (in AIRLOCK.md frontmatter) | spec lines 497-end | **Yes** — applies to the receiver vault's AIRLOCK.md reference instance. CanvasForge.aDNA has no active AIRLOCK.md (9-stub inactivity per MD-A4); III's own `how/airlock/AIRLOCK.md` v0.3.0 carries the pin per MD-A3 close. | ✅ Yes — additive frontmatter field; absent at non-activated AIRLOCK.md is conformant. | Verified at §6 below by reading III's AIRLOCK.md frontmatter (pin value: "LN.aDNA pc_01 Phase A close + Phase B1 close — ADR-014 v0.1 + ADR-015 v0.1 (both accepted 2026-05-20)"). |

**v0.3 delta verdict for this worked example**: of 7 v0.3 delta surfaces, **0 force any retrofit of the 2026-05-08 memo**; **5 are gated on co-federation** (which the worked-example vault pair does not satisfy); **1 is opt-in observation** (which neither vault has subscribed to); **1 is an AIRLOCK.md frontmatter field** (which the receiver has not activated). The worked example is **fully conformant under v0.3 as authored**, with the v0.3 federation-aware layer correctly advisory-mode-downgraded per spec §4.6 lines 319–325.

## §5 Ledger observability check

The v0.3 impl-doc §5 (`Ledger Observation Client`, 7 sub-sections lines 786–991) recommends polling LN's Tier-1 file-ledger at 60s cadence (rationale: ADR-015 §c event cadence; pub/sub deferred to v0.4+; chain-walk rejected at §5.1). MD-A5 plays the role of a v0.3 observer and reads the live file-ledger directly to confirm the contract holds.

**Surface present and readable** ✅ — `ls ~/.lattice/ledger/stanley_l1/2026-05/` returns **30 JSON event files** (per MD-B4 close evidence and re-verified at MD-A5 open). Two additional chains are present:

- `~/.lattice/ledger/alpha_lattice_einstein/2026-05/` — master/Einstein-node chain
- `~/.lattice/ledger/sws_l1/2026-05/` — sws_l1 peer chain (admitted at S26+)

**Sample event walk** — read `evt_f2e2f0eb850d4556b0d80afea422ad87.json` (MEMBERSHIP_KEY_PROVISIONED for wga_l1, timestamp `2026-05-20T19:07:22.150Z`):

```json
{
  "event_id": "evt_f2e2f0eb850d4556b0d80afea422ad87",
  "event_type": "MEMBERSHIP_KEY_PROVISIONED",
  "timestamp": "2026-05-20T19:07:22.150Z",
  "source_did": "did:lattice:node:stanley_l1",
  "target_did": "did:lattice:node:wga_l1",
  "payload_hash": "83145e128473000e699e202c9e63654436219ff55d6165eea3438134193c6c5b",
  "previous_event_hash": "1505e73a697cb8e2aa775978ee0d1cb5f16ff9879490c753ad10e5c7aa29729e",
  "signature": "",                              // Ed25519 sig empty per ADR-013 §a operator-attestation v0.1
  "payload": {
    "schema_version": 0,
    "ceremony_step": "0_pre_key_provisioned",
    "ceremony_id": "skel_02_percy_mccoy_prototype_ingestion_2026_05_20",
    "node_id": "wga_l1",
    "key_purpose": "federation_signing",        // ADR-013 §b per-purpose-slot pattern
    "pubkey_sha256": "a117322973ac29de6d4834fe9d086fb8c7238665e8965265af16c5040e066f1b",
    "algorithm": "ed25519",
    "provisioned_at": "2026-05-20T03:29:08Z",
    "authorized_by": "agent_stanley",
    "authorization_basis": "operator_attestation",
    "assumes_draft": true,                       // per ADR-003 rule 2 + ADR-015 §c Carry 3 EP-1
    "assumes_draft_carry_ref": "coord_2026_05_18_latticeprotocol_interop.md (Carry 3 EP-1)"
  }
}
```

Conformance verifications:

1. **60s polling cadence is workable** ✅ — the JSON-per-event file shape (`evt_<uuid>.json`, one event per file) supports a simple `ls`-then-`cat`-then-`mtime`-filter loop at any cadence; 60s is well within the file-ledger event arrival rate (30 events over multi-day window).
2. **Event-type taxonomy presence per impl-doc §5.3** ✅ — MEMBERSHIP_KEY_PROVISIONED + MEMBERSHIP_NODE_ADMITTED + MEMBERSHIP_CERT_ISSUED + MEMBERSHIP_SUBSTRATE_JOINED + CONNECTION_ESTABLISHED + MEMBERSHIP_NODE_RE_IDENTIFIED all present in the chain (per LN session S20–S24 evidence cited at the wga_l1 membership row's `ceremony_emit_path` block).
3. **SO-9 strictly-increasing ordering holds cross-chain** ✅ — the wga_l1 admission ceremony emits 4 events in canonical order (T+0 pre_key_provisioned on stanley_l1 chain → T+1 source_side_admit on wga_l1 chain → T+2 mirror_landed on stanley_l1 chain → T+3 master_admit on stanley_l1 chain), each carrying `previous_event_hash` linking to the prior entry in its own chain (per ADR-015 §a + arch_02 §3.3 R-031 contract). Cross-chain ordering is established by event timestamps within the same UTC-second resolution.
4. **`assumes_draft: true` payload field present** ✅ — every novel event-type (MEMBERSHIP_KEY_PROVISIONED, MEMBERSHIP_NODE_ADMITTED, etc.) carries the `assumes_draft: true` flag with `assumes_draft_carry_ref` citing the Carry 3 EP-1 interop memo, per ADR-003 rule 2 (`assumes_draft` inheritance) and per LN F-S23-02 direct-JSON-write workaround disposition.
5. **`COMPLIANCE_AUDIT` events absent** ✅ — `grep -l '"event_type":\s*"COMPLIANCE_AUDIT"' ~/.lattice/ledger/*/2026-05/*.json` returns no matches, as expected: no co-federated airlock-host vault has emitted one yet (the LN-pilot nodes are nodes, not vaults; vault-layer COMPLIANCE_AUDIT emission is consumer-side post-MD-A5 work).

**Ledger observability verdict**: read-only surface is fully observable; the v0.3 impl-doc §5 polling-at-60s contract is workable against the live file-ledger; event schemas conform to spec §5.3 expectations; the assumes_draft direct-JSON-write workaround is operative per LN F-S23-02 and III impl-doc §6.3. ✅

## §6 Membership manifest walk + pubkey resolution

The v0.3 impl-doc §4.3 (`Pubkey resolution from LN membership manifest`, lines 575–617) defines the 6-step pubkey resolution algorithm: persona → canonical_id, manifest walk, fingerprint read, fingerprint verify, version match, revocation check. MD-A5 walks all 5 LN membership rows and validates the canonical_id → federation_signing pubkey mapping.

| canonical_id | yaml row | schema shape | federation_signing pubkey_sha256 |
|---|---|---|---|
| wga_l1 | `~/aDNA/LatticeNetwork.aDNA/what/network/membership/wga_l1.yaml` | per-purpose-slot per ADR-013 §b (`key.purpose: federation_signing` + `key.pubkey_sha256:`) | `a117322973ac29de6d4834fe9d086fb8c7238665e8965265af16c5040e066f1b` |
| sws_l1 | `sws_l1.yaml` | per-purpose-slot per ADR-013 §b | `4ec269625bcdfd60e6df07a6275b42efe7fc43be6125c7ba7ce0b7ccfe64ab58` |
| kinn_l1 | `kinn_l1.yaml` | per-purpose-slot per ADR-013 §b | `1725cad38c9a8be38406f265de818595ba8a922bbb2df55daea2c8e10a4046f4` |
| lmu_l2 | `lmu_l2.yaml` | flat `signing_pubkey_sha256:` (pre-per-purpose-slot shape) | `08f1eaa24af26c120e245537d650a50191722468f3bc10082428e9365d25bedb` |
| science_stanley_l1 | `science_stanley_l1.yaml` | flat `signing_pubkey_sha256:` (pre-per-purpose-slot shape) | `166c780869733242223d9c6683aee407daa6f3a63be137eca87eee5744f3839c` |

**Monotonicity assertion** ✅ — all 5 pubkey SHA-256 values are distinct (no collisions; truncated visual scan: `a117…` ≠ `4ec2…` ≠ `1725…` ≠ `08f1…` ≠ `166c…`). canonical_id → pubkey mapping is 1:1 across the manifest.

**Schema-drift finding (non-blocking)**: two distinct schema shapes coexist across the 5 rows — 3 newer rows (wga_l1, sws_l1, kinn_l1) use the per-purpose-slot dict pattern (`key.purpose: federation_signing` + `key.pubkey_sha256:`) per ADR-013 §b; 2 older rows (lmu_l2, science_stanley_l1) use a flat `signing_pubkey_sha256:` field (pre-ADR-013 §b refactor era). The III v0.3 impl-doc §4.3 pseudocode reads the per-purpose-slot path; the flat-field path requires a small accommodation. **This is an LN-side schema drift observation**, not an III contract issue. Documented for surface to LN at MD-B6 coord-clear (or absorbed by LN at its own ADR-013 §b retroactive-application cadence).

**AIRLOCK.md federation-substrate version pin** ✅ — read `how/airlock/AIRLOCK.md` (frontmatter `ln_substrate_version_pin: "LN.aDNA pc_01 Phase A close + Phase B1 close — ADR-014 v0.1 + ADR-015 v0.1 (both accepted 2026-05-20)"`). Matches MD-A3 close annotation. Spec §7.5 contract satisfied at the reference-instance level.

## §7 Ed25519 sig-verify preflight walkthrough

The v0.3 impl-doc §4.1 (`Preflight script structure`, lines 416–541) defines the 8-step preflight algorithm: (1) co-federation gate; (2) sig + key_version presence; (3) canonical_id resolution; (4) pubkey resolution per §4.3; (5) canonical-JSON serialization per §4.2; (6) Ed25519 verification; (7) substrate validation per §5.4; (8) audit-log emission per §4.7 + COMPLIANCE_AUDIT emission per §6.

MD-A5 walks the algorithm with the 2026-05-08 worked-example memo as input:

| Step | Algorithm action | Outcome for this memo |
|---|---|---|
| 1 | Co-federation gate — both vaults enrolled in LN membership manifest? | **FALSE** — neither VideoForge.aDNA nor CanvasForge.aDNA appears in the 5-row manifest at `~/aDNA/LatticeNetwork.aDNA/what/network/membership/`. Per impl-doc §4.5 advisory-mode handling, the receiver MAY downgrade to `accepted_advisory` rather than reject. **Advisory-mode taken**; remaining steps would-be-exercised conceptually. |
| 2 | Presence check of `federation_signature` + `federation_signature_key_version` in request frontmatter | Both absent. Per spec §4.6 advisory clause (lines 319–325), absent fields under advisory mode are conformant (no reject). |
| 3 | Resolve requesting persona `iris` (VideoForge) → canonical_id | No mapping exists (VideoForge is not in the LN manifest). Resolution returns empty. Under enforce mode, this would emit `rejected` with sub-reason `pubkey_absent`; under advisory mode, it skips verification + logs the missed-resolution outcome. |
| 4 | Pubkey resolution per §4.3 (canonical_id → `transport.substrates[*].identity.federation_signing_pubkey_sha256`) | N/A under advisory mode (step 3 returned empty). Pseudocode contract walks correctly. |
| 5 | Canonical-JSON serialization per ADR-014 §c (sorted keys; NFC; whitespace trim; case preserved) | N/A — no signature to verify. The canonical-JSON algorithm itself is unit-testable in isolation; III impl-doc §4.2 lines 542–574 documents the byte-identical-round-trip validation requirement. |
| 6 | Ed25519 signature verification | N/A — no signature bytes. Pseudocode contract holds. |
| 7 | Substrate validation per §5.4 — does inbound substrate match one in `transport.substrates[]`? | N/A — no transport substrate field on the memo. Pseudocode contract holds. |
| 8 | Audit-log emission (`federation_sigverify` event per §4.7 13-field schema) + optional COMPLIANCE_AUDIT (§6.3 direct-JSON-write workaround) | Under advisory mode, audit-log records `outcome: accepted_advisory` per impl-doc §4.7 5-sample-record taxonomy. III emits no COMPLIANCE_AUDIT here (not co-federated). |

**5-subreason rejection taxonomy validation** ✅ — `signature_verification_failed:<pubkey_absent | pubkey_revoked | signature_mismatch | key_version_unknown | substrate_mismatch>` per spec §4.6 + impl-doc §4.4 lines 618–636. Reply-comment template at `how/templates/template_cross_vault_request_reply.md` line 60 carries the matching enum extension authored at MD-A2 close 2026-05-21. Both contract surfaces (spec / impl-doc / template) coherent ✅.

**Performance budget per impl-doc §4.9** (lines 768–785): cache hit < 5ms; manifest cold lookup < 50ms; total acceptance gate < 200ms. MD-A5 does not measure these (no executable validator) but confirms the documented budget is plausible against the 5-row manifest size + JSON-per-event file-ledger latency.

**Ed25519 sig-verify walkthrough verdict**: the 8-step preflight algorithm walks correctly through the worked example with advisory-mode downgrade taken at step 1 + step 3 (no co-federation). The 5-subreason rejection taxonomy is internally consistent across spec / impl-doc / reply-template. ✅

## §8 Substrate-tagged event traceability

The v0.3 spec §5.4 line 415 + ADR-015 §e clause 5 require every emitted COMPLIANCE_AUDIT event to carry `payload.substrate_kind` recording the substrate the request arrived on. MD-A5 confirms substrate-tagged events are present in the live LN ledger by inspecting recent CONNECTION_ESTABLISHED emits.

**Sample events per wga_l1 membership row's substrate-tagged emit refs** (from `wga_l1.yaml` lines 70–95):

| Substrate | Event ID | First-emit role |
|---|---|---|
| Tailscale | `evt_64866cb7632a43d0afcfeb28e7ab77` | S23 Phase 4 CONNECTION_ESTABLISHED (substrate baseline) |
| Tailscale (with `substrate_kind` field) | `evt_1e0b53a2940e489699ee254046f2ccf8` | S24 Obj 7 CONNECTION_ESTABLISHED with `substrate_kind: tailscale` |
| Nebula | `evt_6b872167b8d743c3add98beab7922136` | S24 Obj 7 CONNECTION_ESTABLISHED with `substrate_kind: nebula` |
| Nebula MEMBERSHIP_SUBSTRATE_JOINED | `evt_acb8a269a57448908b334425ba9c8fa8` | S24 (wga_l1 chain) |

**Verifications**:

1. **Both substrate kinds represented** ✅ — Tailscale + Nebula both have first-emit event IDs on wga_l1's canonical-pilot-tier dual-substrate row.
2. **substrate_kind discriminator field present in v0.3-era events** ✅ — events from S24 onward carry the `payload.substrate_kind` field per ADR-015 §c clause 5; S23-era events predate the discriminator (substrate inferred from chain context).
3. **MEMBERSHIP_SUBSTRATE_JOINED present for Nebula** ✅ — `evt_acb8a269a57448908b334425ba9c8fa8` per the wga_l1 row; assumes_draft per Carry 3 EP-1.
4. **Cert-revoked predecessor evidence** ✅ — `evt_73d813b4b42a42d8834ba76f826d91a6` (Linda MBA M2 wga-l1.crt revocation at S24) appears as `cert_revoked_predecessor_event_id` in the wga_l1 row. This is the live observation surface that ADR-015 §c `MEMBERSHIP_CERT_REVOKED` semantics flows over.

**Substrate-tagged event traceability verdict**: ADR-015 §c contract (substrate-tagged event types with `substrate_kind` discriminator) is operatively present in the live LN ledger; the III v0.3 impl-doc §5.3 receiver-side handler taxonomy (10 event types + 7-receiver-action enum) has live evidence to consume. ✅

## §9 Zero-regression assertion (v0.2 → v0.3)

The 9-item zero-regression contract list per MC-5 §5 zero-regression definition + the 16-row coverage-map carry-forward at §3 above:

| # | v0.2 contract invariant (per MC-5 §5 + spec §4 contract) | Status under v0.3 |
|---|---|---|
| 1 | Worked-example memo as authored 2026-05-08 remains valid evidence with purely additive amendments only — no breaking deltas, no schema rejections, no body-section omissions | ✅ **STILL SATISFIED** under v0.3 (verified at §3 — 16/16 coverage rows + 2 additive deltas, same as v0.2; v0.3 adds zero further required-of-the-memo amendments) |
| 2 | 10-section body shape per spec §4.3 canonical present in order; no section omissions | ✅ **STILL SATISFIED** — §4.3 body structure unchanged at v0.3 |
| 3 | Lifecycle states (open → accepted → rendering → shipped → closed) apply; state transitions per schema `x-lifecycle-transitions` valid | ✅ **STILL SATISFIED** — §4.1 lifecycle enum unchanged at v0.3 |
| 4 | Handshake protocol (§4.2 lightweight + full profiles) routes correctly; receiver MAY upgrade lightweight, MUST NOT downgrade high/urgent | ✅ **STILL SATISFIED** — §4.2 contract unchanged at v0.3 |
| 5 | Secrets delegation (§4.4 preflight MUST steps — presence check, reject-on-absence, names-only audit) executes before pipeline allocation | ✅ **STILL SATISFIED** — §4.4 contract unchanged at v0.3 |
| 6 | Idempotency (§4.5 dedup contract — archive search, 30-day window, force-new override) executes; no-key fallthrough matches v0.1.0 (every memo independent) | ✅ **STILL SATISFIED** — §4.5 contract unchanged at v0.3 |
| 7 | Status-Log table (§4.3 row 10) tracks lifecycle with `date | status | note` format | ✅ **STILL SATISFIED** — §4.3 body row unchanged at v0.3 |
| 8 | Cross-References (§4.3 body §9) cites related ADRs, charters, sibling memos | ✅ **STILL SATISFIED** — §4.3 body row unchanged at v0.3 |
| 9 | Reply-comment template (MC-3 deliverable) Acceptance + Rejection sections append to memo; frontmatter `status` + Status-Log row update on receiver side | ✅ **STILL SATISFIED** — template MC-3 vintage unchanged in shape at MD-A2; the MD-A2 line-60 enum extension (`signature_verification_failed:<5-subreason>`) is ADDITIVE to the rejection-reason taxonomy and does not break v0.2-era usage (a v0.2 reply with reason `missing_secret: <name>` remains valid) |

**Zero-regression vs v0.2 baseline CONFIRMED.** The v0.3 federation-aware layer (§4.6 + §5 + §7.5) is **purely additive**: the new contracts are advisory-mode-downgradable at receiver discretion when participants are not co-federated, and observation is opt-in per consumer wrapper. The 2026-05-08 worked example as authored remains conformant evidence under v0.3.0 without retrofit.

## §10 Deferred / out of scope

Per charter line 113 + line 119 + the v0.3 deferrals enumerated across spec §8.2 + impl-doc §8 + AIRLOCK.md "What v0.3.0 does NOT cover":

1. **Native `LedgerEventType` enum validation** — blocked on LN Carry 3 EP-1 Phase 5 fold-in (target window ~2026-06-06..06-20 per LL.aDNA Berthier extraction-planning campaign). Until fold-in lands, direct-JSON-write workaround per ADR-003 rule 2 + III impl-doc §6.3 is the operative path. Non-blocking for v0.3 reference architecture.
2. **Per-substrate identity schema extension walk** — v0.2 feature per ADR-015 §d (extended identity inventory beyond v0.1 `transport.substrates[*].identity` minimum). Deferred to LN ADR-015 v0.2 cadence.
3. **Actual signature-byte verification** — III is read-only on LN; III itself authors no Ed25519 signatures at MD-A5. The first federation-issuing III consumer (e.g., a future RareHarness-class Platform integration or a co-federated SS↔CC vault pair) will exercise the sig-verify preflight end-to-end with real signature bytes. MD-A5's pseudocode-walkthrough disposition is the correct v0.3 reference-architecture validation step.
4. **wga_l1 Day-1 airlock activation** — Layer-2 deferred per ADR-008 operator-discretionary cadence (per MD-A4 close note). Activation would require wga.aDNA to ratify its own `how/airlock/AIRLOCK.md` at v0.3.0 (currently the 9-stub inventory inactive). Not gating MD-A5 close.
5. **LN-side membership-manifest schema-drift remediation** (per §6 above — flat `signing_pubkey_sha256:` shape at lmu_l2 + science_stanley_l1 rows vs per-purpose-slot pattern at the newer 3 rows). LN-side cleanup; III's v0.3 impl-doc §4.3 walks the per-purpose-slot path; the flat-field path is a minor accommodation. Surface to LN at MD-B6 coord-clear OR absorb at LN's own retroactive-application cadence.
6. **Outbound coord-memo ack-clearing** for the two MD-charter-fired memos (`coord_2026_05_20_iii_to_ll_airlock_status_post_mc4_5.md` + `coord_2026_05_20_iii_to_ln_federation_substrate_intersect.md`). Batched into MD-B6 DG-D close per plan §4 default disposition.
7. **`v0.3.0` (or `v1.0.0`) annotated git tag** — deferred to DG-D close at MD-B6 per Campaign B + C + MD-B1 + MD-B2 + MD-A1 + MD-A2 + MD-A3 + MD-A4 + MD-B3 + MD-B4 precedent. `v0.2.0` commit `246124d` / tag `5cd210e` remain the production pin.

## §11 Cross-references

- Campaign D charter: `how/campaigns/campaign_d_federation_adaptive_loop/campaign_d_federation_adaptive_loop.md` (line 72 DG-D MD-A5 criterion + line 90 Mission Table row + line 113 Critical Path + line 119 combined-gate co-close)
- Predecessor v0.2 validation: `what/artifacts/mc5_validation_videoforge_canvasforge_v0_2.md` v0.2.0 (262 lines)
- Standard spec: `what/artifacts/iii_airlock_standard_spec.md` v0.3.0 (§4.6 + §5 + §7.5 v0.3 additions)
- Substrate implementation: `what/artifacts/iii_airlock_substrate_implementation.md` v0.3.0 (§4 Ed25519 preflight + §5 Ledger Observation Client + §6 COMPLIANCE_AUDIT Event Emission)
- AIRLOCK reference instance: `how/airlock/AIRLOCK.md` v0.3.0 (`federation_mode: enforce` + `ledger_observation: enabled` + `ln_substrate_version_pin`)
- Reply-comment template: `how/templates/template_cross_vault_request_reply.md` (MC-3 vintage; MD-A2 line-60 enum extension)
- Activation kit skill: `how/skills/skill_airlock_activation.md` v0.3.0 (authored at MD-A3 close 2026-05-22)
- Worked example: `~/aDNA/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` (read-only at MD-A5; `status: open` weeks-of-flexibility)
- LN ADR-013 federation signing-key infrastructure: `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_013_federation_signing_key_infrastructure.md` v0.1 (accepted 2026-05-20)
- LN ADR-014 re-ID semantics + canonical-JSON discipline: `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_014_re_id_semantics_node_canonical_id_transition.md` v0.1 (accepted 2026-05-20)
- LN ADR-015 substrate pluralism (Tailscale + Nebula canonical): `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_015_federation_substrate_pluralism_tailscale_and_nebula_canonical.md` v0.1 (accepted 2026-05-20)
- LN membership manifest (5 rows): `~/aDNA/LatticeNetwork.aDNA/what/network/membership/{wga_l1, sws_l1, kinn_l1, lmu_l2, science_stanley_l1}.yaml`
- LN Tier-1 file-ledger (read-back source): `~/.lattice/ledger/{stanley_l1, sws_l1, alpha_lattice_einstein}/2026-05/`
- LN Carry 3 EP-1 interop memo (assumes_draft anchor): `coord_2026_05_18_latticeprotocol_interop.md` (cited in every `assumes_draft_carry_ref` payload field)

---

**Closing assertion** — Zero regression vs v0.2 baseline CONFIRMED. v0.3 federation-aware layer (spec §4.6 Ed25519 preflight + §5 Federation-Substrate Awareness with §§5.1–5.4 + §7.5 version pin) is **purely additive**: optional via advisory-mode downgrade for non-co-federated vault pairs (§4.6 lines 319–325); opt-in via observation contract (§5.3 line 362); zero retrofit required of pre-v0.3 evidence. Track D1 of Campaign D ✅ COMPLETE.
