---
type: campaign
campaign_id: campaign_a_iii_genesis
title: "Campaign A: III.aDNA Genesis & Foundation"
status: completed
created: 2026-05-07
updated: 2026-05-08
last_edited_by: agent_stanley
phase: closed
ma3_complete: 2026-05-08
ma4_complete: 2026-05-08
dg_a_closed: 2026-05-08
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
- [x] `how/airlock/AIRLOCK.md` implemented (5 entry paths minimum) (MA-4 ✅, reference implementation v0.1.0 with Path Selection matrix + per-path enrichment + anti-regression section)
- [x] lattice-labs forward stubs verified (active campaigns `campaign_whitepaper_iii_deep_review` + `campaign_kinn_branding_iii` still resolve III context) (MA-1 + MA-2 ✅)
- [x] Initial git tag v0.1.0 committed (MA-4 ✅, annotated tag at MA-4 closure commit)

**DG-A scorecard: 9/9 green — DG-A CLOSED 2026-05-08.**

## Mission Table

| Mission | Objective | Sessions | Status |
|---------|-----------|----------|--------|
| **MA-0** | Fork + identity: vault creation, CLAUDE.md, MANIFEST, STATE, ADRs 000-003 | 1 | ✅ COMPLETE 2026-05-07 |
| **MA-1** | Core migration: skill + module + lattice + corrections.jsonl head-start; stubs in lattice-labs | 1 | ✅ COMPLETE 2026-05-08 |
| **MA-2** | Domain pack migration: 7 .md packs to canonical core_domain_packs/ (KINN carved out as consumer-specific) | 1 | ✅ COMPLETE 2026-05-08 |
| **MA-3** | Module decomposition: 8 module .md + .module.yaml files | 1 | ✅ COMPLETE 2026-05-08 |
| **MA-4** | Airlock reference implementation + v0.1.0 tag | 0.5 | ✅ COMPLETE 2026-05-08 |

## Critical Path

MA-0 ✅ → MA-1 ✅ → MA-2 ✅ → MA-3 ✅ → MA-4 ✅ → **[DG-A gate CLOSED 2026-05-08]** → Campaign B MB-1 (lattice-labs wrapper, CRITICAL — retires lattice-labs operational corrections.jsonl per ADR-003) // Campaign C MC-1 (airlock standard v0.2 spec; can run parallel)

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
| P2 | Module Architecture + Airlock (MA-3, MA-4) | ✅ COMPLETE 2026-05-08 — DG-A CLOSED 9/9 |

## AAR
<!-- Template: how/templates/template_aar_lightweight.md -->

- **Worked**: Sequential mission decomposition (MA-0 fork → MA-1 core → MA-2 packs → MA-3 modules → MA-4 airlock + tag) held throughout. Each mission closed in roughly one session, each surfaced 0–4 non-blocking follow-ups, none required scope-creep into the next mission. Forward-stub pattern (MA-1, MA-2) kept active lattice-labs campaigns (`whitepaper_iii_deep_review`, `kinn_branding_iii`) from breaking — zero consumer disruption during the migration. The MA-3 lattice rewire (adding `accumulate` as a first-class typed-IO module) was a net structural improvement, not just renaming. The MA-4 anti-regression discipline (citing the VideoForge v0.2 proposal in v0.1.0's "What v0.1.0 does NOT cover" section) closed the gate without preempting Campaign C's design space.
- **Didn't**: Two missions had small scope nudges that proved correct in retrospect: MA-3 added a 4-line "Module composition" callout to `skill_iii_review.md` (frontmatter-flagged for auditability), and MA-4 added a Path Selection matrix that the original v0.1 stub didn't have. Both nudges were architecturally load-bearing — the skill needs to advertise the module surface, and the airlock needs a one-read decision surface. Future campaigns: explicitly budget for "structural improvements discovered during execution" so they don't read as scope creep.
- **Finding**: Framework.aDNA as a category sustained through 5 missions without needing a category-level revision — the consumer wrapper pattern (`iii/CLAUDE.md` + `federation_ref` + optional local extensions, per ADR-002) covered every consumer profile that surfaced (lattice-labs, SiteForge, VideoForge, CanvasForge, wga). The v0.1.0 release point sits exactly where the canonical spec said it should: pre-federation baseline, ready for Campaign B consumer wiring.
- **Change**: Establish a "carry-forward follow-ups" pattern in STATE.md per-mission close. Campaign A accumulated four (reviewed_output disposition, multi-voice → SiteForge, missing AGENTS.md wikilink, skill_canvas_iii_review placement) — none were urgent enough to interrupt the critical path, but tracking them as a list ensures Campaign B planning starts with full visibility. Pattern works for Campaigns B and C.
- **Follow-up**: Four MA-3 carry-forwards remain (above) for Campaign B planning. Inbound VideoForge v0.2 proposal (`who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md`) is queued for Campaign C MC-1. Stanley to choose Campaign B vs. C opening order; recommended: Campaign B MB-1 first (retires the operational corrections.jsonl, the one critical-path consumer migration), Campaign C MC-1 second (less time-pressure since v0.1.0 is shipped).
