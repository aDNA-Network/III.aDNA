---
type: skill
skill_name: aggregation_index_intake
governs: III-side intake ceremony for cross-vault `learning_store_graduation` proposals — receive memo, project canonical-eligible fields, normalize join key, check Argus semantic-equivalence, append to receiver-side aggregation index, evaluate ≥2-vault evidence gate, emit reply, audit
contract: what/decisions/adr_008_cross_vault_aggregation.md (§1 transport + §2 boundary-crossing field set + §3 aggregation protocol)
spec_section: what/artifacts/iii_airlock_standard_spec.md §4.3 (cross_vault_request schema; learning_store_graduation type) + §5 (federation gating)
ship_mission: MD-B4
campaign: campaign_d_federation_adaptive_loop
version: "0.3.0"
created: 2026-05-23
updated: 2026-05-23
last_edited_by: agent_argus
related_skills:
  - skill_session_close_ceremony   # ratification commits land via this ceremony
  - skill_iii_review               # the loop that generates the corrections that eventually graduate
  - skill_airlock_activation       # AIRLOCK.md activation prerequisite for federation-mode acceptance
related_templates:
  - template_cross_vault_graduation_proposal   # the inbound memo shape this skill receives
  - template_cross_vault_request_reply         # the outbound memo shape this skill emits
related_adrs:
  - adr_002_consumer_federation_contract   # §3 minor-bump review boundary
  - adr_003_learning_store_ownership       # §3 graduation ceremony + §3.6 ≥50 elevated-scrutiny + ≥2-vault evidence
  - adr_005_rlhf_signal_channel            # §3 rule 5 consumer-namespace exclusion (boundary discipline)
  - adr_007_adaptive_improvement_loop      # §1 CorrectionLifecycle stages this ceremony advances
  - adr_008_cross_vault_aggregation        # the contract this skill implements
tags: [skill, intake, aggregation_index, learning_store_graduation, adr_008, cross_vault, federation, argus_semantic_equivalence, two_vault_gate, mb4_authored]
---

# Skill — Aggregation Index Intake (ADR-008 §3.2 Populating Ceremony)

## Overview

This skill implements the receiver-side populating tool that ADR-008 §3.2 defers to MD-B4. It is the **manual ceremony** III executes when a consumer vault files a `learning_store_graduation` cross-vault request memo (transport per airlock spec §4.3; contract per ADR-008). The skill receives the memo, projects its canonical-eligible field set, normalizes the `(trap, pattern)` join key, checks Argus semantic-equivalence against the existing aggregation index, appends a receiver-side record to the index, evaluates the ≥2-vault evidence gate that ADR-003 §3.6 relies on, emits an Acceptance or Rejection reply, and writes the audit-log record.

**It does not graduate anything.** Filing the proposal advances the affected pattern to `CorrectionLifecycle.GRADUATION_PROPOSED` (per ADR-007 §1). The canonical jsonl mutation that fires `GRADUATION_RATIFIED` is a separate ceremony — `skill_context_graduation.md` + Stanley co-ratification per ADR-003 §3.

The skill is markdown-only (no Python script). Per MD-A2 / MD-B1 / MD-B3 spec-only precedent and per ADR-008 §3.1 which explicitly names Argus semantic-equivalence as "a human/agent gate, not a string match", the populating mechanism is a procedure executed by an agent (or operator + Argus together), not an automated script. Future MD-B5 / DG-D may upgrade to a script if cross-vault proposal volume warrants.

## When to Use

Invoke this skill when III receives a memo filed at `~/aDNA/III.aDNA/who/coordination/coord_<YYYY-MM-DD>_<originating_vault>_<batch_id>.md` whose frontmatter `type:` is `cross_vault_request` AND `artifact_request.type:` is `learning_store_graduation`.

Do NOT invoke for:
- Other `cross_vault_request` types (those routed via their own type-specific intake skills or general airlock intake).
- Standard single-vault graduations originating inside III.aDNA itself (those execute `skill_context_graduation.md` directly with no aggregation-index touch — the index tracks cross-vault evidence only).
- Re-filings of an idempotency-equivalent prior proposal (those reply `duplicate_of` per airlock §4.5 without touching the index — see Procedure §3 below).

## Prerequisites

- The proposal memo file exists at the expected coordination path with `template_cross_vault_graduation_proposal.md` shape.
- `who/coordination/.aggregation/graduation_proposals_index.jsonl` exists (created at MD-B4; mkdir + touch on first invocation if absent, idempotently).
- For federation-mode enforce profiles (per `how/airlock/AIRLOCK.md` frontmatter `federation_mode: enforce`), the proposal carries valid `federation_signature` per airlock §4.6 + impl-doc §4 Ed25519 preflight. If sig-verify fails, reject via `signature_verification_failed:<subreason>` per impl-doc §4.4 enum BEFORE running this skill.
- Argus persona is loaded and operator (Stanley) is present for the §6 semantic-equivalence gate when an existing index entry's pattern key is potentially equivalent to the inbound proposal's pattern key under different naming.

## Inputs

- **`proposal_memo_path`** — absolute path to the received cross-vault request memo
- **`canonical_jsonl_path`** — `~/aDNA/III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl` (READ-ONLY; consulted for `idempotency_key` collision checks only; never written by this skill)
- **`index_jsonl_path`** — `~/aDNA/III.aDNA/who/coordination/.aggregation/graduation_proposals_index.jsonl` (the receiver-side sidecar populated by this skill)
- **`audit_log_path`** — `~/aDNA/III.aDNA/who/coordination/.audit/audit_<YYYY-MM>.jsonl` (the receiver-side audit log; new `event_type: aggregation_intake` per airlock impl-doc §6 schema convention)

## Procedure

### Step 1 — Validate memo shape

Read the proposal memo. Confirm:
- Frontmatter `type:` == `cross_vault_request`
- Frontmatter `artifact_request.type:` == `learning_store_graduation`
- Frontmatter carries `requesting_vault`, `requesting_persona`, `receiving_vault: III.aDNA`, `receiving_persona: argus_panoptes`, `idempotency_key`, `artifact_request.proposed_entries: [<local_id>, ...]`, `artifact_request.source_learning_store: <path>`, `artifact_request.ceremony_authority: ADR-003 §3` (or §3 + §3.6).
- Body section "Proposed Corrections" (or equivalent per template §3) carries one projection table per `proposed_entries` local id.

If any required field absent: emit Rejection reply citing missing-field per `template_cross_vault_request_reply.md` line 60 enum (extend with `malformed_proposal:<missing_field>`). Halt skill.

### Step 2 — Extract projection (boundary discipline per ADR-008 §2)

For each entry in `artifact_request.proposed_entries`:
1. Locate the entry's projection table in the memo body.
2. Verify it transports ONLY the CROSS field set from ADR-008 §2 table:
   `pattern`, `trap`, `description`, `example`, `frequency`, `accepted`, `source_review`, `source_finding`, the 3 ADR-005 §2 required-min RLHF fields (`rlhf_signal_type`, `rlhf_session_id`, `rlhf_captured_at`), `originating_vault`.
3. **REJECT if any `rlhf_consumer_namespace.<vault>.*` field is present** — this is a structural boundary violation per ADR-005 §3 rule 5. Emit Rejection citing `boundary_violation:consumer_namespace_field_leak`. Halt skill.
4. The consumer's local id (e.g., `VFL-001`) does NOT become a canonical id — it persists vault-side per ADR-008 §2 STAY-LOCAL rule. The index records `originating_vault` + `idempotency_key` for traceback.

### Step 3 — Check idempotency (airlock §4.5 + ADR-008 §4)

Compute the `idempotency_key` from the memo frontmatter. Grep the index for an existing record with the same key:

```bash
grep -F "\"idempotency_key\":\"$KEY\"" \
  ~/aDNA/III.aDNA/who/coordination/.aggregation/graduation_proposals_index.jsonl
```

If a matching record exists AND `force_new: false` in the memo: this is a re-file. Emit reply with `duplicate_of: <existing_audit_id>` per airlock §4.5; flip the new memo to `status: cancelled`. **Do NOT append a new index record** (per ADR-008 §4 — re-files do not inflate cross-fork frequency). Halt skill.

If matching record exists AND `force_new: true`: proceed to Step 4 (consumer is explicitly requesting a fresh evaluation, e.g., after pattern semantics changed).

### Step 4 — Normalize join key

For each entry projection (still per ADR-008 §3.1):
- Construct `pattern_key = normalize(trap, pattern)` where `normalize()` lowercases both fields, strips leading/trailing whitespace, and joins with `:` separator. E.g., `forward_link:producer_consumer_pair_unwired`.
- Record the literal `(trap, pattern)` pair alongside the normalized key — Argus may need the literal for semantic-equivalence comparison in Step 5.

### Step 5 — Argus semantic-equivalence gate (per ADR-008 §3.1)

Grep the index for existing records with the same `pattern_key`:

```bash
grep -F "\"pattern_key\":\"$PATTERN_KEY\"" \
  ~/aDNA/III.aDNA/who/coordination/.aggregation/graduation_proposals_index.jsonl
```

For each potentially-equivalent existing record (same `pattern_key` exact match OR semantically-similar pattern names under different lexical forms):

- **Exact-match case**: existing record's literal `(trap, pattern)` equals the inbound's. No semantic-equivalence judgment needed — proceed to Step 6 with this match counted.
- **Lexical-variant case**: existing record has `(forward_link, producer_consumer_pair_unwired)` and inbound is `(forward_link, producer_consumer_unwired_pair)` (or similar near-paraphrase). **Argus makes the call** with operator (Stanley) consultation if uncertain: are these the same pattern?
  - If YES (semantically equivalent): record the merge decision in the reply memo `acceptance_note:` field AND in the new index record's `merged_with: <existing_audit_id>` field. Per ADR-008 §3.1, this is "explicit and auditable".
  - If NO (distinct patterns despite key collision): the two records coexist; no merge.
- **No-match case**: pattern is new to the index. No semantic-equivalence judgment needed — proceed to Step 6 with no prior matches.

Document the semantic-equivalence decision (YES, NO, or N/A) verbatim in the audit-log Step 8 `argus_equivalence_decision:` field.

### Step 6 — Append index record + check ≥2-vault gate

Append a record to the index for each entry projection (one record per proposed local id):

```jsonl
{"pattern_key": "<normalized>", "trap": "<literal>", "pattern": "<literal>", "originating_vault": "<requesting_vault>", "frequency": <int>, "proposal_memo_path": "<relative path>", "idempotency_key": "<from memo>", "status": "<proposal_status>", "received_at": "<ISO-8601>", "merged_with": "<audit_id or null>"}
```

The `status` value evolves over the proposal's lifecycle:
- At first acceptance: `"received"` (or `"received_advisory"` if sig-verify ran in advisory mode per airlock impl-doc §4.5).
- On Stanley ratification: `"ratified_<canonical_id>"` (e.g., `"ratified_C-027_C-028"` for a multi-entry proposal; multiple records can share a single ratification batch).
- On rejection or cancellation: `"rejected_<reason>"` or `"cancelled"`.

The index record is **receiver-side state only** — it never crosses back to the consumer, never enters canonical jsonl, and is safely committable per airlock impl-doc §4.8/§6.5 boundary discipline.

**Then evaluate the ≥2-vault evidence gate** for the pattern_key:

```bash
grep -F "\"pattern_key\":\"$PATTERN_KEY\"" \
  ~/aDNA/III.aDNA/who/coordination/.aggregation/graduation_proposals_index.jsonl \
  | jq -r '.originating_vault' | sort -u | wc -l
```

- **Gate met (`≥2`)**: pattern has independent observation from ≥2 distinct vaults; pattern is eligible for elevated-scrutiny graduation per ADR-003 §3.6 IF its cross-fork frequency (sum of `frequency` across distinct vaults) also meets §3.6's ≥50 threshold. Record the gate-met outcome in the reply memo `evidence_summary:` field.
- **Gate unmet (`1`)**: pattern has only one distinct originating vault; pattern stays `CorrectionLifecycle.GRADUATION_CANDIDATE` and does NOT escalate to elevated-scrutiny. Standard sub-50 graduation per ADR-003 §3 is unchanged and does not require ≥2-vault evidence; this gate matters ONLY for the §3.6 elevated-scrutiny queue.

### Step 7 — Emit reply

Author a reply memo at `who/coordination/reply_<YYYY-MM-DD>_iii_to_<originating_vault>_<batch_id>.md` using `template_cross_vault_request_reply.md`. Variants:

- **Acceptance** — when memo is well-formed, sig-verify passes (or advisory), no boundary violation, and idempotency is fresh: include the `acceptance_note:` with the Argus semantic-equivalence decision, the `evidence_summary:` with the ≥2-vault gate outcome, and the per-entry index record audit_id values. Status flip: original memo `status: open → accepted`.
- **Rejection** — when malformed / sig-verify failed / boundary violated: cite the specific enum value per `template_cross_vault_request_reply.md` line 60. Status flip: original memo `status: open → rejected`.
- **Duplicate-of** — per Step 3: original memo `status: open → cancelled` with `duplicate_of: <existing_audit_id>`.

### Step 8 — Audit-log record

Append one record per memo intake to `~/aDNA/III.aDNA/who/coordination/.audit/audit_<YYYY-MM>.jsonl`:

```jsonl
{"event_type": "aggregation_intake", "audit_id": "<uuid>", "received_at": "<ISO-8601>", "proposal_memo_path": "<relative>", "originating_vault": "<requesting_vault>", "idempotency_key": "<from memo>", "proposed_entry_count": <int>, "disposition": "accepted|rejected|cancelled", "rejection_reason": "<enum value or null>", "pattern_keys_indexed": [<list>], "argus_equivalence_decision": "yes|no|n/a", "merged_with": "<audit_id or null>", "evidence_summary_2_vault_gate": "met|unmet", "reply_memo_path": "<relative>", "airlock_spec_version": "0.3.0", "federation_mode": "enforce|advisory|off", "signature_verified": true|false|null}
```

The audit record is the durable receiver-only state per airlock impl-doc §6.5 boundary discipline. It is safely committable.

## Outputs

- **One or more index records** at `who/coordination/.aggregation/graduation_proposals_index.jsonl` (one per proposed entry, except on duplicate-of which appends nothing).
- **One reply memo** at `who/coordination/reply_<YYYY-MM-DD>_iii_to_<originating_vault>_<batch_id>.md` (Acceptance / Rejection / Duplicate-of variant per Step 7).
- **One audit-log record** at `who/coordination/.audit/audit_<YYYY-MM>.jsonl` (Step 8 shape).
- **Optional status flip** on the original inbound memo per Step 7 (`status: open → accepted | rejected | cancelled`).

**Outputs that this skill does NOT produce**:
- Canonical jsonl mutation — the canonical store is read-only at intake per ADR-008 §5.3.
- Pack-content edits — PACK_DELTA_LANDED stage fires only AFTER ratification, via a separate mission session.
- Coord-memo cascade beyond the single reply — courtesy notifications to other consumers are not auto-fired; operator gates those.

## Seed records (MD-B4 retroactive seeding)

The MD-B4 mission seeded this index with the one real cross-vault graduation precedent: the VFL memo at `~/aDNA/lattice-labs/who/coordination/coord_2026_05_12_vfl_graduation_proposals.md` (VideoForge VFL-001 + VFL-002, ratified at MD-B2 2026-05-21 into C-027 + C-028).

The seed records demonstrate the populator end-to-end without firing any new graduation (the canonical store and its md5 are untouched at MD-B4). Both records are from `originating_vault: VideoForge.aDNA` — so the ≥2-vault gate (Step 6) returns **unmet** for both `pattern_key` values, exercising the gate's protective behavior. Status field `ratified_C-027_C-028` reflects that these were already ratified prior to MD-B4; they are seeded as historical, not as live proposals.

Reading the seed records back through this skill's Step 1–6 procedure is the skill's first end-to-end validation. Result: 2 records present, 1 distinct originating vault → gate unmet → no elevated-scrutiny escalation → no new graduation → canonical md5 invariant. Exactly as expected. Skill validated.

## Anti-patterns

| Anti-pattern | Why it's wrong |
|--------------|----------------|
| Writing index records BEFORE validating boundary (Step 2) | A leaked `rlhf_consumer_namespace.*` field gets pinned in the index; the index becomes a covert-channel pathway. |
| Treating `pattern_key` exact-string-match as authoritative for the ≥2-vault gate | Misses semantic equivalents under different naming. Argus must make the call per ADR-008 §3.1. |
| Auto-graduating when both gates clear | The ≥50 + ≥2-vault gates make patterns *eligible*; canonical mutation still requires Stanley ratification per ADR-008 §5 — never automated. |
| Counting same-vault re-files as additional evidence | Idempotency check (Step 3) prevents this; re-files reply `duplicate_of` and do not append a new index record. ADR-008 §4 explicit. |
| Skipping the audit-log (Step 8) on rejection | The audit log is the durable evidence trail for receiver-only state; rejection paths MUST also emit (with `disposition: rejected` + populated `rejection_reason:`). |
| Editing the canonical jsonl from this skill | Boundary violation per ADR-008 §5.3 and Standing Rule 1 (graduation pulls up via ratification ceremony, never via intake). Canonical mutations happen in `skill_context_graduation.md`, not here. |
| Emitting courtesy notifications to other consumers from this skill | Skill is intake-only. Cross-vault courtesy is operator-gated and lands as a separate coord memo. |

## Cross-references

- **ADR-008** `what/decisions/adr_008_cross_vault_aggregation.md` — the contract this skill implements (§1 transport + §2 boundary-crossing field set + §3 aggregation protocol + §4 idempotency + §5 boundary discipline)
- **ADR-003** `what/decisions/adr_003_learning_store_ownership.md` — §3 standard ceremony gate (frequency ≥ 3 + acceptance ≥ 80%); §3.6 elevated-scrutiny (cross-fork frequency ≥ 50 + ≥2-vault evidence — the gate this skill evaluates at Step 6)
- **ADR-005** `what/decisions/adr_005_rlhf_signal_channel.md` — §2 required-minimum RLHF fields (which cross the boundary per Step 2); §3 rule 5 consumer-namespace exclusion (the boundary violation Step 2 catches)
- **ADR-007** `what/decisions/adr_007_adaptive_improvement_loop.md` — §1 CorrectionLifecycle stages this ceremony advances (filing → `GRADUATION_PROPOSED`; ratification at separate ceremony → `GRADUATION_RATIFIED`)
- **Airlock spec** `what/artifacts/iii_airlock_standard_spec.md` v0.3.0 — §4.3 cross_vault_request schema (where `learning_store_graduation` is registered) + §4.5 idempotency + §4.6 sig-verify preflight + §5 federation gating
- **Airlock impl-doc** `what/artifacts/iii_airlock_substrate_implementation.md` v0.3.0 — §4 Ed25519 sig-verify preflight (executed before this skill); §6 COMPLIANCE_AUDIT emission (separate from this skill's Step 8 receiver-only audit; both schemas coexist)
- **Templates** `how/templates/template_cross_vault_graduation_proposal.md` (inbound memo shape) + `how/templates/template_cross_vault_request_reply.md` (outbound reply shape)
- **Sister skill** `how/skills/skill_context_graduation.md` — the ratification ceremony that, after this intake, executes the canonical jsonl append + md5 rotation per Stanley signature
- **MD-B4 report** `what/artifacts/md_b4_7_pack_pilot_report.md` — the MD-B4 close artifact that initialized this skill + the seed records; §3 RLHF signal-volume table + §4 floor-rule outcomes contextualize the intake surface this skill exposes

## Authoring provenance

- **Mission**: `campaign_d_federation_adaptive_loop` MD-B4 (Track D2, Phase P3)
- **Plan**: `/Users/stanley/.claude/plans/please-read-the-claude-md-pure-patterson.md` (ExitPlanMode 2026-05-23)
- **Session**: `how/sessions/active/session_stanley_20260523_iii_adna_md_b4_7_pack_pilot.md`
- **Resolves**: ADR-008 §3.2 "the populating tool is deferred to MD-B4" forward-reference (RESOLVED 2026-05-23)
