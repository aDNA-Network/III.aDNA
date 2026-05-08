---
type: module_companion
module_ref: module_iii_improve
created: 2026-05-08
updated: 2026-05-08
last_edited_by: agent_stanley
status: active
tags:
  - module
  - agent-module
  - iii-review
  - improve
  - prioritization
fair:
  findable:
    keywords:
      - improve
      - improvement table
      - severity calibration
      - verification result
      - iii loop
    identifier: "III.aDNA/module_iii_improve/1.0.0"
  accessible:
    access_protocol: "vault file read"
    authentication: none
  interoperable:
    input_types: [structured]
    output_types: [structured]
    standards: ["aDNA Module Spec v1.0", "III Review Skill v1.0"]
  reusable:
    license: "Apache-2.0"
    provenance: "Decomposed from skill_iii_review Step 3; III.aDNA MA-3 2026-05-08"
    version: "1.0.0"
migration_provenance:
  previous_home: "lattice-labs (composite via module_iii_semantic_reviewer)"
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-3
---

# Module: III Improve

> Always-active third phase of the III pipeline. Converts the `CalibratedReport` (findings + introspections) into a prioritized `ImprovementTable` with severity-ranked, type-classified rows. Consumes learning-store acceptance rates (when present) for severity calibration. Emits both the table and a `VerificationResult` with `ReviewState` progression for downstream human approval.

## Specification

| Field | Value |
|-------|-------|
| **Module type** | `agent` (Claude-as-runtime) |
| **Runtime** | Claude (Opus) |
| **Tier** | L1 (local) |
| **GPU** | Not required |
| **Memory** | 1024 MB |
| **Timeout** | 60s |
| **Lattice node** | `improve` (always active) |

## Inputs

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `calibrated_report` | structured | yes | Output of `module_iii_introspect`: `{findings, introspections, graduation_candidates}` |
| `learning_store_acceptance_rates` | structured | no | Per-correction historical acceptance rates — used to calibrate severity (high acceptance → high confidence in proposed fix) |

## Outputs

| Name | Type | Description |
|------|------|-------------|
| `improvement_table` | structured | Ordered rows `{#, finding_id, proposed_fix, priority: critical\|high\|medium\|low, type: correction\|expansion\|restructure\|annotation, voices_agreeing?, matched_correction_id?}` |
| `verification_result` | structured | `{review_state: INSPECTED\|INTROSPECTED\|IMPROVED, summary, finding_counts: {critical, high, medium, low}, introspection_counts}` |

## Behavior

For each finding in the calibrated report, propose a specific change and classify its priority + type per `skill_iii_review.md` Step 3. Priority order: Critical (undermines core claims) > High (could mislead) > Medium (missing context, presentation) > Low (style, minor clarification). Type taxonomy: Correction (factual fix), Expansion (add missing material), Restructure (reorganize for clarity), Annotation (mark confidence/scope).

Per the lattice's `improve` node, the `output_format` is `"ImprovementTable + VerificationResult"`. The module never applies edits — that's the downstream `human_review` → `reviewed_output` chain. Module body must NOT modify the artifact.

For procedural detail see `how/skills/skill_iii_review.md` Step 3.

## Integration

- **Lattice node ID**: `improve`
- **Upstream**: `introspect` (calibrated_report)
- **Downstream**: `human_review` (improvement_table for approval); approved decisions then flow to `module_iii_accumulate` and `reviewed_output`
- **Always-on**: yes
