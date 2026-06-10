---
type: context
title: "III Vault Maintenance — Domain Configuration"
created: 2026-04-02
updated: 2026-05-28
status: active
last_edited_by: agent_argus
token_estimate: ~1400
tags: [context, iii, vault, maintenance, quality, review, citation_hardening, mx1, post_dg_d_standalone, reflexive_pack]
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
  re_scored_at: 2026-05-28
  re_scored_by: agent_argus
  re_scoring_mission: mx1_vault_maintenance_pack_citation_hardening
  signal_density: 4
  actionability: 4
  coverage_uniformity: 3
  source_diversity: 4
  cross_topic_coherence: 4
  graduation_yield: 1
  composite: 3.33
  floor_check: triggered
  floor_axes: [graduation_yield]
  prior_score:
    scored_at: 2026-05-23
    scoring_mission: campaign_d_federation_adaptive_loop MD-B4
    source_diversity: 2
    composite: 3.00
    floor_axes: [source_diversity, graduation_yield]
  notes: "MX-1 (2026-05-28) citation-hardening pass. source_diversity 2→4: attached internal-source citations (III ADRs 002/003/007 + skills skill_iii_review/skill_session_close_ceremony/skill_context_quality_audit + cross-campaign precedent from Campaign B AAR + Campaign D MD-B6 cascade discipline + OODA cascade primitive in workspace CLAUDE.md) covering trap-inventory provenance + reader-profile dispatch + quality-rubric origin + application-protocol cascade. Capped at 4 (not 5) per rubric §4 — academic source-type absent because vault_maintenance is a REFLEXIVE pack: its subjects ARE III's own ADRs + skills + cascade discipline; external knowledge-management literature (Wiki maintenance, Wikipedia editorial discipline, organizational-memory research) was evaluated and deferred as non-load-bearing — same precedent F2 set for web_design (`context_iii_domain_packs_web_design.md` frontmatter notes line 35). composite (4+4+3+4+4+1)/6 = 20/6 = 3.33 (was 18/6 = 3.00); +0.33 lift; floor_axes contracts from [source_diversity, graduation_yield] → [graduation_yield] (the real content floor lifts; only the disposed measurement artifact remains). graduation_yield retained at 1 per ADR-007 §3.6 amendment (MD-B6 close 2026-05-25): graduation_yield is a READ-side rule that credits packs via the PACK_DELTA_LANDED stage marker + graduation-memo `pack_delta_target` cross-reference, NOT by floor-matching `graduated_to:\"core\"` against pack names — vault_maintenance has received zero PACK_DELTA_LANDED entries because no graduated correction has targeted it (the 5 canonical-graduated entries C-001/C-002/C-005/C-027/C-028 all target other packs); the 1 is a measurement artifact, NOT a content gap, per ADR-007 §3.6. coverage_uniformity HELD at 3 per MX-1 plan-ratification (Stanley 2026-05-28): citation-add only, no thin-section flesh-out — lifting coverage_uniformity is a separate revision pass if earned. The composite 3.33 vs web_design 4.17 reflects different starting axes (web_design F2 lifted signal_density/actionability/coverage_uniformity/cross_topic_coherence to 5 alongside source_diversity 3→4; MX-1 deliberately holds the other axes to scope-discipline a citation-add). MX-1 is a single standalone mission (modelled on MD-X1/MD-X2): patch bump v0.4.0→v0.4.1, no wrapper sweep, no annotated git tag, no DG ceremony, no new ADR, no canonical jsonl touch, no lattice yaml bump."
---

# III Vault Maintenance — Domain Configuration

Domain-specific configuration for applying the III (Inspect/Introspect/Improve) review framework to vault artifacts. Extends the core Five Traps with vault-specific failure modes and provides reader profiles for different object types.

This pack is **reflexive**: it audits the same vault discipline that III's own ADRs and skills define. Sources cited throughout are predominantly III's own governance artifacts (ADRs + skills + campaign precedent) — that is the legitimate and intended provenance for a pack about *vault* maintenance, not a citation gap. The trailing §Sources & Citations section enumerates the full source set.

## Vault-Specific Trap Library

The core Five Traps (analogy, boundary, confidence, completeness, level) apply universally. These additional traps target failure modes specific to an operational knowledge vault:

| Trap | Description | Where It Hides | Source |
|------|-------------|----------------|--------|
| **Staleness trap** | Metrics or claims reference a state that has since changed. A campaign "at 80%" may have completed or stalled. | STATE.md, campaign docs, context files with old `updated` dates | `skill_session_close_ceremony.md` cascade discipline (frontmatter `updated` refresh + STATE.md prepend); workspace `~/aDNA/CLAUDE.md` Standing Rule 7 "operational/campaign state lives in each project's STATE.md" |
| **Orphan trap** | Wikilinks to files that no longer exist, or files that exist but are unreferenced. Breaks the knowledge graph. | All files with `[[wikilinks]]`, especially after renames or restructures | `skill_iii_review.md` Step 0 DISPATCH (path-resolution against the lattice YAML); `adr_001_module_architecture.md` (module purity — modules never read pack files directly; renames must update orchestrating skills) |
| **Frontmatter drift** | Frontmatter fields diverge from actual file content. Status says `active` but the campaign completed weeks ago. | Campaign/mission/session files after status transitions | `adr_002_consumer_federation_contract.md` §3 (wrapper-integrity discipline — `federation_ref.version` + `pinned_at_commit` + `pinned_at` MUST all advance together at minor-bump review); `skill_session_close_ceremony.md` (status:active → status:completed flip + dg_X_outcome propagation) |
| **Metric inflation** | Campaign summaries round up, aggregate selectively, or cite best-case numbers. "All 12 gaps closed" when 2 were deferred. | Campaign AARs, STATE.md campaign tables, DG assessments | `adr_002_consumer_federation_contract.md` §3 (review integrity at minor-bump cadence); Campaign D MD-B6 DG-D scorecard precedent (`md_b6_dg_d_scorecard.md` §3 11-criterion table with `closed_at` + `hard_invariant_touch` + `evidence_link` per row — auditable, not summarized); Campaign F DG-F scorecard `f6_dg_f_scorecard.md` extends the same shape |
| **Cascade gap** | Information propagates incompletely through the OODA cascade. A session finding isn't reflected in the mission doc; a mission blocker isn't flagged in the campaign doc. | Session→mission→campaign chain; check `updated` dates for temporal gaps | OODA cascade primitive (workspace `~/aDNA/CLAUDE.md` `## Standing Rules` §7 + `aDNA.aDNA/` standard); `skill_session_close_ceremony.md` cascade bookkeeping (the 14-canonical-application precedent through Campaigns A/B/C/D/F; STATE.md frontmatter prepend + Current Phase paragraph + Campaigns table flip in the SAME commit as the close action) |
| **Template debt** | A file was created from a template but placeholder sections were never filled in. "[Concrete outputs]" or "[TBD]" survive into production. | Mission completion summaries, campaign artifacts, context files | Campaign B AAR Change #1 — *grep-discharge habit* (`grep -rn "TBD\|placeholder\|\\[Concrete outputs\\]" .` as a session-close hygiene step before status:completed flip); `how/templates/` discipline (`template_*.md` files explicitly mark required fields with `<<>>` brackets so unsubstituted text becomes grep-detectable) |

### Application gloss for the trap library

Each trap's **Source** column names an internal contract (ADR or skill or campaign-precedent artifact) the trap is enforcing the discipline of. The trap fires when the audited artifact violates the contract its source establishes. This is the trap library's reflexive structure: it does not audit vault state against external "best practices" — it audits vault state against III's own published governance.

## Reader Profiles

When running III on vault objects, adopt the appropriate reader profile to calibrate what "precision" means for that object type. Reader-profile dispatch is the inverse of the `pack_assignment` step in `skill_iii_review.md` Step 0 DISPATCH (lattice YAML matches *path → packs*; the reader profile here matches *artifact type → calibration*):

### Campaign Reader
- **Expects**: Accurate mission counts, session estimates, dates, and status fields
- **Checks**: Do all missions referenced in the campaign doc actually exist as files? Do phase gates reference specific criteria? Does the DG assessment match the exit criteria?
- **Red flags**: "~N sessions" without a calibrated estimate, undated status changes, missions referenced but not created
- **Contract source**: `adr_002_consumer_federation_contract.md` §3 (minor-bump review at version-declare commit — campaign-level cadence) + DG scorecard precedent (`md_b6_dg_d_scorecard.md` 11-criterion shape, replicated at `f6_dg_f_scorecard.md`)

### Doctrine Reader
- **Expects**: Internally consistent rules that don't contradict each other or CLAUDE.md standing orders
- **Checks**: Are all referenced templates/skills/files real? Do rules have clear scope (when they apply and when they don't)? Are version stamps current?
- **Red flags**: Rules that overlap or conflict, references to deprecated files, version stamps older than latest changes
- **Contract source**: III root `CLAUDE.md` § Standing Rules (7 numbered rules — the doctrine reader's enforcement target); `adr_001_module_architecture.md` (module purity invariant — Rule 3 in CLAUDE.md); workspace `~/aDNA/CLAUDE.md` § Standing Rules 1–8

### Context Reader
- **Expects**: Factual accuracy verifiable against current codebase/repo state
- **Checks**: Do code references (function names, file paths, CLI commands) still exist? Are token estimates approximately correct? Is the `updated` date recent enough for the domain?
- **Red flags**: Code examples that reference deleted functions, architectural claims about systems that have been restructured, token estimates >30% off
- **Contract source**: `adr_007_adaptive_improvement_loop.md` §3 (per-pack quality metric — pack-level cadence); `skill_context_quality_audit.md` (the 5-axis rubric ADR-007 §3 extends to 6 axes; the Context Reader is the operational counterpart of running the audit skill against a context file)

### STATE.md Reader
- **Expects**: Accurate snapshot of the current operational state, not a historical accumulation
- **Checks**: Do campaign statuses match their actual files? Are "next steps" still relevant? Are blockers still blocked? Does the schwerpunkt declaration reflect actual session allocation?
- **Red flags**: Campaigns listed as "ACTIVE" that haven't had a session in 10+ days, "next steps" pointing to completed work, stale blocker entries
- **Contract source**: `skill_session_close_ceremony.md` (the STATE.md frontmatter-prepend + Current Phase paragraph + Campaigns table flip discipline — the 14-canonical-application precedent through Campaigns A/B/C/D and Campaign F F6); workspace `~/aDNA/CLAUDE.md` Standing Rule 7 ("operational/campaign state lives in each project's STATE.md, **never in this router**")

## Quality Dimensions for Vault Health

When scoring vault artifacts, use these 6 dimensions (1-5 scale). The rubric is parallel to but distinct from `adr_007_adaptive_improvement_loop.md` §3's per-*pack* 6-axis rubric: ADR-007 §3 scores *domain packs themselves*; this table scores *the vault artifacts a domain pack is applied to* (an artifact-level rubric, not a pack-level rubric). The two rubrics share a numerical scale convention and the floor-rule pattern (any axis ≤ 2 triggers revision regardless of composite), drawn from `skill_context_quality_audit.md` lines 50–60.

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
2. **Load this context**: Apply vault-specific traps in addition to the core Five Traps (per `skill_iii_review.md` Step 0 DISPATCH, this pack loads alongside `core`)
3. **Adopt reader profile**: Use the appropriate reader profile for each artifact type (§ Reader Profiles above)
4. **Score dimensions**: Rate each artifact on the 6 quality dimensions (§ Quality Dimensions above)
5. **Produce improvement table**: Prioritized fixes, routed to appropriate campaigns or backlog (`skill_iii_review.md` Step 3 IMPROVE format)
6. **Feed into ORIENT**: Structural patterns across artifacts feed the OODA ORIENT phase; recurring patterns also feed `skill_iii_review.md` Step 3b ACCUMULATE (consumer-vault local store, never canonical except via `adr_003_learning_store_ownership.md` §3 graduation ceremony)

## Multi-Modal Extensions

III can operate beyond text documents. Each mode corresponds to a typed-pure aDNA module orchestrated by `skill_iii_review.md` (per the lattice composition documented at `adr_001_module_architecture.md`):

| Mode | Tool | Application | Module |
|------|------|-------------|--------|
| **Visual inspection** | Gemini `describe_image` with custom prompt | Screenshot audits, dashboard reviews, banner quality | `module_iii_inspect_visual.md` |
| **Code inspection** | Agent with codebase access | Verify code references in context files, check function signatures | `module_iii_inspect_code.md` |
| **Data inspection** | Read + analyze | Dataset manifests, JSONL artifacts, YAML schema validation | `module_iii_inspect_data.md` |

Load the appropriate mode based on the artifact type. The Inspect phase adapts its "seeing" to the medium; Introspect and Improve remain the same regardless of input modality (per `module_iii_introspect.md` + `module_iii_improve.md` — both pack-agnostic, modality-agnostic).

## Sources & Citations

Internal grounding for the trap inventory + reader profiles + quality dimensions + application protocol (the `source_diversity` corpus). vault_maintenance is a **reflexive pack**: its subjects ARE III's own governance, so its citation set is correctly internal-source-heavy. External knowledge-management literature was evaluated and deferred at MX-1 (2026-05-28) as non-load-bearing — same principled-4 ceiling F2 set for `context_iii_domain_packs_web_design.md` (`§Sources & Citations` notes line 35).

### Internal contracts (III canonical ADRs)

1. **`what/decisions/adr_001_module_architecture.md`** — 8-module composition + purity invariant (module never reads pack files directly; skill orchestrates pack loading + binds typed inputs). Cited at: Orphan trap (rename discipline), Multi-Modal Extensions (the 8 modules the modality table maps to), § Quality Dimensions intro (rubric parallelism).
2. **`what/decisions/adr_002_consumer_federation_contract.md`** §3 — minor-bump consumer-review trigger; `federation_ref.version` + `pinned_at_commit` + `pinned_at` + `lattice_version` MUST advance together at version-declare commit (the wrapper-integrity discipline). Cited at: Frontmatter drift trap, Metric inflation trap, Campaign Reader profile.
3. **`what/decisions/adr_003_learning_store_ownership.md`** §3 — graduation ceremony gates (frequency ≥ 3 + acceptance ≥ 80% + ≥2-vault evidence for elevated scrutiny per §3.6); canonical-vs-consumer-local store boundary (consumer edits never push automatically; canonical receives entries only via graduation ceremony). Cited at: Application Protocol step 6 (ACCUMULATE routing).
4. **`what/decisions/adr_007_adaptive_improvement_loop.md`** §3 — per-pack 6-axis rubric (signal_density, actionability, coverage_uniformity, source_diversity, cross_topic_coherence, graduation_yield); §3.6 amendment (MD-B6 2026-05-25) — graduation_yield credits packs via PACK_DELTA_LANDED stage marker + graduation-memo `pack_delta_target` cross-reference, NOT by floor-matching `graduated_to: "core"`. Cited at: § Quality Dimensions intro (rubric parallelism), this pack's own quality_metric frontmatter (the 6 axes rubric source).

### Internal procedures (III canonical skills)

5. **`how/skills/skill_iii_review.md`** — the orchestration that loads this pack (Step 0 DISPATCH); Step 3b ACCUMULATE (learning-store routing); Step 1 INSPECT (Five Traps + domain packs + corrections injection). Cited at: Reader Profiles section intro (dispatch inverse), Application Protocol steps 2 + 5 + 6, Multi-Modal Extensions section.
6. **`how/skills/skill_session_close_ceremony.md`** — the cascade-bookkeeping discipline (STATE.md frontmatter prepend + Current Phase paragraph + Campaigns table flip in SAME commit as close action); 14 canonical post-adoption applications through Campaigns A/B/C/D and Campaign F F6 (15 including MX-1 close). Cited at: Staleness trap, Frontmatter drift trap, Cascade gap trap, STATE.md Reader profile.
7. **`how/skills/skill_context_quality_audit.md`** — the 5-axis rubric ADR-007 §3 extends to 6 axes; lines 50–60 establish the 1–5 scale convention this pack's § Quality Dimensions adopts. Cited at: § Quality Dimensions intro, Context Reader profile.

### Cross-campaign precedent

8. **Campaign B AAR Change #1** (`how/campaigns/campaign_b_iii_federation/`) — grep-discharge habit for template debt: `grep -rn "TBD\|placeholder\|\\[Concrete outputs\\]"` as a session-close hygiene step. Cited at: Template debt trap.
9. **Campaign D MD-B6 DG-D scorecard** (`what/artifacts/md_b6_dg_d_scorecard.md`) — 11-criterion auditable table shape (`closed_at` + `hard_invariant_touch` + `evidence_link` per row); replicated at Campaign F (`f6_dg_f_scorecard.md`). The anti-metric-inflation discipline. Cited at: Metric inflation trap, Campaign Reader profile.

### Workspace-level grounding

10. **Workspace `~/aDNA/CLAUDE.md`** § Standing Rules 1–8 — particularly Rule 7 ("router rows carry routing identity only; operational/campaign state lives in each project's STATE.md"); the inverse of which IS the STATE.md Reader's enforcement target. Cited at: Staleness trap, Cascade gap trap, Doctrine Reader profile, STATE.md Reader profile.

### Citation ceiling rationale (principled-4)

The `source_diversity` rubric (`adr_007_adaptive_improvement_loop.md` §3 + `skill_context_quality_audit.md` source-diversity definition) reserves the 5-score for source sets that include peer-reviewed academic literature alongside vendor/practitioner/empirical sources. vault_maintenance is a **reflexive pack** — its subjects ARE III's own governance, and the academically-recognized literature for "vault maintenance" (Wikipedia editorial discipline; organizational-memory research; wiki-maintenance practitioner studies) is at the wrong abstraction layer (general-purpose wiki hygiene, not aDNA-specific governance). At MX-1 the operator evaluated and explicitly deferred chasing the 5-score: external citation-padding on a reflexive domain would *reduce* signal density, not increase source diversity in load-bearing fashion. The principled ceiling is **4**, mirroring F2's same decision at `context_iii_domain_packs_web_design.md` (which capped at 4 absent academic citations for web design).

## Related

- [[how/skills/skill_iii_review|III Review Skill]] — the orchestration that loads this pack
- [[how/skills/skill_session_close_ceremony|Session Close Ceremony Skill]] — cascade-bookkeeping discipline the Staleness + Frontmatter drift + Cascade gap traps enforce
- [[what/context/core_domain_packs/context_iii_inspect_procedures|INSPECT Procedures Reference]] — Five Traps + modality procedures (the core this pack extends)
- [[what/context/core_domain_packs/context_iii_introspect_checks|INTROSPECT Checks Reference]] — 7 structural checks run after INSPECT
- [[what/decisions/adr_002_consumer_federation_contract|ADR-002 Consumer Federation Contract]] — Frontmatter drift + Metric inflation + Campaign Reader contracts
- [[what/decisions/adr_007_adaptive_improvement_loop|ADR-007 Adaptive-Improvement Loop]] — § Quality Dimensions parallel rubric; §3.6 amendment governing the graduation_yield artifact
