---
type: manifest
role: project
created: 2026-05-07
updated: 2026-05-20
mb3_closed: 2026-05-11
mb4_closed: 2026-05-11
mb5_closed: 2026-05-11
mb6_closed: 2026-05-12
mb7_closed: 2026-05-12
mb8_closed: 2026-05-12
dg_b_closed: 2026-05-12
mc4_closed: 2026-05-13   # disk-state; retroactive git commit landed 2026-05-20 at c8ce621
mc4_git_retroactive_close_commit: c8ce621
mc4_5_closed: 2026-05-20   # planning-class interstitial alignment recon
mc5_closed: 2026-05-20   # VideoForge → CanvasForge re-exercised against v0.2; zero regression confirmed
dg_c_closed: 2026-05-20   # Campaign C: Airlock Standard v0.2 complete end-to-end 9/9
dg_c_carry_forward_1_closed: 2026-05-20   # skill_session_close_ceremony.md adopted (closure-discipline mitigation; commit c4a5f77)
campaign_d_chartered: 2026-05-20   # Campaign D: D1 federation-aware airlock v0.3 (with D3 activation kit folded as MD-A3) + D2 RLHF/adaptive-improvement loop, parallel-eligible; 2 outbound coord memos fired (LL Berthier + LN Venus); first canonical post-adoption application of skill_session_close_ceremony
last_edited_by: agent_argus
tags: [manifest, governance, iii, framework]
---

# III.aDNA — Project Manifest

## Project Identity

**III.aDNA** — The standalone modular quality improvement framework for the Lattice ecosystem. III implements a three-phase loop — **Inspect** (adversarial precision check), **Introspect** (structural pattern calibration), **Improve** (ranked change orders + learning accumulation) — consumable by any vault or agent via lightweight federation wrappers.

**Category**: Framework.aDNA — vaults that define protocols and methodologies consumed by other context graphs. III.aDNA is the first Framework.aDNA project in the Lattice ecosystem.

**Persona**: Argus Panoptes — the all-seeing giant.

**Version**: `v0.2.0` (cross-vault-request-capable, 2026-05-10) — see § Release History below for full lineage.

**GitHub**: https://github.com/LatticeProtocol/III.aDNA (published at Campaign A wind-down, commit `75aea6f`).

## Architecture

```
III.aDNA/
├── how/
│   ├── airlock/          # External-agent entry point (AIRLOCK.md v0.2.0 reference instance — Entry + Request surfaces)
│   ├── skills/           # Core skill + consumer onboarding skill
│   ├── templates/        # Standard templates + template_cross_vault_request_reply.md (MC-3)
│   └── campaigns/        # Genesis (A), federation (B), airlock standard (C)
├── what/
│   ├── artifacts/        # iii_airlock_standard_spec.md v0.2.0 + iii_airlock_request_schema.yaml (JSON Schema Draft 2020-12)
│   ├── context/
│   │   └── core_domain_packs/   # 7 canonical packs + learning store
│   ├── decisions/         # ADRs 000–004 (founding set); ADR-002 + ADR-003 amended at MB-7 (Amendment History footers)
│   ├── modules/           # 8 composable III modules (.md + .module.yaml each)
│   └── lattices/          # lattice_iii_verification_oracle.lattice.yaml v1.2.1 (reviewed_output retyped at MB-7)
├── who/
│   ├── coordination/      # Cross-vault coord memos (absorbed: VideoForge airlock v0.2 proposal)
│   └── governance/
```

## Entry Points

| If you want to… | Start here |
|-----------------|-----------|
| Run III on a document (any domain) | `how/skills/skill_iii_review.md` |
| Choose between two airlock surfaces (read context locally vs commission work) | `how/airlock/AIRLOCK.md` (Entry — read context + run loop locally; Request — commission work from a vault) |
| Understand the airlock standard contract (both surfaces) | `what/artifacts/iii_airlock_standard_spec.md` (v0.2.0) |
| Validate a cross-vault request memo (mechanical schema) | `what/artifacts/iii_airlock_request_schema.yaml` (JSON Schema Draft 2020-12 in YAML form) |
| Reply to a cross-vault request as the receiving vault | `how/templates/template_cross_vault_request_reply.md` (Acceptance full-profile + Rejection variants) |
| Add III to a new vault | `how/skills/skill_iii_setup.md` |
| Federate a vault against III.aDNA (consumer wrapper contract) | `what/decisions/adr_002_consumer_federation_contract.md` |
| Use III as a composable VaaS oracle | `what/lattices/lattice_iii_verification_oracle.lattice.yaml` |
| Understand the module boundary model | `what/decisions/adr_001_module_architecture.md` |
| Publish a new domain pack | `what/decisions/adr_003_learning_store_ownership.md` (graduation protocol) |

## Active Consumers

| Vault | Wrapper | Packs used | Status |
|-------|---------|-----------|--------|
| `lattice-labs` | `lattice-labs/iii/` | whitepaper, kinn_branding, vault_maintenance, canvas_visual | **MB-1 ✅ 2026-05-08** — pinned at `v0.1.0` (commit `1628793`); consumer-side review pending v0.2.0 minor bump per ADR-002 §3. **MB-7 update 2026-05-12**: KINN pack physically relocated from `what/context/iii_domain_packs/` to `iii/what/context/`; wrapper `path:` field updated; `[MIGRATED]` stub at old location. Active-campaigns table updated (kinn campaign reframed as `completed 2026-04-14`; whitepaper + canvas_visual breadcrumbs now have `MIGRATION_NOTE.md` sidecars). |
| `SiteForge.aDNA` | `SiteForge.aDNA/iii/` | inspect_procedures, introspect_checks, learning_store, web_design, vault_maintenance | **MB-2 ✅ 2026-05-10** — pinned at `v0.2.0` (commit `04ae724`); 5/7 canonical packs; all 8 modules; 2 local_extensions (reviewer_registry → in-place `siteforge_reviewers.yaml` 5-voice registry; learning_store_local seeded empty) |
| `VideoForge.aDNA` | `VideoForge.aDNA/iii/` | inspect_procedures, introspect_checks, learning_store, web_design, vault_maintenance, + ADR-006 bridge_pack | **MB-3 ✅ 2026-05-11** — pinned at `v0.2.0` (commit `246124d` — exact tag commit; annotated tag object `5cd210e`); 5/7 canonical packs; all 8 modules; lattice v1.2.0; 2 `local_extensions` (`bridge_pack` → `videoforge_iii_domain_pack.md` / ADR 006 bridge satisfying Campaign B R3 mitigation; `learning_store_local` → `videoforge_iii_learning_store.jsonl` seeded empty); ratified via `coord_2026_05_11_videoforge_iii_wrapper_authoring` (audit_id `session_stanley_20260511_iii_adna_mb3_videoforge_wrapper`; 12/12 audit checks pass; first inbound v0.2 cross-vault request to traverse full lifecycle `open → accepted → rendering → shipped → closed`) |
| `CanvasForge.aDNA` | `CanvasForge.aDNA/iii/` | inspect_procedures, introspect_checks, learning_store, web_design, vault_maintenance, + canvas-visual `bridge_pack` (10-trap CanvasForge-resident; supersedes upstream 8-trap canonical) | **MB-4 ✅ 2026-05-11** — pinned at `v0.2.0` (commit `246124d` — exact tag commit); 5/7 canonical packs (`canvas_visual` deliberately omitted, superseded by bridge_pack); all 8 modules; lattice v1.2.0; 3 `local_extensions` (`bridge_pack` → `what/context/iii/context_iii_canvas_visual.md` 10-trap pack, `not_graduating_to_canonical: true`, 2 M00 deltas may graduate independently; `local_skill` → in-place `how/skills/skill_canvas_iii_review.md` 5-voice review skill — Visual Critic / Narrative / Accessibility / Comic Consistency / Template Detector — already at canonical location since M-Cleanup-04 wave 3 2026-05-04; `learning_store_local` → new `iii/what/context/canvasforge_iii_learning_store.jsonl` seeded empty); closes MA-3 carry-forward #4 via governance registration (physical move already completed at M-Cleanup-04); downstream-safe — SS/CC `presentationforge/` + `graphicnovelforge/` wrappers don't reference `iii/`, `what/context/iii/`, `how/skills/`, or `CanvasForge.aDNA/CLAUDE.md` Standing Orders |
| `wga.aDNA` | `wga.aDNA/iii/` | inspect_procedures, introspect_checks, learning_store, web_design, vault_maintenance | **MB-5 ✅ 2026-05-11** — pinned at `v0.2.0` (commit `246124d` — exact tag commit); 5/7 canonical packs (omits `whitepaper_communication` — no whitepaper outputs yet; `canvas_visual` — no canvas substrate; `kinn_branding` — lattice-labs-specific); all 8 modules; lattice v1.2.0; 1 `local_extension` (`learning_store_local` → `iii/what/context/wga_iii_learning_store.jsonl` seeded empty); pre-federation `campaign_wga_site_iii` (lattice-labs, complete 2026-04-14, quality 9.26/10) already graduated C-012..C-016 to canonical at MA-1 — no migration needed; clean-slate consumer; minimal-wrapper baseline (matches MB-3 VideoForge selection, no bridge_pack / no local_skill); wga Standing Order 7 added routing III review through wrapper; downstream-safe — zero vaults federate against wga.aDNA as `source_vault` |
| `LPWhitepaper.aDNA` | `LPWhitepaper.aDNA/iii/` | inspect_procedures, introspect_checks, learning_store, whitepaper_communication, canvas_visual, vault_maintenance | **MB-8 ✅ 2026-05-12** — pinned at `v0.2.0` (commit `246124d` — exact tag commit; matches MB-3/4/5 pin precision); **6/7 canonical packs** (first 6/7-pack wrapper in the Lattice ecosystem; first wrapper-managed consumer of `whitepaper_communication`; the 6th-pack inclusion is `canvas_visual` for the 17 TikZ figures in `what/source/figures/`); all 8 modules; lattice v1.2.1; 1 `local_extension` (`learning_store_local` → `iii/what/context/lpwhitepaper_iii_learning_store.jsonl` seeded empty per ADR-003 §2). **Pre-federation migration**: 5 ported context_iii_*.md packs → `[MIGRATED]` forward-pointer stubs at `LPWhitepaper.aDNA/what/context/iii_domain_packs/`; AGENTS.md → forward-pointer stub; 21-entry merged operational `iii_corrections.jsonl` truncated to 0 bytes (19 `source=core` entries already in canonical from MA-1, 2 `source=campaign_iii_deep_review` entries preserved at lattice-labs breadcrumb per MB-7 disposition); pre-federation forked skill `how/skills/skill_iii_cycle.md` → deprecation banner pointing at canonical via wrapper. Sibling `MIGRATION_NOTE.md` documents disposition. LPWhitepaper.aDNA Standing Order 8 added routing III review through wrapper. Downstream-safe — zero vaults federate against LPWhitepaper.aDNA as `source_vault`. **First production test of `skill_iii_setup.md`**: 10-step procedure executed verbatim; no rough edges surfaced; skill is production-stable. Active campaigns at LPWhitepaper (`campaign_iii_deep_review` 25-cycle work + `campaign_sharing_prep`) survive stub redirect via the [MIGRATED] forward-pointer pattern (MB-1/MB-2 precedent). |

## Release History

| Tag | Date | Closure commit | Summary |
|-----|------|----------------|---------|
| `v0.1.0` | 2026-05-08 | `1628793` (MA-4) | Campaign A genesis baseline — AIRLOCK.md reference implementation (5 entry paths: A text / B web+visual / C code / D video / E vault maintenance); Path Selection matrix; anti-regression block deferring cross-vault request patterns to v0.2. Initial publication to `LatticeProtocol/III.aDNA`. |
| `v0.2.0` | 2026-05-10 | `246124d` (MC-3) | Airlock Standard v0.2.0 reference instance — adds cross-vault request surface (spec §4 + JSON Schema Draft 2020-12 request schema + Acceptance/Rejection reply-comment template); 5 v0.1.0 entry paths preserved byte-identical against `v0.1.0` tag; VideoForge proposal (5 gaps) absorbed in full; first cross-vault-request-capable release. |
| `v0.2.0` (campaign closure) | 2026-05-20 | (no new tag) | **Campaign C: Airlock Standard v0.2 ✅ CLOSED 2026-05-20 9/9** — MC-4 substrate-implementation guidance (490 lines; §2 secrets_preflight + §3 idempotency_key dedup); MC-4.5 alignment recon interstitial (planning-class single-session; LL.aDNA Phase 3+4 + LN.aDNA pc_01 Phase A federation substrate landscape survey + 9-stub AIRLOCK.md ecosystem inventory + MC-5 scope decision tree + 3 Campaign D candidates sketched); MC-5 validation (`what/artifacts/mc5_validation_videoforge_canvasforge_v0_2.md`; 16/16 §5.1 coverage map rows conform + 2 additive deltas + 5/5 substrate-impl sample records; zero regression confirmed); inbound VideoForge proposal `absorbed → closed`; DG-C scorecard 9/9 green; AAR populated inline. **No tag bump** (additive disposition matches MB-6/MB-7/MB-8/MC-4/DG-B precedent); `v0.2.0` (commit `246124d`, annotated tag object `5cd210e`) remains the production pin. Spec / schema / AIRLOCK.md / substrate-implementation / reply-template versions all unchanged. Canonical learning store md5 `dde2cbd88c0b45956fb22285a2a0f856` invariant preserved. |

Future releases follow ADR-002 §3 and airlock spec §6: patch transparent (no consumer review); minor consumer-reviewed (additive only); major reserved for v1.0 breaking changes (explicit human decision per consumer).

## Active Airlock-spec Consumers

This register tracks downstream adopters of the **III.aDNA airlock standard** (`what/artifacts/iii_airlock_standard_spec.md`). It is parallel to (but distinct from) the **Active Consumers** register above: that register tracks `iii/` framework consumers (vaults that consume the III review skill + packs + modules); this register tracks vaults that have authored an `AIRLOCK.md` instance, a template stub, or a worked example exercising the airlock vault-to-vault interaction pattern. Many vaults appear in both registers (e.g., VideoForge.aDNA + CanvasForge.aDNA federate III via `iii/` AND adopt the airlock surface independently). Adoption is not contract-enforced from III.aDNA; this register is observational governance, parallel to publishing a citation index — bumps to `iii_airlock_standard_spec.md` flow downstream via each adopter's `federation_ref` review at their own pace per ADR-002 §3.

| Vault | Instance Type | III.aDNA Spec Version | Status |
|-------|---------------|-----------------------|--------|
| `aDNA.aDNA` | Template stub spec — `.adna/how/airlock/AIRLOCK.md` (~80 lines) + `aDNA.aDNA/what/decisions/adr_008_airlock_template_stub.md` (ADR-008) | **v0.2.0 (explicit `federation_ref.version`)** | **2026-05-11** — Ratified at aDNA.aDNA M03 phase gate Session 3 (status: accepted). Template-level adoption: any new vault forked from `.adna/` inherits the airlock entry point unmodified; operator flips frontmatter `status: inactive → active` to opt in to cross-vault traffic. ADR-008 explicitly declines to duplicate III's spec content — "future III spec bumps are absorbed via federation_ref version review per ADR-002 §3." This is the **template-level downstream adoption** of III's airlock pattern; it lifts the airlock surface from per-vault opt-in to default-inherited-with-opt-in-flip. No back-pressure on III.aDNA — strictly a downstream adoption. |
| `VideoForge.aDNA` | Full reference instance — `VideoForge.aDNA/how/airlock/AIRLOCK.md` (4 entry paths: consumer wrapper entry, speech tuning entry, Q&A analysis entry, training-data curation entry) | **v0.1 era (implicit; pre-MC-3)** | **2026-05-08** — VideoForge authored a full airlock reference instance during Iris-led genesis planning, mirroring the III.aDNA pattern from the v0.1.0 reference at `how/airlock/AIRLOCK.md`. Filed the v0.2 cross-vault request proposal (5 gaps in v0.1) at `~/lattice/III.aDNA/who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md` — the proposal was **absorbed** at MC-3 (2026-05-10) when AIRLOCK.md v0.2.0 + the request schema + the reply-comment template + the spec forward-references all landed. VideoForge subsequently exercised the v0.2 surface as a request originator at MB-3 close (2026-05-11) — first inbound v0.2 cross-vault request to traverse the full lifecycle (`open → accepted → rendering → shipped → closed`). |
| `CanvasForge.aDNA` | Worked example (cross-vault request coord memo, no own AIRLOCK.md) — `CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` | **v0.1 era (implicit; pre-MC-3)** | **2026-05-08** — CanvasForge served as the receiving vault in a worked-example cross-vault request from VideoForge (Carly + Herb sprint onboarding deck). Treats the airlock pattern as already-standardized; no own `how/airlock/AIRLOCK.md` instance authored. Cited in III.aDNA AIRLOCK.md v0.2.0 Entry Points table as the canonical worked example. Could author a full reference instance in a future hygiene mission if the vault accumulates more cross-vault traffic; currently lives entirely on the III.aDNA-owned spec + receives traffic. |
| `M08a multilateral` | 17-vault one-shot exercise — 17 per-vault coord memos at `aDNA.aDNA/who/coordination/coord_2026_05_09_v7_*.md` (canvasforge / comfyforge / context_commons / iii / la_startup / lpwhitepaper / rarearchive / rareharness / science_stanley / siteforge / strategic_interface_protocol / superleague / vaaslattice / videoforge / wga / wilhelmai / zeta) | **v0.2.0 (per ADR-008 companion mission)** | **2026-05-09** — Multilateral airlock exercise rendering the M01-Obj-8 per-vault coord memo template against the M02-locked baseline (14+ variable substitutions × 17 = 238+ substitutions; zero residual drift). AAR at `aDNA.aDNA/how/campaigns/campaign_adna_v2_infrastructure/missions/artifacts/aar_m08a_upgrade_guide_and_coord_memos.md`. One-shot: not an ongoing instance (no `how/airlock/AIRLOCK.md` per-vault), but exercises the airlock pattern at scale and is the empirical evidence base for ADR-008's "the airlock has converged toward a vault-to-vault traffic pattern" framing. |
| `node.aDNA` | Inherited template stub — `node.aDNA/how/airlock/AIRLOCK.md` (forwarded from `.adna/how/airlock/AIRLOCK.md` per ADR-008 template adoption) | **v0.2.0 (inherited from template)** | **2026-05-11** — node.aDNA carries the airlock template stub via `.adna/`-forked inheritance (frontmatter `status: inactive` by default; activates by flipping to `active`). At MB-8 audit time the file is in inherited template state (no node.aDNA-specific customization); illustrative of the template-stub default-inherited model from ADR-008. Local-by-default per workspace CLAUDE.md `node.aDNA/` standing rule (Standing Rule #5); not pushed to a remote unless operator opts in. |

The five rows above are the currently-known instances at MB-8 close (2026-05-12). New adopters land at this register at their adoption events; III.aDNA does not gate the addition. For the historical lineage (III canonical → VideoForge reference → CanvasForge worked example → M08a multilateral → aDNA template stub), see `what/artifacts/iii_airlock_standard_spec.md` §8.4.

## Known Carry-Forwards

Captured at MC-3 wind-down (2026-05-10) for future sessions. **MA-3 carry-forwards (1, 2, 3, 4) all closed at MB-2 / MB-4 / MB-7**; the MB-7 closure (2026-05-12) discharged the last two (#1 `reviewed_output` retype + #3 `AGENTS.md` wikilink) along with the 2 Plan-Agent findings from MB-1 plan validation and the `kind` registry formalization in ADR-002 §1a. **Campaign D charter routing** (2026-05-20): the `lattice-labs/iii/CLAUDE.md` minor-bump carry-forward below is folded into Campaign D MD-A4 wrapper sweep (v0.1.0 → v0.3.0); see `how/campaigns/campaign_d_federation_adaptive_loop/campaign_d_federation_adaptive_loop.md` §Carry-Forward Disposition for full disposition table.

- **AIRLOCK.md Path E "v0.1.0" wording** — v0.2.x patch candidate. Path E `Out of scope` line reads `not yet covered by v0.1.0; track in Campaign C v0.2 follow-ups`. Verbatim preservation contract (spec §3.3) carried this line forward through the v0.2.0 bump; Campaign C v0.2 did not add federation_ref pin drift coverage (deferred to v0.3+). Fix wording AND the corresponding spec §3 reference simultaneously to keep the verbatim contract enforceable.
- **`lattice-labs/iii/CLAUDE.md` federation_ref pin at `v0.1.0`** — consumer-side ADR-002 §3 minor-bump review. Per `lattice-labs/iii/CLAUDE.md:99`, the wrapper agent reviews the upstream CHANGELOG diff before updating `version:`. Not gating any current work; not III.aDNA's responsibility.
- **`CHANGELOG.md` formalization at vault root** — v0.3 candidate. ADR-002 §3 names "the airlock changelog" as the consumer review surface. Today the `v0.1.0` and `v0.2.0` annotated tag messages carry release-note prose; a dedicated `CHANGELOG.md` would consolidate that surface and make consumer-side diffing easier.

## Originating System

Migrated from `lattice-labs/how/skills/skill_iii_review.md` (created 2026-04-01, campaign_iii_evolution COMPLETED 2026-04-03). Learning store: 26 corrections accumulated, 5 graduated to domain packs. Cross-vault III campaigns active at migration: whitepaper_iii_deep_review (M02/25 cycles), campaign_kinn_branding_iii (50/100 cycles).

v0.1.0 (Campaign A) and v0.2.0 (Campaign C) shipped within 3 days of vault inception; both releases governed by ADR-002 federation contract.
