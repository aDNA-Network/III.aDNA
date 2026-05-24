---
type: template
template_name: cross_vault_graduation_proposal
governs: a consumer vault's cross_vault_request proposing local learning-store entries for upstream canonical graduation
spec_section: what/artifacts/iii_airlock_standard_spec.md §4.3 (payload schema; learning_store_graduation type), §5 (federation gating)
contract: what/decisions/adr_008_cross_vault_aggregation.md
reply_template: how/templates/template_cross_vault_request_reply.md
ship_mission: MD-B3
campaign: campaign_d_federation_adaptive_loop
version: "0.3.0"
created: 2026-05-23
updated: 2026-05-23
last_edited_by: agent_argus
tags: [template, airlock, cross_vault_request, learning_store_graduation, rlhf, aggregation, adr_008, v0_3]
---

# Cross-Vault Graduation Proposal Template

> Reference shape for a consumer vault proposing one or more local `*_iii_learning_store.jsonl` entries for
> upstream canonical graduation at III.aDNA. It is a `cross_vault_request` (airlock spec §4.3) of type
> `learning_store_graduation`, governed by `adr_008_cross_vault_aggregation.md`. III replies on this same memo
> via `template_cross_vault_request_reply.md` (Acceptance / Rejection). The canonical precedent this template
> generalizes is `who/coordination/coord_2026_05_12_vfl_graduation_proposals.md` (VideoForge VFL-001/VFL-002 →
> C-027/C-028).
>
> **Boundary rule (ADR-008 §2):** the proposal transports a *projection* of each local entry — the
> canonical-eligible subset only. NEVER include any `rlhf_consumer_namespace.<vault>.*` field (ADR-005 §3 rule
> 5 — canonical never carries consumer-namespace fields).

## Frontmatter

```yaml
---
type: cross_vault_request
title: "Cross-Vault Request: <ConsumerVault> → III.aDNA — <local_ids> Graduation Proposal(s) (ADR-003 §3 ceremony)"
status: open                       # open | accepted | absorbed | closed | rejected | cancelled
direction: "inbound (III.aDNA receives)"
requesting_vault: <ConsumerVault.aDNA>
requesting_persona: <persona>
receiving_vault: III.aDNA
receiving_persona: argus_panoptes
requesting_agent: <agent_id>
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
priority: low                      # graduation is rarely calendar-urgent; consumer does not block on it
deadline: "no_calendar_urgency"
audit_id: pending                  # III populates on acceptance
artifact_request:
  type: learning_store_graduation  # canonical (ADR-008 §1); registered at airlock §4.3
  source_learning_store: <path to this vault's *_iii_learning_store.jsonl>
  candidate_target: ~/lattice/III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl
  ceremony_authority: ADR-003 §3 (graduation ceremony — frequency ≥ 3 + sessions ≥ 2 + acceptance true + Argus + Stanley co-ratification); add §3.6 if elevated-scrutiny (cross-fork frequency ≥ 50 → requires ≥2-vault independent evidence)
  proposed_entries: [<local_id>, ...]   # e.g. ["VFL-001", "VFL-002"]
constraints:
  max_revisions: 2
  human_gate_required: true
  argus_review_required: true
  stanley_co_ratification_required: true
secrets_handled:
  needed: []
  not_passed: "No secrets cross the boundary; review operates on filed vault content only."
idempotency_key: <vault_short>_graduation_<batch_id>     # airlock §4.5; re-file with same key is a no-op
force_new: false
federation_signature: <base64_ed25519_sig>               # required when consumer + III are co-federated (airlock §4.6); omit otherwise
federation_signature_key_version: <ln_manifest_pin>      # companion to federation_signature
tags: [coordination, cross_vault_request, learning_store_graduation, <local_ids>, adr_003_sec_3_ceremony]
---
```

**Required minimum** (airlock §4.3 + ADR-008 §1): `type: cross_vault_request`, `artifact_request.type:
learning_store_graduation`, `artifact_request.source_learning_store`, `artifact_request.candidate_target`,
`artifact_request.proposed_entries`. Everything else is optional and shaped by the proposal.

## Body sections

The full airlock §4.3 10-section body shape applies. Sections 3–6 are specialized for graduation:

### 1. TL;DR
≤ 3 sentences: which local entries, which patterns, why they now meet ADR-003 §3 criteria, and that there is
(typically) no calendar urgency.

### 2. Request Context
The review history that produced the observations (which missions/sessions; how acceptance was recorded). Cite
the self-review artifacts so Argus can verify per §6.

### 3. Proposed Corrections — the projection table
One block per proposed entry. **Carry only the CROSS fields from ADR-008 §2** — never a consumer-namespace
field. Recommended shape per entry:

```markdown
#### <local_id> — `<pattern>` (trap: `<trap>`)

- **description**: <canonical description>
- **example**: <concrete instance>
- **frequency**: <N> across <M> sessions; **accepted**: true on each
- **source_review / source_finding**: <ids>
- **rlhf_signal_type**: <accept | reject | defer | accept_with_modification>
- **rlhf_session_id**: <session id> · **rlhf_captured_at**: <ISO-8601 UTC>
- **originating_vault**: <ConsumerVault.aDNA>
- **proposed canonical entry** (Argus assigns the C-NNN id at ratification):
  ```jsonl
  {"id":"C-N","trap":"<trap>","pattern":"<pattern>","description":"...","example":"...","source_review":"...","source_finding":"...","frequency":<N>,"accepted":true,"graduated":true,"graduated_to":"core","created":"<YYYY-MM-DD>","graduated_date":"<ratification date>","rlhf_signal_type":"<...>","rlhf_session_id":"<...>","rlhf_captured_at":"<...>"}
  ```
```

### 4. Evidence of Independent Observation (ADR-003 §3.6 / ADR-008 §3)
State whether this pattern has been independently surfaced in other vaults. For standard sub-50 graduation
this is informational; for elevated-scrutiny (cross-fork frequency ≥ 50) it is **required** — name the ≥ 2
distinct vaults and their local-entry ids. If only this vault has observed the pattern, say so explicitly
(III will hold it at `GRADUATION_CANDIDATE` for the §3.6 queue until a second vault proposes; standard §3
graduation may still proceed).

### 5. What III Receives / Does NOT Receive
- **Receives**: the §3 projection (CROSS fields only).
- **Does NOT receive**: any `rlhf_consumer_namespace.<vault>.*` field; the consumer's local id as canonical id;
  forge internals; secrets.

### 6. Acceptance Protocol (for Argus)
The verification path + the lifecycle mapping. Filing this memo = `GRADUATION_PROPOSED`; Argus + Stanley
co-ratification = `GRADUATION_RATIFIED` (canonical append + md5 rotation); pack-trap edit =
`PACK_DELTA_LANDED`. Argus replies via `template_cross_vault_request_reply.md`. Enumerate the artifacts Argus
should read to verify the frequency/acceptance claims.

### 7. Rollback / Cancel
Consumer-side cancel path (withdraw before ratification); III-side rejection path (`request_revisions` or
`reject`); idempotency behavior on re-file.

### 8. Why This Memo
One line: graduation pulls a proven local correction into canonical so other consumer vaults inherit it; the
request pattern (not a direct canonical edit) honors Standing Rule 1 (consumers never write canonical).

### 9. Cross-References
Local learning store; self-review artifacts; ADR-003 §3 (+ §3.6); ADR-008; the consumer's `iii/CLAUDE.md`
routing note; predecessor coord memos.

### 10. Status Log
| Date | Transition | Note |
|------|-----------|------|
| <YYYY-MM-DD> | open | Filed by <persona> at <mission> close; awaiting Argus + Stanley co-ratification. |

## Disposition fields (added by III at close — mirrors the VFL precedent)

When III closes the proposal, it adds to the frontmatter (per the VFL memo precedent):
`closed_at`, `closed_at_mission`, `disposition` (`accept_all` | `accept_partial` | `request_revisions` |
`reject`), `canonical_ids_assigned`, `reply_memo`, `lifecycle_path`, and — only if a graduation actually
fired — `canonical_md5_before` / `canonical_md5_after`.

## Cross-references
- **Contract**: `what/decisions/adr_008_cross_vault_aggregation.md` (boundary-crossing field set; aggregation)
- **Reply template**: `how/templates/template_cross_vault_request_reply.md`
- **Airlock payload schema**: `what/artifacts/iii_airlock_standard_spec.md` §4.3 (+ §5 federation gating)
- **Graduation ceremony**: `what/decisions/adr_003_learning_store_ownership.md` §3 (+ §3.6 elevated-scrutiny)
- **RLHF fields + consumer-namespace exclusion**: `what/decisions/adr_005_rlhf_signal_channel.md` §2, §3 rule 5
- **Lifecycle stages**: `what/decisions/adr_007_adaptive_improvement_loop.md` §1
- **Canonical precedent**: `who/coordination/coord_2026_05_12_vfl_graduation_proposals.md`
