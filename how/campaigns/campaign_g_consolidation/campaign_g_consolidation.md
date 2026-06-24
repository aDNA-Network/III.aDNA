---
campaign_id: campaign_g_consolidation
type: campaign
title: "Campaign G — Consolidation & ISS-Surface III Pack (Operation Atrium)"
codename: "Operation Atrium"
owner: stanley
status: completed        # OPENED at DP-1 2026-05-29; G1-G4 closed 2026-05-30/31; G5 validation GO 2026-06-23; G6 DG-G 11/11 CLOSE 2026-06-23 — annotated v0.5.0 tag cut at close commit
phase: G6
dg_g_closed: 2026-06-23
dg_g_outcome: GO
release_tag: "v0.5.0"
mission_count: 7         # G0..G6
estimated_sessions: 7
estimation_class: calibrated_against_campaign_f
priority: high
created: 2026-05-29
updated: 2026-06-23       # G6 DG-G 11/11 CLOSE — G5 validation GO (6/6 recommended residue axes caught across 3/3 live adopters; negative control clean) + DG-G scorecard + AAR inline + annotated v0.5.0 tag at close commit; canonical jsonl md5 INVARIANT (5adb0dfa…/28); lattice 1.2.6; zero new III ADR (count stays 7)
last_edited_by: agent_argus
authored_by_mission: plan_campaign_g_consolidation_charter
predecessor: campaign_web_design_deep_review   # Campaign F (Operation Tell); v0.4.0
boundary_posture: boundary_preserving   # III stays semantic; SiteForge owns ISS runtime + audit execution; III packs the contract surface + reads audit signals only
graduation_posture: conditional         # T4 fires C-029 graduation ONLY if a consumer-vault proposal arrives by G3/DP-4; campaign NOT block-closed on graduation
version_target: "0.5.0"                  # clean minor bump (NOT v1.0.0 — preserves R3 documentation-grade posture)
adr_posture: zero_new_iii_adr            # ADR-009 ISS-Surface Artifact Class pre-disposed REJECTED; final III ADR count stays 7
tags: [campaign, campaign_g, operation_atrium, consolidation, iss_surfaces, audit_ingest_seam, learning_store_deepening, graduation_conditional, v0_5_0, boundary_preserving, zero_new_iii_adr, draft]
---

# Campaign G — Consolidation & ISS-Surface III Pack ("Operation Atrium")

> **DRAFT charter** authored at `plan_campaign_g_consolidation_charter` execution (2026-05-29). Opens to `status: active` only at the **DP-1 operator gate**. Models on the Campaign F charter (`campaign_web_design_deep_review.md`), which extended the Campaign D skeleton.

## §1 Goal

Bring III from a scattered post-DG-D/post-DG-F follow-up queue to a single coherent **v0.4.1 → v0.5.0** push, anchored on **net-new III coverage of ISS (operator-decision-gate) interaction quality** — so the surfaces agents share with operators are high-quality, well-composed, and RLHF-rich. Thesis: each pass tightens III's coverage of agent–operator interaction surfaces; the live ISS-adopter wrappers consume the revised pack; the audit-ingest seam becomes a load-bearing read-pattern; the next generation of III-tuned ISS surfaces is measurably better-composed and RLHF-richer than the last.

## §2 Context

Campaign F (Operation Tell) closed at DG-F 11/11 (`v0.4.0`, 2026-05-27); MX-1 (`vault_maintenance` citation-hardening) closed `v0.4.1` (2026-05-28); VisualDNA coord intake closed (track-defer to v1.0 GA). No campaign is open. The forward queue carries 36 deduplicated items across 5 themes; only Campaign E is hard-blocked (LiteratureForge forge-BUILD). The operator's anchor: **III support for the ISS system**.

**Reality the campaign works within** (G0 recon, deliverables `g0_*` 2026-05-29):
- **III has zero ISS coverage today** — net-new pack alongside `web_design`/`canvas_visual`/`whitepaper_communication`/`learning_store`/`vault_maintenance`/`inspect_procedures`/`introspect_checks`.
- **The ISS substrate is mature** — `skill_create_iss.md` (D1-D10 directives) + ADR-028 (AD-1..10) + ADR-029 (ST-1..6) + SiteForge `what/lib/iss/` (primitives/templates/tokens/skins/runtime/adaptation-guides). The III-as-consumer adaptation guide (`framework_orgvault_orggraph_guide.md`) names III-pattern gate types + a worked III ADR-ratification gate.
- **The real gap (L3):** III's review surface has no traps for *operator-decision-gate quality*. SiteForge owns L1 (library contract) + L2 (its internal AD-10 6-axis design ranker, run only during substrate build). The semantic residue at consumer-authoring time is unowned.

**Two G0 empirical corrections** (verified against the filesystem; surfaced not silently carried — per F AAR Change b):
1. **ISS-adopter landscape:** the card's "2 live wrappers" undercounts and ADR-029 ST-3's "3 incl. RareHarness" is stale. Actual: **MoleculeForge** (1 completed `.output.json`) + **WilhelmAI** (config, 0 gates) + **ZenZachary** (14 rendered gates — the richest corpus) live; **RareHarnessOld** legacy (the orphaned P4.M1 wrapper, post-rename); current **RareHarness** has none.
2. **T3 baseline:** `learning_store` is **composite 3.83 / source_diversity 3** (floor passes) — NOT the card's "3.50 / sd 2" (a conflation of `vault_maintenance` pre-MX-1). T3 is re-scoped from floor-remediation to citation-formalization (sd 3→4, composite 3.83→~4.00).

## §3 Boundary posture (operator-locked)

III **stays semantic**. The ISS pack is Markdown trap definitions reviewing the ISS *contract surface* (gate output JSON per AD-7 + rendered HTML structure + wrapper config) — **no ISS runtime in III** (SiteForge owns library + generator + receiver). The audit-ingest seam is a **documented schema + one worked example** — **no III audit runtime; no validator built**. Consumption-only ADR discipline: Campaign F shipped 0 new III ADRs; Campaign G targets **0 new III ADRs**, with the one explicit candidate (ADR-009 ISS-Surface Artifact Class) **pre-disposed REJECTED at G0** (ISS is a consumed pattern from ADR-028/029 — same precedent that `web_design` packs an external artifact class with no III-side structural ADR). Operator may override at G0.

## §4 Scope

### IN (4 tracks)
| Track | Scope | Lands |
|---|---|---|
| **T1 — ISS-surface pack (ANCHOR)** | New `context_iii_iss_surfaces.md` carrying **≥5 traps** from 9 candidate axes (spec: `g0_iss_pack_revision_spec.md`; recommended landing set {Trap 1 decision-frame, 3 evidence-lede, 4 RLHF-completeness, 5 option-set, 7 confidence}; +{2 persona-voice, 6 SR-under-glass} to 7; a11y traps 6/8 contingent on G1 re-inspection) | G1 |
| **T2 — audit-ingest seam** | `g2_audit_ingest_schema_note` (Lighthouse/axe/CWV field map) + Trap-6 canonical worked example. No runtime. | G2 |
| **T3 — learning_store deepening** | MX-1 citation-recipe replay; formalize §Sources; sd 3→4, composite 3.83→~4.00 (re-baselined; lower stakes than card) | G2 |
| **T4 — graduation-maturation (CONDITIONAL)** | Fire C-029 ceremony IF a consumer-vault proposal arrives via ADR-008 by G3/DP-4 (canonical md5 rotates); ELSE candidates-only continuation. Campaign NOT block-closed on graduation. | G3 |

### OUT
- **Campaign E** (generalized writing-III) — hard-gated on LiteratureForge forge-BUILD per MD-X2 §6; NOT subsumed.
- **VisualDNA bundle-review** — track-deferred to v1.0 GA; acknowledged via §G0 coord memo only.
- **`whitepaper_communication`** — Campaign E territory.
- **SiteForge ISS runtime / audit execution** — SiteForge owns (boundary).
- **III ISS adoption** (III authoring its own gates via an `iss/` wrapper) — *reviewing* ISS quality ≠ *adopting* ISS; the CF→Framework-Pilot is acknowledged but out of Campaign G scope (operator-discretionary; could be a future standalone).

### Subsumes (formerly free-standing queue items)
graduation ceremony for C-029 (→ T4); `learning_store` deepening (→ T3); audit-ingest seam formalization (→ T2); ISS-surface integration (→ T1).

## §5 Phases & Missions

### Phase G0 — Charter & Recon
| Mission | Title | Sessions | Deps | Status |
|---|---|---|---|---|
| G0 | Charter authoring + 5 recon artifacts + audit-ingest note + architecture note + 4 coord drafts | 1 | — | ✅ authored 2026-05-29 (this planning-mission execution); ratify at DP-1/DP-2 |

**Exit gate:** 8 deliverables authored (`g0_*` + charter + architecture note + 4 coord drafts); DP-1 (open) + DP-2 (scope/trap-set ratification) cleared with operator; 4 coord memos fired.

### Phase G1 — ISS-Surface Pack Landing (ANCHOR)
| Mission | Title | Sessions | Deps | Status |
|---|---|---|---|---|
| G1 | Author `context_iii_iss_surfaces.md` (≥5 traps) + a11y re-inspection (Traps 6/8) + ADR-007 §3 score | 1-2 | G0 | ✅ **CLOSED 2026-05-30** — pack landed (6 traps {1,2,3,4,5,7}; composite 4.00); a11y Traps 6/8 held as candidates (evidence-locked); **DP-3 ratified** (operator "let's continue") |

**Exit gate:** pack landed with ≥5 traps each carrying Description/Example/Where-Hides/Severity/Detection + substrate citation; quality_metric scored (target composite ≥4.0); DP-3 trap-set ratified; canonical jsonl INVARIANT. ✅ **MET** (6 traps; composite 4.00; DP-3 cleared; jsonl `5adb0dfa…`/28 invariant).

### Phase G2 — learning_store Deepening + Audit-Ingest Seam
| Mission | Title | Sessions | Deps | Status |
|---|---|---|---|---|
| G2 | T3 (`learning_store` §Sources formalization, sd 3→4) + T2 (audit-ingest schema note + Trap-6 worked example) + optional lattice 1.2.5→1.2.6 oracle registration | 1 | G1 | ✅ **CLOSED 2026-05-30** — T3 + T2 + lattice registration all landed (decisions: register-now + wired-T2) |

**Exit gate:** `learning_store` re-scored ~4.00 (citation-add only; canonical INVARIANT); audit-ingest note landed (boundary held, no runtime); lattice yaml registered if ISS pack added to oracle. ✅ **MET** — `learning_store` **3.83→4.00** (sd 3→4; §Sources 5-cluster MX-1 replay; canonical INVARIANT); `g0_audit_ingest_schema_note.md` landed `draft→reference` v1.0.0 + **wired load-bearing** into `web_design` Trap-6 detection + §Related (boundary held: no runtime/validator); lattice **1.2.5→1.2.6** (`iss_surfaces` registered in `pack_assignment` + `depth_assignment`). DG-G crit **3 + 4 satisfied**.

### Phase G3 — Candidate Ledger + Conditional Graduation
| Mission | Title | Sessions | Deps | Status |
|---|---|---|---|---|
| G3 | ISS candidate ledger (≥3 candidates, ≥2-vault evidence where present) + **conditional** C-029 graduation fire at DP-4 | 1 | G2 | ✅ **CLOSED 2026-05-30** — `g3_iss_candidate_ledger.md` landed (3 ISS trap-axis candidates ISS-C6/C8/C9, honest 0-vault-live/latent labels); **DP-4 resolved candidates-only** (no consumer C-029 proposal; canonical INVARIANT) |

**Exit gate:** candidate ledger authored (Markdown, `f3`-shape); **DP-4** — IF consumer-vault C-029 proposal present → graduation ceremony fires (canonical md5 ROTATES with full provenance in DG-G scorecard); ELSE candidates-only ratified (canonical INVARIANT). **The sole non-invariant path in Campaign G.** ✅ **MET** — ledger landed (ISS-C6 SR-under-glass + ISS-C8 touch-target + ISS-C9 gate-path, each substrate-grounded, all 0-vault-live; §"Reinforced landed traps" maps EGL-1..6 to landed {1,2,3,4,5,7}; §DP-4 disposition documents the candidates-only determination). DP-4 = **candidates-only**: aggregation index holds only 2 VideoForge seeds (C-027/C-028), zero C-029; MoleculeForge store 0 bytes; VisualDNA pre-GA → **canonical jsonl `5adb0dfa…`/28 INVARIANT** (charter R2-expected path). DG-G crit **5 + 6 satisfied**.

### Phase G4 — Minor Bump + Wrapper Sweep
| Mission | Title | Sessions | Deps | Status |
|---|---|---|---|---|
| G4 | III v0.4.1 → v0.5.0 minor bump + ADR-002 §3 wrapper sweep | 1 | G3 | ✅ **CLOSED 2026-05-31** — v0.5.0 DECLARED (MANIFEST commit `0f06aa6`; tag deferred to G6); 3 carriers re-pinned (VideoForge `ab5b178` + CanvasForge `3ebd55a` + wga `cfc5d7e`); SiteForge/LPWhitepaper/lattice-labs + 4 non-tracked DEFERRED; ISS-adopter invite FIRED |

**Exit gate:** MANIFEST Version + Release-History row; ADR-002 §3 review fired at all minor-policy consumers; ISS-adopter wrappers (MoleculeForge + WilhelmAI + ZenZachary) re-pin IF they add `iss_surfaces` to `packs_used`; `learning_store` carriers re-pin (additive, backward-compat per F4 LPWhitepaper precedent); SiteForge F4-residue + LPWhitepaper defer per own cadence. Annotated tag DEFERRED to G6. ✅ **MET** — III `v0.4.1 → v0.5.0` DECLARED at MANIFEST `0f06aa6` (Version prose + Release-History v0.5.0 row; annotated tag DEFERRED to G6 per F4/v0.4.0 precedent). ADR-002 §3 sweep fired at all minor-policy consumers; operator-elected mechanic = **Argus edits+commits in-vault** (MD-A4/F4 precedent), clean-tree pre-flight, `local_extensions` byte-identical: **VideoForge `ab5b178` + CanvasForge `3ebd55a` + wga `cfc5d7e` re-pinned @ `v0.5.0`** (`pinned_at_commit 0f06aa6` / lattice 1.2.6). **DEFERRED per own cadence** (review-fired, recorded in MANIFEST Active Consumers): SiteForge (F4-residue edited-uncommitted) + LPWhitepaper (no `web_design`/`iss_surfaces`; additive backward-compat) + lattice-labs/iii (TRACKED-DEFERRED) + non-MANIFEST-tracked ContextCommons / LatticeLabs / MoleculeForge / ZenZachary (own cadence; the two ISS-adopters fold into the `iss_surfaces` opt-in). **ISS-adopter opt-in/validation invite FIRED** (`coord_2026_05_31_iii_to_iss_adopters_validation_invite.md` draft→sent; consumer-discretionary — Argus does NOT unilaterally add `iss_surfaces`; no re-pin forced; seeds G5). Hard invariants HELD: canonical jsonl md5 `5adb0dfa…`/28 INVARIANT; lattice 1.2.6 unchanged; zero new III ADR (re-pin governed by existing ADR-002 §3); no annotated tag (G6); workspace router UNTOUCHED (G6 cascade item). DG-G crit **7 + 8 satisfied**.

### Phase G5 — Validation (inspection-grade)
| Mission | Title | Sessions | Deps | Status |
|---|---|---|---|---|
| G5 | Inspection-grade validation on live ISS adopters | 1 | G4 | ✅ **CLOSED 2026-06-23 — VERDICT GO** — `missions/g5_validation_20260623.md`; 6/6 recommended L3 residue axes caught across **3 of 3** live adopters (≥2-of-3 bar exceeded); negative control clean; `zero_regression_confirmed: true` |

**Exit gate:** `g5_validation_*.md` (MX-1/`md_b5` shape) — re-inspect ISS gates against the landed pack: each recommended residue caught with pack-line citation + negative control clean (zero new FPs); `zero_regression_confirmed: true`. **Targets: ZenZachary (rendered-gate residue, richest) + MoleculeForge/Molecules (output-JSON exemplar) + WilhelmAI (config exemplar)** — recommend ≥2 of 3. ✅ **MET** — `how/campaigns/campaign_g_consolidation/missions/g5_validation_20260623.md`: **VERDICT GO**. The G5 corpus had grown materially since G0 (Molecules `p6_1`+`sc_*`; ZenZachary ~13 answered P1 gates; WilhelmAI `p6_3`+`lh0` 2026-06-23) — upgrading **Trap 4** (RLHF-completeness; `rlhf_reviewer_persona` absent though top-level `persona` known) from 1-vault to **3-vault** and **Trap 7** (confidence; no confidence on an IRREVERSIBLE `lh0` security-model signoff + charter ratification) to **2-vault**, plus an honest new **Trap 1** observation (`decision_significance` rendered in HTML but dropped from output JSON, WilhelmAI `lh0`). All caught with exact pack-line citations. Held a11y candidates 6/8/9 **negative-control clean** (SiteForge primitives cover the surface — zero new FPs on the richer rendered-gate corpus). DG-G crit **9 satisfied**.

### Phase G6 — DG-G + AAR + Tag
| Mission | Title | Sessions | Deps | Status |
|---|---|---|---|---|
| G6 | DG-G gate + AAR + annotated `v0.5.0` tag + cascade bookkeeping | 1 | G5 | ✅ **CLOSED 2026-06-23 — DG-G 11/11** — `missions/g6_dg_g_scorecard.md`; AAR §13 inline; cascade complete; annotated `v0.5.0` tag cut at close commit |

**Exit gate:** DG-G 11/11; `g6_dg_g_scorecard.md` (10-section, `f6_dg_f` shape); AAR populated inline; cascade (STATE.md + root CLAUDE.md Campaign State + MANIFEST + workspace router production-pin); annotated `v0.5.0` tag at close commit; `status: completed`. ✅ **MET** — `how/campaigns/campaign_g_consolidation/missions/g6_dg_g_scorecard.md` (10-section, 11-criterion table; `zero_regression_confirmed: true`; closing assertion "DG-G 11/11 CONFIRMED. Campaign G end-to-end shipped at v0.5.0."); AAR populated inline at §13 below; cascade complete (STATE.md frontmatter + Current Phase; root CLAUDE.md Campaign State; MANIFEST Version + Release-History v0.5.0 row already carried from G4; workspace router `~/aDNA/CLAUDE.md` III-row production pin v0.4.0→v0.5.0 — router is outside any git repo, Berthier handles); annotated `v0.5.0` tag cut at close commit. Hard invariants HELD: canonical jsonl md5 `5adb0dfa…`/28 INVARIANT; lattice 1.2.6; zero new III ADR (count stays 7). DG-G crit **11 satisfied**.

## §6 Decision Points

| # | When | Decision | Status |
|---|---|---|---|
| DP-1 | G0 close | **Open Campaign G** (charter draft → active) | ✅ **CLEARED 2026-05-29** (operator "let's continue") |
| DP-2 | G0 close | Ratify trap selection (≥5 from 9) + T3 re-scope + adopter-landscape scope (include ZenZachary?) + coord-memo firing set | ✅ **CLEARED 2026-05-29** — trap set **6 core {1,2,3,4,5,7}** (a11y 6/8 evaluated at G1 re-inspection, added iff confirmed); **T3 KEEP** as polish (G2); ZenZachary **INCLUDED** as primary G5 validation corpus; coord-firing set = **SiteForge + VisualDNA** (ISS-adopters + LiteratureForge HELD, own cadence) |
| DP-3 | G1 close | Ratify landed trap set + a11y-trap inclusion (6/8) | ✅ **CLEARED 2026-05-30** — operator "let's continue" ratifies the 6 landed traps {1,2,3,4,5,7} + composite 4.00 + a11y Traps 6/8 held-as-candidates (evidence-locked from the G1 ZenZachary re-inspection) |
| DP-4 | G3 | **Conditional graduation** — fire C-029 IF consumer proposal present (canonical md5 rotation gate) ELSE candidates-only | ✅ **CLEARED 2026-05-30 — candidates-only** (verified: no consumer-vault C-029 proposal — aggregation index = 2 VideoForge seeds only, zero C-029; MoleculeForge local store 0 bytes; VisualDNA pre-v1.0-GA). Canonical md5 `5adb0dfa…`/28 **INVARIANT**; C-029 stays in `f3_correction_candidates.md`. The charter R2-expected outcome; operator-ratified at close. Evidence: `g3_iss_candidate_ledger.md` §DP-4 |
| DP-5 | G6 | DG-G GO/NO-GO | ✅ **GO 2026-06-23** — DG-G 11/11 (scorecard `missions/g6_dg_g_scorecard.md`); G5 validation VERDICT GO (6/6 recommended residue axes caught across 3/3 live adopters; negative control clean) is the load-bearing crit-9 evidence |
| DP-6 | G6 | Cut annotated `v0.5.0` tag | ✅ **CUT 2026-06-23** — annotated `v0.5.0` tag at the G6 close commit (F4/v0.4.0 precedent: declared at G4 `0f06aa6`, tagged at the DG-G close commit, NOT at the declaration commit); local only, push operator-gated |

## §7 Decision Gate DG-G (11 criteria — all must pass)

1. ISS-surface pack `context_iii_iss_surfaces.md` landed with **≥5 traps** (each Description/Example/Where-Hides/Severity/Detection + substrate citation). *(G1)*
2. Pack `quality_metric` scored against ADR-007 §3; composite **≥4.0**. *(G1)*
3. `learning_store` pack deepened (T3): §Sources formalized; **source_diversity 3→4**; composite **3.83→~4.00**; canonical jsonl INVARIANT. *(G2)*
4. Audit-ingest schema note (T2) landed with Lighthouse/axe/CWV field map + Trap-6 worked example; **boundary held** (no III audit runtime; no validator; no audit-execution ADR). *(G2)*
5. ISS candidate ledger authored (≥3 candidates, honest evidence-strength labels). *(G3)*
6. **DP-4 graduation disposition resolved**: IF consumer proposal → C-029 graduated (canonical md5 ROTATED, provenance documented) ELSE candidates-only (canonical md5 INVARIANT). Either path satisfies the criterion. *(G3)*
7. III v0.4.1 → **v0.5.0** minor bump (MANIFEST Version + Release-History row). *(G4)*
8. ADR-002 §3 wrapper sweep fired; ISS-adopter + `learning_store` carriers re-pinned (or explicitly deferred per own cadence). *(G4)*
9. Inspection-grade validation (G5): recommended residues caught with pack-line citations + negative control clean; `zero_regression_confirmed: true`. *(G5)* ✅ **MET** — `missions/g5_validation_20260623.md` VERDICT GO; 6/6 recommended L3 residue axes caught across 3/3 live adopters; negative control clean.
10. **Zero new III ADR** (count stays 7: 000+001+002+003+005+007+008); ADR-009 disposition REJECTED recorded. *(G0/G6)* ✅ **MET** — III-authored ADR count stays 7 (the 3 extra `adr_*.md` in `what/decisions/` are template-inherited base-standard ADRs, not III-authored — see scorecard §6 note); ADR-009 REJECTED at G0.
11. DG-G scorecard (10-section) + AAR inline + annotated `v0.5.0` tag; cascade bookkeeping complete. *(G6)* ✅ **MET** — `missions/g6_dg_g_scorecard.md` + §13 AAR inline + annotated `v0.5.0` tag at close commit + full cascade.

**DG-G 11/11 — GO 2026-06-23.**

## §8 Risk Register

| Risk | Severity | Mitigation |
|---|---|---|
| **R1 — ISS pattern externally evolving** (SiteForge ISS substrate may bump) | Medium | Pin SiteForge ISS commit-SHA at G0 (`903f461` / `^0.3.0` snapshot); pack reviews the pinned contract surface |
| **R2 — No organic C-029 proposal by G3** | Medium | T4 is **conditional, not block-closing** (DG-G crit 6 satisfied either path); candidates-only continuation per F precedent |
| **R3 — Audit-ingest land-vs-build tension** | Medium | **Doc + ONE worked example** (Trap 6); no validator; no runtime; boundary restated in T2 note §6 |
| **R4 — ADR-028/029 instability mid-campaign** | Low | Snapshot ADR-028/029 + adaptation-guide at G0 (all `status: accepted`, held 0-ADR across D1-D10+P4+P5); pack cites the snapshot |
| **R5 — Multi-track scope creep** (4 IN tracks vs F's tight single-track) | Medium | Defer order **T4 → T3 → T2 → T1 (anchor never defers)**; T3 is now low-stakes (floor passes) so it's a natural early-defer; T4 conditional |
| **R6 — Empirical-drift in the card's numbers** (already materialized: adopter count, T3 baseline) | Low (caught) | G0 verified both against the filesystem; charter carries corrected values; G-discipline = verify card claims at recon |

## §9 Verification Strategy

| Level | Check | Gate? |
|---|---|---|
| Per-mission | Hard-invariant snapshot pre+post (jsonl md5+wc, lattice version, ADR count, wrapper pins) | yes |
| Per-phase | Phase-exit gate assertion (each phase §5) | yes |
| Campaign | G5 inspection-grade validation (residues caught + negative control clean) | yes (DG-G crit 9) |
| Boundary | No III audit runtime / no ISS runtime / no new ADR — asserted in DG-G scorecard §boundary | yes (DG-G crit 4+10) |
| Canonical | jsonl md5 INVARIANT except DP-4 conditional rotation | yes (DG-G crit 6) |

## §10 Timeline

| Phase | Missions | Sessions |
|---|---|---|
| G0 | charter+recon | 1 (done 2026-05-29) |
| G1 | ISS pack | 1-2 |
| G2 | T3+T2 | 1 |
| G3 | ledger+conditional graduation | 1 |
| G4 | minor bump+sweep | 1 |
| G5 | validation | 1 |
| G6 | DG-G+tag | 1 |
| **Total** | **7 missions** | **~7-8 sessions** |

## §11 Notes

Structurally models on `campaign_web_design_deep_review.md` (Campaign F), which extended Campaign D. Phases G0..G6 mirror F0..F6; DG-G mirrors DG-F's 11-criterion shape; the G6 scorecard will mirror `f6_dg_f_scorecard.md` (10 sections); the G3 candidate ledger mirrors `f3_correction_candidates.md`; the G5 validation mirrors `mx1_validation_self_review.md` / `md_b5`. Three operator decisions are locked: **boundary-preserving**, **conditional graduation**, **v0.5.0**. ADR-009 pre-disposed REJECTED. The G0 recon artifacts (`g0_*`) are this campaign's P0 record. Two card numbers were corrected at recon (adopter landscape; T3 baseline) and are reflected throughout this charter.

## §12 Completion Summary

**Campaign G ("Operation Atrium") shipped end-to-end at `v0.5.0`, DG-G 11/11, 2026-06-23.** Boundary-preserving
consolidation push anchored on net-new III coverage of ISS operator-decision-gate quality, delivered across four
IN tracks:

- **T1 (anchor) — ISS-surface pack.** `context_iii_iss_surfaces.md` landed (G1) with **6 traps** {decision-frame,
  persona-voice, evidence-lede, RLHF-completeness, option-set bias, confidence}; composite **4.00**; a11y
  candidates 6/8 + gate-path candidate 9 held with honest evidence labels. Pack count 7→8.
- **T2 — audit-ingest seam.** `g0_audit_ingest_schema_note.md` (draft→reference v1.0.0) wired load-bearing into
  web_design Trap-6; lattice oracle 1.2.5→**1.2.6** (iss_surfaces registered). Boundary held — doc + one worked
  example, no runtime, no validator.
- **T3 — learning_store deepening.** `## Sources & Citations` 5-cluster MX-1 replay; `source_diversity` 3→4;
  composite 3.83→**4.00**; canonical jsonl INVARIANT.
- **T4 — conditional graduation.** DP-4 resolved **candidates-only** (no consumer-vault C-029 proposal); canonical
  md5 `5adb0dfa…`/28 INVARIANT — the sole non-invariant gate held INVARIANT.

**Release:** III `v0.4.1 → v0.5.0` (declared G4 at MANIFEST `0f06aa6`; annotated tag cut at the G6 close commit per
F4/v0.4.0 precedent). ADR-002 §3 wrapper sweep: VideoForge `ab5b178` + CanvasForge `3ebd55a` + wga `cfc5d7e`
@ v0.5.0; SiteForge/LPWhitepaper→LatticeProtocol/lattice-labs deferred per own cadence; ISS-adopter validation
invite fired.

**Validation (G5):** VERDICT GO — 6/6 recommended L3 residue axes caught across **3 of 3** live adopters
(ZenZachary + Molecules + WilhelmAI; ≥2-of-3 bar exceeded); Trap 4 upgraded to 3-vault + Trap 7 to 2-vault on
live evidence as the adopters' corpus grew since G0; held a11y candidates negative-control clean.

**Hard invariants end-to-end:** canonical jsonl md5 `5adb0dfa38d9224649c3b2cba83852ae` (28) INVARIANT across all
of G0..G6; **zero new III ADR** (III-authored count stays 7; ADR-009 REJECTED at G0); boundary held (no ISS
runtime, no III audit runtime, no SiteForge encroachment).

Scorecard: `missions/g6_dg_g_scorecard.md` (10-section; 11-criterion table). Validation:
`missions/g5_validation_20260623.md`.

## §13 Campaign AAR

*Full AAR authored inline in the DG-G scorecard `missions/g6_dg_g_scorecard.md` §11 (Worked / Didn't / Finding /
Change / Follow-up); restated here per Campaign A-F precedent.*

**Worked:** (1) the operator-locked semantic boundary made every mission decision crisp — zero scope drift across
four IN tracks; (2) substrate-grounded trap authoring (land from the contract surface even at 1-vault live
evidence) meant the pack was *ready* before adopters had multi-vault outputs; (3) the G0 prediction came true —
multi-vault confirmation accrued as the adopters' corpus grew from 1 completed output to ~20 across 3 vaults by
G5; (4) candidates-only / DP-4 kept the canonical md5 invariant; (5) the negative control proved the traps are
judgments, not linters; (6) the F6 10-section/11-criterion scorecard shape transferred 1:1.

**Didn't / surprised:** (1) the richest corpus was ZenZachary, not MoleculeForge — recon adopter-landscape
vigilance paid off twice (G0 + G5); (2) a genuinely new residue surfaced at G5 — `decision_significance` rendered
correctly in HTML but dropped from the captured output JSON (WilhelmAI `lh0`), owned by Trap 1 at the
output-completeness layer; (3) the MoleculeForge→Molecules rename required naming both to stay honest (back-compat
symlink active).

**Finding:** a substrate-grounded pack can be authored ahead of multi-vault live evidence and then validated as
the evidence arrives. The ISS residue clustered exactly where recon predicted (RLHF-completeness #4 + confidence
#7 strongest), and the highest-value live instances were *consequential* gates (a charter ratification + an
IRREVERSIBLE security-model signoff) captured with no confidence and no `rlhf_reviewer_persona` — the most
important decisions were the ones losing the most training signal, precisely the residue the pack was built to
catch.

**Change (next consolidation campaign):** (1) verify the card's adopter-landscape against the filesystem at recon
*and* at validation; (2) author traps from the contract surface where substrate-strong even at 1-vault, labeling
evidence strength honestly; (3) hold thin-but-grounded axes as candidates rather than force-landing; (4) at
validation run the inverse (negative-control) test on the held candidates, not just the positive test on the
landed traps; (5) keep the canonical jsonl invariant unless a consumer-vault proposal arrives (candidates + ISS
trap-axes land into the pack, not the canonical store); (6) cut the annotated tag at the DG close commit, not the
declaration commit.

**Follow-up:** (1) ISS-C6/C8/C9 → pack when template-local a11y residue actualizes; (2) Trap 2 (persona-voice)
live actualization watch; (3) C-029 graduation (2-vault SS+wga) at the next natural-frequency gate via a
consumer-vault ADR-008 proposal; (4) `vault_maintenance` pack revision (MD-B4 §8 carry); (5) audit-ingest seam
construction (v0.5+; future Platform.aDNA); (6) ISS-adopter wrapper `packs_used` opt-in at own cadence;
(7) Campaign E (generalized writing-III) gated on LiteratureForge forge-BUILD, re-points the LatticeProtocol
(ex-LPWhitepaper) wrapper.
