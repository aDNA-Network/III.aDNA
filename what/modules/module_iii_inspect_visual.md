---
type: module_companion
module_ref: module_iii_inspect_visual
created: 2026-05-08
updated: 2026-05-08
last_edited_by: agent_stanley
status: active
tags:
  - module
  - agent-module
  - iii-review
  - inspect
  - visual-modality
fair:
  findable:
    keywords:
      - visual inspect
      - image cross-reference
      - label accuracy
      - iii loop
    identifier: "III.aDNA/module_iii_inspect_visual/1.0.0"
  accessible:
    access_protocol: "vault file read"
    authentication: none
  interoperable:
    input_types: [path, structured, context_pack]
    output_types: [structured]
    standards: ["aDNA Module Spec v1.0", "III Review Skill v1.0"]
  reusable:
    license: "Apache-2.0"
    provenance: "Decomposed from skill_iii_review Step 1 (visual) + context_iii_inspect_procedures Modality 3; III.aDNA MA-3 2026-05-08"
    version: "1.0.0"
migration_provenance:
  previous_home: "lattice-labs (composite via module_iii_semantic_reviewer)"
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-3
---

# Module: III Inspect — Visual

> Conditional visual modality of the III INSPECT phase. Calls `describe_image` on each referenced screenshot/diagram/UI mockup with an III-calibrated prompt; cross-references the description against the artifact's textual claims; flags label/count mismatches, misleading presentation (truncated axes, unclear legends), and accessibility gaps; emits `Finding[]` with `F-VIS-NNN` prefix. Activates when `inspection_config.modalities.visual == active`.

## Specification

| Field | Value |
|-------|-------|
| **Module type** | `agent` (Claude-as-runtime + Gemini describe_image) |
| **Runtime** | Claude (Opus) |
| **Tier** | L1 (local) |
| **GPU** | Not required (image describe via remote MCP) |
| **Memory** | 2048 MB |
| **Timeout** | 240s (allows multiple image calls) |
| **Lattice node** | `visual_inspect` (conditional) |

## Inputs

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `image_references` | path[] | yes | Paths to images referenced by the artifact (embedded or linked) |
| `artifact_textual_claims` | structured | yes | Excerpted text claims about each image — labels, counts, statuses, narrative |
| `inspection_config` | structured | yes | Output of `module_iii_dispatch`; module asserts `modalities.visual == active` |
| `inspect_procedures_pack` | context_pack | yes | `context_iii_inspect_procedures.md` Modality 3 (Visual) |
| `tool_handles` | config | yes | `{describe_image: mcp__gemini-image__describe_image}` — passed by skill |

## Outputs

| Name | Type | Description |
|------|------|-------------|
| `visual_findings` | structured | `Finding[]` with `F-VIS-NNN` prefix; per-finding `{id, image_ref, claim, image_description_excerpt, issue, severity}` |

## Behavior

For each image: call `describe_image` with the III-calibrated prompt ("Describe this image precisely. List every label, number, status indicator, and UI element visible. Note any text that appears cut off, inconsistent, or potentially misleading."), then diff the description against the artifact's textual claims about that image. Severity per inspect_procedures: label/number mismatches = High; misleading presentation = Medium; minor accessibility issues = Low.

For the operational procedure see `how/skills/skill_iii_review.md` Step 1 and `what/context/core_domain_packs/context_iii_inspect_procedures.md` §Modality 3. Skill orchestrates pack loading and tool binding.

## Integration

- **Lattice node ID**: `visual_inspect`
- **Upstream**: `dispatch` (config; activates when image references detected)
- **Downstream**: `introspect` (visual findings → merged findings)
- **Always-on**: no — conditional on dispatch's modality scan
