---
type: campaign
campaign_id: campaign_b_iii_federation
title: "Campaign B: III.aDNA Ecosystem Federation"
status: open
created: 2026-05-08
updated: 2026-05-12
last_edited_by: agent_stanley
phase: P3
mb1_open: 2026-05-08
mb1_closed: 2026-05-08
mb2_closed: 2026-05-10
mb3_closed: 2026-05-11
mb4_closed: 2026-05-11
mb5_closed: 2026-05-11
mb6_closed: 2026-05-12
mb7_closed: 2026-05-12
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
- [x] **MB-2**: SiteForge `iii/CLAUDE.md` live; multi-voice orchestration (MA-3 carry-forward #2) absorbed as consumer-side composition pattern in SiteForge wrapper — ✅ COMPLETE 2026-05-10 (`SiteForge.aDNA/iii/CLAUDE.md` authored; federation_ref pinned at `v0.2.0` commit `04ae724`; `siteforge_reviewers.yaml` 5-voice registry declared as `local_extensions.reviewer_registry` per `module_iii_semantic_reviewer.md` lines 56-69 contract; pre-migration `iii_corrections.jsonl` truncated + local `context_iii_domain_packs_web_design.md` retired as `[MIGRATED]` stub; two SiteForge lattice yamls rerouted; SiteForge Standing Order 7 added)
- [x] **MB-3**: VideoForge `iii/CLAUDE.md` live; ADR-006 bridge contract resolved (R3 risk closed) — ✅ COMPLETE 2026-05-11 (VideoForge-authored wrapper at `VideoForge.aDNA/iii/` ratified by Argus via `coord_2026_05_11_videoforge_iii_wrapper_authoring`; audit_id `session_stanley_20260511_iii_adna_mb3_videoforge_wrapper`; 12/12 audit checks pass; pin commit `246124d` exact against `v0.2.0` tag; bridge_pack via `local_extensions` materializes the R3 mitigation language; III.aDNA core stays modality-agnostic per ADR-002 §6; first inbound v0.2 cross-vault request to traverse full lifecycle `open → accepted → rendering → shipped → closed`)
- [x] **MB-4**: CanvasForge `iii/CLAUDE.md` live; `skill_canvas_iii_review.md` relocated from lattice-labs to CanvasForge wrapper (MA-3 carry-forward #4) — ✅ COMPLETE 2026-05-11 (`CanvasForge.aDNA/iii/CLAUDE.md` authored; federation_ref pinned at `v0.2.0` commit `246124d` — exact tag commit; 5/7 canonical packs — `canvas_visual` deliberately omitted in favor of the bridge_pack local_extension; all 8 modules; lattice v1.2.0; 3 `local_extensions` — `bridge_pack` points at in-place `~/lattice/CanvasForge.aDNA/what/context/iii/context_iii_canvas_visual.md` 10-trap pack (8 canonical + 2 M00 net-new; `not_graduating_to_canonical: true` per ADR-002 §6 modality-agnostic-core boundary; 2 M00 deltas may graduate independently via ADR-003 §3 in a future ceremony); `local_skill` points at in-place `~/lattice/CanvasForge.aDNA/how/skills/skill_canvas_iii_review.md` 5-voice review skill (already at canonical CanvasForge location since M-Cleanup-04 wave 3 2026-05-04 — the carry-forward closes via governance registration, the physical move had already happened); `learning_store_local` points at new `~/lattice/CanvasForge.aDNA/iii/what/context/canvasforge_iii_learning_store.jsonl` seeded empty; CanvasForge.aDNA Standing Order 12 added routing III review through wrapper; CanvasForge project map updated to list `iii/`; downstream-safety verified — SS/CC `presentationforge/` + `graphicnovelforge/` wrappers don't reference `iii/`, `what/context/iii/`, `how/skills/`, or CanvasForge.aDNA Standing Orders; MA-3 carry-forward #4 closed)
- [x] **MB-5**: wga `iii/CLAUDE.md` live — ✅ COMPLETE 2026-05-11 (`wga.aDNA/iii/CLAUDE.md` authored; federation_ref pinned at `v0.2.0` commit `246124d` — exact tag commit; 5/7 canonical packs (inspect_procedures + introspect_checks + learning_store + web_design + vault_maintenance — omits `whitepaper_communication` / `canvas_visual` / `kinn_branding` as out-of-scope, documented inline); all 8 modules; lattice v1.2.0; 1 `local_extension` (`learning_store_local` → `iii/what/context/wga_iii_learning_store.jsonl` seeded empty); minimal-wrapper baseline mirrors MB-3 VideoForge pack selection but drops bridge_pack — no ADR to bridge, no local skill, no canvas substrate; clean-slate consumer with zero pre-existing III infrastructure; pre-federation `campaign_wga_site_iii` (lattice-labs, complete 2026-04-14) already graduated C-012..C-016 to canonical at MA-1 (no migration needed); wga Standing Order 7 added routing III review through wrapper (mirrors lattice-labs Rule 12 / SiteForge SO 7 / CanvasForge SO 12); wga Vault Layout + Domain Subdirectories table updated to list `iii/`; downstream-safety verified — zero vaults federate against wga.aDNA as `source_vault` (additive change only at wga root))
- [x] **MB-6**: `skill_iii_setup.md` published to adna base template — ✅ COMPLETE 2026-05-12 (canonical skill authored at `~/lattice/III.aDNA/how/skills/skill_iii_setup.md` — 395 lines; covers federation_ref skeleton, packs-selection decision table, `kind:` enum walkthrough across all 5 values per ADR-002 §1a, learning-store seeding, Standing Order template, downstream-safety check, wikilink sweep for pre-federation migrations, 4 wrapper variants (minimal / full-extension / bridge / multi-voice) backed by all 5 live wrappers as worked precedents; byte-identical copy published to `~/lattice/.adna/how/skills/skill_iii_setup.md` per Campaign B R5 additive-only contract — adna remote is `LatticeProtocol/adna` public repo; commit local-only this session, Stanley pushes when ready; workspace `~/lattice/CLAUDE.md` Framework Ecosystem section updated — "Consumer integration pattern" paragraph gained a pointer at the new skill (also notes publication to `.adna/`); III.aDNA maturity column refreshed from "Genesis (MA-0 complete 2026-05-07; content migration + federation wiring in progress)" to **Production** with the full v0.1.0 + v0.2.0 + Campaign B P2 ✅ + MB-6/MB-7 ✅ closure timeline; consumers column corrected to "all live via `iii/`" + LPWhitepaper MB-8 pending. No git tag bump (additive skill addition; no canonical surface touched).
- [x] **MB-7**: vault-hygiene closeout — ✅ COMPLETE 2026-05-12 (six items closed: (1) `reviewed_output` lattice node retyped `module` → `process` matching `human_review` precedent — MA-3 carry-forward #1 resolved per Stanley call this session; lattice yaml `1.2.0` → `1.2.1`; (2) `core_domain_packs/AGENTS.md` wikilink removed at `context_iii_learning_store.md:163` — MA-3 carry-forward #3 resolved; line rephrased as natural-language reference to the directory; (3) canonical jsonl schema drift resolved via **ADR-003 §4 amendment** per Stanley call this session — fields aligned with live schema (`trap`/`graduated_to`/`accepted` boolean); canonical jsonl byte-identical; md5 `dde2cbd88c0b45956fb22285a2a0f856` invariant preserved; Amendment History footer added to ADR-003; (4) **ADR-002 §1 amended** — formal `kind:` enum codified (§1a new table: 5 values — `domain_pack`, `reviewer_registry`, `bridge_pack`, `local_skill`, `learning_store_local`) + extension policy (new kinds require ADR amendment + co-registration at next minor bump); Amendment History footer added to ADR-002; (5) two vestigial campaign-scoped JSONLs left in place as historical breadcrumbs with `MIGRATION_NOTE.md` sidecars (whitepaper 13,144 B / 21 entries; canvas_visual 659 B deprecated-stub); (6) KINN pack physically relocated from `lattice-labs/what/context/iii_domain_packs/` to `lattice-labs/iii/what/context/` — campaign `campaign_kinn_branding_iii` `status: completed` 2026-04-14 unblocked the move; md5 `1624fb138c0bebb0c87585eb7d62dca5` invariant preserved; `[MIGRATED]` forward-pointer stub authored at old location; `lattice-labs/iii/CLAUDE.md:50` path field updated. **Bonus correction**: Plan-Agent Findings (b) prose at line 77 of this charter previously named `campaign_kinn_branding_iii/iii_corrections_campaign.jsonl` — that file does not exist; the 13,144 B file is at `campaign_whitepaper_iii_deep_review/`; corrected this session)
- [ ] **MB-8**: LPWhitepaper.aDNA `iii/` consumer wrapper live (gap surfaced during MB-1 wikilink sweep)
- [ ] All actively-running consumers survive Campaign B without disruption — at MB-1 close 2026-05-08, only `campaign_kinn_branding_iii` (50/100) is actively writing at lattice-labs; `campaign_whitepaper_iii_deep_review` migrated to LPWhitepaper.aDNA 2026-04-17; `campaign_canvas_visual_command` superseded by CanvasForge.aDNA M-Cleanup-06 cutover 2026-05-04

## Mission Table

| Mission | Objective | Sessions | Status |
|---------|-----------|----------|--------|
| **MB-1** | lattice-labs `iii/` wrapper; retire operational corrections.jsonl per ADR-003; lattice-labs/CLAUDE.md III routing | 1 | ✅ **COMPLETE 2026-05-08** |
| **MB-2** | SiteForge `iii/` wrapper; multi-voice orchestration absorbed (MA-3 carry-forward #2) | 1 | ✅ **COMPLETE 2026-05-10** |
| **MB-3** | VideoForge `iii/` wrapper; ADR-006 bridge resolved | 1 | ✅ **COMPLETE 2026-05-11** |
| **MB-4** | CanvasForge `iii/` wrapper; relocate `skill_canvas_iii_review.md` (MA-3 carry-forward #4) | 1 | ✅ **COMPLETE 2026-05-11** |
| **MB-5** | wga `iii/` wrapper | 0.5 | ✅ **COMPLETE 2026-05-11** |
| **MB-6** | `skill_iii_setup.md` in adna template + workspace CLAUDE.md note | 0.5 | ✅ **COMPLETE 2026-05-12** |
| **MB-7** | Vault-hygiene closeout (4 carry-forwards + 2 Plan-agent findings + `kind` registry formalization); KINN relocation completed (campaign closed 2026-04-14); vestigial campaign-scoped JSONLs at frozen breadcrumb directories (whitepaper 13 144 B + canvas_visual 659 B) given `MIGRATION_NOTE.md` sidecars and left in place | 0.5 | ✅ **COMPLETE 2026-05-12** |
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

(a) **Canonical jsonl schema drift vs ADR-003 §4** — ✅ **CLOSED at MB-7 2026-05-12** via ADR-003 §4 amendment (Stanley call this session). Live `iii_corrections_canonical.jsonl` uses `trap`/`graduated_to`/`accepted` field names; pre-amendment ADR-003 §4 specified `trap_pack`/`acceptance_rate`/`graduated_from`. Amendment aligns the ADR to the live schema (canonical jsonl byte-identical; md5 `dde2cbd88c0b45956fb22285a2a0f856` invariant preserved). The backfill alternative (rewrite all 26 entries to spec'd names) was rejected because it would invalidate the canonical hash invariant referenced in many downstream files. Amendment-history footer added to ADR-003.

(b) **Vestigial campaign-scoped third corrections stores at lattice-labs breadcrumb directories** — ✅ **CLOSED at MB-7 2026-05-12** (left in place with `MIGRATION_NOTE.md` sidecars). **Correction (MB-7)**: this finding originally named `lattice-labs/how/campaigns/campaign_kinn_branding_iii/iii_corrections_campaign.jsonl` (13 144 B). That file does not exist — the 13,144 B jsonl actually lives at `campaign_whitepaper_iii_deep_review/iii_corrections_campaign.jsonl` (21 JSONL entries; campaign migrated to LPWhitepaper.aDNA 2026-04-17). A second smaller vestigial file (659 B deprecated stub) lives at `campaign_canvas_visual_command/iii_corrections.jsonl` (campaign superseded by CanvasForge.aDNA M-Cleanup-06 cutover 2026-05-04). The MB-7 row text at line 53 already had the corrected identification; only this Plan-Agent Findings prose drifted. MB-7 resolution: leave both jsonls in place as historical breadcrumbs; author `MIGRATION_NOTE.md` sidecar at each documenting the disposition decision. Future seed-into-LPWhitepaper-local-store for the 21-entry whitepaper jsonl is MB-8's call, not MB-7's.

## Risk Register

| # | Risk | Sev | Mitigation |
|---|------|-----|------------|
| R1 | Active lattice-labs campaigns break during wrapper migration | HIGH | Wikilink sweep on each wrapper close; pause closure if any unresolved; MA-1/MA-2 forward-stub pattern preserved |
| R2 | Consumer version drift over time | MED | ADR-002 §3 minor-bump review policy; consumer agent reads CHANGELOG diff before updating pinned version |
| R3 | VideoForge ADR-006 bridge complexity | MED — **MITIGATED 2026-05-11** | Per ADR-002 §6 III.aDNA does not absorb consumer-specific bridges; ADR-006 stays in VideoForge; MB-3 wrapper points at it via `local_extensions`. **Materialized**: VideoForge `iii/` wrapper ratified at MB-3 closure 2026-05-11 carries `local_extensions: [{kind: bridge_pack, path: videoforge_iii_domain_pack.md}, ...]`; the bridge pack is pointer-only (no ADR-006 content duplication), carries `not_graduating_to_canonical: true` per ADR-002 §6, and was sanctioned by this risk-row mitigation language verbatim (cited at `iii/CLAUDE.md:53` rationale field). III.aDNA core remains modality-agnostic. |
| R4 | Canonical jsonl schema reconciliation surfaces unresolved questions | LOW | Deferred to MB-7; v0.1.0 pin remains stable; consumer wrappers depend on path semantics, not field-name semantics |
| R5 | aDNA template is public repo — additive changes only for MB-6 | MED | MB-6 last in campaign; PR if upstream-incompatible |

## Phase Plan

| Phase | Name | Missions | Status |
|-------|------|----------|--------|
| **P1** | Critical-path migration | MB-1 | ✅ **COMPLETE 2026-05-08** |
| **P2** | Forge-consumer wrappers (parallel-eligible) | MB-2, MB-3, MB-4, MB-5 | ✅ **COMPLETE 2026-05-11** — MB-2 ✅; MB-3 ✅; MB-4 ✅; MB-5 ✅ |
| **P3** | Template + closeout | MB-6, MB-7, MB-8, DG-B gate | partial — MB-6 ✅ 2026-05-12; MB-7 ✅ 2026-05-12; MB-8 + DG-B gate pending |

## AAR
<!-- Template: how/templates/template_aar_lightweight.md — populate at DG-B closure -->

(pending)

## Cross-References

- ADR-002 (consumer federation contract): `~/lattice/III.aDNA/what/decisions/adr_002_consumer_federation_contract.md`
- ADR-003 (learning store ownership): `~/lattice/III.aDNA/what/decisions/adr_003_learning_store_ownership.md`
- Predecessor campaign (Campaign A genesis): `~/lattice/III.aDNA/how/campaigns/campaign_a_iii_genesis/campaign_a_iii_genesis.md`
- Sibling campaign (Campaign C airlock standard, parallel-eligible): `~/lattice/III.aDNA/how/campaigns/campaign_c_airlock_standard/campaign_c_airlock_standard.md`
- Forge canonical spec (MB-2..MB-5 reference pattern): `~/lattice/SiteForge.aDNA/what/artifacts/sf_forge_pattern_spec.md`
