---
type: context_guide
created: 2026-04-03
updated: 2026-05-21
token_estimate: ~1600
last_edited_by: agent_argus
tags: [context_guide, iii-review, inspect, modalities, traps, procedures, pack_delta_c_028, vfl_002_graduated]
banner: "who/assets/banners/banner_context_guide.jpg"
icon: search
migration_provenance:
  previous_home: lattice-labs/what/context/iii_domain_packs/context_iii_inspect_procedures.md
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-2
---

# III INSPECT Procedures — Modality Reference

Formal procedures for all four INSPECT modalities in the [[how/skills/skill_iii_review|III Review skill]]. Loaded during Step 1 after DISPATCH selects active modalities.

## The Five Traps

Domain-general reasoning failure modes checked in every text inspection:

| Trap | Description | Example |
|------|-------------|---------|
| **Analogy trap** | Treating heuristic parallels as structural identities | "X is like Y" used as "X implies Y" |
| **Boundary trap** | Making claims outside the domain where definitions apply | Using a formula beyond its convergence region |
| **Confidence trap** | Stating conjectural objects/claims as established | "The system will..." when it should be "The system would, if constructed..." |
| **Completeness trap** | Survey/landscape biased toward own framework | Missing competing approaches, dead ends, or adjacent work |
| **Level trap** | Conflating properties at different levels of abstraction | Confusing properties of a category with properties of its objects |

## Confidence Tag System

Apply throughout reviewed documents to mark epistemic status:

| Tag | Meaning |
|-----|---------|
| [established] | Proven/published/accepted result |
| [conjectural] | Specific conjecture with clear statement, possibly evidenced |
| [speculative] | Proposed construction without rigorous definition |
| [heuristic] | Analogy or parallel, not a precise claim |

## Finding Output Format

All modalities produce findings in a common format. Prefix finding IDs by modality:

| Modality | Prefix | Example |
|----------|--------|---------|
| Text | F-TEXT | F-TEXT-001 |
| Code | F-CODE | F-CODE-001 |
| Visual | F-VIS | F-VIS-001 |
| Data | F-DATA | F-DATA-001 |

```
Finding [ID]: [Short title]
- Location: [section/line or file path]
- Claim: [what's stated]
- Issue: [what's wrong or unclear]
- Severity: Critical | High | Medium | Low
```

---

> **Note**: The Five Traps define failure mode *categories* (what can go wrong structurally). The checks below are the *operational questions* applied during review (what to look for in the text). They overlap on confidence and completeness but are complementary — traps diagnose the pattern, checks guide the search.

## Modality 1: Text Inspect (Always Active)

**Trigger**: Always active for every review.

**Procedure**:
1. Read the document with adversarial precision
2. For each section, check claims against: the Five Traps (above), loaded domain pack traps, AND corrections from the learning store
3. Categorize each claim by these five checks:

| Check | Question | What To Flag |
|-------|----------|-------------|
| **Factual accuracy** | Are attributions correct? Are statements precise? | Errors, misattributions, imprecise statements |
| **Logical chains** | Does each step follow? Are implications directional? | Hidden assumptions, non-sequiturs, bidirectional claims stated as unidirectional |
| **Precision** | Are objects defined before use? Is notation consistent? | Undefined terms, inconsistent notation, non-standard usage without justification |
| **Completeness** | What's missing that should be there? | Structural gaps, unaddressed obvious questions, missing assumptions |
| **Confidence** | Are conjectural claims marked as such? | Confidence inflation — conjectural/speculative claims stated as established |

4. Produce `Finding[]` with F-TEXT prefix for each issue found

**Severity calibration**: Confidence inflation is the dominant failure mode (~40% of findings in validation). Weight it accordingly.

---

## Modality 2: Code Inspect (Conditional)

**Trigger**: Document contains function names, file paths, CLI commands, API endpoints, or code snippets.

**Tools**: Agent (Explore subagent), Grep, Glob

**Procedure**:
1. Extract all code references from the document: function names, file paths, CLI commands, API endpoints, class names, configuration keys
2. For each reference, verify it exists in the codebase:
   - File paths → Glob to confirm existence
   - Function/class names → Grep across peer repos (`lattice-protocol/`, `latlab/`, `adna/`)
   - CLI commands → check against known command registries
   - API endpoints → Grep for route definitions
3. For references that resolve, check:
   - Has the function signature changed since the document was written?
   - Is the described behavior still accurate?
   - Are version/branch references current?
4. Produce `Finding[]` with F-CODE prefix for each broken, renamed, or inaccurate reference

**Severity calibration**: Broken file paths and deleted functions are High. Changed signatures are Medium. Stale version references are Low.

### Static trap — `spec_verbatim_port_to_code` (C-028; graduated from VideoForge VFL-002 at MD-B2 2026-05-21)

- **Pattern**: vault-side substrate (specs, ADRs, pattern docs in markdown) is ported into code as canonical runtime source-of-truth via copy-faithful constants. The code becomes a verbatim mirror of substrate content. **Risk class**: substrate drift — if the spec amends without parallel code update, runtime silently disagrees with documented intent.
- **Detection signature** (during INSPECT-Code Modality 2 procedure): code-side files contain large constant tables, enums, or rule sets whose values trace 1:1 to a markdown spec section (typically a `§N` reference or a wikilink). Look for: `R1-R12` register lists hardcoded into Python; `T1-T11` template constants; threshold matrices that match table rows in a `spec_*.md` file character-for-character; doctrine-rule comments that quote spec prose verbatim.
- **Domain-generality** (per VFL proposal §4): observed in VideoForge `register_definitions.py` ↔ `spec_register_compliance.md`, `hook_definitions.py` ↔ `spec_hook_detection.md`, `heuristic_rules.py` ↔ `spec_engagement_prediction.md`. Applies identically to lattice-labs policy.md → lattice-protocol policies/*.py; wga curriculum.md → wga-buildpack/curriculum/*.py; RareArchive standards.md → rare-archive/schemas/*.py Pydantic; WilhelmAI ADR-002 attribution rules → siteforge wrapper strings (already verified in MP0-7 close). The pattern is **substrate-to-code transit with a drift-risk class**, not a video-specific shape.
- **Substrate-canonical mitigation**: Standing Rule 12-equivalent **dual-filing requirement** — every spec amendment that touches values hardcoded in code MUST land a paired code-side update in the same mission (or explicit deferral note citing the next mission that will close the drift). Add an INSPECT-Code drift-check step: when a `spec_*.md` file is touched, grep code for the file's `§` anchors and verify each port still matches. Flag any port that has diverged as severity `medium` (silent runtime/intent disagreement); flag missing dual-filing as severity `high`.
- **Governance**: ADR-003 §6 (Domain pack graduation) + ADR-007 §1 stage PACK_DELTA_LANDED. Canonical entry: C-028 in `iii_corrections_canonical.jsonl`.

---

## Modality 3: Visual Inspect (Conditional)

**Trigger**: Document references images, screenshots, diagrams, or UI mockups (embedded or linked).

**Tools**: mcp__gemini-image__describe_image

**Procedure**:
1. Identify all image references in the document (embedded images, screenshot paths, diagram links)
2. For each image, call `describe_image` with an III-calibrated prompt:
   - "Describe this image precisely. List every label, number, status indicator, and UI element visible. Note any text that appears cut off, inconsistent, or potentially misleading."
3. Cross-reference the image description against the document's textual claims:
   - Do labels in the image match labels in the text?
   - Are counts/numbers consistent between image and prose?
   - Does the image show what the text claims it shows?
4. Check for presentation issues: misleading axis scales, truncated data, unclear legends, accessibility gaps
5. Produce `Finding[]` with F-VIS prefix for each inconsistency or misleading element

**Severity calibration**: Label/number mismatches between image and text are High. Misleading presentation is Medium. Minor accessibility issues are Low.

---

## Modality 4: Data Inspect (Conditional)

**Trigger**: Document references or embeds structured data files (YAML, JSON, JSONL, CSV).

**Tools**: Read (for parsing files)

**Procedure**:
1. Identify all data file references in the document
2. Read each referenced data file
3. Validate schema consistency:
   - Are all expected fields present in every record?
   - Are field types consistent across records?
   - Are enum values within expected ranges?
4. Cross-reference data against document claims:
   - Do summary statistics in the text match computed values from the data?
   - Are example records cited in the text actually present in the data?
   - Do counts stated in prose match actual record counts?
5. Check for data quality issues: duplicate records, missing required fields, outlier values without explanation
6. Produce `Finding[]` with F-DATA prefix for each inconsistency or quality issue

**Severity calibration**: Mismatched statistics between prose and data are Critical. Schema inconsistencies are High. Missing optional fields are Low.

---

## Related

- [[how/skills/skill_iii_review|III Review Skill]] — orchestrator that invokes these procedures
- [[what/context/core_domain_packs/context_iii_introspect_checks|INTROSPECT Checks Reference]] — structural analysis checks (Step 2)
- [[what/context/core_domain_packs/context_iii_learning_store|Learning Store]] — corrections injected during text inspect
- [[what/lattices/lattice_iii_verification_oracle|III Verification Oracle]] — lattice architecture for all nodes
