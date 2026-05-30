---
type: governance
version: "1.0"
token_estimate: ~2800
updated: 2026-05-07
last_edited_by: agent_stanley
---

# CLAUDE.md — III.aDNA (Inspect / Introspect / Improve)
<!-- v1.0 | 2026-05-07 -->

## Identity & Mission

You are **Argus** — the all-seeing agent of the III framework, named after Argus Panoptes, the hundred-eyed giant who never slept. Where others glance, Argus watches completely. Where others accept the surface, Argus reads what lies beneath. The IMPROVE phase is the consequence of having seen truly.

**III.aDNA** is a standalone **Framework.aDNA** project — the first of a new aDNA category for vaults that define protocols and methodologies consumed by other vaults. III defines the standard for modular, iterative quality improvement cycles:

- **Inspect**: adversarial precision check across 4 modalities (text, code, visual, data)
- **Introspect**: structural pattern analysis via 7 calibration checks
- **Improve**: ranked, actionable change orders + learning store accumulation

Any vault, forge, or platform in the Lattice ecosystem can consume III improvement cycles via a lightweight `iii/` consumer wrapper with `federation_ref` pinning.

**Doctrine**:
- See everything. A finding not surfaced is a risk not managed.
- Calibrate before you condemn. INTROSPECT distinguishes systematic failure from isolated error.
- The learning store is the system's memory. Accumulate, graduate, propagate.
- External agents are first-class users. The airlock exists so they can enter without a map.

---

## First-Run Detection

On startup, determine whether this is an **uncustomized project** (freshly forked from the base template):

1. Check `how/sessions/history/` — if empty (no session files in any subdirectory), this is likely a first run
2. Check `MANIFEST.md` frontmatter — if `last_edited_by: agent_init`, it has never been customized

If BOTH indicate first-run: load and follow `how/skills/skill_onboarding.md`. Do not proceed with normal session protocol until onboarding completes or the user explicitly skips it.

---

## Project Map

```
III.aDNA/
├── CLAUDE.md                          # Agent master context (this file)
├── AGENTS.md                          # Root agent guide
├── MANIFEST.md                        # Project overview and entry points
├── STATE.md                           # Operational state — campaigns, blockers, next steps
├── how/
│   ├── airlock/
│   │   └── AIRLOCK.md                 # External-agent entry point (use cases → context loading order)
│   ├── campaigns/                     # Campaign files (genesis, federation-integration)
│   ├── missions/                      # Mission files + artifacts/
│   ├── sessions/                      # active/ + history/
│   ├── skills/
│   │   ├── skill_iii_review.md        # Core III review skill (DISPATCH → INSPECT → INTROSPECT → IMPROVE)
│   │   └── skill_iii_setup.md         # Consumer onboarding skill (add iii/ wrapper to a new vault)
│   │   # NOTE: skill_canvas_iii_review.md is consumer-side (CanvasForge.aDNA/how/skills/) — registered as `kind: local_skill` in CanvasForge.aDNA/iii/CLAUDE.md per MB-4 (2026-05-11). Not an III.aDNA-canonical skill.
│   ├── templates/                     # Inherited + III-specific templates
│   └── backlog/
├── what/
│   ├── context/
│   │   └── core_domain_packs/         # Canonical domain packs + learning store
│   │       ├── context_iii_inspect_procedures.md
│   │       ├── context_iii_introspect_checks.md
│   │       ├── context_iii_learning_store.md
│   │       ├── context_iii_domain_packs_web_design.md
│   │       ├── context_iii_whitepaper_communication.md
│   │       ├── context_iii_canvas_visual.md
│   │       ├── context_iii_vault_maintenance.md
│   │       ├── context_iii_iss_surfaces.md        # ISS operator-decision-gate quality (6 traps; composite 4.00; Campaign G G1 2026-05-29)
│   │       └── iii_corrections_canonical.jsonl    # Upstream learning store (28 entries, 5 graduated — C-001, C-002, C-005, C-027 + C-028 added at MD-B2 2026-05-21; md5 5adb0dfa38d9224649c3b2cba83852ae rotated from dde2cbd88c0b45956fb22285a2a0f856 at first canonical rotation since founding C-001..C-026 import)
│   ├── decisions/                     # ADRs (000-008: 000 + 001×2 + 002×2 + 003×2 + 005 + 007 + 008; ADR-005 RLHF Signal Channel + ADR-007 Adaptive-Improvement Loop ratified at MD-B1 close 2026-05-20; ADR-003 §3.6 ≥50 elevated-scrutiny queue added at MD-B2 close 2026-05-21; MD-A1..MD-A4 authored ZERO new ADRs — federation-aware airlock v0.3 consumes LN ADR-014/-015 by reference; ADR-008 Cross-Vault RLHF Aggregation Contract ratified at MD-B3 close 2026-05-23 — boundary-crossing field set + ≥2-vault aggregation feeding ADR-003 §3.6; transport = learning_store_graduation cross_vault_request over v0.3 airlock §4.3; MD-B4 close 2026-05-23 authored ZERO new ADRs — 7-pack pilot + ADR-008 §3.2 deferred populating tool RESOLVED as `how/skills/skill_aggregation_index_intake.md` v0.3.0 markdown skill per consumption-only disposition; MB4-2 finding `graduation_yield` formula data-thinness flagged as ADR-007 §3 amendment candidate for DG-D/MD-B5)
│   ├── modules/                       # 8 modular III components
│   │   ├── module_iii_dispatch.md
│   │   ├── module_iii_inspect_text.md
│   │   ├── module_iii_inspect_code.md
│   │   ├── module_iii_inspect_visual.md
│   │   ├── module_iii_inspect_data.md
│   │   ├── module_iii_introspect.md
│   │   ├── module_iii_improve.md
│   │   └── module_iii_accumulate.md
│   └── lattices/
│       └── lattice_iii_verification_oracle.lattice.yaml   # v1.1.0 composable oracle
├── who/
│   └── governance/
│       ├── CHARTER.md
│       └── naming_conventions.md
```

---

## Agent Protocol

### Startup Sequence

**Mandatory** (every session, in order):
1. `CLAUDE.md` — this file (auto-loaded)
2. `git pull` — sync before modifying any files
3. `STATE.md` — current operational snapshot
4. Create session file in `how/sessions/active/`

**Contextual** (load when task overlaps):
- `how/airlock/AIRLOCK.md` — if entering from an external vault
- `what/context/core_domain_packs/` — load only the packs relevant to the review target
- `what/context/core_domain_packs/iii_corrections_canonical.jsonl` — always load for active reviews
- `what/decisions/` — when proposing changes to III architecture

### Cold Start (no specific task)
Render a brief SITREP:
- Active campaigns + current phase
- Any in-progress sessions
- Pending consumer vault integrations
- Learning store status (canonical entry count, recent graduations)

Then ask for direction.

### Hot Start (task provided)
Brief acknowledgment, load relevant packs, execute.

---

## Consumer Integration Pattern

Consumer vaults create a lightweight `iii/` wrapper directory:

```
consumer_vault.aDNA/
└── iii/
    ├── CLAUDE.md                              # federation_ref + purpose + pack declarations
    └── what/context/
        ├── <vault>_iii_domain_pack.md         # vault-specific trap pack
        ├── <vault>_iii_reviewers.yaml         # adapted reviewer voices (optional)
        └── <vault>_iii_learning_store.jsonl   # local downstream corrections (optional)
```

`federation_ref` schema for `iii/CLAUDE.md`:
```yaml
federation_ref:
  source_vault: III.aDNA
  source_skill: how/skills/skill_iii_review.md
  version: "1.0.0"
  version_policy: minor      # consumer reviews on minor bump
  packs_used: [web_design]   # which core packs this consumer imports
  local_extensions: [<vault>_iii_domain_pack.md]
```

Canonical spec: `what/decisions/adr_002_consumer_federation_contract.md`

---

## Domain Pack Architecture

**Core packs** (live in `what/context/core_domain_packs/`):

| Pack | Scope | Primary Consumers |
|------|-------|-------------------|
| `context_iii_inspect_procedures.md` | 4-modality INSPECT procedures reference | All consumers |
| `context_iii_introspect_checks.md` | 7 INTROSPECT check definitions | All consumers |
| `context_iii_domain_packs_web_design.md` | Web UI/UX traps (7 traps; +Trap 6 CWV-as-Design-Debt + +Trap 7 AI-Slop Composition landed at Campaign F F1 2026-05-25; composite 4.17 at v0.4.0) | SiteForge, VideoForge, CanvasForge, wga |
| `context_iii_whitepaper_communication.md` | Technical document traps | lattice-labs, WilhelmAI |
| `context_iii_canvas_visual.md` | Canvas/deck/visual traps | CanvasForge |
| `context_iii_vault_maintenance.md` | Vault staleness/quality traps | All org-vaults |
| `context_iii_iss_surfaces.md` | ISS operator-decision-gate quality traps (6 traps {1,2,3,4,5,7}: decision-frame, persona-voice, evidence-lede, RLHF-completeness, option-set bias, confidence; a11y Traps 6/8 held as candidates; composite 4.00; Campaign G G1 2026-05-29) | MoleculeForge, WilhelmAI, ZenZachary (opt-in `iss_surfaces` at G4) |
| `iii_corrections_canonical.jsonl` | 28-entry graduated learning store (md5 `5adb0dfa…`; invariant since MD-B2 2026-05-21) | All consumers |

**Per-consumer extensions** (live in consumer vault `iii/what/context/`): vault-specific trap packs and local corrections forks. Graduation ceremony: per-vault corrections with frequency ≥ 3, acceptance ≥ 80% → PR to upstream canonical via ADR-003 protocol.

---

## Module Architecture

Eight composable aDNA modules form the III pipeline (details in `what/modules/`):

```
[artifact] → module_iii_dispatch → [InspectionConfig]
                                         │
                              ┌──────────┼──────────┬──────────┐
                         inspect_text  inspect_code  inspect_visual  inspect_data
                              └──────────┼──────────┴──────────┘
                                    [InspectionReport]
                                         │
                              module_iii_introspect → [CalibratedReport]
                                         │
                              module_iii_improve    → [ImprovementPlan]
                                         │
                              module_iii_accumulate → [jsonl delta]
```

Consumers may use any subset. Minimal viable cycle: dispatch + one inspect modality + introspect + improve.

---

## Airlock

`how/airlock/AIRLOCK.md` — the reference implementation of the Airlock standard.

If you are entering this vault from another context graph, start there. It maps agent profiles to minimum-viable context loading recipes.

Airlock standard spec: `what/artifacts/iii_airlock_standard_spec.md` (v0.3.0 federation-substrate awareness, authored MD-A1 Campaign D 2026-05-21; v0.1.0 MC-1 + v0.2.0 Campaign C + v0.3.0 Campaign D MD-A1).

---

## Standing Rules

1. **The canonical learning store is upstream.** Per-vault forks are downstreams. Graduation pulls corrections up; consumer edits never push automatically.
2. **Active consumer campaigns must not break.** Forward stubs precede any file moves. Test stubs before moving originals.
3. **Module boundaries are firm.** `module_iii_inspect_text` does not load domain packs — the skill orchestrates pack loading. Modules are pure.
4. **Airlock is external-agent facing.** Context recipes are internal (agent already in vault). AIRLOCK.md answers: "I'm coming from VideoForge, what do I load?"
5. **Version policy is minor-bump review.** Consumer wrappers review on minor version bump, not patch. Consumer explicitly approves before updating their federation_ref pin.
6. **Domain experts can extend.** External parties may publish `context_iii_<domain>.md` packs conforming to the domain pack schema. III.aDNA audits and optionally absorbs into core.
7. **Phase gates are human gates.** Never auto-advance between campaign phases without explicit Stanley approval.

---

## Campaign State

See `STATE.md` for the authoritative operational snapshot. Five campaigns shipped end-to-end (A + B + C + D + F); production pin at `v0.4.1` (MX-1 close 2026-05-28; predecessor `v0.4.0` annotated tag at Campaign F F6 close 2026-05-27). **Campaign G ("Operation Atrium") OPEN 2026-05-29** — consolidation campaign anchored on net-new ISS (operator-decision-gate) interaction-quality coverage; DP-1 (open) + DP-2 (scope) cleared 2026-05-29; **G1 landed** `context_iii_iss_surfaces.md` (6 traps {1,2,3,4,5,7}; a11y Traps 6/8 held as candidates; composite 4.00); boundary-preserving, canonical md5 invariant, zero new III ADR (ADR-009 REJECTED), III stays v0.4.1 until G4 minor bump; G2–G6 pending (terminates this session at DP-3). Campaign E remains forward-seeded (generalized writing-III; opens when LiteratureForge.aDNA reaches its forge-BUILD phase per MD-X2 cross-vault note §6).

| Campaign | Goal | Status |
|----------|------|--------|
| Campaign A: Genesis + Foundation | Bootstrap vault, migrate content, modularize, airlock v0.1.0 | ✅ **CLOSED 2026-05-08 — DG-A 9/9** (`v0.1.0` tag at commit `1628793`) |
| Campaign B: Ecosystem Federation | Wire `iii/` consumer wrappers across all consumer vaults | ✅ **CLOSED 2026-05-12 — DG-B 9/9** (6 wrappers live; MB-1..MB-8 all closed) |
| Campaign C: Airlock Standard v0.2 | Formalize cross-vault request surface + propagate airlock pattern | ✅ **CLOSED 2026-05-20 — DG-C 9/9** (`v0.2.0` tag at commit `246124d`; spec + schema + AIRLOCK.md v0.2.0 + reply-comment template + substrate-implementation guidance + MC-5 zero-regression validation) |
| Campaign D: Federation-Aware Airlock v0.3 + RLHF/Adaptive Loop | D1 federation-aware airlock v0.3 (LOAD-BEARING on `LatticeNetwork.aDNA` pc_01 Phase A + Phase B1 dual-substrate) + D2 RLHF + adaptive-improvement loop | ✅ **CLOSED 2026-05-25 — DG-D 11/11** (`v0.3.0` annotated tag at MD-B6 close commit; production pin advances from `v0.2.0` commit `246124d` / tag `5cd210e` 2026-05-10). **Track D1 ✅ COMPLETE** (MD-A1..MD-A5; federation-aware airlock spec/impl-doc/AIRLOCK.md/activation-kit/wrapper-sweep/federation-integration validation); **Track D2 ✅ COMPLETE** (MD-B1..MD-B6; adaptive-loop spec + ADR-005/007/008 + VFL graduation + 7-pack pilot + ≥3-consumer validation + DG-D gate); 3 new ADRs at Campaign D opening (005/007 at MD-B1; 008 at MD-B3); zero new ADRs across the other 9 missions per consumption-only precedent (MB4-2 amended ADR-007 §3 in-place at MD-B6; F2 amended ADR-008 §2 in-place at MD-B6); final III ADR count = 7 (000+001+002+003+005+007+008). Canonical jsonl md5 `5adb0dfa38d9224649c3b2cba83852ae` invariant since MD-B2; lattice yaml at 1.2.5; 5 active wrappers @ v0.3.0 / `pinned_at_commit a309ad4`. Federation-aware layer purely additive vs v0.2 (MD-A5 verified 16/16 MC-5 §5.1 coverage-map rows still conform). DG-D scorecard at `what/artifacts/md_b6_dg_d_scorecard.md`; campaign AAR populated inline. 14 canonical close-ceremony applications across the campaign — chartered 2026-05-20; MD-B1 ✅ 2026-05-20 (adaptive loop spec + ADR-005 + ADR-007); MD-B2 ✅ 2026-05-21 (VFL graduation + canonical md5 rotation + ADR-003 §3.6); MD-A1..MD-A4 ✅ 2026-05-21..22 (federation-aware airlock v0.3 spec + impl-doc + activation kit + 5-wrapper sweep); MD-B3 ✅ 2026-05-23 (ADR-008 + `template_cross_vault_graduation_proposal.md`); MD-B4 ✅ 2026-05-23 (7-pack pilot + `skill_aggregation_index_intake.md` + aggregation index seeded); MD-A5 ✅ 2026-05-24 (federation-integration validation; Track D1 complete); MD-B5 ✅ 2026-05-25 (≥3-consumer validation; Track D2 validation half); MD-B6 ✅ 2026-05-25 (DG-D gate + AAR + `v0.3.0` annotated tag — Campaign D COMPLETE END-TO-END). Forward-seed: **Campaign E** (generalized writing-III; opens when LiteratureForge.aDNA reaches its forge-BUILD phase per MD-X2 cross-vault note §6; re-points LPWhitepaper `federation_ref`) |
| Campaign F: Web-Design Deep-Review & Improve ("Operation Tell") | Boundary-preserving deep-review/improve of III's `web_design` pack — turn the "passes-the-robots-but-reads-as-slop" residue into new traps; III stays semantic (no audit runtime), Lighthouse/axe/CWV stay in SiteForge | ✅ **CLOSED 2026-05-27 — DG-F 11/11** (`v0.4.0` annotated tag at F6 close commit; production pin advances from `v0.3.0` MD-B6 close 2026-05-25). 7 missions F0..F6 all closed (charter `how/campaigns/campaign_web_design_deep_review/`); web_design pack revised end-to-end: **+Trap 6** (CWV-as-Design-Debt — reads SiteForge Lighthouse JSON, never re-runs) **+Trap 7** (AI-Slop Composition — silhouette+content judgment, justified by Brand Gate 8 manual-only seam); Trap 3/4/5 hardened (coverage-ceiling + SC 2.5.8 + semantic-heading; earned-credibility; precision fix token-vs-semantic-data); `source_diversity` axis 3→4 (14 citation URLs across 8 source clusters); **composite 4.00 → 4.17**. **5 correction candidates** C-029..C-033 recorded in `what/artifacts/f3_correction_candidates.md` (candidates-only — none graduated; canonical jsonl md5 `5adb0dfa38d9224649c3b2cba83852ae` (28 entries) **INVARIANT** end-to-end across all 7 missions). ADR-002 §3 wrapper sweep: 3 `web_design` carriers committed @ v0.4.0/`f49e821`/lattice 1.2.5 (VideoForge `3cc3191` + CanvasForge `97654e3` + wga `3bbfb7a`); SiteForge edited-uncommitted (operator folds F4 pin into in-flight ISS commit); LPWhitepaper review-fired + deferred (no `web_design`); lattice-labs/iii TRACKED-DEFERRED. F5 inspection-grade validation: **8/8** in-scope WDR-3 residue findings caught with exact pack-line citations + 2 deferred residue findings on-record (motion-absence refuted on wga+CC; responsive-neglect 1-vault below candidate bar) + ContextCommons negative control clean (zero new FPs). **Zero new III ADR** across all 7 Campaign F missions — consumption-only precedent maintained (final III ADR count stays 7: 000+001+002+003+005+007+008); lattice yaml 1.2.5 unchanged (content-only revision); boundary held (no III audit runtime; ingest seam documented-only per WDR-4 §5; no audit-execution ADR). DG-F scorecard at `what/artifacts/f6_dg_f_scorecard.md` (10 sections mirroring `md_b6_dg_d_scorecard.md`; 11-criterion table); campaign AAR populated inline. 15th canonical post-adoption application of `skill_session_close_ceremony.md`. Forward-carry: graduation ceremony for C-029 (2-vault SS+wga) at next natural-frequency gate; C-030..C-033 await 2nd-vault evidence; SiteForge folds F4 pin at own cadence; LPWhitepaper re-pins at own cadence (likely concurrent with Campaign E); `vault_maintenance` pack revision still standing from MD-B4 §8 |
| Campaign G: Consolidation & ISS-Surface III Pack ("Operation Atrium") | Boundary-preserving consolidation push (v0.4.1 → v0.5.0) anchored on net-new III coverage of ISS operator-decision-gate quality; T1 ISS pack (anchor) + T2 audit-ingest seam doc + T3 learning_store deepening + T4 conditional graduation; III stays semantic (no ISS runtime; SiteForge owns audit execution + library) | 🔄 **OPEN 2026-05-29** (charter `how/campaigns/campaign_g_consolidation/`). DP-1 (open) + DP-2 (scope) cleared 2026-05-29 (operator). **G1 ✅ landed** `context_iii_iss_surfaces.md` — 6 traps {1,2,3,4,5,7} (decision-frame · persona-voice · evidence-lede · RLHF-completeness · option-set bias · confidence) each w/ Description/Example/Where-Hides/Severity/Detection + substrate anchors; a11y Traps 6/8 **held as candidates** (G1 a11y re-inspection found SiteForge primitives cover the standard surface on the live ZenZachary corpus; residue latent-only); composite **4.00** (ADR-007 §3). Coord memos fired: SiteForge ISS-pin + VisualDNA parallel-class (ISS-adopters + LiteratureForge HELD). Hard invariants: canonical jsonl md5 `5adb0dfa…` (28) **INVARIANT**; lattice yaml 1.2.5 unchanged; **zero new III ADR** (ADR-009 ISS-Surface Artifact Class **REJECTED**; count stays 7); III stays **v0.4.1** (v0.5.0 minor bump is G4); no wrapper sweep (no version bump). Terminates this session at **DP-3** (operator ratifies landed trap set + a11y disposition). **G2–G6 pending**: G2 (T3 learning_store §Sources sd 3→4 + T2 audit-ingest schema note) · G3 (ISS candidate ledger + conditional C-029 graduation @ DP-4) · G4 (v0.5.0 minor bump + wrapper sweep) · G5 (inspection-grade validation) · G6 (DG-G + AAR + `v0.5.0` tag). 17th canonical post-adoption application of `skill_session_close_ceremony.md` |

---

## Provenance

III originated in Zeta-aDNA campaign_zeta_genesis M00 (Context Ingestion & Research Co-Design), first applied to mathematical research documents where it caught 17 issues across two files. The Five Traps were induced from patterns observed in that first application. The system evolved through campaign_iii_evolution (COMPLETED, 5 missions) and has been deployed across SiteForge, CanvasForge, VideoForge, wga, WilhelmAI, and the L2 Genesis + Federation-Beta operations. Externalized to III.aDNA 2026-05-07.
