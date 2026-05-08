---
type: module_companion
module_ref: module_iii_inspect_text
created: 2026-05-08
updated: 2026-05-08
last_edited_by: agent_stanley
status: active
tags:
  - module
  - agent-module
  - iii-review
  - inspect
  - text-modality
fair:
  findable:
    keywords:
      - text inspect
      - five traps
      - confidence calibration
      - precision review
      - iii loop
    identifier: "III.aDNA/module_iii_inspect_text/1.0.0"
  accessible:
    access_protocol: "vault file read"
    authentication: none
  interoperable:
    input_types: [structured, context_pack]
    output_types: [structured]
    standards: ["aDNA Module Spec v1.0", "III Review Skill v1.0"]
  reusable:
    license: "Apache-2.0"
    provenance: "Decomposed from skill_iii_review Step 1 (text) + context_iii_inspect_procedures Modality 1; III.aDNA MA-3 2026-05-08"
    version: "1.0.0"
migration_provenance:
  previous_home: "lattice-labs (composite via module_iii_semantic_reviewer)"
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-3
---

# Module: III Inspect — Text

> Always-on text modality of the III INSPECT phase. Reads a document with adversarial precision, applies the Five Traps + dispatched domain pack traps + non-graduated learning-store corrections, and emits `Finding[]` with `F-TEXT-NNN` prefix. Pure module — domain packs and corrections arrive as typed inputs; module body never opens `core_domain_packs/` or `iii_corrections_*.jsonl` directly.

## Specification

| Field | Value |
|-------|-------|
| **Module type** | `agent` (Claude-as-runtime) |
| **Runtime** | Claude (Opus) |
| **Tier** | L1 (local) |
| **GPU** | Not required |
| **Memory** | 2048 MB |
| **Timeout** | 120s |
| **Lattice node** | `text_inspect` (always active) |

## Inputs

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `artifact_content` | structured | yes | Document body + frontmatter as parsed structure |
| `inspection_config` | structured | yes | Output of `module_iii_dispatch` — provides depth and active modalities |
| `inspect_procedures_pack` | context_pack | yes | `context_iii_inspect_procedures.md` Modality 1 (Text) — Five Traps, confidence tags, finding format |
| `domain_packs` | context_pack[] | yes | Domain-specific trap packs selected by dispatch (e.g. `web_design`, `whitepaper_communication`, `vault_maintenance`) |
| `corrections_subset` | structured | yes | Non-graduated entries from canonical + consumer-local stores, prioritized by frequency |

## Outputs

| Name | Type | Description |
|------|------|-------------|
| `text_findings` | structured | `Finding[]` with `F-TEXT-NNN` prefix; each finding `{id, location, claim, issue, severity, trap?}` |

## Behavior

Per-section adversarial pass over the artifact: each claim is checked against (a) the Five Traps from the procedures pack, (b) every loaded domain pack's trap definitions, and (c) every non-graduated correction in the corrections_subset. Findings categorized by the five operational checks (factual accuracy, logical chains, precision, completeness, confidence). Severity weighted toward confidence inflation (~40% of validation findings).

For the operational procedure see `how/skills/skill_iii_review.md` Step 1 and `what/context/core_domain_packs/context_iii_inspect_procedures.md` §Modality 1. The skill orchestrates pack loading and binds the typed inputs above; the module body MUST NOT path-read pack files.

## Integration

- **Lattice node ID**: `text_inspect`
- **Upstream**: `dispatch` (config), `corrections_store` (subset injection)
- **Downstream**: `introspect` (text findings → merged findings)
- **Always-on**: yes — every III cycle invokes this modality
