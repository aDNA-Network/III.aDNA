---
type: module_companion
module_ref: module_iii_accumulate
created: 2026-05-08
updated: 2026-05-08
last_edited_by: agent_stanley
status: active
tags:
  - module
  - agent-module
  - iii-review
  - accumulate
  - learning-store
fair:
  findable:
    keywords:
      - accumulate
      - learning store
      - corrections jsonl
      - graduation candidates
      - iii loop
    identifier: "III.aDNA/module_iii_accumulate/1.0.0"
  accessible:
    access_protocol: "vault file read"
    authentication: none
  interoperable:
    input_types: [structured, context_pack, path]
    output_types: [structured]
    standards: ["aDNA Module Spec v1.0", "III Review Skill v1.0", "ADR-003 Graduation Protocol", "ADR-005 RLHF Signal Channel", "ADR-007 Adaptive-Improvement Loop"]
  reusable:
    license: "Apache-2.0"
    provenance: "Decomposed from skill_iii_review Step 3b + context_iii_learning_store; III.aDNA MA-3 2026-05-08"
    version: "1.0.0"
migration_provenance:
  previous_home: "lattice-labs (composite via module_iii_semantic_reviewer)"
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-3
---

# Module: III Accumulate

> Post-approval phase of the III pipeline. Walks the user's accept/reject decisions on the `ImprovementTable` and writes a `LearningStoreDelta` to the consumer-vault local corrections store: incrementing frequencies on matched recurring patterns, appending new pattern entries with `frequency: 1`, and skipping one-off factual errors. Surfaces graduation signals (`frequency ≥ 3` AND `acceptance ≥ 80%`) for the ADR-003 ceremony. Never writes to the canonical store — that path runs through Stanley-gated graduation only.

## Specification

| Field | Value |
|-------|-------|
| **Module type** | `agent` (Claude-as-runtime) |
| **Runtime** | Claude (Opus) |
| **Tier** | L1 (local) |
| **GPU** | Not required |
| **Memory** | 1024 MB |
| **Timeout** | 60s |
| **Lattice node** | `accumulate` (downstream of `human_review`, upstream of `corrections_store`) |

## Inputs

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `improvement_decisions` | structured | yes | Per-row user accept/reject from human_review: `{finding_id, decision: accept\|reject, decision_rationale?}` |
| `findings` | structured | yes | Original `Finding[]` for pattern-extraction (recurring vs one-off classification) |
| `learning_store_schema_pack` | context_pack | yes | `context_iii_learning_store.md` — entry schema, lifecycle protocol, accumulation rules |
| `consumer_local_store_path` | path | yes | Where to write the delta — `<consumer_vault>/iii/what/context/<vault>_iii_corrections_local.jsonl` |
| `canonical_store_path` | path | yes | Read-only — `III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl`. Used for match check; never written |
| `tool_handles` | config | yes | `{read: Read, write_append: Write}` — passed by skill |

## Outputs

| Name | Type | Description |
|------|------|-------------|
| `corrections_jsonl_delta` | structured | Operation list: `[{op: append\|update_frequency\|update_acceptance, entry_id?, new_entry?, target_path}]` — applied by skill via `tool_handles.write_append` |
| `graduation_signals` | structured | Corrections meeting `frequency ≥ 3` AND `acceptance_rate ≥ 80%` — fires CorrectionLifecycle GRADUATION_CANDIDATE transition (ADR-007 §1 stage 3); flagged for ADR-003 §3 ceremony |
| `accumulate_report` | structured | `{n_appended, n_updated, n_skipped_one_offs, n_graduation_candidates, target_store}` |

## Behavior

Per `skill_iii_review.md` Step 3b and `context_iii_learning_store.md` lifecycle protocol:

1. For each `(finding, decision)` pair where the finding represents a *recurring pattern* (not a one-off factual error):
   - Match by `pattern` field against canonical + local entries.
   - **Match**: emit `update_frequency` + `update_acceptance` op against the local store (canonical is read-only outside graduation).
   - **No match**: emit `append` op with `frequency: 1`, `accepted: <decision>`, `graduated: false`, `source_review: <session_id>`, `source_finding: <finding_id>`, `created: <ISO date>`.
2. Skip one-off errors — port numbers wrong, specific name misspellings — only patterns that could recur in other documents qualify.
3. Compute `graduation_signals` over the *resulting* local store: any entry with `frequency ≥ 3` AND `acceptance_rate ≥ 80%`.
4. Emit ops list; the skill applies them via `tool_handles.write_append`. Module body MUST NOT directly write to the JSONL files — purity rule.
5. When emitting an `append` op for a new entry, populate the three required-minimum RLHF fields per ADR-005 §2 (`rlhf_signal_type` derived from `decision`, `rlhf_session_id` from the active session, `rlhf_captured_at` from current UTC). When emitting an `update_*` op for an existing entry, append new RLHF signal fields additively without rewriting prior ones (cross-session signal history preserved per ADR-005 §5 clause 5 "no retroactive backfill").

## Integration

- **Lattice node ID**: `accumulate`
- **Upstream**: `human_review` (improvement_decisions); `corrections_store` (read-only match check)
- **Downstream**: `corrections_store` (delta writes against consumer-local path)
- **ADR**: writes governed by ADR-003 (canonical/local split, graduation protocol)
- **Always-on**: yes when at least one improvement decision was made
