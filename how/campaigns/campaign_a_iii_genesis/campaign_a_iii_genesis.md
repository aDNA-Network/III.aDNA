---
type: campaign
campaign_id: campaign_a_iii_genesis
title: "Campaign A: III.aDNA Genesis & Foundation"
status: in_progress
created: 2026-05-07
updated: 2026-05-07
last_edited_by: agent_stanley
phase: P0
tags: [campaign, genesis, migration, framework]
---

# Campaign A: III.aDNA Genesis & Foundation

## Mission

Bootstrap III.aDNA as a standalone Framework.aDNA project, migrate all core system content from lattice-labs, decompose into composable modules, and implement the reference Airlock.

## DG-A Criteria

- [ ] CLAUDE.md authored with Argus identity + full architecture
- [ ] ADRs 000-003 ratified (Stanley approval)
- [ ] Core skill + module + lattice migrated from lattice-labs; forward stubs in place
- [ ] All 7 domain packs + corrections.jsonl at canonical paths in `what/context/core_domain_packs/`
- [ ] 8 module files present in `what/modules/`
- [ ] `how/airlock/AIRLOCK.md` implemented (5 entry paths minimum)
- [ ] lattice-labs forward stubs verified (active campaigns `campaign_whitepaper_iii_deep_review` + `campaign_kinn_branding_iii` still resolve III context)
- [ ] Initial git tag v0.1.0 committed

## Mission Table

| Mission | Objective | Sessions | Status |
|---------|-----------|----------|--------|
| **MA-0** | Fork + identity: vault creation, CLAUDE.md, MANIFEST, STATE, ADRs 000-003 | 1 | ✅ COMPLETE 2026-05-07 |
| **MA-1** | Core migration: skill + module + lattice + corrections.jsonl head-start; stubs in lattice-labs | 1 | ✅ COMPLETE 2026-05-08 |
| **MA-2** | Domain pack migration: 7 .md packs to canonical core_domain_packs/ | 1 | NEXT |
| **MA-3** | Module decomposition: 8 module .md + .module.yaml files | 1-2 | queued |
| **MA-4** | Airlock reference implementation + v0.1.0 tag | 0.5 | queued |

## Critical Path

MA-0 → MA-1 → MA-2 → [DG-A gate, only after MA-3 + MA-4 complete] → Campaign B MA-1 (lattice-labs wrapper, CRITICAL — unblocks active campaigns)

## Risk Register

| # | Risk | Sev | Mitigation |
|---|------|-----|------------|
| R1 | Active lattice-labs campaigns break during migration | HIGH | Forward stubs before any file moves; test stubs before moving originals |
| R2 | Corrections.jsonl canonical ownership ambiguity | MED | ADR-003 graduation ceremony; canonical path established in MA-2 |

## Phase History

| Phase | Name | Status |
|-------|------|--------|
| P0 | Bootstrap (MA-0) | ✅ COMPLETE 2026-05-07 |
| P1 | Content Migration (MA-1, MA-2) | ⚙ IN PROGRESS — MA-1 done; MA-2 next |
| P2 | Module Architecture + Airlock (MA-3, MA-4) | queued |

## AAR (on completion)
<!-- Template: how/templates/template_aar_lightweight.md -->
<!-- To be filled at DG-A close -->
