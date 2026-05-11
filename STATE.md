---
type: state
created: 2026-05-07
updated: 2026-05-11
status: active
last_edited_by: agent_stanley
last_session: session_stanley_20260511_iii_adna_mb3_videoforge_wrapper
tags: [state, governance]
---

# III.aDNA — Operational State

## Current Phase

**Campaign B: Federation — P2 IN-FLIGHT (MB-1 ✅ 2026-05-08; MB-2 ✅ 2026-05-10; MB-3 ✅ 2026-05-11; MB-4..MB-8 pending) — Campaign C: Airlock Standard v0.2 — P2 partial (MC-1 ✅ + MC-2 ✅ 2026-05-08; MC-3 ✅ 2026-05-10; MC-4 pending). Annotated `v0.2.0` tag cut at MC-3 closure. MB-3 doubles as first inbound v0.2 cross-vault request to traverse full lifecycle `open → accepted → rendering → shipped → closed` end-to-end — strengthens MC-5 validation case.**

> **MB-2 closure note** (2026-05-10): Campaign B P2 opened. `~/lattice/SiteForge.aDNA/iii/CLAUDE.md` authored — federation_ref pinned at III.aDNA `v0.2.0` (commit `04ae724`); 5 of 7 canonical packs declared (web_design primary; `whitepaper_communication` + `canvas_visual` intentionally omitted as out-of-scope for web archetype work — § "Packs out of scope (intentionally)" documents the boundary); all 8 modules; 2 `local_extensions` (`kind: reviewer_registry` pointing at in-place `~/lattice/SiteForge.aDNA/what/context/siteforge/siteforge_reviewers.yaml` — MA-3 carry-forward #2 absorbed per `module_iii_semantic_reviewer.md` lines 56-69 explicit landing-site citation; `kind: learning_store_local` pointing at empty new `~/lattice/SiteForge.aDNA/iii/what/context/siteforge_iii_learning_store.jsonl`). Multi-voice orchestration (5 voices: Voice Critic / Design / UX / SEO / Brand Alignment with `cross_voice_threshold: 3` escalation) stays at established SiteForge path — three in-flight references (`skill_quality_validation.md`, `lattice_partner_website_scaffold.lattice.yaml`, `lattice_quality_validation.lattice.yaml`) hardcode it; wrapper formalizes the pointer without forcing relocation (mirrors MB-1 KINN pack decision). Pre-migration operational `iii_corrections.jsonl` at `SiteForge.aDNA/what/context/iii_domain_packs/` truncated to 0 bytes (canonical md5 `dde2cbd88c0b45956fb22285a2a0f856` re-verified unchanged); pre-migration local web_design pack replaced with `[MIGRATED]` forward-pointer stub (diff against canonical confirmed zero substantive trap-content drift — only stale `updated:` frontmatter and old wikilinks); sibling `MIGRATION_NOTE.md` authored at SiteForge documenting Stage 3 closure. Two SiteForge lattice yamls (`lattice_partner_website_scaffold.lattice.yaml:163` + `skills/lattice_quality_validation.lattice.yaml:57`) updated — `corrections_store` paths rerouted to wrapper-managed local store. SiteForge.aDNA/CLAUDE.md gained Standing Order 7 (III review via wrapper; mirrors lattice-labs Rule 12 / CanvasForge Rule 11); project-map line annotated; Context-Loading Quality line repointed at wrapper. Wikilink sweep clean — all `iii_corrections` / `iii_domain_packs` / `context_iii_domain_packs_web_design` hits accounted for; design-doc references at `sf_m14_context_graft_design.md` (lines 97, 185, 187, 188, 235, 236, 244) + `sf_m14_learning_loop_design.md` (lines 24, 25, 39, 48, 75, 87) survive stub redirect, flagged for Campaign B MB-7 disposition per the new `MIGRATION_NOTE.md`. No git tag bump (MB-2 touches no III.aDNA core; `v0.2.0` stays). Campaign B charter MB-2 row + DG-B box flipped ✅; MANIFEST.md Active Consumers SiteForge row updated.

> **MC-3 closure note** (2026-05-10): Campaign C P2 partial — MC-3 ships the operational reference instance for Airlock Standard v0.2.0. `how/airlock/AIRLOCK.md` bumped v0.1.0 → v0.2.0 (frontmatter `version: "0.2.0"`, `covers: [entry_paths, cross_vault_request_patterns]`, `governed_by: what/artifacts/iii_airlock_standard_spec.md`, `absorbs_proposal: who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md`); new Surface Selection table routes Entry vs Request before path selection; new § Cross-Vault Requests section routes the v0.2 surface (where the memo lives, required-minimum payload, 8-state lifecycle, lightweight vs full handshake profiles, secrets, idempotency, worked example) — all by pointer at spec §4 and schema extensions, not prose duplication; 5 entry paths preserved verbatim against `v0.1.0` tag (Path A–E Profile/Use cases/Out of scope/Expected output/Context budget/Load in order blocks byte-identical); "What v0.2.0 does NOT cover" anti-regression replaces the v0.1.0 deferral block — now lists v0.3+ deferrals from spec §7.2 (`proposed/` channel, multi-vault transactional, async batched, executable substrate). Reply-comment template authored at `how/templates/template_cross_vault_request_reply.md` (Acceptance full-profile per spec §4.2 field requirements; lightweight-profile descriptive note; Rejection variants with 7-value reason enum; Status Log row update). Spec patch edits (no version bump): §7.2 row 6 flipped ✅ MC-3 2026-05-10; §7.3 forward-reference list expanded from "three originally cited / one resolved / two pending" to "four originally cited / three resolved (MC-2, MC-3 template, MC-3 AIRLOCK.md bump) / two pending (MC-4 × 2)"; §8.4 reference-instance row updated from "v0.1.0 ships; v0.2.0 bump at MC-3" to "v0.2.0 — bumped at MC-3 2026-05-10". VideoForge proposal flipped `accepted → absorbed` (frontmatter + Status Log row date populated with full-paragraph MC-3 closure note). Campaign C charter: DG-C MC-3 box ✅ + tag-cut box ✅ + proposal-status box ✅ (closed pending MC-5); mission table MC-3 row ✅ COMPLETE 2026-05-10; Phase Plan P2 row marked partial (MC-3 ✅; MC-4 pending); Promises section Promise 2 (coord-memo fallback) flipped RETIRED. Annotated `v0.2.0` git tag cut at MC-3 closure commit (first release-grade tag since `v0.1.0` MA-4 close). No remote push yet — Stanley pushes when ready.

> **MC-2 closure note** (2026-05-08): Campaign C P1 closed. `what/artifacts/iii_airlock_request_schema.yaml` authored (JSON Schema Draft 2020-12 in YAML form) — 9/9 mechanical validation checks pass (`Draft202012Validator.check_schema` clean; required-minimum positive + 4 negative-path tests + closed-shape constraint typo + secret-name pattern enforcement all pass). Required-minimum (`type: cross_vault_request` + `artifact_request.{spec_path, output_sink}`) enforced exactly per spec §4.3. Open at root + `artifact_request` (consumer-extension friendly); closed on substrate-critical sub-objects (`constraints`, `secrets_handled`). Worked example at `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` correctly non-conforms (top-level `type: coordination` not `cross_vault_request`; `spec_path`/`output_sink` body-promoted) — both gaps expected per spec §5.2 (pre-v0.2 authoring) and documented in schema header. Spec forward-references resolved (`defers_schema_to` frontmatter comment + §4.3 prose + §7.2 row + §7.3 bullet 1 — patch-level edits, no version bump). Campaign C charter MC-2 row + DG-C MC-2 box + P1 phase row flipped; charter frontmatter `phase: P1 → P2`. P2 (MC-3 + MC-4) now open.

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
| **Campaign B: Federation** | Wire iii/ consumer wrappers across all vaults | **P2 IN-FLIGHT** | **OPEN 2026-05-08** — MB-1 ✅; MB-2 ✅ 2026-05-10; **MB-3 ✅ 2026-05-11**; MB-4..MB-8 pending |
| **Campaign C: Airlock** | Formalize airlock standard v0.2 + propagate to major vaults | **P2 partial** | **OPEN 2026-05-08** — MC-1 ✅; MC-2 ✅; **MC-3 ✅ 2026-05-10** (AIRLOCK.md v0.2.0 + reply-comment template + `v0.2.0` git tag); MC-4 + MC-5 pending; parallel-eligible with B |

## Campaign B Mission Queue (active)

| Mission | Goal | Est. | Status |
|---------|------|------|--------|
| MB-1: lattice-labs wrapper | Retire operational corrections.jsonl per ADR-003; lattice-labs/CLAUDE.md III routing | 1 sess | ✅ **COMPLETE 2026-05-08** |
| MB-2: SiteForge wrapper | Multi-voice orchestration absorbed (MA-3 carry-forward #2) | 1 sess | ✅ **COMPLETE 2026-05-10** |
| MB-3: VideoForge wrapper | ADR-006 bridge resolved | 1 sess | ✅ **COMPLETE 2026-05-11** |
| MB-4: CanvasForge wrapper | Relocate `skill_canvas_iii_review.md` (MA-3 carry-forward #4) | 1 sess | pending |
| MB-5: wga wrapper | Standard wrapper; minimal extensions | 0.5 sess | pending |
| MB-6: adna template | `skill_iii_setup.md` + workspace CLAUDE.md note | 0.5 sess | pending |
| MB-7: vault hygiene | 4 carry-forwards + 2 Plan-agent findings + KINN relocation | 0.5 sess | pending |
| MB-8: LPWhitepaper wrapper | Gap surfaced during MB-1 wikilink sweep | 1 sess | pending |

## Campaign C Mission Queue (P1 ✅; P2 partial — MC-3 ✅; MC-4 pending)

| Mission | Goal | Est. | Status |
|---------|------|------|--------|
| MC-1: Standard spec | `iii_airlock_standard_spec.md` (entry paths + cross-vault request patterns) | 1 sess | ✅ **COMPLETE 2026-05-08** |
| MC-2: Request schema | YAML payload schema canonicalized at `what/artifacts/iii_airlock_request_schema.yaml`; 9/9 mechanical validation checks; worked-example walk | 0.5 sess | ✅ **COMPLETE 2026-05-08** |
| MC-3: AIRLOCK.md v0.2 | + handshake reply-comment template + annotated `v0.2.0` git tag | 1 sess | ✅ **COMPLETE 2026-05-10** |
| MC-4: Substrate enforcement | `secrets_handled` preflight + `idempotency_key` contract | 1 sess | pending |
| MC-5: Validation | Re-exercise VideoForge → CanvasForge against v0.2; flip proposal status `absorbed → closed` | 0.5 sess | pending |

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
- corrections.jsonl canonical upstream operational; **lattice-labs `iii/` consumer wrapper live (MB-1 ✅ 2026-05-08)** — federation_ref pinned at v0.1.0, local store seeded empty, operational pre-migration jsonl retired; **SiteForge.aDNA `iii/` consumer wrapper live (MB-2 ✅ 2026-05-10)** — federation_ref pinned at v0.2.0 (commit `04ae724`); 5 of 7 packs; all 8 modules; 2 local_extensions (reviewer_registry pointing at in-place `siteforge_reviewers.yaml` 5-voice multi-voice registry — MA-3 carry-forward #2 absorbed; learning_store_local seeded empty); pre-migration `iii_corrections.jsonl` + local `context_iii_domain_packs_web_design.md` retired (truncate + `[MIGRATED]` stub); `MIGRATION_NOTE.md` documents Stage 3 closure; two SiteForge lattice yamls rerouted to wrapper-managed local store; SiteForge.aDNA Standing Order 7 added; **VideoForge.aDNA `iii/` consumer wrapper live (MB-3 ✅ 2026-05-11)** — federation_ref pinned at v0.2.0 (commit `246124d` — exact tag commit, annotated tag object `5cd210e`); 5 of 7 canonical packs (inspect_procedures + introspect_checks + learning_store + web_design + vault_maintenance); all 8 modules; lattice v1.2.0; 2 `local_extensions` (**new `kind: bridge_pack`** → `videoforge_iii_domain_pack.md` 148-line pointer-only ADR 006 bridge satisfying R3 mitigation per ADR-002 §6 modality-agnostic-core boundary; `learning_store_local` → `videoforge_iii_learning_store.jsonl` seeded empty); wrapper authored VideoForge-side under ADR 007 § D3 authority and ratified III.aDNA-side via the v0.2 cross-vault request handshake at `coord_2026_05_11_videoforge_iii_wrapper_authoring` (12/12 audit checks pass); MB-4..MB-8 wrappers pending
- skill_iii_review.md wikilinks all resolve to canonical core_domain_packs/ paths; no remaining `iii_domain_packs/` references
- 8 composable III modules live at `what/modules/` (`module_iii_dispatch`, `_inspect_text`, `_inspect_code`, `_inspect_visual`, `_inspect_data`, `_introspect`, `_improve`, `_accumulate`) — typed I/O matching lattice contract; pure (pack content arrives as typed input)
- Lattice `lattice_iii_verification_oracle.lattice.yaml` v1.2.0: all 8 module nodes carry `ref:` pointing at their `.module.yaml`; `accumulate` node added; ADR-003 write-policy expressed in node config rather than implicit in `human_review`
- Legacy `module_iii_semantic_reviewer.{md,yaml}` rebranded `status: composite_reference` with redirect banner to the 8 decomposed modules — preserves backward-compat for SiteForge consumers pointing at the composite (multi-voice orchestration is consumer-side, lands in `SiteForge.aDNA/iii/` at Campaign B MB-2)
- `how/airlock/AIRLOCK.md` v0.2.0 reference implementation (bumped at MC-3 2026-05-10): `status: reference_implementation`, `covers: [entry_paths, cross_vault_request_patterns]`, `governed_by: what/artifacts/iii_airlock_standard_spec.md`; Surface Selection routes Entry vs Request before path selection; § Entry Paths carries the 5 v0.1.0 entry paths verbatim (zero text drift); § Cross-Vault Requests routes the v0.2 surface (where the memo lives, required-minimum payload, lifecycle states, handshake profiles, secrets, idempotency, reply-comment template, worked example) — all by pointer at spec §4 and schema extensions; "What v0.2.0 does NOT cover" anti-regression lists v0.3+ deferrals (`proposed/` channel, multi-vault transactional, async batched, executable substrate)
- Reply-comment template `how/templates/template_cross_vault_request_reply.md` (MC-3 deliverable): Acceptance full-profile fenced block per spec §4.2 field requirements (status / audit_id / eta / reserved_capacity / input validation / secret presence names-only / idempotency check / preconditions / next state); lightweight-profile descriptive note (no fenced block; skipped under that profile); Rejection fenced block with 7-value reason enum; Status Log row update guidance
- Annotated git tags: `v0.1.0` cut at MA-4 closure (Campaign A baseline); **`v0.2.0` cut at MC-3 closure (2026-05-10)** — AIRLOCK.md v0.2.0 + reply-comment template + spec forward-references resolved + VideoForge proposal absorbed; first cross-vault-request-capable release

## Blockers

None currently. **DG-A CLOSED 9/9.** Campaign B is OPEN **P2 IN-FLIGHT** (MB-1 ✅ 2026-05-08; MB-2 ✅ 2026-05-10; **MB-3 ✅ 2026-05-11**; MB-4..MB-8 pending). Campaign C is OPEN — P1 ✅ COMPLETE (MC-1 ✅; MC-2 ✅); **P2 partial** (MC-3 ✅ 2026-05-10; MC-4 pending); P3 pending (MC-5 + DG-C gate). Annotated `v0.2.0` git tag cut at MC-3 closure — does not gate further missions; MB-2 pins SiteForge against it (commit `04ae724`, post-tag); MB-3 pins VideoForge against it (commit `246124d`, exact tag commit). MB-3 doubles as the first inbound v0.2 cross-vault request to traverse the full lifecycle `open → accepted → rendering → shipped → closed` end-to-end — strengthens MC-5 validation case (the v0.2 reply-comment template now has one production-grade Acceptance reply on file).

## Risk Register

Campaign-A risks (R1-R5) have been absorbed into the per-campaign registers in `campaign_b_iii_federation.md` (5 risks) and `campaign_c_airlock_standard.md` (4 risks). R1 (active lattice-labs campaigns break during wrapper migration) carried forward into Campaign B as the wikilink-sweep mitigation pattern (proven at MB-1 close).

## Learning Store Status

- Canonical entries: 26 (live at `what/context/core_domain_packs/iii_corrections_canonical.jsonl`, md5 `dde2cbd88c0b45956fb22285a2a0f856`)
- Graduated to domain packs: 5 (C-001 projective_claim_as_fact, C-002 aspiration_as_current_capability, C-005 + 2 more)
- Graduation candidates: C-003 (freq=?, acceptance=?), C-009 (freq=?, acceptance=?)
- lattice-labs operational pre-migration copy (`iii_corrections.jsonl` at `what/context/iii_domain_packs/`) **retired at MB-1 2026-05-08** (truncated to 0 bytes); lattice-labs local downstream fork now lives at `~/lattice/lattice-labs/iii/what/context/lattice_labs_iii_learning_store.jsonl` (empty; populated by future review cycles per ADR-003 §2)
- SiteForge operational pre-migration copy (`iii_corrections.jsonl` at `what/context/iii_domain_packs/`) **retired at MB-2 2026-05-10** (truncated from 19 entries / 10 561 bytes to 0 bytes); all 5 SiteForge-originated correction entries (C-012..C-016, all WGA M10/M11 review work) already exist upstream at canonical — no data loss; SiteForge local downstream fork now at `~/lattice/SiteForge.aDNA/iii/what/context/siteforge_iii_learning_store.jsonl` (empty)
- Two vestigial campaign-scoped JSONLs remain at lattice-labs (whitepaper breadcrumb 13 144 B + canvas_visual breadcrumb 659 B) — disposition deferred to Campaign B charter MB-7
- Schema reconciliation against ADR-003 §4 deferred to MB-7 (Plan-agent finding (a))

## Latest Direction — 2026-05-11 (MB-3 ✅; VideoForge `iii/` wrapper ratified; R3 closed; first inbound v0.2 cross-vault request closed end-to-end)

**MB-3 executed as a ratification audit + acceptance ceremony**, not authoring. VideoForge.aDNA (Iris) had already authored its wrapper under ADR 007 § D3 authority (M_3_R Session 2, 2026-05-11) and filed a v0.2-conformant cross-vault request memo at `who/coordination/coord_2026_05_11_videoforge_iii_wrapper_authoring.md` proposing the wrapper satisfies MB-3. Argus's job this session was to audit it, accept (or reject / revise), and flip the III.aDNA-side registers. Three release-grade deliverables landed:

1. **Audit pass (12/12 checks)**. Surfaced in the Acceptance reply at the coord memo. Key results: `federation_ref` schema ADR-002 §1 compliant (all required + key optional fields); pin commit `246124d` resolves exactly to `v0.2.0` tag (verified `git rev-parse v0.2.0^{commit}`; cleaner than MB-2 SiteForge's `04ae724` post-tag pin); packs selectivity (5/7 canonical) sound — drops `whitepaper_communication` + `canvas_visual` + `kinn_branding` with explicit justifications per coord memo §3.4; all 8 modules consumed; lattice v1.2.0; **new `kind: bridge_pack` local-extension** sanctioned by Campaign B R3 mitigation language verbatim ("MB-3 wrapper points at it via `local_extensions`" — III.aDNA STATE.md:80 cited in the bridge pack's frontmatter authority field); graduation statement preserves modality-agnostic core per ADR-002 §6 (`not_graduating_to_canonical: true`); learning store 0 bytes per ADR-003 §2 first-write expectation; mechanical `jsonschema.validate` against `iii_airlock_request_schema.yaml` returns **0 errors** (Draft 2020-12; required-minimum + all optional fields validate); canonical `iii_corrections_canonical.jsonl` md5 invariant at `dde2cbd88c0b45956fb22285a2a0f856`. Three optional observations recorded as **non-gating** carry-forwards (VideoForge `CLAUDE.md` Standing Rule routing analogue to SiteForge Order 7; VideoForge project-map listing of `iii/`; future `bridge_pack` kind documentation in ADR-002 or a new ADR — likely MB-7 disposition territory).

2. **Acceptance reply written + lifecycle traversed**. New § Acceptance section appended to the coord memo (full-profile per template `template_cross_vault_request_reply.md` because coord memo §5.4 `human_gate_required: true` + `priority: medium` mandate full profile per spec §4.2): status / audit_id (`session_stanley_20260511_iii_adna_mb3_videoforge_wrapper`) / ETA (same-session) / reserved_capacity / input_validation (jsonschema + path-existence) / secret_presence (N/A) / idempotency_check (`videoforge_iii_wrapper_mb_3_v0_2_pinned` first instance, no duplicate) / preconditions (v0.2.0 tag live; 4 wrapper files present; canonical md5 invariant; R3 mitigation language sanctions bridge_pack) / next_state. 4-row Status Log captures full lifecycle: `2026-05-11 open` (VF filing) → `2026-05-11 open → accepted` (Argus audit + audit_id assigned) → `2026-05-11 accepted → rendering → shipped` (III.aDNA-side registers flipped) → `2026-05-11 shipped → closed` (no revisions; observations recorded). Frontmatter `status: open → closed`. **This is the first inbound v0.2 cross-vault request to traverse the full lifecycle end-to-end** — exercises the MC-3 reply-comment template in production for the first time and strengthens Campaign C MC-5 validation case (which will re-exercise the VideoForge → CanvasForge outbound surface against v0.2 to flip the airlock proposal `absorbed → closed`).

3. **III.aDNA-side artifact updates** (the "rendering" phase): `MANIFEST.md` Active Consumers row 66 flipped — VideoForge `MB-3 ✅ 2026-05-11`; pin manifest with v0.2.0 commit `246124d`; 5/7 packs + 8 modules + 2 local_extensions enumerated; coord-memo provenance cited. Campaign B charter — DG-B MB-3 box ✅ with full closure paragraph; mission table MB-3 row ✅ COMPLETE 2026-05-11; **R3 risk row flipped from `MED` to `MED — MITIGATED 2026-05-11`** with materialization paragraph naming the bridge_pack's `not_graduating_to_canonical: true` field + the verbatim-cited mitigation language at `iii/CLAUDE.md:53`; Phase Plan P2 row updated `MB-2 ✅; MB-3..MB-5 pending` → `MB-2 ✅; MB-3 ✅; MB-4..MB-5 pending`; charter frontmatter gains `mb3_closed: 2026-05-11`. STATE.md (this file) — Current Phase summary includes MB-3 ✅; What's Working VideoForge bullet appended; Blockers updated; Campaign B mission queue MB-3 row flipped; frontmatter `last_session` updated.

**Boundary discipline**: Argus did **NOT** edit any file under `/Users/stanley/lattice/VideoForge.aDNA/`. The three optional observations are VideoForge's to action (their own CLAUDE.md / their own project-map / future ADR work); recorded as **observations** in the Acceptance reply, not as revision requests.

**Campaign B charter updated**: MB-3 row ✅ COMPLETE 2026-05-11; DG-B MB-3 box ✅; R3 risk row MITIGATED; Phase Plan P2 partial updated; frontmatter `updated` + new `mb3_closed`. MANIFEST.md Active Consumers VideoForge row updated to **MB-3 ✅ 2026-05-11** with full pin/pack/extension manifest.

**No git tag bump**. MB-3 touched no III.aDNA core (only registers + coord memo + session file). `v0.2.0` stays — still the right pin for newcomer consumers and the third consumer (VideoForge) now pinned against it.

### Fresh-session boot (post-MB-3)

For the next agent that opens this vault:

1. **Read this STATE.md from the top.** Current Phase + MB-3 closure note summarize where Campaign B (P2 in-flight) and Campaign C (P2 partial) stand. `v0.2.0` git tag cut and now has **three consumer wrappers** pinned against it (SiteForge MB-2, VideoForge MB-3; lattice-labs MB-1 still pinned at v0.1.0 awaiting consumer-side minor-bump review per coord memo).
2. **Confirm `how/sessions/active/` is empty**. MB-3 session moves to `how/sessions/history/2026-05/` at closeout.
3. **Choose next mission.** Two parallel-eligible queues:
   - **Campaign B** (federation): **MB-4 CanvasForge wrapper** (relocates `skill_canvas_iii_review.md` from lattice-labs to CanvasForge wrapper; MA-3 carry-forward #4; 1 sess) → MB-5 wga (0.5 sess) → MB-8 LPWhitepaper (1 sess) → MB-6 adna template (0.5 sess) → MB-7 vault hygiene (0.5 sess; closes Plan-agent findings + 4 carry-forwards + possible new ADR for `bridge_pack` `kind` registration) → DG-B.
   - **Campaign C** (airlock standard): **MC-4 Substrate enforcement** (closes Campaign C P2 — preflight script structure for `secrets_handled` per spec §4.4; idempotency cache mechanics + 30-day archive-search performance per spec §4.5; 1 sess) → MC-5 validation (re-exercise VideoForge → CanvasForge against v0.2 schema; capture deltas; flip proposal `absorbed → closed`; **MB-3 already validated the inbound surface in production** which significantly de-risks MC-5; 0.5 sess) → DG-C gate.
4. **Recommended next**: **MB-4 CanvasForge** (closes MA-3 carry-forward #4 — the only remaining MA-3 follow-up that needs an actual wrapper move; pairs naturally with MB-3's bridge_pack model precedent if CanvasForge wants a similar bridge pattern for its 19-trap canvas-specific catalog) OR **MC-4 substrate enforcement** (closes Campaign C P2 cleanly so only MC-5 validation remains before DG-C, and MB-3 already provided the inbound-validation evidence MC-5 would otherwise need to construct). Either advances a P-phase. Stanley signals.
5. **Open a session file** using the MB-3 / MB-2 / MC-3 / MB-1 pattern at `how/sessions/active/session_stanley_<YYYYMMDD>_iii_adna_<mission>_<descriptor>.md`.

## Latest Direction — 2026-05-10 (MB-2 ✅; SiteForge iii/ wrapper live; Campaign B P2 opened)

**MB-2 executed**. Three release-grade deliverables landed in this session:

1. **`~/lattice/SiteForge.aDNA/iii/CLAUDE.md`** authored — second consumer wrapper after MB-1 lattice-labs reference; first to pin against the freshly-cut `v0.2.0` tag, locking SiteForge into the cross-vault request surface from day one. `federation_ref` declares: `source_vault: III.aDNA`; `version: "0.2.0"`; `pinned_at_commit: "04ae724"` (MC-3 wind-down); `version_policy: minor`; 5 of 7 canonical packs (`inspect_procedures`, `introspect_checks`, `learning_store`, `domain_packs_web_design`, `vault_maintenance` — `whitepaper_communication` + `canvas_visual` intentionally omitted as out-of-scope, documented in § "Packs out of scope (intentionally)"); all 8 modules; lattice `v1.2.0`; 2 `local_extensions` — (1) `kind: reviewer_registry` pointing at in-place `~/lattice/SiteForge.aDNA/what/context/siteforge/siteforge_reviewers.yaml` (5-voice multi-voice orchestration: Voice Critic / Design / UX / SEO / Brand Alignment; `cross_voice_threshold: 3` escalation rule; rationale section cites `module_iii_semantic_reviewer.md` lines 56-69 as the explicit landing-site contract for MA-3 carry-forward #2), (2) `kind: learning_store_local` pointing at empty new `~/lattice/SiteForge.aDNA/iii/what/context/siteforge_iii_learning_store.jsonl`. Wrapper mirrors MB-1's structural pattern (federation_ref block + Active consumers table + Local extensions explained + Routing notes + Cross-References) with SiteForge-specific deltas.

2. **Pre-migration operational artifacts retired** at `~/lattice/SiteForge.aDNA/what/context/iii_domain_packs/`. `iii_corrections.jsonl` (19 entries / 10 561 bytes; SiteForge-side operational learning store; subset of canonical 26-entry upstream) truncated to 0 bytes — canonical md5 `dde2cbd88c0b45956fb22285a2a0f856` re-verified unchanged before and after. All 5 SiteForge-originated entries (C-012 brand_color_dual_role, C-013 nested_interactive_elements, C-014 missing_structured_data_on_generated_pages, C-015 hardcoded_entity_strings, C-016 inline_data_instead_of_collection — all WGA M10/M11 review work) already exist upstream; **no data loss**. Local stale-fork `context_iii_domain_packs_web_design.md` replaced with `[MIGRATED]` forward-pointer stub — diff against canonical confirmed zero substantive trap-content drift (only stale `updated: 2026-04-04` frontmatter and old `iii_domain_packs/` wikilinks). Sibling `MIGRATION_NOTE.md` authored documenting Stage 3 closure (mirrors lattice-labs MB-1 pattern).

3. **SiteForge governance updates** — `SiteForge.aDNA/CLAUDE.md` gained **Standing Order 7** (III review via wrapper; mirrors lattice-labs Rule 12 / CanvasForge Rule 11); project-map line 63 annotated (`iii_domain_packs/` flagged `[MIGRATED] Stage 3`; new `iii/` directory entry added); Context-Loading Quality line 95 repointed at wrapper. Two SiteForge lattice yamls updated — `~/lattice/SiteForge.aDNA/what/lattices/lattice_partner_website_scaffold.lattice.yaml:163` + `~/lattice/SiteForge.aDNA/what/lattices/skills/lattice_quality_validation.lattice.yaml:57` — `corrections_store:` paths rerouted from pre-migration path to wrapper-managed local store. Wikilink sweep clean — all `iii_corrections` / `iii_domain_packs` / `context_iii_domain_packs_web_design` hits resolved (design-doc references at `sf_m14_context_graft_design.md` lines 97, 185, 187, 188, 235, 236, 244 + `sf_m14_learning_loop_design.md` lines 24, 25, 39, 48, 75, 87 survive stub redirect — flagged for Campaign B MB-7 disposition per the new `MIGRATION_NOTE.md`).

**Campaign B charter updated**: MB-2 row ✅ COMPLETE 2026-05-10; DG-B MB-2 box ✅; Phase Plan P2 row marked partial (MB-2 ✅; MB-3..MB-5 pending); campaign phase frontmatter `phase: P1 → P2`. MANIFEST.md Active Consumers SiteForge row updated to **MB-2 ✅ 2026-05-10** with `v0.2.0` (commit `04ae724`) pin.

**No git tag bump**. MB-2 touches no III.aDNA core (only STATE.md + Campaign B charter + MANIFEST.md + session file in this repo; wrapper itself lives in SiteForge.aDNA repo). `v0.2.0` stays — still the right pin for newcomer consumers.

### Fresh-session boot (post-MB-2)

For the next agent that opens this vault:

1. **Read this STATE.md from the top.** Current Phase + MB-2 closure note summarize where Campaign B (P2 in-flight) and Campaign C (P2 partial) stand. `v0.2.0` git tag is cut and now has a second consumer pinned against it.
2. **Confirm `how/sessions/active/` is empty**. MB-2 session moves to `how/sessions/history/2026-05/` at closeout.
3. **Choose next mission.** Two parallel-eligible queues:
   - **Campaign B** (federation): **MB-3 VideoForge wrapper** (resolves ADR-006 bridge contract; R3 risk closes; 1 sess) → MB-4 CanvasForge → MB-5 wga → MB-8 LPWhitepaper → MB-6 adna template → MB-7 vault hygiene → DG-B. OR **MB-4 CanvasForge wrapper** (relocates `skill_canvas_iii_review.md` from lattice-labs to CanvasForge wrapper; MA-3 carry-forward #4; 1 sess).
   - **Campaign C** (airlock standard): **MC-4 Substrate enforcement** (closes Campaign C P2 — preflight script structure for `secrets_handled` per spec §4.4; idempotency cache mechanics + 30-day archive-search performance per spec §4.5; 1 sess) → MC-5 validation → DG-C gate.
4. **Recommended next**: **MB-3 VideoForge** (closes the largest open R3 risk in Campaign B and resolves ADR-006 bridge ambiguity; pairs naturally with MB-2's v0.2 pin so VideoForge can act as a cross-vault request originator immediately) OR **MC-4** (closes Campaign C P2 cleanly so only MC-5 validation remains before DG-C). Either advances a P-phase. Stanley signals.
5. **Open a session file** using the MB-2 / MC-3 / MB-1 pattern at `how/sessions/active/session_stanley_<YYYYMMDD>_iii_adna_<mission>_<descriptor>.md`.

## Latest Direction — 2026-05-10 (MC-3 ✅; AIRLOCK.md v0.2.0 + `v0.2.0` git tag)

**MC-3 executed**. Three release-grade deliverables landed in this session:

1. **`how/airlock/AIRLOCK.md` bumped v0.1.0 → v0.2.0**. Frontmatter rewritten: `version: "0.2.0"`, `covers: [entry_paths, cross_vault_request_patterns]`, `governed_by: what/artifacts/iii_airlock_standard_spec.md`, `absorbs_proposal: who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md`. Body restructured: new Surface Selection table sits above Path Selection (Entry vs Request vs out-of-scope federation); § Entry Paths preserves the 5 v0.1.0 entry paths byte-identical against the `v0.1.0` tag (Profile / Use cases / Out of scope / Expected output / Context budget / Load in order blocks reproduced verbatim — zero text drift, per spec §3.3 reference-instance contract); new § Cross-Vault Requests is operational pointers (1–3 sentences per subsection: what / where / required-minimum payload / 8-state lifecycle / lightweight vs full handshake profiles / secrets / idempotency / reply-comment template / worked example); § Version Contract updated for v0.2.0 minor-bump semantics (additive surfaces, no v0.1 path removed); "What v0.2.0 does NOT cover" replaces the v0.1.0 anti-regression block — lists v0.3+ deferrals from spec §7.2 (`proposed/` channel; multi-vault transactional; async batched; executable substrate enforcement).

2. **Reply-comment template authored** at `how/templates/template_cross_vault_request_reply.md`. Three sections: Acceptance reply (full-profile fenced markdown block carrying status / audit_id / eta / reserved_capacity / input validation / secret presence names-only / idempotency check / preconditions / next state — exactly spec §4.2 field requirements); Acceptance under lightweight-profile (descriptive note; no fenced block; explains why the section is empty under that profile and when upgrading to full is mandatory); Rejection reply (fenced block with 7-value reason enum: validation_failed | missing_secret: <NAME> | scope_mismatch | lattice_incompatible | budget_ceiling_exceeded | over_capacity_no_queue | other). Status Log row update guidance follows.

3. **Spec patch edits (no version bump)**: §7.2 row 6 "Reply-comment template for handshake | MC-3" flipped status to ✅ 2026-05-10 with body pointer to canonical template path; §7.3 forward-reference list expanded from "three originally cited / one resolved / two pending" to "four originally cited / three resolved (MC-2, MC-3 template, MC-3 AIRLOCK.md bump) / two pending (MC-4 × 2)"; §8.4 reference-instance row updated from "v0.1.0 ships; v0.2.0 bump at MC-3" to "v0.2.0 — bumped at MC-3 2026-05-10". Spec frontmatter `version: "0.2.0"` unchanged (patch-level per §6.1).

**VideoForge proposal flipped `accepted → absorbed`** at `who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md`. Frontmatter status + updated date; Status Log row 3 ("(pending) | absorbed") populated with today's date and full closure paragraph naming AIRLOCK.md v0.2.0 ship, template canonical path, spec forward-reference resolution, and tag-cut. Row 4 ("(pending) | closed") still pending — reserved for MC-5 validation re-exercise.

**Campaign C charter updated**: DG-C MC-3 box ✅ + tag-cut box ✅ + proposal-status box ✅ ("`closed` pending MC-5"); mission table MC-3 row ✅ COMPLETE 2026-05-10; Phase Plan P2 row marked partial ("MC-3 ✅; MC-4 pending"); P3 row noted "`v0.2.0` git tag already cut at MC-3 close"; Promises section Promise 2 (coord-memo fallback) flipped RETIRED ("how/airlock/AIRLOCK.md v0.2.0 routes cross-vault requests via § Cross-Vault Requests; the spec §4 contract is now operational"); Promise 3 status appended `accepted → absorbed at MC-3`.

**Annotated `v0.2.0` git tag cut** at MC-3 closure commit (per Campaign C charter Critical Path + STATE.md post-MC-2 boot recommendation). First cross-vault-request-capable III.aDNA release. No remote push yet — Stanley pushes when ready (the v0.1.0 release went to `LatticeProtocol/III.aDNA`; v0.2.0 will follow the same path).

### Fresh-session boot (post-MC-3)

For the next agent that opens this vault:

1. **Read this STATE.md from the top.** Current Phase + MC-3 closure note summarize where Campaigns B and C stand. Campaign C P2 is partial (MC-3 ✅; MC-4 pending). `v0.2.0` git tag is cut.
2. **Confirm `how/sessions/active/` is empty**. MC-3 session moves to `how/sessions/history/2026-05/` at closeout.
3. **Choose next mission.** Two parallel-eligible queues:
   - **Campaign B** (federation): **MB-2 SiteForge wrapper** (largest architectural value — multi-voice orchestration absorption from MA-3 carry-forward #2). MB-2 pins federation against `v0.2.0` (the freshly cut tag) rather than `v0.1.0`, locking SiteForge into the cross-vault request surface from day one. → MB-4 CanvasForge → MB-3 VideoForge → MB-5 wga → MB-8 LPWhitepaper → MB-6 adna template → MB-7 vault hygiene → DG-B.
   - **Campaign C** (airlock standard): **MC-4 Substrate enforcement** (closes Campaign C P2 — preflight script structure for `secrets_handled` per spec §4.4; idempotency cache mechanics + 30-day archive-search performance per spec §4.5; 1 sess) → MC-5 validation (re-exercise VideoForge → CanvasForge against v0.2 schema; capture deltas; flip proposal `absorbed → closed`; 0.5 sess) → DG-C gate.
4. **Recommended next**: **MB-2** (largest architectural value in flight; pins consumers against fresh `v0.2.0`) OR **MC-4** (closes Campaign C P2 cleanly so only MC-5 validation remains before DG-C). Either advances a P-phase. Stanley signals.
5. **Open a session file** using the MC-3 / MC-2 / MC-1 / MB-1 pattern at `how/sessions/active/session_stanley_<YYYYMMDD>_iii_adna_<mission>_<descriptor>.md`.

## Latest Direction — 2026-05-08 (MC-2 ✅; Campaign C P1 closed; P2 open)

**MC-2 executed**. `what/artifacts/iii_airlock_request_schema.yaml` authored as JSON Schema Draft 2020-12 expressed in YAML form (per spec §4.3 + §7.2 "validator-friendly YAML" language). Top-level `required: [type, artifact_request]` + nested `artifact_request.required: [spec_path, output_sink]` enforce required-minimum exactly per spec §4.3. Open at root + `artifact_request` (`additionalProperties: true`) for consumer-extension friendliness; closed on substrate-critical sub-objects (`constraints`, `secrets_handled` use `additionalProperties: false`). Five schema extensions (`x-body-sections`, `x-lifecycle-transitions`, `x-handshake-profiles`, `x-required-minimum`, `x-worked-example`) capture markdown-body shape, valid state transitions, profile selection rules, smallest-valid-memo example, and worked-example conformance note.

**Mechanical validation** — 9/9 checks pass:
1. `Draft202012Validator.check_schema` clean — schema is valid JSON Schema 2020-12.
2. Required-minimum memo (`type` + `artifact_request.spec_path` + `artifact_request.output_sink`) validates.
3. Negative: missing top-level `type` rejected.
4. Negative: missing `artifact_request.spec_path` rejected.
5. Negative: invalid `status` enum rejected.
6. Full conformant memo with all optional blocks (`constraints`, `secrets_handled`, `idempotency_key`, `forbidden_registers`, etc.) validates.
7. Novel `artifact_request.type` value (`notebook`) accepted (open string by design; documented examples include the 6 known kinds).
8. Closed-shape typo (`constraints.max_revsions` instead of `max_revisions`) rejected by `additionalProperties: false`.
9. Secret name pattern enforced (UPPER_SNAKE_CASE only in `secrets_handled.needed`).

**Worked-example walk** — CanvasForge memo at `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` correctly fails validation with two non-conformances:
1. Top-level `type: coordination` — should be `cross_vault_request` for v0.2 conformance.
2. `spec_path`/`output_sink` body-promoted (under § "What CanvasForge Receives" body sections) — should be nested under frontmatter `artifact_request` object.

Both gaps expected per spec §5.2 "additive-deltas" clause (worked example pre-dates v0.2). Schema header documents both gaps; the worked example is evidence of *shape* and not a v0.2 conformance test case. Any new v0.2-era memo following spec §4.3 frontmatter passes the schema (proven by check #6).

**Spec forward-references resolved** — patch-level edits, no spec version bump (v0.2.0 stays):
- Frontmatter `defers_schema_to: ... # MC-2 (pending)` → `# authored MC-2 2026-05-08`
- §4.3 prose "(authored at MC-2; pending)" → "(authored at MC-2 2026-05-08; JSON Schema Draft 2020-12 expressed in YAML form)"
- §7.2 row 7 status flipped to "MC-2 ✅ 2026-05-08"
- §7.3 forward-reference list updated: "Three sections originally cited deliverables that did not yet exist; one is now resolved (MC-2), two remain pending (MC-4)"

**Campaign C charter updated**: MC-2 mission row + DG-C MC-2 box flipped ✅; P1 phase row in Phase Plan flipped ✅ COMPLETE; charter frontmatter `phase: P1 → P2`.

**No git tag bump**. v0.1.0 stays. Annotated `v0.2.0` tag still waits for MC-3 close (per charter Critical Path) — that's where AIRLOCK.md text actually shifts to v0.2.

### Fresh-session boot (post-MC-2) — superseded by post-MC-3 boot block above

(Original post-MC-2 boot guidance retained as historical reference; the post-MC-3 boot block is the current authority.)

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

### Fresh-session boot (post-MC-1) — superseded by post-MC-2 boot block above

(Original post-MC-1 boot guidance retained as historical reference; the post-MC-2 boot block is the current authority.)

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
