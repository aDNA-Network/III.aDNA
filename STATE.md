---
type: state
created: 2026-05-07
updated: 2026-05-08
status: active
last_edited_by: agent_stanley
last_session: session_stanley_20260508_iii_adna_ma4_airlock_v0_1_0
tags: [state, governance]
---

# III.aDNA — Operational State

## Current Phase

**Campaign A: Genesis + Foundation — DG-A CLOSED 9/9 — Campaign A WIND-DOWN; Campaigns B + C ready to open**

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

MA-3 (module decomposition) closed 2026-05-08: 8 composable aDNA modules authored
at `what/modules/` (16 new files — `.md` + `.module.yaml` per module): `dispatch`,
`inspect_text`, `inspect_code`, `inspect_visual`, `inspect_data`, `introspect`,
`improve`, `accumulate`. Each declares typed I/O matching lattice contract names
(`Finding[]`, `Introspection[]`, `ImprovementTable + VerificationResult`,
`LearningStoreDelta + GraduationSignals`); each pure (pack and corrections inputs
declared as typed `context_pack` inputs — modules never read pack files directly).
Legacy `module_iii_semantic_reviewer.{md,yaml}` rebranded `status: composite_reference`
with banner pointing to the 8 decomposed modules + lattice. Lattice yaml bumped
1.1.0 → 1.2.0: `accumulate` node added (post-approval learning-store writer);
edge `human_review → corrections_store` rewired through `accumulate`; all 8 module
nodes now carry `ref:` pointing at their `.module.yaml` file. Skill annotated with
"Module composition" callout + frontmatter `ma3_module_composition: 2026-05-08`.

MA-4 (Airlock + v0.1.0) closed 2026-05-08: `how/airlock/AIRLOCK.md` elevated
from v0.1 stub to v0.1.0 reference implementation. Frontmatter status flipped
`stub` → `reference_implementation`; Path Selection matrix added upfront so an
external agent self-routes in one read; each of the 5 entry paths (A text,
B web/visual, C code, D video, E vault maintenance) gained Use Cases / Out of
Scope / Expected Output / Context Budget — recipe "Load in order" lists kept
verbatim. Anti-regression section "What v0.1.0 does NOT cover" makes the
v0.1.0/v0.2 boundary explicit: entry paths (covered) vs. cross-vault request
patterns (deferred to Campaign C MC-1 per the on-file VideoForge proposal at
`who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md`).
Until v0.2 ships, cross-vault requests use the coord-memo fallback (worked
example: CanvasForge VideoForge→Carly+Herb deck memo). Initial annotated
git tag `v0.1.0` cut at MA-4 closure commit — first III.aDNA release.

## Campaigns

| Campaign | Goal | Phase | Status |
|----------|------|-------|--------|
| **Campaign A: Genesis** | Fork, identity, content migration, module architecture, airlock v0.1.0 | P0-P2 | **✅ COMPLETE 2026-05-08 — DG-A CLOSED 9/9** |
| **Campaign B: Federation** | Wire iii/ consumer wrappers across all vaults | P1-P3 | **READY TO OPEN** (DG-A unblocked) |
| **Campaign C: Airlock** | Formalize airlock standard v0.2 + propagate to major vaults | P1-P2 | **READY TO OPEN** (DG-A unblocked; can overlap B) |

## Campaign A Mission Queue

| Mission | Goal | Est. | Status |
|---------|------|------|--------|
| MA-0: Fork + Identity | Vault created, CLAUDE.md/MANIFEST/STATE/ADRs authored | 1 sess | ✅ **COMPLETE 2026-05-07** |
| MA-1: Core migration | skill_iii_review, module, lattice + corrections.jsonl head-start migrated; stubs added | 1 sess | ✅ **COMPLETE 2026-05-08** |
| MA-2: Domain pack migration | 7 core domain pack .md files migrated to core_domain_packs/ | 1 sess | ✅ **COMPLETE 2026-05-08** |
| MA-3: Module decomposition | 8 module .md + .module.yaml files authored | 1 sess | ✅ **COMPLETE 2026-05-08** |
| MA-4: Airlock + v0.1.0 | AIRLOCK.md reference implementation; initial git tag | 0.5 sess | ✅ **COMPLETE 2026-05-08** |

## DG-A Gate Criteria

- [x] CLAUDE.md authored with Argus identity + full architecture (MA-0 ✅)
- [x] ADRs 000-003 ratified (Stanley approval) (MA-0 ✅)
- [x] Core skill + module + lattice migrated from lattice-labs (MA-1 ✅)
- [x] lattice-labs forward stubs in place (active campaigns unblocked) (MA-1 ✅)
- [x] corrections.jsonl canonical upstream operational (MA-1 ✅, head-start scope)
- [x] All 7 core domain packs at canonical paths (MA-2 ✅, KINN carved out as consumer-specific)
- [x] 8 module files present in what/modules/ (MA-3 ✅; legacy `module_iii_semantic_reviewer` rebranded composite_reference)
- [x] how/airlock/AIRLOCK.md implemented (5 entry paths) (MA-4 ✅; reference implementation v0.1.0 with Path Selection matrix + per-path enrichment + anti-regression section)
- [x] Initial git tag v0.1.0 (MA-4 ✅; annotated tag at MA-4 closure commit)

**DG-A scorecard: 9/9 green — DG-A CLOSED 2026-05-08.**

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
- 8 composable III modules live at `what/modules/` (`module_iii_dispatch`, `_inspect_text`, `_inspect_code`, `_inspect_visual`, `_inspect_data`, `_introspect`, `_improve`, `_accumulate`) — typed I/O matching lattice contract; pure (pack content arrives as typed input)
- Lattice `lattice_iii_verification_oracle.lattice.yaml` v1.2.0: all 8 module nodes carry `ref:` pointing at their `.module.yaml`; `accumulate` node added; ADR-003 write-policy expressed in node config rather than implicit in `human_review`
- Legacy `module_iii_semantic_reviewer.{md,yaml}` rebranded `status: composite_reference` with redirect banner to the 8 decomposed modules — preserves backward-compat for SiteForge consumers pointing at the composite (multi-voice orchestration is consumer-side, lands in `SiteForge.aDNA/iii/` at Campaign B MB-2)
- `how/airlock/AIRLOCK.md` v0.1.0 reference implementation: `status: reference_implementation`, Path Selection matrix routes external agents in one read, 5 entry paths (A text / B web+visual / C code / D video / E vault maintenance) each with Use Cases / Out of Scope / Expected Output / Context Budget, anti-regression section "What v0.1.0 does NOT cover" defers cross-vault request patterns to v0.2 / Campaign C MC-1
- Initial annotated git tag `v0.1.0` cut at MA-4 closure commit — first III.aDNA release; pre-federation baseline for consumer wrappers in Campaign B

## Blockers

None currently. **DG-A is CLOSED.** Campaigns B (Federation) and C (Airlock Standard v0.2) are unblocked and ready to open.

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

## Latest Direction — 2026-05-08 (post-MA-4 — DG-A CLOSED)

MA-4 closed. `how/airlock/AIRLOCK.md` elevated stub → v0.1.0 reference
implementation. Frontmatter status flipped `stub` → `reference_implementation`.
**Path Selection matrix** added upfront: a 5-row decision table (artifact ×
profile → path) lets an external agent self-route in one read instead of
skimming 5 path sections. Each of the 5 entry paths (A text, B web/visual, C
code, D video, E vault maintenance) gained four uniform additions: **Use Cases**
(2–4 concrete examples), **Out of Scope** (pointers to the right path),
**Expected Output** (the typed-IO triple `Finding[]` → `Introspection[]` →
`ImprovementTable + VerificationResult`, plus optional `LearningStoreDelta`),
**Context Budget** (rough file count). The "Load in order" recipes from the
v0.1 stub were preserved verbatim — the recipes were already correct.

The new **"What v0.1.0 does NOT cover"** section makes the v0.1.0/v0.2 boundary
explicit. v0.1.0 covers entry (inbound, pull-based, agent enters this vault to
run the loop). v0.2 will add request patterns (bidirectional, ephemeral, two
agents handshake across vault boundaries — schema, status, idempotency). Until
v0.2 ships, cross-vault requests use the **coord-memo fallback**, with the
canonical worked example pointed at `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md`.
The inbound VideoForge proposal (`who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md`)
is cited in the section as the v0.2 spec input.

**Initial annotated git tag `v0.1.0` cut at MA-4 closure commit** — first
III.aDNA release. Pre-federation baseline for Campaign B consumer wrappers.

**Verification**: AIRLOCK.md frontmatter shows `status: reference_implementation`
+ `version: "0.1.0"`; all 5 `### Path` headers present; anti-regression section
present with both v0.2 and Campaign C MC-1 references; STATE.md DG-A scorecard
shows 9/9; campaign tracker MA-4 row ✅; `git tag -l` shows `v0.1.0`.

**DG-A CLOSED 9/9.** Campaigns B (Federation, consumer wrappers) and C
(Airlock Standard v0.2) are unblocked and ready to open. Next session is at
Stanley's discretion: open Campaign B (priority recommendation: MB-1
lattice-labs `iii/` wrapper retires the operational corrections.jsonl per
ADR-003 — the only critical-path consumer migration), open Campaign C, or
queue both with C following the VideoForge findings.

### Carry-forward follow-ups (Campaign B planning input)

The four follow-ups from MA-3 close did not require action in MA-4. They
remain on the books for Campaign B planning:

1. **`reviewed_output` lattice node**: declared `type: module` since pre-MA-3
   but has no `ref:` (no module file at `what/modules/`). Either author a 9th
   module `module_iii_reviewed_output.{md,yaml}` (apply approved edits to the
   artifact) or retype `reviewed_output` as `process` (matching `human_review`).
   Stanley call.
2. **Multi-voice orchestration** in the rebranded
   `module_iii_semantic_reviewer.md` composite_reference body: SiteForge
   consumer-side composition pattern. Lands in `SiteForge.aDNA/iii/` via
   consumer wrapper at Campaign B MB-2. No action in III.aDNA core.
3. **`core_domain_packs/AGENTS.md`** referenced by `context_iii_learning_store.md`
   wikilink does not exist. Author or remove the wikilink during Campaign B
   vault-hygiene pass.
4. **`skill_canvas_iii_review.md` placement**: currently at lattice-labs, listed
   in CLAUDE.md project map as III.aDNA-canonical, but is CanvasForge-coupled.
   Likely belongs in `CanvasForge.aDNA/iii/` consumer wrapper. Decision deferred
   to Campaign B planning.

### Inbound coordination — Campaign C / Airlock Standard v0.2

`who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md`
(committed `1bf3074`). Inbound from VideoForge.aDNA (Iris). 5 cross-forge
request-pattern gaps surfaced by exercising AIRLOCK.md v0.1.0 against a
real cross-forge commission. **MA-4's AIRLOCK.md cited and acknowledged the
proposal in its anti-regression section** — the v0.1.0 release is consistent
with the proposed v0.2 direction. Argus reads + responds when Campaign C
opens (now unblocked).
