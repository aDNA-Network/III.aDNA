---
type: module_companion
module_ref: module_iii_introspect
created: 2026-05-08
updated: 2026-05-08
last_edited_by: agent_stanley
status: active
tags:
  - module
  - agent-module
  - iii-review
  - introspect
  - structural-analysis
fair:
  findable:
    keywords:
      - introspect
      - structural pattern analysis
      - confidence gradient
      - meta-patterns
      - corrections pattern match
    identifier: "III.aDNA/module_iii_introspect/1.0.0"
  accessible:
    access_protocol: "vault file read"
    authentication: none
  interoperable:
    input_types: [structured, context_pack]
    output_types: [structured]
    standards: ["aDNA Module Spec v1.0", "III Review Skill v1.0"]
  reusable:
    license: "Apache-2.0"
    provenance: "Decomposed from skill_iii_review Step 2 + context_iii_introspect_checks (7 checks); III.aDNA MA-3 2026-05-08"
    version: "1.0.0"
migration_provenance:
  previous_home: "lattice-labs (composite via module_iii_semantic_reviewer)"
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-3
---

# Module: III Introspect

> Always-active second phase of the III pipeline. Lifts analysis from individual claims to structural patterns by running the 7 INTROSPECT checks (`confidence_gradient`, `analogy_vs_argument`, `structural_completeness`, `meta_patterns`, `argumentation_structure`, `denominator_check`, `corrections_pattern_match`) on the merged `Finding[]` output of every active inspect module. Emits `Introspection[]` plus a `CalibratedReport` and surfaces graduation candidates from the corrections store.

## Specification

| Field | Value |
|-------|-------|
| **Module type** | `agent` (Claude-as-runtime) |
| **Runtime** | Claude (Opus) |
| **Tier** | L1 (local) |
| **GPU** | Not required |
| **Memory** | 2048 MB |
| **Timeout** | 180s |
| **Lattice node** | `introspect` (always active when ≥1 inspect modality ran) |

## Inputs

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `merged_findings` | structured | yes | Concatenation of `Finding[]` outputs from text/code/visual/data inspect modules — the modality prefix preserves provenance |
| `introspect_checks_pack` | context_pack | yes | `context_iii_introspect_checks.md` — definitions of the 7 checks |
| `corrections_store` | structured | yes | Full canonical + consumer-local entries for check 2g (`corrections_pattern_match`) |
| `artifact_metadata` | structured | yes | Frontmatter + section structure of the original artifact (for argumentation_structure + structural_completeness checks that need document-level context) |

## Outputs

| Name | Type | Description |
|------|------|-------------|
| `introspections` | structured | `Introspection[]` — one entry per check: `{check_id, output: <check-specific shape>, severity, observations}` |
| `calibrated_report` | structured | `{findings: Finding[], introspections: Introspection[], graduation_candidates: []}` — passed to `improve` |
| `graduation_candidates` | structured | Corrections matching `frequency ≥ 3` AND `acceptance ≥ 80%` — flagged for ADR-003 ceremony |

## Behavior

Sequential execution of the 7 checks per `context_iii_introspect_checks.md`. Modality-agnostic — operates on the common `Finding` format. Check 2g (`corrections_pattern_match`) closes the learning-store feedback loop: it scans merged findings against the corrections store, increments observed frequencies (mentally; the actual write happens in `module_iii_accumulate`), and surfaces graduation candidates.

For check definitions see `what/context/core_domain_packs/context_iii_introspect_checks.md`. For procedural context see `how/skills/skill_iii_review.md` Step 2. Skill orchestrates pack loading.

## Integration

- **Lattice node ID**: `introspect`
- **Upstream**: text_inspect, code_inspect, visual_inspect, data_inspect (merged findings); `corrections_store` (pattern match reference)
- **Downstream**: `improve` (calibrated_report)
- **Always-on**: yes when at least one inspect modality produced findings; gated otherwise
