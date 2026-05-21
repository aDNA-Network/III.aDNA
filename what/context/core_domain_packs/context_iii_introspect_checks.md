---
type: context_guide
topic: iii_domain_packs
subtopic: introspect_checks
created: 2026-04-03
updated: 2026-05-21
context_version: "1.1"
token_estimate: ~550
last_edited_by: agent_argus
tags: [context, iii-review, introspect, checks, reasoning, pattern-matching, pack_delta_c_027, vfl_001_graduated]
banner: "who/assets/banners/banner_context_guide.jpg"
icon: search
migration_provenance:
  previous_home: lattice-labs/what/context/iii_domain_packs/context_iii_introspect_checks.md
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-2
---

# III INTROSPECT Checks — Reference Library

## Purpose

Formal definitions for all 7 INTROSPECT checks used in Step 2 of the [[how/skills/skill_iii_review|III Review]] procedure. These checks lift analysis from individual claims (INSPECT) to structural patterns across the document.

## Check Catalog

### 2a. Confidence Gradient

- **ID**: `confidence_gradient`
- **Question**: Where does the document transition from [established] → [conjectural] → [speculative]? Is the transition marked?
- **What to flag**: Unmarked transitions, confidence inflation zones, established claims that rest on speculative foundations
- **Output**: Epistemic landscape map with transition boundaries marked

### 2b. Analogy vs Argument

- **ID**: `analogy_vs_argument`
- **Question**: Are analogies used as structural identities without proof?
- **What to flag**: Heuristic parallels treated as precise correspondences, "X is like Y" used as "X implies Y"
- **Output**: Analogy inventory with classification (heuristic / precise correspondence)

### 2c. Structural Completeness

- **ID**: `structural_completeness`
- **Question**: Does the document address failure modes and map dependencies?
- **What to flag**: Missing forward/backward connections, unaddressed risks, absent dependency maps
- **Output**: Completeness assessment with gap inventory

#### 2c.i Static trap — `producer_consumer_pair_unwired` (C-027; graduated from VideoForge VFL-001 at MD-B2 2026-05-21)

- **Pattern**: a producer module emits a structured field per spec, but no consumer aggregator yet reads it. The field exists at the producer's output boundary; downstream wiring lands in a later phase.
- **Detection signature**: emitted field appears in the producer's typed output without a corresponding `read_by:` / `consumer_uses:` declaration anywhere in the same artifact-class graph (mission lattice, ADR consequence map, or runtime lattice yaml).
- **Domain-generality** (per VFL proposal §3): observed in VideoForge across 3 separate scorer missions (M_3_02 RegisterCoherenceScorer + M_3_03 HookStrengthScorer + M_3_04 EngagementScorer); applies identically to SiteForge archetype-lattice-emits-quality-scores / CanvasForge per-canvas VR1–VR5 scoring / RareArchive FAIR-fields-without-search-aggregator / wga course-module-learning-outcomes-without-curriculum-aggregator. The pattern is **temporal coordination failure across phase boundaries**, not a video-specific shape.
- **Substrate-canonical mitigation**: track as F-INTROSPECT finding with severity `low` (the field is not broken, just unconsumed); attach a Phase-N forward-link tag naming the consumer module that will wire it; require the Phase-N mission's deliverable list to explicitly enumerate the wiring as a closed-loop deliverable. Closes the producer-consumer pair across phase boundary without forcing premature consumer authoring.
- **Governance**: ADR-003 §6 (Domain pack graduation) + ADR-007 §1 stage PACK_DELTA_LANDED. Canonical entry: C-027 in `iii_corrections_canonical.jsonl`.

### 2d. Meta-Patterns

- **ID**: `meta_patterns`
- **Question**: Are there recurring weakness types or reasoning patterns that could fail silently?
- **What to flag**: Repeated reasoning shapes, missing perspectives, systematic blind spots
- **Output**: Pattern catalog with risk assessment

### 2e. Argumentation Structure

- **ID**: `argumentation_structure`
- **Question**: Are the document's arguments well-formed with explicit support relationships?
- **What to flag**: Implicit support relationships, straw alternatives, correlation presented as causation
- **What to look for**:
  - Thesis/claim identification — does each section have a clear claim?
  - Support type — evidence, authority, analogy, or deduction?
  - Counterargument handling — acknowledged, dismissed, or absent?
  - Causal vs correlational — does the strength of the claim match the evidence type?
- **Output**: Argumentation map with support-type annotations

### 2f. Denominator Check

- **ID**: `denominator_check`
- **Question**: Are claims grounded in appropriate base rates, or do they present numerators without denominators?
- **What to flag**: Missing denominators, cherry-picked examples, survivorship bias, percentages without absolutes
- **What to look for**:
  - "N validated patterns" — out of how many attempted?
  - Success stories — where are the failures?
  - Statistics — source cited? Base rate provided?
  - Selection criteria — why these examples and not others?
- **Related corrections**: C-003 (`overstated_validation_scope`), C-005 (`unsourced_consequential_statistic`)
- **Output**: Denominator inventory with missing-base-rate flags

### 2g. Corrections Pattern Match

- **ID**: `corrections_pattern_match`
- **Question**: Do this review's findings match known recurring failure patterns?
- **Mechanical procedure**:
  1. Load corrections from `iii_corrections.jsonl` (already available from DISPATCH)
  2. For each INSPECT finding, compare `trap` category + description against corrections
  3. Flag matches: "Recurring pattern: matches C-NNN (pattern_name), freq N"
  4. Aggregate: "N of M findings match known corrections"
  5. Flag graduation candidates (frequency ≥ 3, acceptance ≥ 80%)
- **Output**: Match report with recurrence statistics and graduation candidates

## Token Budget

~1500 tokens total. All 7 checks are loaded together during Step 2 — they cannot be selectively omitted since they form a coherent structural analysis pass.

## Related

- [[how/skills/skill_iii_review|III Review Skill]] — Step 2 procedure
- [[what/lattices/lattice_iii_verification_oracle|III Verification Oracle]] — introspect node checks list
- [[what/context/core_domain_packs/context_iii_learning_store|Learning Store]] — corrections schema (for 2g)
