---
type: module_companion
module_ref: module_iii_semantic_reviewer
created: 2026-04-04
updated: 2026-05-08
last_edited_by: agent_stanley
status: active
tags:
  - module
  - agent-module
  - iii-review
  - semantic-review
  - multi-voice
  - web-design
  - siteforge
fair:
  findable:
    keywords:
      - semantic review
      - iii loop
      - web design
      - multi-voice
      - content quality
      - brand alignment
    identifier: "III.aDNA/module_iii_semantic_reviewer/1.0.0"
  accessible:
    access_protocol: "vault file read"
    authentication: none
  interoperable:
    input_types: [structured, path, config]
    output_types: [structured]
    standards: ["aDNA Module Spec v1.0", "III Review Skill v1.0", "SiteForge Reviewers v1.0"]
  reusable:
    license: "Apache-2.0"
    provenance: "SiteForge Campaign M11; canonical home migrated to III.aDNA 2026-05-08 (Campaign A MA-1)"
    version: "1.0.0"
migration_provenance:
  previous_home: lattice-labs/what/modules/module_iii_semantic_reviewer.md
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-1
---

# Module: III Semantic Reviewer

> Agent module that runs the III (Inspect/Introspect/Improve) review loop on generated websites, applying web_design domain traps and multi-voice reviewer orchestration. Complements Playwright's mechanical validation with semantic quality assessment. **Canonical home: III.aDNA**; SiteForge-specific integration details (lattice node placement, sibling modules) belong in SiteForge.aDNA via consumer wrapper.

## Specification

| Field | Value |
|-------|-------|
| **Module type** | `agent` (Claude-as-runtime) |
| **Runtime** | Claude (Opus) |
| **Tier** | L1 (local) |
| **GPU** | Not required |
| **Memory** | 4096 MB |
| **Timeout** | 300s (5 min overall) |
| **Lattice node** | `iii_semantic_reviewer` in SiteForge `partner_website_scaffold` lattice (consumer-side) |

## Inputs

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `quality_gate_report` | structured | yes | JSON report from quality_gate_validator — mechanical gate context |
| `content_files` | path | yes | Generated content (src/pages/, src/content/) |
| `code_files` | path | yes | Generated code (src/components/, src/layouts/, src/utils/, config) |
| `evidence_screenshots` | path | yes | Gate 8/9 screenshots for visual review |
| `branding_config` | config | no | Entity branding.json for Brand Alignment voice |

## Outputs

| Name | Type | Description |
|------|------|-------------|
| `semantic_review_report` | structured | Full III loop report (dispatch, findings, introspection, improvements) |
| `findings_list` | structured | Flat list with voice-prefixed IDs and severity |
| `improvement_table` | structured | Prioritized recommendations with cross-voice escalation |

## Architecture

### Two-Layer Quality Model

The SiteForge pipeline runs two complementary quality layers in sequence:

```
Generated Site → Playwright (mechanical) → III (semantic) → Human Review
```

| Layer | What It Validates | Failure Means |
|-------|-------------------|---------------|
| Playwright | Build succeeds, a11y passes, meta present, responsive, links, sitemap | Site is broken — cannot ship |
| III + web_design | Content authenticity, design coherence, token discipline, voice compliance | Site is mediocre — should not ship |

### Execution Flow

The module executes the III loop with SiteForge-specific configuration:

#### Phase 1: DISPATCH

1. Load domain packs: `[core, web_design]` (always)
2. Load corrections from `iii_corrections_canonical.jsonl` (III.aDNA upstream) + consumer-vault local store
3. Load reviewer registry from `siteforge_reviewers.yaml` (consumer vault path)
4. Set modalities: text (always), code (always), visual (always), data (conditional)
5. Set depth: standard (default), deep (on mission closeout)

#### Phase 2: INSPECT (4 Modalities + 5 Voices)

**Modality execution** (sequential):

| Modality | Targets | Checks |
|----------|---------|--------|
| **text_inspect** | Content collections, page copy, meta descriptions | Five Traps + web_design traps (template voice, SEO stuffing) |
| **code_inspect** | Astro components, layouts, utilities, config | Astro 6 API correctness, design token discipline, a11y patterns |
| **visual_inspect** | Gate 8/9 evidence screenshots | Visual hierarchy, brand consistency, responsive coherence |

**Multi-voice reviewer pass** (within INSPECT, after modalities):

| Voice | ID Prefix | Focus |
|-------|-----------|-------|
| **Voice Critic** | F-VCRIT | Anti-slop + register compliance, banned phrases, entity-name substitution |
| **Design Reviewer** | F-DESIGN | Visual hierarchy, component usage, whitespace balance, color harmony |
| **UX Reviewer** | F-UX | Navigation flow, information architecture, user journey, CTA clarity |
| **SEO Specialist** | F-SEO | Meta tags, JSON-LD accuracy, heading structure, keyword naturalness |
| **Brand Alignment** | F-BRAND | Entity-specific branding consistency, tone match, visual identity |

Each voice produces findings with its prefixed ID. All findings merge into a single list before INTROSPECT.

#### Phase 3: INTROSPECT

Run 7 structural checks on merged findings. Cross-voice pattern detection: if 3+ voices flag the same content location, escalate that finding to Critical severity.

#### Phase 4: IMPROVE

Produce prioritized improvement table. Cross-voice escalations appear first. Include `voices_agreeing` count for each finding.

### Cross-Voice Pattern Detection

When multiple voices independently flag the same file or content section:

| Voices Agreeing | Action |
|----------------|--------|
| 1 | Normal severity as assigned by voice |
| 2 | Severity + 1 level (Low → Medium, Medium → High) |
| 3+ | Escalate to Critical — systemic issue |

### Self-Improvement Loop

The module participates in the III learning store feedback cycle:

1. **Generate** → Playwright gates → III review → findings
2. **Agent applies** approved improvements → re-run gates → verify
3. **Accepted findings** seed consumer-vault local store → next generation inherits corrections
4. **High-frequency corrections** graduate to III.aDNA canonical store via ADR-003 ceremony → propagate to all consumers

Each entity's site benefits from every previous entity's review — compound leverage.

## Integration with Partner Website Scaffold Lattice

The `iii_semantic_reviewer` node sits between `quality_gate_validator` and `canvas_preview_renderer` in the SiteForge `partner_website_scaffold` lattice (consumer-side, lives in SiteForge.aDNA):

```
quality_gate_validator ──→ iii_semantic_reviewer ──→ canvas_preview_renderer ──→ deploy_verification
   (mechanical gates)         (semantic review)        (visual preview)            (human approval)
```

### Data Flow

| From | To | Data |
|------|----|------|
| `quality_gate_validator` | `iii_semantic_reviewer` | quality_gate_report + evidence_dir |
| `astro_project_assembler` | `iii_semantic_reviewer` | content_files + code_files |
| `brand_config_loader` | `iii_semantic_reviewer` | branding_config |
| `iii_semantic_reviewer` | `canvas_preview_renderer` | semantic_review_report + findings_list |

The canvas preview renders both mechanical gate status and semantic review findings for operator decision.

## Related Modules (consumer-side, in SiteForge.aDNA / lattice-labs)

- Quality Gate Validator (SiteForge consumer-side) — upstream mechanical validation
- Canvas Preview Renderer (SiteForge consumer-side) — downstream visual review
- Voice Critic (SiteForge consumer-side) — content quality upstream; this module re-evaluates the output
- Page Composer (SiteForge consumer-side) — generates the pages this module reviews

> **Note**: Wikilinks to sibling SiteForge modules from the canonical lattice-labs version of this file have been deferred to plain references during MA-1 migration. Cross-vault wikilink hygiene will be handled when SiteForge consumer wrapper lands (Campaign B MB-2).
