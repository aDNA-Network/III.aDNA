---
type: state
created: 2026-05-07
updated: 2026-05-08
status: active
last_edited_by: agent_stanley
last_session: session_stanley_20260508_iii_adna_mc1_airlock_standard_spec
tags: [state, governance]
---

# III.aDNA — Operational State

## Current Phase

**Campaign B: Federation — P1 IN-FLIGHT (MB-1 ✅ 2026-05-08) — Campaign C: Airlock Standard v0.2 — P1 IN-FLIGHT (MC-1 ✅ 2026-05-08)**

> **MB-1 closure note** (2026-05-08): Both Campaign B and Campaign C charters authored at `how/campaigns/campaign_b_iii_federation/` and `how/campaigns/campaign_c_airlock_standard/`. MB-1 (lattice-labs `iii/` consumer wrapper) executed in-session: wrapper at `~/lattice/lattice-labs/iii/CLAUDE.md` (federation_ref pinned at III.aDNA `v0.1.0`, commit `1628793`); local store seeded empty at `~/lattice/lattice-labs/iii/what/context/lattice_labs_iii_learning_store.jsonl`; operational `~/lattice/lattice-labs/what/context/iii_domain_packs/iii_corrections.jsonl` truncated to 0 bytes; lattice-labs/CLAUDE.md gained Standing Rule #12 routing III review through the new wrapper; wikilink sweep across active campaigns clean.

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

MA-4 (Airlock + v0.1.0) closed 2026-05-08 (III.aDNA commit `1628793`,
annotated tag `v0.1.0` at object `7f32799`, pushed `LatticeProtocol/III.aDNA`):
`how/airlock/AIRLOCK.md` elevated
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
| **Campaign A: Genesis** | Fork, identity, content migration, module architecture, airlock v0.1.0 | P0-P2 | ✅ COMPLETE 2026-05-08 — DG-A CLOSED 9/9 |
| **Campaign B: Federation** | Wire iii/ consumer wrappers across all vaults | **P1 IN-FLIGHT** | **OPEN 2026-05-08** — MB-1 ✅; MB-2..MB-8 pending |
| **Campaign C: Airlock** | Formalize airlock standard v0.2 + propagate to major vaults | **P1 IN-FLIGHT** | **OPEN 2026-05-08** — MC-1 ✅; MC-2..MC-5 pending; parallel-eligible with B |

## Campaign B Mission Queue (active)

| Mission | Goal | Est. | Status |
|---------|------|------|--------|
| MB-1: lattice-labs wrapper | Retire operational corrections.jsonl per ADR-003; lattice-labs/CLAUDE.md III routing | 1 sess | ✅ **COMPLETE 2026-05-08** |
| MB-2: SiteForge wrapper | Multi-voice orchestration absorbed (MA-3 carry-forward #2) | 1 sess | pending |
| MB-3: VideoForge wrapper | ADR-006 bridge resolved | 1 sess | pending |
| MB-4: CanvasForge wrapper | Relocate `skill_canvas_iii_review.md` (MA-3 carry-forward #4) | 1 sess | pending |
| MB-5: wga wrapper | Standard wrapper; minimal extensions | 0.5 sess | pending |
| MB-6: adna template | `skill_iii_setup.md` + workspace CLAUDE.md note | 0.5 sess | pending |
| MB-7: vault hygiene | 4 carry-forwards + 2 Plan-agent findings + KINN relocation | 0.5 sess | pending |
| MB-8: LPWhitepaper wrapper | Gap surfaced during MB-1 wikilink sweep | 1 sess | pending |

## Campaign C Mission Queue (P1 in-flight)

| Mission | Goal | Est. | Status |
|---------|------|------|--------|
| MC-1: Standard spec | `iii_airlock_standard_spec.md` (entry paths + cross-vault request patterns) | 1 sess | ✅ **COMPLETE 2026-05-08** |
| MC-2: Request schema | YAML payload schema canonicalized | 0.5 sess | pending |
| MC-3: AIRLOCK.md v0.2 | + handshake reply-comment template | 1 sess | pending |
| MC-4: Substrate enforcement | `secrets_handled` preflight + `idempotency_key` contract | 1 sess | pending |
| MC-5: Validation | Re-exercise VideoForge → CanvasForge against v0.2; flip proposal status | 0.5 sess | pending |

## Campaign A Mission Queue (closed, historical reference)

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
- lattice-labs forward stubs in place; active campaigns continue uninterrupted — at MB-1 close 2026-05-08 only `campaign_kinn_branding_iii` (50/100) is actively writing at lattice-labs (`campaign_whitepaper_iii_deep_review` migrated to LPWhitepaper.aDNA 2026-04-17; `campaign_canvas_visual_command` superseded by CanvasForge.aDNA M-Cleanup-06 cutover 2026-05-04)
- corrections.jsonl canonical upstream operational; **lattice-labs `iii/` consumer wrapper live (MB-1 ✅ 2026-05-08)** — federation_ref pinned at v0.1.0, local store seeded empty, operational pre-migration jsonl retired (truncated to 0 bytes); MB-2..MB-8 wrappers pending
- skill_iii_review.md wikilinks all resolve to canonical core_domain_packs/ paths; no remaining `iii_domain_packs/` references
- 8 composable III modules live at `what/modules/` (`module_iii_dispatch`, `_inspect_text`, `_inspect_code`, `_inspect_visual`, `_inspect_data`, `_introspect`, `_improve`, `_accumulate`) — typed I/O matching lattice contract; pure (pack content arrives as typed input)
- Lattice `lattice_iii_verification_oracle.lattice.yaml` v1.2.0: all 8 module nodes carry `ref:` pointing at their `.module.yaml`; `accumulate` node added; ADR-003 write-policy expressed in node config rather than implicit in `human_review`
- Legacy `module_iii_semantic_reviewer.{md,yaml}` rebranded `status: composite_reference` with redirect banner to the 8 decomposed modules — preserves backward-compat for SiteForge consumers pointing at the composite (multi-voice orchestration is consumer-side, lands in `SiteForge.aDNA/iii/` at Campaign B MB-2)
- `how/airlock/AIRLOCK.md` v0.1.0 reference implementation: `status: reference_implementation`, Path Selection matrix routes external agents in one read, 5 entry paths (A text / B web+visual / C code / D video / E vault maintenance) each with Use Cases / Out of Scope / Expected Output / Context Budget, anti-regression section "What v0.1.0 does NOT cover" defers cross-vault request patterns to v0.2 / Campaign C MC-1
- Initial annotated git tag `v0.1.0` cut at MA-4 closure commit — first III.aDNA release; pre-federation baseline for consumer wrappers in Campaign B

## Blockers

None currently. **DG-A CLOSED 9/9.** Campaign B is OPEN P1 IN-FLIGHT (MB-1 ✅; MB-2..MB-8 pending). Campaign C is OPEN P1 IN-FLIGHT (MC-1 ✅; MC-2..MC-5 pending).

## Risk Register

Campaign-A risks (R1-R5) have been absorbed into the per-campaign registers in `campaign_b_iii_federation.md` (5 risks) and `campaign_c_airlock_standard.md` (4 risks). R1 (active lattice-labs campaigns break during wrapper migration) carried forward into Campaign B as the wikilink-sweep mitigation pattern (proven at MB-1 close).

## Learning Store Status

- Canonical entries: 26 (live at `what/context/core_domain_packs/iii_corrections_canonical.jsonl`, md5 `dde2cbd88c0b45956fb22285a2a0f856`)
- Graduated to domain packs: 5 (C-001 projective_claim_as_fact, C-002 aspiration_as_current_capability, C-005 + 2 more)
- Graduation candidates: C-003 (freq=?, acceptance=?), C-009 (freq=?, acceptance=?)
- lattice-labs operational pre-migration copy (`iii_corrections.jsonl` at `what/context/iii_domain_packs/`) **retired at MB-1 2026-05-08** (truncated to 0 bytes); lattice-labs local downstream fork now lives at `~/lattice/lattice-labs/iii/what/context/lattice_labs_iii_learning_store.jsonl` (empty; populated by future review cycles per ADR-003 §2)
- Two vestigial campaign-scoped JSONLs remain at lattice-labs (whitepaper breadcrumb 13 144 B + canvas_visual breadcrumb 659 B) — disposition deferred to Campaign B charter MB-7
- Schema reconciliation against ADR-003 §4 deferred to MB-7 (Plan-agent finding (a))

## Latest Direction — 2026-05-08 (MC-1 ✅; Campaign C P1 in-flight)

**MC-1 executed** (Campaign C opener). `what/artifacts/iii_airlock_standard_spec.md` v0.2.0 authored (`status: reference_implementation`). 8-section body codifies both surfaces:

- §3 **Entry path standard** — generalizes v0.1.0; required-fields schema (profile / use-cases / out-of-scope / expected-output / context-budget / load-in-order); Path Selection matrix as a §3.2 mandate; AIRLOCK.md cited as the v0.2 reference instance (entry paths NOT reproduced verbatim — drift surface avoided).
- §4 **Cross-vault request standard** — absorbs all 5 VideoForge gaps:
  - §4.1 Initiation ceremony (Gap 1) — filename pattern `coord_<date>_<requesting>_requests_<artifact>.md`; lifecycle states (`open|accepted|rendering|shipped|closed|rejected|cancelled|queued`); 3-step acceptance + 1-step rejection ceremonies. No new `inbox/` directory.
  - §4.2 Handshake (Gap 2) — OPTIONAL; lightweight profile (`open→shipped→closed`) for trivial 2-forge requests, full profile required only when both vaults are in active rendering/serving mode.
  - §4.3 Payload schema (Gap 3) — frontmatter (illustrative; canonical YAML at MC-2) + 10-section body shape; required minimum is `type` + `spec_path` + `output_sink`.
  - §4.4 Secret delegation (Gap 4) — `secrets_handled.{needed,passthrough,not_passed}`; receiver MUST verify presence before flipping `accepted`; names-only audit.
  - §4.5 Idempotency (Gap 5) — caller-defined `idempotency_key`; receiver MUST surface in-flight or last-30-day match; `force_new: true` overrides.
- §5 Worked-example reference — coverage table maps CanvasForge memo against §4.3 (15 of 17 spec slots populated; 2 additive deltas — `secrets_handled` block + `idempotency_key` field — both non-blocking).
- §6 Versioning policy — federates with ADR-002 §3 (patch transparent / minor reviewed / major reserved for v1.0); §6.2 explicitly avoids a separate `airlock_version` consumer field (drift surface).
- §7 Coverage and boundaries — explicit deferred surfaces (proposed/RLHF channel; multi-vault transactional; async batched). §7.3 names three forward-references (MC-2 schema; MC-4 implementation guidance × 2) and declares contracts in §4.3–§4.5 normative regardless of those files' presence.

**VF proposal status flipped** `open → accepted` (`who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md`). Status-log entries reserved for `absorbed` (MC-3 ship) and `closed` (MC-5 validation).

**Campaign C charter updated**: MC-1 row ✅; DG-C MC-1 box checked; P1 phase IN-FLIGHT; campaign frontmatter `status: in_flight`. Promises section updated — Promise #1 (cross-vault patterns in v0.2) marked DELIVERED 2026-05-08; Promise #2 (coord-memo fallback) holds until MC-3; Promise #3 (proposal acknowledgment) DELIVERED.

**No git tag bump**. MC-1 ships the spec at version `0.2.0` in frontmatter; the annotated `v0.2.0` git tag waits for MC-3 closure (per charter Critical Path) — that's where AIRLOCK.md text actually shifts to v0.2.

### Fresh-session boot (post-MC-1)

For the next agent that opens this vault:

1. **Read this STATE.md from the top.** Current Phase + MC-1 closure note summarize where Campaigns B and C stand.
2. **Confirm `how/sessions/active/` is empty**. MC-1 session moved to `how/sessions/history/2026-05/` at closeout.
3. **Choose next mission.** Two parallel-eligible queues:
   - **Campaign B** (federation): **MB-2 SiteForge** (most architecturally meaningful — multi-voice orchestration absorption) → MB-4 CanvasForge → MB-3 VideoForge → MB-5 wga → MB-8 LPWhitepaper → MB-6 adna template (after ≥ 2 wrappers in flight) → MB-7 vault hygiene → DG-B.
   - **Campaign C** (airlock standard): **MC-2 schema** (next; canonical YAML at `what/artifacts/iii_airlock_request_schema.yaml`; 0.5 sess) → MC-3 AIRLOCK.md v0.2.0 + reply-comment template (1 sess; cuts `v0.2.0` git tag) → MC-4 substrate enforcement (1 sess) → MC-5 validation (0.5 sess) → DG-C.
4. **Recommended next**: **MC-2** (small, sequential dependency for MC-3, low cognitive load) OR **MB-2** (largest architectural value in flight). Either advances a P1 phase. Stanley signals.
5. **Open a session file** using the MC-1 / MB-1 pattern at `how/sessions/active/session_stanley_<YYYYMMDD>_iii_adna_<mission>_<descriptor>.md`.

## Latest Direction — 2026-05-08 (Campaigns B + C opened; MB-1 ✅)

Three things landed in this session:

**Campaign B charter authored** at `how/campaigns/campaign_b_iii_federation/campaign_b_iii_federation.md`. 8-mission queue (MB-1 lattice-labs ✅; MB-2 SiteForge; MB-3 VideoForge; MB-4 CanvasForge; MB-5 wga; MB-6 adna template + workspace CLAUDE.md note; MB-7 vault hygiene incl. canonical jsonl schema reconciliation against ADR-003 §4 + vestigial campaign-scoped JSONLs at frozen breadcrumb directories; **MB-8 LPWhitepaper.aDNA wrapper** — surfaced during MB-1 wikilink sweep). 5-risk register. P1 → P2 (parallel-eligible wrappers) → P3 (template + closeout) phase plan.

**Campaign C charter authored** at `how/campaigns/campaign_c_airlock_standard/campaign_c_airlock_standard.md`. 5-mission queue seeded directly from the on-file VideoForge proposal at `who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md` (5 specific gaps in v0.1.0). MC-1..MC-5 → DG-C gate + annotated `v0.2.0` tag. Parallel-eligible with Campaign B; lower priority since v0.1.0 is shipped.

**MB-1 executed**:
1. `~/lattice/lattice-labs/iii/CLAUDE.md` authored — federation_ref pinned at III.aDNA `v0.1.0` (commit `1628793`); declares all 7 core packs + 8 modules + the v1.2.0 oracle lattice; `local_extensions` for the in-place KINN pack and the new local store
2. `~/lattice/lattice-labs/iii/what/context/lattice_labs_iii_learning_store.jsonl` seeded empty (per ADR-003 §2; ACCUMULATE writes target this file)
3. Pre-migration operational `~/lattice/lattice-labs/what/context/iii_domain_packs/iii_corrections.jsonl` truncated to 0 bytes; canonical `dde2cbd88c0b45956fb22285a2a0f856` md5 re-verified unchanged at the III.aDNA upstream
4. `~/lattice/lattice-labs/CLAUDE.md` gained Standing Rule #12 (III review via wrapper; mirrors CanvasForge precedent at rule 11)
5. `~/lattice/lattice-labs/what/context/iii_domain_packs/MIGRATION_NOTE.md` Stage 3 closure entry added

**Wikilink sweep result** (MB-1 verification gate): zero broken III wikilinks across all three previously-named consumer campaigns. Important sweep finding — only `campaign_kinn_branding_iii` (50/100 cycles) is actively writing at lattice-labs. `campaign_whitepaper_iii_deep_review` migrated to LPWhitepaper.aDNA on 2026-04-17 (commit `234ed8b`); `campaign_canvas_visual_command` was superseded by CanvasForge.aDNA at M-Cleanup-06 cutover 2026-05-04. Two vestigial campaign-scoped JSONLs remain (whitepaper breadcrumb 13 144 B + canvas_visual breadcrumb 659 B); MB-7 disposes.

**No git tag bump**. MB-1 touched no III.aDNA core; v0.1.0 stays. Plan-agent's recommended schema-backfill patch bump → MB-7 (vault hygiene closeout).

### Carry-forwards

The four MA-3 follow-ups + the two Plan-agent findings (canonical jsonl schema drift; vestigial campaign-scoped JSONLs) have moved out of STATE.md and into Campaign B charter MB-7. Single owner. Don't carry them in this file going forward.

### Fresh-session boot (post-MB-1) — superseded by post-MC-1 boot block above

(Original post-MB-1 boot guidance retained as historical reference; the post-MC-1 boot block is the current authority.)
