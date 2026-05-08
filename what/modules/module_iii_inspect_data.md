---
type: module_companion
module_ref: module_iii_inspect_data
created: 2026-05-08
updated: 2026-05-08
last_edited_by: agent_stanley
status: active
tags:
  - module
  - agent-module
  - iii-review
  - inspect
  - data-modality
fair:
  findable:
    keywords:
      - data inspect
      - schema validation
      - prose-data crosscheck
      - iii loop
    identifier: "III.aDNA/module_iii_inspect_data/1.0.0"
  accessible:
    access_protocol: "vault file read"
    authentication: none
  interoperable:
    input_types: [path, structured, context_pack]
    output_types: [structured]
    standards: ["aDNA Module Spec v1.0", "III Review Skill v1.0"]
  reusable:
    license: "Apache-2.0"
    provenance: "Decomposed from skill_iii_review Step 1 (data) + context_iii_inspect_procedures Modality 4; III.aDNA MA-3 2026-05-08"
    version: "1.0.0"
migration_provenance:
  previous_home: "lattice-labs (composite via module_iii_semantic_reviewer)"
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-3
---

# Module: III Inspect — Data

> Conditional data modality of the III INSPECT phase. Reads YAML/JSON/JSONL/CSV files referenced by the artifact, validates schema consistency, cross-checks prose statistics + example citations + counts against actual records, and emits `Finding[]` with `F-DATA-NNN` prefix. Activates when `inspection_config.modalities.data == active`. Critical-severity for prose↔data statistical mismatches.

## Specification

| Field | Value |
|-------|-------|
| **Module type** | `agent` (Claude-as-runtime) |
| **Runtime** | Claude (Opus) |
| **Tier** | L1 (local) |
| **GPU** | Not required |
| **Memory** | 2048 MB |
| **Timeout** | 180s |
| **Lattice node** | `data_inspect` (conditional) |

## Inputs

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `data_file_references` | path[] | yes | YAML/JSON/JSONL/CSV file paths referenced by the artifact |
| `artifact_data_claims` | structured | yes | Prose statistics, example record citations, count assertions, schema descriptions to verify |
| `inspection_config` | structured | yes | Output of `module_iii_dispatch`; module asserts `modalities.data == active` |
| `inspect_procedures_pack` | context_pack | yes | `context_iii_inspect_procedures.md` Modality 4 (Data) |
| `tool_handles` | config | yes | `{read: Read}` — passed by skill |

## Outputs

| Name | Type | Description |
|------|------|-------------|
| `data_findings` | structured | `Finding[]` with `F-DATA-NNN` prefix; per-finding `{id, file_ref, claim, actual_value, issue, severity, mismatch_type?}` |

## Behavior

For each referenced data file: parse → validate schema (field presence + type consistency + enum range) → compute claim-relevant statistics (counts, sums, averages) → diff against the artifact's prose claims. Severity per inspect_procedures: prose↔data statistical mismatches = Critical; schema inconsistencies = High; missing optional fields = Low.

For the operational procedure see `how/skills/skill_iii_review.md` Step 1 and `what/context/core_domain_packs/context_iii_inspect_procedures.md` §Modality 4. Skill orchestrates pack loading and tool binding.

## Integration

- **Lattice node ID**: `data_inspect`
- **Upstream**: `dispatch` (config; activates when data file references detected)
- **Downstream**: `introspect` (data findings → merged findings)
- **Always-on**: no — conditional on dispatch's modality scan
