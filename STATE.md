---
type: state
created: 2026-05-07
updated: 2026-05-07
status: active
last_edited_by: agent_stanley
last_session: session_stanley_20260507_iii_adna_genesis
tags: [state, governance]
---

# III.aDNA — Operational State

## Current Phase

**Campaign A: Genesis + Foundation — Phase 0 MA-0 IN PROGRESS**

Vault bootstrapped from aDNA template. CLAUDE.md, MANIFEST.md, STATE.md authored. ADRs 000-003 being drafted. Core content migration (MA-1, MA-2) queued. Module architecture (MA-3) and airlock implementation (MA-4) follow.

## Campaigns

| Campaign | Goal | Phase | Status |
|----------|------|-------|--------|
| **Campaign A: Genesis** | Fork, identity, content migration, module architecture | P0-P2 | **MA-0 IN PROGRESS** |
| **Campaign B: Federation** | Wire iii/ consumer wrappers across all vaults | P1-P3 | Pending DG-A |
| **Campaign C: Airlock** | Formalize airlock standard + propagate to major vaults | P1-P2 | Pending DG-A (overlaps B) |

## Campaign A Mission Queue

| Mission | Goal | Est. | Status |
|---------|------|------|--------|
| MA-0: Fork + Identity | Vault created, CLAUDE.md/MANIFEST/STATE/ADRs authored | 1 sess | **IN PROGRESS** |
| MA-1: Core migration | skill_iii_review, module, lattice migrated; lattice-labs stubs added | 1 sess | queued |
| MA-2: Domain pack migration | 7 core packs + corrections.jsonl migrated | 1 sess | queued |
| MA-3: Module decomposition | 8 module .md + .module.yaml files authored | 1-2 sess | queued |
| MA-4: Airlock + v0.1.0 | AIRLOCK.md reference implementation; initial git tag | 0.5 sess | queued |

## DG-A Gate Criteria

- [ ] CLAUDE.md authored with Argus identity + full architecture
- [ ] ADRs 000-003 ratified (Stanley approval)
- [ ] Core skill + module + lattice migrated from lattice-labs
- [ ] All 7 domain packs + corrections.jsonl at canonical paths
- [ ] 8 module files present in what/modules/
- [ ] how/airlock/AIRLOCK.md implemented (5 entry paths)
- [ ] lattice-labs forward stubs in place (active campaigns unblocked)
- [ ] Initial git tag v0.1.0

## What's Working

- Vault bootstrapped (fork from adna/.adna/ 2026-05-07)
- CLAUDE.md v1.0 authored (Argus persona, Framework.aDNA category, consumer pattern)
- MANIFEST.md authored (project identity, active consumer table, entry points)
- STATE.md authored (this file)
- ADR directory structure created

## Blockers

None currently. MA-1 (core migration) is the critical path to DG-A.

## Risk Register

| # | Risk | Sev | Mitigation |
|---|------|-----|------------|
| R1 | Active lattice-labs campaigns break during migration | HIGH | Forward stubs before any file moves |
| R2 | Consumer version drift | MED | ADR-002 version policy; minor-bump review gate |
| R3 | VideoForge ADR-006 bridge complexity | MED | ADR-006 stays in VideoForge; III.aDNA provides core only |
| R4 | iii_corrections.jsonl canonical ownership | MED | ADR-003 graduation ceremony protocol |
| R5 | aDNA template is public repo — additive changes only | MED | MB-5 last in Campaign B; PR if needed |

## Learning Store Status

- Canonical entries: 26 (pending migration from `lattice-labs/what/context/iii_domain_packs/iii_corrections.jsonl`)
- Graduated to domain packs: 5 (C-001 projective_claim_as_fact, C-002 aspiration_as_current_capability, C-005 + 2 more)
- Graduation candidates: C-003 (freq=?, acceptance=?), C-009 (freq=?, acceptance=?)
- Canonical path (post-MA-2): `what/context/core_domain_packs/iii_corrections_canonical.jsonl`

## Latest Direction — 2026-05-07

Executing Campaign A MA-0. Next session: MA-1 core system migration (skill + module + lattice from lattice-labs).
