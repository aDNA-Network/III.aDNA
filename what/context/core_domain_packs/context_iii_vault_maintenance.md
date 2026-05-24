---
type: context
title: "III Vault Maintenance — Domain Configuration"
created: 2026-04-02
updated: 2026-05-08
status: active
last_edited_by: agent_stanley
token_estimate: ~1000
tags: [context, iii, vault, maintenance, quality, review]
banner: "who/assets/banners/banner_context_guide.jpg"
icon: microscope
fair:
  keywords: [iii-review, vault-maintenance, quality-assurance, trap-library]
  license: Apache-2.0
migration_provenance:
  previous_home: lattice-labs/what/context/iii_vault_maintenance/context_iii_vault_maintenance.md
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-2
quality_metric:
  rubric_version: "adr_007_v0"
  scored_at: 2026-05-23
  scored_by: agent_argus
  scoring_mission: campaign_d_federation_adaptive_loop MD-B4
  signal_density: 4
  actionability: 4
  coverage_uniformity: 3
  source_diversity: 2
  cross_topic_coherence: 4
  graduation_yield: 1
  composite: 3.00
  floor_check: triggered
  floor_axes: [source_diversity, graduation_yield]
  notes: "MD-B4 7-pack pilot scoring. source_diversity=2 is the only REAL floor trigger across the 7-pack pilot — pack cites only frontmatter migration_provenance; no ADR / skill / external sources for the trap inventory or reader profiles. Pack revision candidate: add ADR-references for trap-inventory provenance + cross-references to skill_iii_review (which uses these profiles). graduation_yield=1 reflects MB4-2 data-thinness (canonical records graduated_to:\"core\" not pack-specific; no graduated patterns observable in pack body); axis collapse not a pack-quality issue."
---

# III Vault Maintenance — Domain Configuration

Domain-specific configuration for applying the III (Inspect/Introspect/Improve) review framework to vault artifacts. Extends the core Five Traps with vault-specific failure modes and provides reader profiles for different object types.

## Vault-Specific Trap Library

The core Five Traps (analogy, boundary, confidence, completeness, level) apply universally. These additional traps target failure modes specific to an operational knowledge vault:

| Trap | Description | Where It Hides |
|------|-------------|----------------|
| **Staleness trap** | Metrics or claims reference a state that has since changed. A campaign "at 80%" may have completed or stalled. | STATE.md, campaign docs, context files with old `updated` dates |
| **Orphan trap** | Wikilinks to files that no longer exist, or files that exist but are unreferenced. Breaks the knowledge graph. | All files with `[[wikilinks]]`, especially after renames or restructures |
| **Frontmatter drift** | Frontmatter fields diverge from actual file content. Status says `active` but the campaign completed weeks ago. | Campaign/mission/session files after status transitions |
| **Metric inflation** | Campaign summaries round up, aggregate selectively, or cite best-case numbers. "All 12 gaps closed" when 2 were deferred. | Campaign AARs, STATE.md campaign tables, DG assessments |
| **Cascade gap** | Information propagates incompletely through the OODA cascade. A session finding isn't reflected in the mission doc; a mission blocker isn't flagged in the campaign doc. | Session→mission→campaign chain; check `updated` dates for temporal gaps |
| **Template debt** | A file was created from a template but placeholder sections were never filled in. "[Concrete outputs]" or "[TBD]" survive into production. | Mission completion summaries, campaign artifacts, context files |

## Reader Profiles

When running III on vault objects, adopt the appropriate reader profile to calibrate what "precision" means for that object type:

### Campaign Reader
- **Expects**: Accurate mission counts, session estimates, dates, and status fields
- **Checks**: Do all missions referenced in the campaign doc actually exist as files? Do phase gates reference specific criteria? Does the DG assessment match the exit criteria?
- **Red flags**: "~N sessions" without a calibrated estimate, undated status changes, missions referenced but not created

### Doctrine Reader
- **Expects**: Internally consistent rules that don't contradict each other or CLAUDE.md standing orders
- **Checks**: Are all referenced templates/skills/files real? Do rules have clear scope (when they apply and when they don't)? Are version stamps current?
- **Red flags**: Rules that overlap or conflict, references to deprecated files, version stamps older than latest changes

### Context Reader
- **Expects**: Factual accuracy verifiable against current codebase/repo state
- **Checks**: Do code references (function names, file paths, CLI commands) still exist? Are token estimates approximately correct? Is the `updated` date recent enough for the domain?
- **Red flags**: Code examples that reference deleted functions, architectural claims about systems that have been restructured, token estimates >30% off

### STATE.md Reader
- **Expects**: Accurate snapshot of the current operational state, not a historical accumulation
- **Checks**: Do campaign statuses match their actual files? Are "next steps" still relevant? Are blockers still blocked? Does the schwerpunkt declaration reflect actual session allocation?
- **Red flags**: Campaigns listed as "ACTIVE" that haven't had a session in 10+ days, "next steps" pointing to completed work, stale blocker entries

## Quality Dimensions for Vault Health

When scoring vault artifacts, use these 6 dimensions (1-5 scale):

| Dimension | 1 (Critical) | 3 (Acceptable) | 5 (Excellent) |
|-----------|--------------|-----------------|---------------|
| **Freshness** | `updated` >30 days, content stale | `updated` <14 days, mostly current | `updated` <3 days, fully current |
| **Coherence** | Contradicts other vault files | Consistent within its scope | Cross-referenced and verified against related files |
| **Traceability** | No links to sources or related files | Key relationships linked | Full bidirectional linking with provenance |
| **Completeness** | Major sections empty or placeholder | Core content present, some gaps noted | All sections filled, gaps explicitly scoped out |
| **Precision** | Vague claims, undated assertions, "[TBD]" | Specific but unverified claims | All claims dated, sourced, and cross-referenced |
| **Actionability** | No clear next steps or integration points | Next steps listed but vague | Specific next actions with owners and references |

## Application Protocol

When running III against vault artifacts during an OODA evaluation:

1. **Select targets**: Pick 3-5 artifacts spanning different object types (1 campaign doc, 1 doctrine file, 1 context file, STATE.md, 1 mission)
2. **Load this context**: Apply vault-specific traps in addition to the core Five Traps
3. **Adopt reader profile**: Use the appropriate reader profile for each artifact type
4. **Score dimensions**: Rate each artifact on the 6 quality dimensions
5. **Produce improvement table**: Prioritized fixes, routed to appropriate campaigns or backlog
6. **Feed into ORIENT**: Structural patterns across artifacts feed the OODA ORIENT phase

## Multi-Modal Extensions

III can operate beyond text documents:

| Mode | Tool | Application |
|------|------|-------------|
| **Visual inspection** | Gemini `describe_image` with custom prompt | Screenshot audits, dashboard reviews, banner quality |
| **Code inspection** | Agent with codebase access | Verify code references in context files, check function signatures |
| **Data inspection** | Read + analyze | Dataset manifests, JSONL artifacts, YAML schema validation |

Load the appropriate mode based on the artifact type. The Inspect phase adapts its "seeing" to the medium; Introspect and Improve remain the same regardless of input modality.
