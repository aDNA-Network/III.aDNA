---
type: module_companion
module_ref: module_iii_inspect_code
created: 2026-05-08
updated: 2026-05-08
last_edited_by: agent_stanley
status: active
tags:
  - module
  - agent-module
  - iii-review
  - inspect
  - code-modality
fair:
  findable:
    keywords:
      - code inspect
      - reference verification
      - signature check
      - iii loop
    identifier: "III.aDNA/module_iii_inspect_code/1.0.0"
  accessible:
    access_protocol: "vault file read"
    authentication: none
  interoperable:
    input_types: [structured, context_pack, path]
    output_types: [structured]
    standards: ["aDNA Module Spec v1.0", "III Review Skill v1.0"]
  reusable:
    license: "Apache-2.0"
    provenance: "Decomposed from skill_iii_review Step 1 (code) + context_iii_inspect_procedures Modality 2; III.aDNA MA-3 2026-05-08"
    version: "1.0.0"
migration_provenance:
  previous_home: "lattice-labs (composite via module_iii_semantic_reviewer)"
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-3
---

# Module: III Inspect — Code

> Conditional code modality of the III INSPECT phase. Extracts function names, file paths, CLI commands, API endpoints, and configuration keys from a document; verifies each reference exists in the codebase via Grep/Glob; checks signature drift, behavior changes, and stale version references; emits `Finding[]` with `F-CODE-NNN` prefix. Activates when `inspection_config.modalities.code == active`.

## Specification

| Field | Value |
|-------|-------|
| **Module type** | `agent` (Claude-as-runtime) |
| **Runtime** | Claude (Opus) + Explore subagent |
| **Tier** | L1 (local) |
| **GPU** | Not required |
| **Memory** | 2048 MB |
| **Timeout** | 180s |
| **Lattice node** | `code_inspect` (conditional) |

## Inputs

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `artifact_content` | structured | yes | Document body — module extracts code references from prose |
| `inspection_config` | structured | yes | Output of `module_iii_dispatch`; module asserts `modalities.code == active` |
| `inspect_procedures_pack` | context_pack | yes | `context_iii_inspect_procedures.md` Modality 2 (Code) |
| `codebase_roots` | path[] | yes | Peer repos to verify against (e.g. `lattice-protocol/`, `latlab/`, `adna/`, current vault) |
| `tool_handles` | config | yes | Handles for Agent (Explore subagent), Grep, Glob — supplied by the skill |

## Outputs

| Name | Type | Description |
|------|------|-------------|
| `code_findings` | structured | `Finding[]` with `F-CODE-NNN` prefix; per-finding `{id, reference, ref_type, location, issue, severity, expected, actual?}` |

## Behavior

Reference extraction → existence check → signature/behavior/version check, three sequential passes. Severity per inspect_procedures: broken file paths and deleted functions = High; changed signatures = Medium; stale version refs = Low. Tools delegated via the `tool_handles` input — module body does not directly invoke filesystem search; it requests them through the orchestrating skill's tool layer.

For the operational procedure see `how/skills/skill_iii_review.md` Step 1 and `what/context/core_domain_packs/context_iii_inspect_procedures.md` §Modality 2. Skill orchestrates pack loading and tool binding; module is pure.

## Integration

- **Lattice node ID**: `code_inspect`
- **Upstream**: `dispatch` (config; activates when code references detected)
- **Downstream**: `introspect` (code findings → merged findings)
- **Always-on**: no — conditional on dispatch's modality scan
