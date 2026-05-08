---
type: module_companion
module_ref: module_iii_dispatch
created: 2026-05-08
updated: 2026-05-08
last_edited_by: agent_stanley
status: active
tags:
  - module
  - agent-module
  - iii-review
  - dispatch
fair:
  findable:
    keywords:
      - dispatch
      - iii loop
      - pack assignment
      - modality routing
    identifier: "III.aDNA/module_iii_dispatch/1.0.0"
  accessible:
    access_protocol: "vault file read"
    authentication: none
  interoperable:
    input_types: [path, structured, config]
    output_types: [structured]
    standards: ["aDNA Module Spec v1.0", "III Review Skill v1.0"]
  reusable:
    license: "Apache-2.0"
    provenance: "Decomposed from skill_iii_review Step 0 + module_iii_semantic_reviewer Phase 1; III.aDNA MA-3 2026-05-08"
    version: "1.0.0"
migration_provenance:
  previous_home: "lattice-labs (composite via module_iii_semantic_reviewer)"
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-3
---

# Module: III Dispatch

> Object-type dispatcher for the III review pipeline. Reads document path + frontmatter + queue entries, consults the lattice's `pack_assignment` / `depth_assignment` / `frontmatter_dispatch` config, and emits an `InspectionConfig` describing which domain packs to load, which INSPECT modalities to activate, and which review depth to use. Pure module — does not load packs itself; the orchestrating skill loads packs against this config.

## Specification

| Field | Value |
|-------|-------|
| **Module type** | `agent` (Claude-as-runtime) |
| **Runtime** | Claude (Opus) |
| **Tier** | L1 (local) |
| **GPU** | Not required |
| **Memory** | 1024 MB |
| **Timeout** | 30s |
| **Lattice node** | `dispatch` (always-on entry) |

## Inputs

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `artifact` | path | yes | Document to review (markdown, YAML, code file, image, or structured data) |
| `lattice_dispatch_config` | structured | yes | The `dispatch.config` block from `lattice_iii_verification_oracle.lattice.yaml` (path & frontmatter assignment maps; modality signals) |
| `queue_entry` | structured | no | Pre-assigned dispatch from `~/.claude/iii_review_queue.json` — when present, skips path/frontmatter resolution |
| `corrections_store_paths` | structured | yes | `{canonical: <III.aDNA path>, local: <consumer-vault path>}` — pointers only; module emits a count, does not load entries |
| `user_overrides` | config | no | Explicit `{packs?, depth?, modalities?}` from user — wins over all other sources |

## Outputs

| Name | Type | Description |
|------|------|-------------|
| `inspection_config` | structured | `{packs: [], depth: shallow\|standard\|deep, modalities: {text, code, visual, data}, corrections_count: int, dispatch_source: queue\|path\|frontmatter\|fallback\|user}` |
| `dispatch_report` | structured | Single-line provenance: "Dispatched via X: packs [Y], depth [Z], modalities [..], corrections (N items)" |

## Behavior

Resolves the configuration in priority order: `user_overrides` → `queue_entry` → path-glob match against `pack_assignment` → frontmatter `type`-based fallback against `frontmatter_dispatch.type_to_packs` → `["core"]` last-resort. Depth resolves analogously against `depth_assignment` / `frontmatter_dispatch.type_to_depth`. After loading the artifact, scans for `code_references`, `image_references`, `data_references` per the lattice's `modality_signals` map and activates the corresponding inspect modules.

For exact source procedure see `how/skills/skill_iii_review.md` Step 0; for the assignment maps see the `dispatch.config` block in `what/lattices/lattice_iii_verification_oracle.lattice.yaml`. This module references those configs by typed input — it does not read pack files or corrections files itself.

## Integration

- **Lattice node ID**: `dispatch`
- **Upstream**: `document_input` (raw artifact), `corrections_store` (path + count, not entries)
- **Downstream**: `text_inspect` (always), `code_inspect`/`visual_inspect`/`data_inspect` (conditional on modality signals)
