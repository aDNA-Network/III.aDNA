---
type: skill
lattice_type: skill
skill_type: agent
created: 2026-04-01
updated: 2026-05-08
status: active
category: review
trigger: "When reviewing any document that makes technical, mathematical, or architectural claims for precision, reasoning quality, and completeness"
last_edited_by: agent_stanley
tags: [skill, review, iii-loop, quality, rigor, inspection]
banner: "who/assets/banners/banner_skill.jpg"
icon: microscope
fair:
  keywords: [review, inspection, introspection, improvement, quality, rigor]
  license: Apache-2.0
federation:
  discoverable: true
  source_instance: III.aDNA
  version_policy: minor
migration_provenance:
  previous_home: lattice-labs/how/skills/skill_iii_review.md
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-1
---

# Skill: III Review (Inspect / Introspect / Improve)

## Overview

A structured three-phase review loop: **Inspect** (adversarial precision check), **Introspect** (structural pattern analysis), **Improve** (prioritized changes). Catches precision failures, reasoning gaps, confidence inflation, and structural weaknesses. Generalizes to any domain where claims need rigorous checking.

## When to Use

1. **Reviewing research or technical documents** — any document making substantive claims
2. **Before finalizing deliverables** — context files, ADRs, PRDs, RFCs, lattice definitions
3. **Post-draft quality gate** — after an agent produces a document, before user approval
4. **Audit requests** — user asks to "review", "audit", "check", or "III" a document
5. **Mission closeout** — reviewing deliverables before marking objectives complete

## Prerequisites

- The document to review (must be readable)
- Domain context files relevant to the document's claims (for cross-referencing)
- Familiarity with the document's domain (or explicit acknowledgment of limits)

## Procedure

### Step 0: DISPATCH — What Context to Load?

Before inspecting, determine the review configuration:

1. **Check queue** — if this review was triggered from `~/.claude/iii_review_queue.json`, read the queue entry for pre-assigned `packs`, `depth`, and `modalities`. If found, skip to step 5.
2. **Auto-select packs (path-based)** — match the document's file path against `pack_assignment` globs in [[what/lattices/lattice_iii_verification_oracle|lattice_iii_verification_oracle.lattice.yaml]] (dispatch node config). Use the first matching pattern. If no match, use `["core"]`.
3. **Frontmatter fallback** — if path matching yields only `["core"]`, read the document's frontmatter `type` field and check `frontmatter_dispatch.type_to_packs` in the lattice YAML. If a more specific pack set is found, use it instead.
4. **Auto-select depth** — match the document's path against `depth_assignment` in the lattice YAML. Override rules:
   - Mission closeout context → always `deep`
   - User explicitly requests a depth → user wins
   - Queue entry specifies depth → queue wins
5. **Load corrections** — read `what/context/core_domain_packs/iii_corrections_canonical.jsonl`. For each non-graduated correction, add it to the active check list alongside domain pack traps. Prioritize by frequency. Report: "Loaded N corrections from learning store."
6. **Scan for modality signals** — after loading the document, scan for content that warrants additional INSPECT modalities:
   - Code paths, function names, CLI commands → activate **code_inspect** (Step 1b)
   - Image references, screenshot paths → activate **visual_inspect** (Step 1c)
   - YAML/JSON/CSV data file references → activate **data_inspect** (Step 1d)
7. **Report dispatch** — tell the user: "Dispatched via [path match / frontmatter / queue]: packs [X, Y], depth [Z], modalities [text, ...], corrections (N items)."

### Step 1: INSPECT — What's Claimed?

Run active modalities as determined by DISPATCH. All modalities produce findings in the common `Finding[ID]` format (prefixed by modality — see procedures reference). Severity: Critical/High/Medium/Low.

| Modality | Trigger | What It Checks |
|----------|---------|----------------|
| **Text** (always) | Every review | Five Traps + domain packs + corrections against all claims |
| **Code** (conditional) | Code refs in document | Function names, file paths, CLI commands verified against codebase |
| **Visual** (conditional) | Image refs in document | Label accuracy, consistency with text, misleading presentation |
| **Data** (conditional) | Data file refs in document | Schema consistency, statistics vs prose, data quality |

For full procedures, trap definitions, and output format, see [[what/context/iii_domain_packs/context_iii_inspect_procedures|INSPECT Procedures Reference]].

### Step 2: INTROSPECT — How's the Reasoning Structured?

Lift from individual claims to structural patterns. Run all 7 checks on merged findings from INSPECT:

2a. Confidence gradient · 2b. Analogy vs argument · 2c. Structural completeness · 2d. Meta-patterns · 2e. Argumentation structure · 2f. Denominator check · 2g. Corrections pattern match

Check 2g creates a feedback loop: INSPECT finds patterns → ACCUMULATE stores them → INTROSPECT matches against them. Flag graduation candidates (frequency ≥ 3, acceptance ≥ 80%).

For formal check definitions, see [[what/context/iii_domain_packs/context_iii_introspect_checks|INTROSPECT Checks Reference]].

### Step 3: IMPROVE — What Changes Are Needed?

Convert findings into a prioritized improvement table:

| # | Finding | Proposed Fix | Priority | Type |
|---|---------|-------------|----------|------|
| 1 | [from Inspect/Introspect] | [specific change] | Critical/High/Medium/Low | Correction/Expansion/Restructure/Annotation |

**Priority**: Critical (undermines core claims) > High (could mislead) > Medium (missing context, presentation) > Low (style, minor clarification).

**Present to user**: Show findings, ask which improvements to implement. Never apply without approval.

### Step 3b: ACCUMULATE — Update the Learning Store

After the user responds to the improvement table (accepts/rejects each item):

1. **For each finding that represents a recurring pattern** (not a one-off factual error):
   - Check if it matches an existing correction in `iii_corrections_canonical.jsonl` (canonical upstream) or the consumer-vault local store (match by `pattern` field)
   - **Match found**: Increment `frequency`. Update `accepted` based on user's decision. Append delta to consumer-vault local store, NOT canonical (canonical receives entries only via graduation ceremony per ADR-003).
   - **No match**: Append a new correction entry to consumer-vault local store with `frequency: 1`.
2. **Skip one-off errors** — a specific port number being wrong is not a pattern. Only accumulate patterns that could recur in other documents.
3. **Check for graduation candidates** — if any correction has `frequency >= 3` and acceptance rate >= 80%, flag it: "Correction C-NNN is a candidate for graduation to the III.aDNA canonical store (ADR-003 ceremony)."

### Step 4: APPLY (After User Approval)

Apply approved improvements. For each change:
1. Make the edit
2. Verify no new issues introduced
3. Update frontmatter (`updated`, `last_edited_by`)

## Outputs

| Output | Type | Description |
|--------|------|-------------|
| Findings list | Structured text | All findings with IDs, severity, and location |
| Introspection notes | Structured text | Meta-patterns and structural issues |
| Improvement table | Table | Prioritized changes with fix descriptions |
| Updated document | File | Improved document (after user approval) |
| Learning store update | JSONL append | New/updated corrections from this review |

## Depth Levels

| Level | Scope | When to Use |
|-------|-------|-------------|
| **Shallow** | Step 1 only (precision check) | Quick pass, time-constrained |
| **Standard** | Steps 1-3 (full III loop) | Default for most documents |
| **Deep** | Steps 1-3 + cross-reference against related documents | Before publication, major decisions, or mission closeout |

## Reviewer Orchestra (Multi-Voice Mode)

When the III review targets SiteForge-generated content (web_design domain pack loaded), activate multi-voice reviewer orchestration. This extends INSPECT with parallel reviewer voices that provide domain-specific semantic perspectives.

### Activation

Multi-voice mode activates when:
1. DISPATCH selects `web_design` pack (path-based or frontmatter dispatch), AND
2. A reviewer registry exists at the configured path (default: `what/context/siteforge/siteforge_reviewers.yaml` in the consumer vault)

### Voice Loading

1. Read the reviewer registry YAML
2. Load all `standard_voices` entries — these always participate
3. Load any `dynamic_voices` entries — user-created voices from prior feedback
4. Each voice defines: `voice_id`, `finding_prefix`, `check_criteria`, `severity_default`

### Execution (Within INSPECT)

After the standard INSPECT modalities (text, code, visual) complete:

1. **Run each voice sequentially** against the merged content + modality findings
2. Each voice produces findings with its own prefix: `F-VCRIT-NNN`, `F-DESIGN-NNN`, `F-UX-NNN`, `F-SEO-NNN`, `F-BRAND-NNN`
3. **Merge all voice findings** into the master findings list alongside modality findings
4. **Cross-voice pattern detection**: scan for content locations flagged by multiple voices
   - 2 voices on same location → severity + 1 level
   - 3+ voices on same location → escalate to Critical (systemic issue)

### Standard Voices

| Voice | Prefix | Focus |
|-------|--------|-------|
| Voice Critic | F-VCRIT | Anti-slop, register compliance, banned phrases, entity-name substitution |
| Design Reviewer | F-DESIGN | Visual hierarchy, design token discipline, component reuse, whitespace |
| UX Reviewer | F-UX | Navigation flow, CTA clarity, user journey, information architecture |
| SEO Specialist | F-SEO | Meta descriptions, JSON-LD accuracy, heading hierarchy, keyword naturalness |
| Brand Alignment | F-BRAND | Color consistency, typography adherence, messaging coherence, visual identity |

### Dynamic Voice Lifecycle

1. **Creation**: User provides feedback → system parses into check criteria → generates persona YAML
2. **Participation**: Dynamic voices run alongside standard voices in subsequent reviews
3. **Promotion**: Dynamic voices with ≥ 5 reviews, ≥ 80% finding acceptance rate, and human approval can be promoted to standard voices
4. **Registry**: All voices stored in `siteforge_reviewers.yaml` under `standard_voices` or `dynamic_voices`

### Interaction with INTROSPECT

INTROSPECT (Step 2) runs on the merged findings list from all sources — modalities AND voices. Check 2d (Meta-patterns) is particularly valuable here: it detects systemic patterns that no single voice would catch alone.

## Integration Points

- **Learning store** (`iii_corrections_canonical.jsonl`): Patterns injected at INSPECT, updated at ACCUMULATE. Schema: [[what/context/iii_domain_packs/context_iii_learning_store|learning store reference]] (pack migration in MA-2 will move to `core_domain_packs/`).
- **VaaS verification oracle** ([[what/lattices/lattice_iii_verification_oracle|lattice YAML]]): III as a composable semantic verification node.
- **Quality gates**: ADR review, PRD/RFC pipeline (stages 02-03), mission closeout, lattice validation.
- **Context quality audit** (`skill_context_quality_audit.md`): III as part of broader audit workflows.
- **SiteForge pipeline** (consumer vault `partner_website_scaffold` lattice): `iii_semantic_reviewer` node between quality_gate_validator and canvas_preview_renderer. Consumer-side details belong in SiteForge.aDNA.

## Provenance

Developed during Zeta-aDNA campaign_zeta_genesis M00 (Context Ingestion & Research Co-Design). First applied to mathematical research documents (`approach.md`, `landscape.md`), catching 17 issues across both documents. The Five Traps were induced from patterns observed during that first application. Migrated to canonical home `III.aDNA/how/skills/skill_iii_review.md` on 2026-05-08 (Campaign A MA-1).

## Related

- **INSPECT Procedures**: [[what/context/iii_domain_packs/context_iii_inspect_procedures|INSPECT modality reference]] (will rebase to `core_domain_packs/` in MA-2)
- **Skills Protocol**: `how/skills/AGENTS.md`
- **Context Quality Rubric**: `what/docs/context_quality_rubric.md`
- **Context Quality Audit**: `how/skills/skill_context_quality_audit.md`
