---
type: decision
adr_id: "005"
title: RLHF Signal Channel — additive per-finding signal capture on the corrections store
status: ratified
created: 2026-05-20
updated: 2026-05-21
last_edited_by: agent_argus
signed_by: Stanley (Chief Steward; ratified at MD-B1 close 2026-05-20 commit b1f1bc4)
supersedes:
superseded_by:
amendments:
  - date: 2026-05-21
    mission: campaign_d_federation_adaptive_loop MD-B2
    summary: Informational note (no semantic change to ADR-005) — the canonical jsonl md5 `dde2cbd88c0b45956fb22285a2a0f856` referenced in §1 (additive-only constraint motivation) + §4 (invariance contract) rotated at MD-B2 close per the §4-specified trigger (ADR-003 §3 graduation ceremony for VFL-001 + VFL-002 → C-027 + C-028). New post-rotation md5 = `5adb0dfa38d9224649c3b2cba83852ae` (first canonical md5 rotation since founding C-001..C-026 import). ADR-005 semantics unchanged — operations defined by this ADR (additive `rlhf_*` fields; consumer namespace; required-min + optional-open partition) do NOT trigger md5 rotation; the rotation was triggered exclusively by ADR-003 §3 ceremony as §4 specifies. The prose in §1 + §4 still cites the founding md5 value as the pre-rotation baseline; readers post-MD-B2 should read those §-references as "the canonical md5 prior to the MD-B2 rotation event" and consult this amendment row for the post-rotation value.
tags: [adr, rlhf, learning_store, signal_capture, adaptive_loop, schema, iii, v0_3]
related_adrs:
  - adr_002_consumer_federation_contract  # §1a kind enum + extension policy pattern source
  - adr_003_learning_store_ownership      # §4 schema this ADR additively extends
  - adr_007_adaptive_improvement_loop     # co-ratified architectural anchor
mission_origin: campaign_d_federation_adaptive_loop MD-B1
governs: per-finding RLHF signal capture during ACCUMULATE; format + storage of rlhf_* fields on corrections entries
---

# ADR-005: RLHF Signal Channel

## Status

Proposed — 2026-05-20 (authored by agent_argus; pending Stanley ratification at MD-B1 close).

## Context

The III loop already captures one binary RLHF-class signal: the `accepted` boolean on each corrections entry (`adr_003_learning_store_ownership.md` §4 lines 75–86), set when a user accepts the IMPROVE phase's suggested fix. This was sufficient through Campaigns A, B, and C — `frequency` accumulation plus `accepted` is the substrate ADR-003 §3 graduation ceremony reads.

Three forces motivate moving past binary:

1. **Real ad-hoc growth in a consumer fork.** `~/aDNA/VideoForge.aDNA/iii/what/context/videoforge_iii_learning_store.jsonl` already carries additive fields not in the ADR-003 §4 schema: `last_updated`, `graduation_proposal_filed`, `graduation_proposal_path`. These were added during VideoForge self-review cycles 2026-05-11..05-12 with no III-side authorization — the schema-bloat risk (Campaign D R2) materializing in real time. Either we normalize the namespace or each consumer invents its own.

2. **Binary accept/reject is too coarse for the adaptive loop.** The operator's Campaign D framing ("modular agentic + adaptive + RLHF + improvement", charter line 35) requires distinguishing accept-as-proposed, accept-with-modification, reject, and defer — at minimum. Without that distinction, MD-B2's "≥50 corrections threshold" (charter line 74) collapses to a count of `accepted: true` entries with no fidelity on partial-acceptance or modification deltas.

3. **MD-B3 cross-vault RLHF aggregation needs a normative schema.** The cross-track interface lands at MD-B3 per Campaign D charter line 75 — but only if D2 has produced something for D1's federation surface to aggregate. Without an ADR ratifying the signal channel, MD-B3 has nothing to design against.

The schema must be **additive-only** (no field renames; no required new fields on existing entries) so the canonical jsonl md5 `dde2cbd88c0b45956fb22285a2a0f856` (ADR-003 §5) remains invariant until a graduation ceremony rotates it. ADR-002 §1a's `kind:` enum + extension policy (`adr_002_consumer_federation_contract.md` lines 85–91) is the closest existing pattern; this ADR mirrors its required-min + open-extension + namespace-discipline structure.

## Decision

### 1. Additive extension of the ADR-003 §4 schema

`rlhf_*`-prefixed fields are added to the existing corrections entry schema. No field in `adr_003_learning_store_ownership.md` §4 lines 65–80 is renamed, removed, or changes type. All `rlhf_*` fields are **optional on existing entries** — absence is valid; the canonical 26-entry store remains byte-identical at md5 `dde2cbd88c0b45956fb22285a2a0f856`.

Pre-existing fields and their relationship to RLHF signals:

| Existing field | Role under ADR-005 |
|----------------|--------------------|
| `accepted` (bool) | Retained as-is. Derivable from `rlhf_signal_type` (`accept` ⊕ `accept_with_modification` ⇒ `accepted: true`; `reject` ⇒ `accepted: false`; `defer` ⇒ `accepted` absent). Parsers MAY back-derive when only one is present. |
| `frequency` (int) | Retained as-is. Counts entry recurrences across sessions; orthogonal to signal type. |
| `source_review`, `source_finding` | Retained; provide forensic provenance for the originating finding from which the signal was captured. |

### 2. Required-minimum and optional-open partition

When an entry captures an RLHF signal, it MUST declare three required-minimum fields:

| Field | Type | Values / Format | Semantics |
|-------|------|------------------|-----------|
| `rlhf_signal_type` | enum | `accept` \| `reject` \| `defer` \| `accept_with_modification` | Discriminator for the signal kind. `defer` distinguishes "user did not act" from `reject` ("user actively rejected"). |
| `rlhf_session_id` | string | matches active session id (e.g. `session_stanley_20260520_iii_adna_md_b1_adaptive_loop_spec`) | Originating review session; ties signal to audit trail at `how/sessions/`. |
| `rlhf_captured_at` | ISO 8601 timestamp | `YYYY-MM-DDTHH:MM:SSZ` (UTC) | Capture instant; distinct from `created` (which is entry-creation date). For first-observation entries, `rlhf_captured_at >= created`. |

Optional-open fields MAY appear; consumers and III-canonical alike may populate any subset:

| Field | Type | When populated |
|-------|------|----------------|
| `rlhf_modification_delta` | string \| object | Required if `rlhf_signal_type: accept_with_modification`; carries the diff or rationale of what the reviewer changed before accepting. String form for short rationales; object form for structured patches. |
| `rlhf_reviewer_persona` | string | Multi-voice review contexts (SiteForge 5-voice; CanvasForge 5-voice). Identifies which voice produced this signal. |
| `rlhf_severity_calibration` | float | Range `0.0–1.0`. Reviewer's confidence in their accept/reject. Used by `module_iii_improve` for severity weighting. |
| `rlhf_cross_session_link` | string | Pointer (entry id or session id) to a prior occurrence of this pattern that surfaced in a different session. Wires cross-session pattern continuity for MD-B3 cross-vault aggregation. |

**Adding new top-level `rlhf_*` fields requires an ADR-005 amendment.** Optional-open is not "anything goes" — it is the four-field set above. Growth requires the same amendment ceremony ADR-002 §1a clause 1 specifies for new `kind:` values.

### 3. Consumer-namespace extension policy

Consumer vaults may declare vault-specific signals under the prefix:

```
rlhf_consumer_namespace.<vault>.<field>
```

`<vault>` is the lowercased vault name without the `.aDNA` suffix (`videoforge`, `siteforge`, `canvasforge`, `wga`, `lattice_labs`, `lpwhitepaper`, `latticelabs`). `<field>` is a snake_case identifier.

Examples (the existing VideoForge ad-hoc fields, normalized retroactively):

```jsonl
{
  ...existing fields...,
  "rlhf_consumer_namespace.videoforge.last_updated": "2026-05-12T19:47Z",
  "rlhf_consumer_namespace.videoforge.graduation_proposal_filed": true,
  "rlhf_consumer_namespace.videoforge.graduation_proposal_path": "who/coordination/coord_2026_05_12_vfl_graduation_proposals.md"
}
```

**Namespace rules**:

1. Consumer-namespace fields never trigger ADR-005 amendment. Consumers freely experiment under their own prefix.
2. A consumer-namespace field that turns out to be domain-general (≥2 vaults independently introduce a semantically-identical field) is a **promotion candidate** for top-level `rlhf_*` status; that promotion requires an ADR-005 amendment per §2.
3. Reading parsers MUST treat unknown namespace fields as opaque preserved-on-write.
4. Writers MUST NOT mint `rlhf_consumer_namespace.<vault>.<field>` entries on behalf of vaults they do not own (no impersonation across consumer boundaries).
5. The III.aDNA canonical store NEVER carries `rlhf_consumer_namespace.*` fields. Canonical-only entries use top-level fields exclusively.

### 4. md5-invariance contract

The canonical jsonl md5 `dde2cbd88c0b45956fb22285a2a0f856` is preserved by ADR-005 trivially: this ADR adds no required field on existing entries, mandates no rewrite of canonical entries, and forbids canonical use of consumer-namespace fields.

The invariant rotates only when:
- A new entry is appended via ADR-003 §3 graduation ceremony (canonical jsonl append + md5 recompute), OR
- An existing entry is modified during a graduation event (e.g., `graduated: false → true` flip on canonical promotion).

Both events are normal ADR-003 §3 operations; this ADR does not add new md5-rotation triggers.

### 5. Extension policy

Mirrors `adr_002_consumer_federation_contract.md` §1a "Extension policy" clauses, adapted for signal fields:

1. **New top-level `rlhf_*` fields require an ADR-005 amendment.** The proposer coordinates via the v0.2 cross-vault request surface (`iii_airlock_standard_spec.md` §4) to authorize; then proposes the amendment co-registering the new field in §2 above.
2. **New consumer-namespace fields are additive.** Consumer vaults declare additional `rlhf_consumer_namespace.<vault>.<field>` entries without amendment; no III-side authorization needed.
3. **Co-registration at minor bump.** A consumer-namespace field that promotes to top-level (per §3 clause 2) lands its amendment at or before the next III.aDNA minor version bump — same backstop as ADR-002 §1a clause 3.
4. **Required-minimum field set is closed.** Adding new required-minimum fields requires (a) an ADR-005 amendment, AND (b) an ADR-003 amendment if it changes the canonical jsonl schema invariants. Two-ADR amendment for the rare case the loop needs a new always-required signal.
5. **No retroactive backfill.** Existing entries (canonical or consumer-fork) are NOT modified to add `rlhf_*` fields retroactively. New entries created after ADR-005 ratification populate the required-minimum; older entries remain valid without them. (Parsers MAY back-derive `rlhf_signal_type` from `accepted` per §1 when only the latter is present.)

## Consequences

### Positive
- VideoForge's existing ad-hoc fields (`last_updated`, `graduation_proposal_filed`, `graduation_proposal_path`) have a normative home retroactively under `rlhf_consumer_namespace.videoforge.*`; no breaking migration required.
- MD-B2's "≥50 corrections threshold" can use `rlhf_signal_type` distribution counts (not just `accepted: true`) for finer-grained graduation gating.
- MD-B3 cross-vault RLHF aggregation has a stable schema to design over; v0.3 airlock from MD-A1 can normatively reference ADR-005 signals.
- `module_iii_improve` gains an additive input (`rlhf_severity_calibration` distribution) for severity calibration beyond the current `learning_store_acceptance_rates`.
- Schema-bloat risk (R2) capped: required-minimum=3 fields; optional-open=4 fields; consumer namespace unbounded but isolated; canonical untouched.

### Negative
- Parsers reading entries that mix old-shape (binary `accepted`) and new-shape (`rlhf_signal_type`) must handle both — adds modest implementation complexity.
- Two-ADR amendment for new required-minimum fields (§5 clause 4) is friction; intentional, to prevent silent loop-protocol drift.
- Consumer-namespace fields are opaque to III canonical; cross-vault aggregation (MD-B3) must explicitly schema-promote them to be cross-vault visible. Trade-off: protects consumer autonomy.

### Neutral
- This ADR does not change the canonical learning store byte representation; md5 invariant preserved.
- `accepted` (bool) field retained for backward compatibility; new entries SHOULD populate both `rlhf_signal_type` and `accepted` for parser compatibility through Campaign D.

## Amendment History

| Date | Mission | Amendment summary |
|------|---------|-------------------|
| 2026-05-20 | campaign_d_federation_adaptive_loop MD-B1 | Initial ratification at MD-B1 close (commit `b1f1bc4`). |
| 2026-05-21 | campaign_d_federation_adaptive_loop MD-B2 | Informational note — the canonical jsonl md5 `dde2cbd88c0b45956fb22285a2a0f856` referenced in §1 + §4 rotated at MD-B2 close per the §4-specified trigger (ADR-003 §3 graduation ceremony for VFL-001 + VFL-002 → C-027 + C-028). New md5 = `5adb0dfa38d9224649c3b2cba83852ae` (first canonical md5 rotation since founding C-001..C-026 import). ADR-005 semantics unchanged. §1 + §4 prose still cites the founding md5 as the pre-rotation baseline; readers should consult this amendment row + STATE.md + reply memo (`who/coordination/reply_2026_05_21_iii_to_videoforge_vfl_graduation_ratified.md`) for the post-rotation value. ADR-005 §3 rule 5 (canonical never carries `rlhf_consumer_namespace.*` fields) verified at MD-B2 close — C-027 + C-028 carry only top-level RLHF fields; namespace fields remain in VideoForge local store. |
