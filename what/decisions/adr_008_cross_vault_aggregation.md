---
type: decision
adr_id: "008"
title: Cross-Vault RLHF Aggregation Contract — boundary-crossing field set, proposal transport over the v0.3 airlock, ≥2-vault evidence aggregation
status: accepted
created: 2026-05-23
updated: 2026-05-25
last_edited_by: agent_argus
signed_by: Stanley (Chief Steward) — ratified at MD-B3 close 2026-05-23
supersedes:
superseded_by:
amendments:
  - { date: 2026-05-25, mission: MD-B6, summary: "§2 one-paragraph temporal note re: C-001..C-028 pre-ADR-008 originating_vault:null (F2 disposition; preserves canonical jsonl md5 invariant)" }
tags: [adr, cross_vault_aggregation, rlhf, learning_store_graduation, federation, airlock_v0_3, iii, v0_3, md_b3]
related_adrs:
  - adr_002_consumer_federation_contract  # the wrappers whose local stores propose upstream; §3 minor-bump review
  - adr_003_learning_store_ownership       # §3 graduation ceremony + §3.6 ≥2-vault evidence rule this contract feeds
  - adr_005_rlhf_signal_channel            # the RLHF fields that cross + §3 rule 5 consumer-namespace exclusion
  - adr_007_adaptive_improvement_loop      # CorrectionLifecycle stages; SIGNAL_CAPTURED + CORRECTION_TRACKED are the cross-vault-visible join points
mission_origin: campaign_d_federation_adaptive_loop MD-B3
governs: which learning-store fields cross the vault boundary in a graduation proposal; the cross_vault_request transport for proposals; III-side same-pattern evidence aggregation feeding ADR-003 §3.6
---

# ADR-008: Cross-Vault RLHF Aggregation Contract

## Status

Accepted — 2026-05-23 (authored by agent_argus; ratified by Stanley at MD-B3 close).

## Context

III's adaptive-improvement loop lets a consumer vault accumulate corrections in its own
`*_iii_learning_store.jsonl` fork and — when a pattern proves out — graduate it upstream into the canonical
store. The mechanics of a *single-vault* graduation are settled: ADR-003 §3 defines the ceremony
(`frequency ≥ 3` across `≥ 2` sessions, `acceptance ≥ 80%`, Argus + Stanley co-ratification); ADR-007 §1
names the lifecycle stages it threads through (`GRADUATION_PROPOSED → GRADUATION_RATIFIED →
PACK_DELTA_LANDED`); ADR-005 defines the RLHF signal fields a correction carries.

Three gaps remain, all *cross-vault*:

1. **No contract for what crosses the boundary.** A consumer's local entry carries both canonical-eligible
   data (`pattern`, `trap`, RLHF signals) and consumer-private bookkeeping (`rlhf_consumer_namespace.<vault>.*`
   per ADR-005 §3). ADR-005 §3 rule 5 forbids the latter from ever entering canonical, but no document says
   which fields a *proposal* transports versus which stay vault-local. The MD-B1 adaptive-loop spec §5.3
   emits this exactly as a forward-reference to MD-B3.

2. **No contract for how proposals travel.** The one real cross-vault graduation to date — VideoForge's
   VFL-001/VFL-002, ratified into C-027/C-028 at MD-B2 — travelled as a hand-authored coord memo
   (`coord_2026_05_12_vfl_graduation_proposals.md`). It happened to use `type: cross_vault_request` with
   `artifact_request.type: learning_store_graduation`, but nothing made that shape normative. The airlock
   spec §5.5/§8.3 defer "cross-vault RLHF aggregation over the §5 contracts" to MD-B3.

3. **No contract for aggregation.** ADR-003 §3.6 (added at MD-B2) requires that high-volume patterns
   (cross-fork frequency ≥ 50) carry independent observation evidence from **≥ 2 distinct vaults** before
   elevated-scrutiny graduation. That rule is unenforceable without a mechanism for III to *collect and count*
   same-pattern proposals across vaults. ADR-003 §3.6's own worked-example note says the first such candidate
   surfaces "when cross-fork aggregation lands at MD-B4 or later" — but the *contract* for that aggregation is
   undefined.

ADR-008 closes all three. It is the D1+D2 cross-track interface the Campaign D charter names: the
federation-aware airlock (Track D1) is the **transport**; the RLHF correction (Track D2) is the **payload**.

**Scope boundary.** ADR-008 defines the *contract*. It does NOT execute aggregation across the 7 packs (that
is MD-B4) and fires NO graduation this mission (the canonical store and its md5 are untouched at MD-B3).

## Decision

### 1. Proposal transport — a `cross_vault_request` of type `learning_store_graduation`

A consumer proposes graduation by filing a cross-vault request per the airlock spec §4.3 schema, with:

```yaml
type: cross_vault_request
receiving_vault: III.aDNA
receiving_persona: argus_panoptes
artifact_request:
  type: learning_store_graduation          # canonical value registered at airlock spec §4.3 by MD-B3
  source_learning_store: <path to the consumer's *_iii_learning_store.jsonl>
  candidate_target: ~/lattice/III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl
  ceremony_authority: ADR-003 §3 (+ §3.6 if elevated-scrutiny)
  proposed_entries: [<local_id>, ...]       # e.g. ["VFL-001", "VFL-002"]
idempotency_key: <vault_short>_graduation_<batch_id>
constraints:
  human_gate_required: true
  argus_review_required: true
  stanley_co_ratification_required: true
```

The value `learning_store_graduation` is **canonical** (it is the value the VFL precedent used; MD-B3 registers
it as a named member of the airlock §4.3 open `artifact_request.type` enum). The reusable memo shape is
`how/templates/template_cross_vault_graduation_proposal.md`. This makes the VFL memo a conforming instance of
the contract *retroactively* — the contract must explain the one precedent it generalizes.

The full §4.3 10-section body shape applies; the template specializes sections 3–6 for the proposal context
(Proposed Corrections projection table; Evidence of Independent Observation; What III Receives / Does Not
Receive; Acceptance Protocol mapping to the lifecycle transitions).

### 2. Boundary-crossing field set (resolves adaptive-loop spec §5.3)

A graduation proposal transports a **projection** of each proposed local entry — the canonical-eligible
subset, never the consumer-private subset.

**CROSS the boundary** (canonical-eligible; map to top-level canonical fields per ADR-003 §4 schema):

| Field | Source | Canonical role |
|-------|--------|----------------|
| `pattern` | local entry | pattern equivalence-class id (`snake_case`) — the join key (§3) |
| `trap` | local entry | trap class |
| `description` | local entry | canonical description |
| `example` | local entry | concrete instance |
| `frequency` | local entry | per-vault observation count (summed across vaults at §3) |
| `accepted` | local entry | acceptance evidence (ADR-003 §3 ≥80% gate) |
| `source_review` / `source_finding` | local entry | provenance of the observations |
| `rlhf_signal_type` | ADR-005 §2 required-min | the RLHF signal (`accept`/`reject`/`defer`/`accept_with_modification`) |
| `rlhf_session_id` | ADR-005 §2 required-min | originating session |
| `rlhf_captured_at` | ADR-005 §2 required-min | ISO-8601 capture time |
| `originating_vault` | the request `requesting_vault` | provenance; recorded at ceremony per ADR-003 §4 note |

**STAY vault-local** (MUST NOT cross — per ADR-005 §3 rule 5):

- Every `rlhf_consumer_namespace.<vault>.*` field (e.g. `graduation_proposal_filed`, `graduation_note`,
  `graduated_to_canonical_id`, `last_updated`). These are receiver-opaque bookkeeping; the canonical store
  **never** carries them. The proposal projection is the local entry with its consumer namespace stripped.
- The consumer's local id (`VFL-001`) does not become the canonical id; Argus assigns the next `C-NNN` at
  ratification. The local id persists vault-side for backward compatibility (ADR-003 §3.6 evidence trail
  references it).

Per ADR-007 §1, only the `SIGNAL_CAPTURED` + `CORRECTION_TRACKED` stages are cross-vault-visible — a proposal
carries the *observations* (those stages' data); the `GRADUATION_*` stages are III-canonical-only and are
entered by III, not asserted by the requester.

**Temporal note (added MD-B6 2026-05-25; F2 disposition).** Canonical entries `C-001` through `C-028` all
graduated *before* ADR-008's ratification (2026-05-23) and therefore carry `originating_vault: null` as their
documented historical state. `originating_vault` is a required boundary-crossing projection from the
`requesting_vault` of a `learning_store_graduation` cross_vault_request (§1's transport); pre-ADR-008
graduations had no such transport and no field to project from. The single retroactive case — VFL-001 +
VFL-002 (VideoForge.aDNA) graduating into C-027 + C-028 at MD-B2 (2026-05-21, two days before ADR-008) — has
its provenance recoverable via `who/coordination/.aggregation/graduation_proposals_index.jsonl` seed records
authored at MD-B4 (2026-05-23) per `how/skills/skill_aggregation_index_intake.md`. The canonical jsonl md5
`5adb0dfa38d9224649c3b2cba83852ae` (rotated at MD-B2) is **preserved by design** — backfilling C-027/C-028
would (a) misrepresent them as post-ADR-008 graduations and (b) break the invariant Campaign D protected
across 8 mission closures. Post-MD-B6, every new graduation transported via `learning_store_graduation`
MUST populate `originating_vault` per the §2 boundary-crossing field set (unchanged contract). For the §3
≥2-vault evidence aggregation, C-027/C-028's null does NOT count toward any future gate-distinct-vault
total (they pre-date the field's enforcement; future evidence comes from future proposals).

### 3. III-side aggregation protocol (feeds ADR-003 §3.6 ≥2-vault evidence)

When III receives a `learning_store_graduation` request, it aggregates by pattern across all received
proposals:

1. **Pattern-identity join key** = normalized `(trap, pattern)`. When two vaults name the same pattern
   slightly differently, **Argus makes the semantic-equivalence call** (a human/agent gate, not a string
   match) and records the merge in the ratification record. Names may differ; the equivalence decision is
   explicit and auditable.

2. **Aggregation index** — III maintains a receiver-side sidecar at
   `who/coordination/.aggregation/graduation_proposals_index.jsonl`. One record per received proposal:
   `{pattern_key, trap, originating_vault, frequency, proposal_memo_path, idempotency_key, status, received_at}`.
   The index is **receiver-side state** — it is NOT part of the crossing payload, NOT in canonical, and
   (mirroring the audit-log boundary discipline of the airlock impl-doc §4.8/§6.5) is safely committable.
   The index's schema is normative here; the populating tool is deferred to MD-B4.

3. **Cross-fork frequency** = the sum of `frequency` across **distinct originating vaults** for one pattern
   key. This is the quantity ADR-003 §3.6's ≥50 threshold measures. Same-vault re-files (matching
   `idempotency_key`) do not double-count (§4).

4. **Independent-observation gate** (the aggregation's reason for existing): a pattern satisfies ADR-003 §3.6's
   "≥ 2 distinct vaults" requirement iff the index holds proposals for its pattern key from ≥ 2 distinct
   `originating_vault` values. A pattern with proposals from only one vault — regardless of its accumulated
   frequency — stays `GRADUATION_CANDIDATE` and does NOT clear elevated scrutiny until a second vault
   independently proposes it. (Standard sub-50 graduation per ADR-003 §3 is unchanged and does not require
   ≥2-vault evidence; aggregation matters specifically for the §3.6 elevated-scrutiny queue.)

### 4. Idempotency + lifecycle mapping

- **Idempotency**: the proposal carries `idempotency_key` per airlock §4.5 (VFL precedent
  `vfl_graduation_proposals_m_3_04_close`). On a matching re-file with `force_new: false`, III replies
  `duplicate_of` and the re-file flips to `cancelled` — the index record is not duplicated and the
  cross-fork frequency (§3.3) is not inflated.
- **Lifecycle** (per ADR-007 §1): filing the proposal = `GRADUATION_PROPOSED`; Argus + Stanley
  co-ratification = `GRADUATION_RATIFIED` (canonical append + md5 rotation); pack-trap edit =
  `PACK_DELTA_LANDED`. The receiver replies via the existing
  `how/templates/template_cross_vault_request_reply.md` (Acceptance / Rejection variants); a federation
  sig-verify failure rejects with `signature_verification_failed:<sub-reason>` per §5.

### 5. Boundary discipline — what this contract does NOT permit

- **Consumers never write canonical.** A proposal is a *request*; III ratifies (Standing Rule 1 — graduation
  pulls up, never pushes). There is no path by which a consumer's edit reaches `iii_corrections_canonical.jsonl`
  without an III-side ratification ceremony.
- **No auto-graduation.** Meeting frequency/acceptance/≥2-vault evidence makes a pattern *eligible*; the
  `human_gate_required` + `stanley_co_ratification_required` constraints still gate the actual canonical
  append.
- **No canonical mutation at proposal time.** The canonical store and its md5 rotate only at an actual
  ratification ceremony. Receiving, indexing, and aggregating proposals are read-and-record operations on
  receiver-side state; they never touch canonical. (MD-B3 itself fires zero graduations — md5
  `5adb0dfa38d9224649c3b2cba83852ae` is invariant across this mission.)
- **Federation gating** (per §5 of the airlock spec): when requester and receiver are co-federated, the
  proposal MUST carry a valid `federation_signature` (Ed25519, per airlock §4.6); III verifies before
  accepting. Partial-federation requesters use the advisory-downgrade path. `COMPLIANCE_AUDIT` emission on
  proposal disposition is optional per airlock §5.3.1.

## Consequences

### Positive
- The VFL graduation is no longer a one-off — any consumer can file a conforming `learning_store_graduation`
  request and know exactly what crosses, what stays local, and how III will adjudicate it.
- ADR-003 §3.6's ≥2-vault rule becomes enforceable: the aggregation index gives III a concrete count of
  distinct-vault evidence per pattern.
- MD-B4's 7-pack pilot inherits a defined intake surface (the index schema) to populate; MD-B5's ≥3-vault
  validation has a contract to exercise.
- ADR-005 §3 rule 5 is honored structurally — the projection (§2) is defined as the local entry *minus* its
  consumer namespace, so the contract cannot leak consumer-private fields into canonical.

### Negative
- The pattern-identity join (§3.1) is an Argus judgment call, not a mechanical match — two vaults can describe
  the same pattern under different names and a merge can be missed or mis-made. Deliberate trade-off:
  semantic equivalence across vaults is not reliably string-decidable; the decision is made explicit and
  auditable rather than automated.
- The aggregation index is receiver-side state that must be kept consistent with the proposal memo corpus;
  divergence (a memo closed without an index update) reintroduces a smaller version of the disk-vs-record
  drift the session-close ceremony guards against. Mitigated by deferring the populating tool to MD-B4 with
  the index schema fixed here.

### Neutral
- No change to the ADR-003 §4 canonical entry schema, to ADR-005's field set, or to the canonical jsonl —
  the contract reuses existing fields and adds only receiver-side state (the index) plus a registered airlock
  request-type value.
- The new request type is additive to the airlock §4.3 open `type` enum; no airlock spec version bump (the
  contract surface lives here in ADR-008, not in an airlock reshape).

## Amendment History

| Date | Mission | Amendment summary |
|------|---------|-------------------|
| 2026-05-23 | MD-B3 | Initial ratification at MD-B3 close. Stanley signed. |
| 2026-05-25 | MD-B6 | §2 one-paragraph temporal note (F2 disposition): canonical C-001..C-028 graduated pre-ADR-008 carry `originating_vault: null` as documented historical state; VFL-001/-002 → C-027/C-028 provenance recoverable via aggregation-index seed records authored at MD-B4. Preserves canonical jsonl md5 `5adb0dfa38d9224649c3b2cba83852ae` invariant (rotated at MD-B2; held across 8 Campaign D closures). Non-breaking; no data rewrite; no version bump per consumption-only precedent. |
