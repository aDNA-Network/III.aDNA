---
type: campaign
campaign_id: campaign_a_iii_genesis
title: "Campaign A: III.aDNA Genesis & Foundation"
status: in_progress
created: 2026-05-07
updated: 2026-05-08
last_edited_by: agent_stanley
phase: P2
ma3_complete: 2026-05-08
tags: [campaign, genesis, migration, framework]
---

# Campaign A: III.aDNA Genesis & Foundation

## Mission

Bootstrap III.aDNA as a standalone Framework.aDNA project, migrate all core system content from lattice-labs, decompose into composable modules, and implement the reference Airlock.

## DG-A Criteria

- [x] CLAUDE.md authored with Argus identity + full architecture (MA-0 ✅)
- [x] ADRs 000-003 ratified (Stanley approval) (MA-0 ✅)
- [x] Core skill + module + lattice migrated from lattice-labs; forward stubs in place (MA-1 ✅)
- [x] All 7 core domain packs + corrections.jsonl at canonical paths in `what/context/core_domain_packs/` (MA-2 ✅, KINN carved out as consumer-specific)
- [x] 8 module files present in `what/modules/` (MA-3 ✅, +legacy `module_iii_semantic_reviewer` rebranded as `composite_reference`)
- [ ] `how/airlock/AIRLOCK.md` implemented (5 entry paths minimum) (MA-4 — NEXT)
- [x] lattice-labs forward stubs verified (active campaigns `campaign_whitepaper_iii_deep_review` + `campaign_kinn_branding_iii` still resolve III context) (MA-1 + MA-2 ✅)
- [ ] Initial git tag v0.1.0 committed (MA-4)

**DG-A scorecard: 7/9 green** (was 6/9 before MA-3).

## Mission Table

| Mission | Objective | Sessions | Status |
|---------|-----------|----------|--------|
| **MA-0** | Fork + identity: vault creation, CLAUDE.md, MANIFEST, STATE, ADRs 000-003 | 1 | ✅ COMPLETE 2026-05-07 |
| **MA-1** | Core migration: skill + module + lattice + corrections.jsonl head-start; stubs in lattice-labs | 1 | ✅ COMPLETE 2026-05-08 |
| **MA-2** | Domain pack migration: 7 .md packs to canonical core_domain_packs/ (KINN carved out as consumer-specific) | 1 | ✅ COMPLETE 2026-05-08 |
| **MA-3** | Module decomposition: 8 module .md + .module.yaml files | 1 | ✅ COMPLETE 2026-05-08 |
| **MA-4** | Airlock reference implementation + v0.1.0 tag | 0.5 | NEXT |

## Critical Path

MA-0 ✅ → MA-1 ✅ → MA-2 ✅ → MA-3 ✅ → **MA-4** → [DG-A gate] → Campaign B MA-1 (lattice-labs wrapper, CRITICAL — retires lattice-labs operational corrections.jsonl per ADR-003)

## Risk Register

| # | Risk | Sev | Mitigation |
|---|------|-----|------------|
| R1 | Active lattice-labs campaigns break during migration | HIGH | Forward stubs before any file moves; test stubs before moving originals |
| R2 | Corrections.jsonl canonical ownership ambiguity | MED | ADR-003 graduation ceremony; canonical path established in MA-2 |

## Phase History

| Phase | Name | Status |
|-------|------|--------|
| P0 | Bootstrap (MA-0) | ✅ COMPLETE 2026-05-07 |
| P1 | Content Migration (MA-1, MA-2) | ✅ COMPLETE 2026-05-08 |
| P2 | Module Architecture + Airlock (MA-3, MA-4) | ⚙ IN PROGRESS — MA-4 next (MA-3 ✅ 2026-05-08) |

## AAR (on completion)
<!-- Template: how/templates/template_aar_lightweight.md -->
<!-- To be filled at DG-A close -->
