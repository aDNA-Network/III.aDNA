---
type: campaign
campaign_id: campaign_b_iii_federation
title: "Campaign B: III.aDNA Ecosystem Federation"
status: open
created: 2026-05-08
updated: 2026-05-08
last_edited_by: agent_stanley
phase: P1
mb1_open: 2026-05-08
predecessor: campaign_a_iii_genesis
inbound_findings: []
tags: [campaign, federation, consumer_wrappers, iii]
---

# Campaign B: III.aDNA Ecosystem Federation

## Mission

Wire `iii/` consumer wrappers into every Lattice ecosystem vault that consumes III.aDNA capabilities (lattice-labs, SiteForge, VideoForge, CanvasForge, wga). Each wrapper conforms to ADR-002 (consumer federation contract) and ADR-003 (learning store ownership). Retire the pre-migration operational `iii_corrections.jsonl` at lattice-labs as the critical-path first move; close the four MA-3 carry-forward follow-ups + the two Plan-agent findings (canonical jsonl schema reconciliation, kinn campaign-scoped jsonl disposition) along the way; ship `skill_iii_setup.md` into the adna base template so future vaults can self-onboard.

## Predecessor

Campaign A: Genesis & Foundation — DG-A CLOSED 2026-05-08 9/9. v0.1.0 tag at commit `1628793`. Pre-federation baseline locked.

## DG-B Criteria

- [ ] **MB-1**: lattice-labs `iii/CLAUDE.md` live; operational `iii_corrections.jsonl` retired; lattice-labs/CLAUDE.md III routing in place; wikilink sweep across whitepaper_iii_deep_review (10 missions) + kinn_branding_iii + canvas_visual_command shows zero unresolved III references
- [ ] **MB-2**: SiteForge `iii/CLAUDE.md` live; multi-voice orchestration (MA-3 carry-forward #2) absorbed as consumer-side composition pattern in SiteForge wrapper
- [ ] **MB-3**: VideoForge `iii/CLAUDE.md` live; ADR-006 bridge contract resolved (R3 risk closed)
- [ ] **MB-4**: CanvasForge `iii/CLAUDE.md` live; `skill_canvas_iii_review.md` relocated from lattice-labs to CanvasForge wrapper (MA-3 carry-forward #4)
- [ ] **MB-5**: wga `iii/CLAUDE.md` live
- [ ] **MB-6**: `skill_iii_setup.md` published to adna base template (PR if needed); workspace `~/lattice/CLAUDE.md` "Forge Ecosystem" section updated to note `iii/` wrapper convention
- [ ] **MB-7**: vault-hygiene closeout — missing `core_domain_packs/AGENTS.md` wikilink (MA-3 carry-forward #3) resolved; `reviewed_output` lattice node disposition decided (MA-3 carry-forward #1); canonical jsonl schema reconciled against ADR-003 §4 (Plan-agent finding (a)); vestigial campaign-scoped JSONLs disposed (Plan-agent finding (b) corrected — actually 2 files: whitepaper breadcrumb 13 144 B + canvas_visual breadcrumb 659 B); KINN pack physical relocation considered (carry-forward #3 from MB-1)
- [ ] **MB-8**: LPWhitepaper.aDNA `iii/` consumer wrapper live (gap surfaced during MB-1 wikilink sweep)
- [ ] All actively-running consumers survive Campaign B without disruption — at MB-1 close 2026-05-08, only `campaign_kinn_branding_iii` (50/100) is actively writing at lattice-labs; `campaign_whitepaper_iii_deep_review` migrated to LPWhitepaper.aDNA 2026-04-17; `campaign_canvas_visual_command` superseded by CanvasForge.aDNA M-Cleanup-06 cutover 2026-05-04

## Mission Table

| Mission | Objective | Sessions | Status |
|---------|-----------|----------|--------|
| **MB-1** | lattice-labs `iii/` wrapper; retire operational corrections.jsonl per ADR-003; lattice-labs/CLAUDE.md III routing | 1 | 🟡 OPEN 2026-05-08 |
| **MB-2** | SiteForge `iii/` wrapper; multi-voice orchestration absorbed (MA-3 carry-forward #2) | 1 | pending |
| **MB-3** | VideoForge `iii/` wrapper; ADR-006 bridge resolved | 1 | pending |
| **MB-4** | CanvasForge `iii/` wrapper; relocate `skill_canvas_iii_review.md` (MA-3 carry-forward #4) | 1 | pending |
| **MB-5** | wga `iii/` wrapper | 0.5 | pending |
| **MB-6** | `skill_iii_setup.md` in adna template + workspace CLAUDE.md note | 0.5 | pending |
| **MB-7** | Vault-hygiene closeout (4 carry-forwards + 2 Plan-agent findings); KINN relocation decision; vestigial campaign-scoped JSONLs at frozen breadcrumb directories (whitepaper 13 144 B + canvas_visual 659 B) | 0.5 | pending |
| **MB-8** | LPWhitepaper.aDNA `iii/` consumer wrapper — gap surfaced during MB-1 wikilink sweep (whitepaper campaign migrated from lattice-labs 2026-04-17; needs first-class wrapper at LPWhitepaper.aDNA) | 1 | pending |

## Critical Path

**MB-1 (CRITICAL)** — sole path that retires the operational `lattice-labs/what/context/iii_domain_packs/iii_corrections.jsonl`. Until MB-1 closes, lattice-labs ACCUMULATE writes still target the pre-migration upstream rather than the wrapper-routed local store. All other consumer wrappers (MB-2..MB-5) can sequence independently in parallel after MB-1.

→ MB-2..MB-5 (parallel-eligible) → MB-6 (depends on at least 2 wrappers shipped to validate the pattern in `skill_iii_setup.md`) → MB-7 (final hygiene closeout) → **DG-B gate**.

## Carry-Forward Inputs (from Campaign A)

Four MA-3 follow-ups inherit into Campaign B planning (per Campaign A AAR + STATE.md "Carry-forward follow-ups" block; relocation under MB-7 single ownership):

1. **`reviewed_output` lattice node disposition** — declared `type: module` since pre-MA-3 but has no `ref:` (no module file at `what/modules/`). Either author 9th module `module_iii_reviewed_output.{md,yaml}` (apply approved edits to artifact) or retype as `process` (matching `human_review`). Stanley call. → MB-7.
2. **Multi-voice orchestration** in rebranded `module_iii_semantic_reviewer.md` composite_reference body. Consumer-side composition pattern; lands in SiteForge wrapper. → MB-2.
3. **`core_domain_packs/AGENTS.md`** wikilinked from `context_iii_learning_store.md` does not exist. Author or remove. → MB-7.
4. **`skill_canvas_iii_review.md` placement** — currently at lattice-labs, listed in workspace CLAUDE.md project map as III.aDNA-canonical, but is CanvasForge-coupled. → MB-4 (relocate to CanvasForge `iii/` wrapper).

## Plan-Agent Findings (from MB-1 plan validation)

Two findings surfaced during MB-1 plan validation, scoped *out* of MB-1 to keep the wrapper move tight; closed at MB-7:

(a) **Canonical jsonl schema drift vs ADR-003 §4**: live `iii_corrections_canonical.jsonl` uses `trap`/`graduated_to`/`accepted` field names; ADR-003 §4 specifies `trap_pack`/`acceptance_rate`/`graduated_from`. Zero `graduated_from` fields populated across 26 founding entries. Resolution at MB-7 = backfill (preferred; canonical read-only + consumers pin v0.1.0; patch-bump → v0.1.1) OR amend ADR-003 §4 to match live schema.

(b) **kinn campaign-scoped third corrections store**: `lattice-labs/how/campaigns/campaign_kinn_branding_iii/iii_corrections_campaign.jsonl` (13 144 B). ADR-003 §2 implies one consumer = one local store. Resolution at MB-7 = merge into `lattice_labs_iii_learning_store.jsonl`, leave in place, or treat as campaign-internal. Lower-risk = leave in place during the campaign's active 50/100 cycle window.

## Risk Register

| # | Risk | Sev | Mitigation |
|---|------|-----|------------|
| R1 | Active lattice-labs campaigns break during wrapper migration | HIGH | Wikilink sweep on each wrapper close; pause closure if any unresolved; MA-1/MA-2 forward-stub pattern preserved |
| R2 | Consumer version drift over time | MED | ADR-002 §3 minor-bump review policy; consumer agent reads CHANGELOG diff before updating pinned version |
| R3 | VideoForge ADR-006 bridge complexity | MED | Per ADR-002 §6 III.aDNA does not absorb consumer-specific bridges; ADR-006 stays in VideoForge; MB-3 wrapper points at it via `local_extensions` |
| R4 | Canonical jsonl schema reconciliation surfaces unresolved questions | LOW | Deferred to MB-7; v0.1.0 pin remains stable; consumer wrappers depend on path semantics, not field-name semantics |
| R5 | aDNA template is public repo — additive changes only for MB-6 | MED | MB-6 last in campaign; PR if upstream-incompatible |

## Phase Plan

| Phase | Name | Missions | Status |
|-------|------|----------|--------|
| **P1** | Critical-path migration | MB-1 | 🟡 IN-FLIGHT 2026-05-08 |
| **P2** | Forge-consumer wrappers (parallel-eligible) | MB-2, MB-3, MB-4, MB-5 | pending |
| **P3** | Template + closeout | MB-6, MB-7, DG-B gate | pending |

## AAR
<!-- Template: how/templates/template_aar_lightweight.md — populate at DG-B closure -->

(pending)

## Cross-References

- ADR-002 (consumer federation contract): `~/lattice/III.aDNA/what/decisions/adr_002_consumer_federation_contract.md`
- ADR-003 (learning store ownership): `~/lattice/III.aDNA/what/decisions/adr_003_learning_store_ownership.md`
- Predecessor campaign (Campaign A genesis): `~/lattice/III.aDNA/how/campaigns/campaign_a_iii_genesis/campaign_a_iii_genesis.md`
- Sibling campaign (Campaign C airlock standard, parallel-eligible): `~/lattice/III.aDNA/how/campaigns/campaign_c_airlock_standard/campaign_c_airlock_standard.md`
- Forge canonical spec (MB-2..MB-5 reference pattern): `~/lattice/SiteForge.aDNA/what/artifacts/sf_forge_pattern_spec.md`
