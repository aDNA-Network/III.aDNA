---
type: state
created: 2026-05-07
updated: 2026-05-08
status: active
last_edited_by: agent_stanley
last_session: session_stanley_20260508_iii_adna_ma2_domain_pack_migration
tags: [state, governance]
---

# III.aDNA — Operational State

## Current Phase

**Campaign A: Genesis + Foundation — Phase 1 MA-2 COMPLETE; MA-3 NEXT**

MA-0 (vault bootstrap, CLAUDE.md, MANIFEST, STATE, 4 ADRs, AIRLOCK.md v0.1) closed
2026-05-07 (commits `4df4d9b`, `a28045b`).

MA-1 (core system migration) closed 2026-05-08 (III.aDNA commit `f2374af`,
lattice-labs commit `f889ea71` pushed): canonical skill, module .md/.yaml,
lattice .yaml, and `iii_corrections_canonical.jsonl` (md5 verified) landed in
III.aDNA. lattice-labs originals replaced with `[MIGRATED]` stubs + MIGRATION_NOTE.md
sibling for the corrections.jsonl (which stays operational until MB-1).

MA-2 (domain pack migration) closed 2026-05-08: 7 canonical core domain packs
landed at `III.aDNA/what/context/core_domain_packs/` (inspect_procedures,
introspect_checks, learning_store, web_design, whitepaper_communication,
canvas_visual, vault_maintenance). Each pack carries a `migration_provenance`
frontmatter block; intra-pack wikilinks rebased from `iii_domain_packs/` →
`core_domain_packs/` and `lattices/skills/` → `lattices/`. lattice-labs originals
replaced with `[MIGRATED]` forward-pointer stubs; both `iii_domain_packs/` and
`iii_vault_maintenance/` source dirs now have MIGRATION_NOTE.md siblings.
KINN pack (`context_iii_kinn_branding.md`) carved out as consumer-specific —
remains operational at lattice-labs unchanged. Skill `skill_iii_review.md`
wikilinks rebased; deferred path-warnings removed.

## Campaigns

| Campaign | Goal | Phase | Status |
|----------|------|-------|--------|
| **Campaign A: Genesis** | Fork, identity, content migration, module architecture | P0-P2 | **P1 IN PROGRESS — MA-3 next** |
| **Campaign B: Federation** | Wire iii/ consumer wrappers across all vaults | P1-P3 | Pending DG-A |
| **Campaign C: Airlock** | Formalize airlock standard + propagate to major vaults | P1-P2 | Pending DG-A (overlaps B) |

## Campaign A Mission Queue

| Mission | Goal | Est. | Status |
|---------|------|------|--------|
| MA-0: Fork + Identity | Vault created, CLAUDE.md/MANIFEST/STATE/ADRs authored | 1 sess | ✅ **COMPLETE 2026-05-07** |
| MA-1: Core migration | skill_iii_review, module, lattice + corrections.jsonl head-start migrated; stubs added | 1 sess | ✅ **COMPLETE 2026-05-08** |
| MA-2: Domain pack migration | 7 core domain pack .md files migrated to core_domain_packs/ | 1 sess | ✅ **COMPLETE 2026-05-08** |
| MA-3: Module decomposition | 8 module .md + .module.yaml files authored | 1-2 sess | **NEXT** |
| MA-4: Airlock + v0.1.0 | AIRLOCK.md reference implementation; initial git tag | 0.5 sess | queued |

## DG-A Gate Criteria

- [x] CLAUDE.md authored with Argus identity + full architecture (MA-0 ✅)
- [x] ADRs 000-003 ratified (Stanley approval) (MA-0 ✅)
- [x] Core skill + module + lattice migrated from lattice-labs (MA-1 ✅)
- [x] lattice-labs forward stubs in place (active campaigns unblocked) (MA-1 ✅)
- [x] corrections.jsonl canonical upstream operational (MA-1 ✅, head-start scope)
- [x] All 7 core domain packs at canonical paths (MA-2 ✅, KINN carved out as consumer-specific)
- [ ] 8 module files present in what/modules/ (MA-3 — NEXT)
- [ ] how/airlock/AIRLOCK.md implemented (5 entry paths) (MA-4 — partial via v0.1 stub from MA-0)
- [ ] Initial git tag v0.1.0 (MA-4)

**DG-A scorecard: 6/9 green** (was 5/9 before MA-2).

## What's Working

- Vault bootstrapped (fork from adna/.adna/ 2026-05-07)
- CLAUDE.md v1.0 authored (Argus persona, Framework.aDNA category, consumer pattern)
- MANIFEST.md authored (project identity, active consumer table, entry points)
- STATE.md authored (this file)
- ADRs 000-003 ratified at `what/decisions/`
- Core III canonical files live at III.aDNA (skill, module, lattice, corrections.jsonl)
- 7 canonical core domain packs live at `what/context/core_domain_packs/`: inspect_procedures, introspect_checks, learning_store, web_design, whitepaper_communication, canvas_visual, vault_maintenance (all carry `migration_provenance` provenance)
- lattice-labs forward stubs in place; active campaigns continue uninterrupted (whitepaper_iii_deep_review at 7/25 cycles, kinn_branding_iii at 50/100)
- corrections.jsonl canonical upstream operational; consumer-vault local store pattern ready (per ADR-003)
- skill_iii_review.md wikilinks all resolve to canonical core_domain_packs/ paths; no remaining `iii_domain_packs/` references

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

- Canonical entries: 26 (live at `what/context/core_domain_packs/iii_corrections_canonical.jsonl`, md5 `dde2cbd8...`)
- Graduated to domain packs: 5 (C-001 projective_claim_as_fact, C-002 aspiration_as_current_capability, C-005 + 2 more)
- Graduation candidates: C-003 (freq=?, acceptance=?), C-009 (freq=?, acceptance=?)
- lattice-labs operational copy (`iii_corrections.jsonl`, md5 `dde2cbd8...`) remains in place until MB-1 retires it via the `lattice-labs/iii/` consumer wrapper

## Latest Direction — 2026-05-08 (post-MA-2)

MA-2 closed. 7 core domain packs migrated to canonical paths; KINN pack carved out
as consumer-specific (stays at lattice-labs unchanged). All wikilinks in
skill_iii_review.md rebased; path warnings removed. Both vaults staged for commit.

**Next session: MA-3 — Module decomposition.** Decompose the existing single
module (`module_iii_semantic_reviewer.{md,yaml}`) and the conceptual III pipeline
into 8 composable aDNA modules per CLAUDE.md project-map architecture:

- `module_iii_dispatch.md` (+ `.module.yaml`)
- `module_iii_inspect_text.md` (+ yaml)
- `module_iii_inspect_code.md` (+ yaml)
- `module_iii_inspect_visual.md` (+ yaml)
- `module_iii_inspect_data.md` (+ yaml)
- `module_iii_introspect.md` (+ yaml)
- `module_iii_improve.md` (+ yaml)
- `module_iii_accumulate.md` (+ yaml)

Each module pure (no domain pack loading); skill orchestrates pack selection.
Existing `module_iii_semantic_reviewer.{md,yaml}` either retires or rebrands as a
composite reference.

### Open finding for Campaign B intake

`lattice-labs/how/skills/skill_canvas_iii_review.md` is listed in CLAUDE.md project
map as III.aDNA-canonical (`how/skills/skill_canvas_iii_review.md`) but currently
sits at lattice-labs. Likely belongs to CanvasForge.aDNA's `iii/` wrapper rather
than III.aDNA core. Decision deferred to Campaign B planning. Not in MA-2 scope;
not blocking MA-3.

### MA-2 scope-adjacent change to learning_store.md

The migrated `context_iii_learning_store.md` body received doctrinal alignment to
ADR-003 (canonical/local split) beyond pure path rebases. Original content described
a single in-place corrections file that predates ADR-003. The update brings the
pack current with the ratified architecture. Documented in the pack's
`migration_provenance` block and the MA-2 stub at lattice-labs.

### Architecture confirmed (Stanley 2026-05-08)

III is **one aDNA vault, one context graph**. The 8 MA-3 modules live as 8 files
inside `III.aDNA/what/modules/` — NOT as 8 separate aDNA projects. "Composable"
means each module is a typed-IO aDNA primitive (per ADR-001) wired by
`what/lattices/lattice_iii_verification_oracle.lattice.yaml`; consumers (other
vaults) federate to III.aDNA as a single source via their `iii/` wrapper and pick
which subset of modules to invoke through the lattice. Module boundaries are
units of decomposition WITHIN III.aDNA — not vault boundaries.

### MA-3 jump-start — module source mapping

Each future module decomposes from existing canonical content already at III.aDNA.
The work is partition + typing, not authorship from scratch.

| Module | Primary source (already at III.aDNA) | Notes |
|--------|--------------------------------------|-------|
| `module_iii_dispatch` | `how/skills/skill_iii_review.md` Step 0 | Path/frontmatter/queue dispatch; loads no domain packs (skill orchestrates) |
| `module_iii_inspect_text` | skill Step 1 + `core_domain_packs/context_iii_inspect_procedures.md` Modality 1 | Always-on text modality |
| `module_iii_inspect_code` | skill Step 1 + inspect_procedures Modality 2 | Conditional |
| `module_iii_inspect_visual` | skill Step 1 + inspect_procedures Modality 3 | Conditional |
| `module_iii_inspect_data` | skill Step 1 + inspect_procedures Modality 4 | Conditional |
| `module_iii_introspect` | skill Step 2 + `core_domain_packs/context_iii_introspect_checks.md` (7 checks) | Operates on merged inspect findings |
| `module_iii_improve` | skill Step 3 | Findings → ranked change orders |
| `module_iii_accumulate` | skill Step 3b + `core_domain_packs/context_iii_learning_store.md` (post-MA-2 ADR-003 doctrine) | Writes to consumer-vault local store; canonical via graduation ceremony |

Existing `what/modules/module_iii_semantic_reviewer.{md,yaml}` (MA-1 migration)
is the legacy composite. MA-3 closeout decision: retire it OR rebrand as a
backward-compat composite reference that loads the 8 decomposed modules.

Module boundary rule (ADR-001): modules are pure. Domain pack loading happens in
the skill, not in modules. `module_iii_inspect_text` does NOT read packs — it
receives them as typed input from dispatch.

### Inbound coordination — Campaign C / Airlock Standard v0.2

`who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md` (committed
`1bf3074`, separate from MA-2). Inbound from VideoForge.aDNA (proposing persona:
Iris). 5 cross-forge request-pattern gaps in current `AIRLOCK.md` v0.1.0. Not
blocking MA-3; queued for Campaign C (Airlock Standard, currently pending DG-A).
Argus reads + responds when Campaign C opens.
