---
type: context_guide
topic: iii_domain_packs
subtopic: introspect_checks
created: 2026-04-03
updated: 2026-05-08
context_version: "1.0"
token_estimate: ~500
last_edited_by: agent_stanley
tags: [context, iii-review, introspect, checks, reasoning, pattern-matching]
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
