---
campaign_id: campaign_web_design_deep_review
type: campaign
title: "Campaign F — Web-Design Deep-Review & Improve (boundary-preserving)"
codename: "Operation Tell"
owner: stanley
status: completed
phase: 5
campaign_letter: F
phase_count: 5
mission_count: 9
estimated_sessions: "5-8"
calibrated_sessions: "4-6"
estimation_class: governance-tight
priority: medium
created: 2026-05-25
updated: 2026-05-27
opened: 2026-05-25
closed: 2026-05-27
dg_f_closed: 2026-05-27
dg_f_outcome: GO
release_tag: "v0.4.0"
last_edited_by: agent_argus
authored_by_mission: plan_web_design_deep_review_charter
predecessor: campaign_d_federation_adaptive_loop
boundary_posture: boundary_preserving
graduation_posture: "candidates only — graduated zero in-campaign (operator decision 2026-05-25)"
tags: [campaign, completed, web_design, siteforge, boundary_preserving, anti_slop, campaign_f, operation_tell, phase_5, f6_complete, dg_f_closed, dg_f_11_of_11, v0_4_0_tagged, campaign_f_complete, candidates_only, canonical_md5_invariant]
---

# Campaign F — Web-Design Deep-Review & Improve ("Operation Tell")

> **Status: ACTIVE** — opened 2026-05-25 by operator gate (Decision Point 1). Codename **Operation Tell** ratified at F0 (refs the AI-slop "tells" the flagship Trap 7 catches). The planning mission `plan_web_design_deep_review_charter` (2026-05-25, completed) is this campaign's P0 charter-authoring record; the four `wdr_*` research artifacts authored alongside are its source-of-record. F0 closed + Phase 1 entered in the same session (`session_stanley_20260525_iii_adna_campaign_f_open_pack_revision`).

## Goal

When Campaign F completes, **III's `web_design` review surface measurably improves and demonstrably learns from the audit signals SiteForge already produces** — without III gaining any audit runtime. Concretely: the pack gains the **CWV-as-Design-Debt** and **AI-Slop-Composition** traps (the two best-justified residue traps), three existing traps are citation-hardened (lifting the weak `source_diversity` axis from 3 toward 5), five correction *candidates* carry `≥2-vault` evidence, and the documented audit-signal-ingest seam is on record. The thesis: each pass tightens III's web advice, the SiteForge wrapper consumes it, and the next generation of III-tuned Astro sites is better than the last.

## Context

Campaign D closed 2026-05-25 (DG-D 11/11, `v0.3.0`). III's web-design review is *semantic* (5 traps, composite 4.00) but carries two weak axes — `source_diversity=3` (no citations) and `graduation_yield=1` (an MB4-2 measurement artifact, resolved at MD-B6 via ADR-007 §3.6). Mechanical audits (Lighthouse/axe/CWV) already live in SiteForge's 10-gate framework. The real gap, established by the planning mission's recon, is that **III's semantic advice does not yet learn from the audit signals SiteForge produces**, and the "passes the robots but still reads as slop / weak design" residue has no traps.

The empirical corpus (WDR-3) is three *already-high-quality, already-III-reviewed* sites (wga, ScienceStanley, ContextCommons). That makes the residue **thinner than a naïve hypothesis** — and the campaign is scoped to match: evidence-disciplined, not aspirational. Two new traps are well-justified; three trap-hardenings remediate `source_diversity`; thin candidates ship as candidates (graduate later).

**Predecessor**: `campaign_d_federation_adaptive_loop`. **Source-of-record**: the four `wdr_*` artifacts + this charter.

## Boundary posture (operator-locked)

III **stays semantic**. Mechanical audit *execution* stays in SiteForge (Gates 1–10). III does **not** gain an audit runtime and authors **no** audit-execution ADR. The audit-signal-ingest seam (III *reading* SiteForge's Lighthouse/axe JSON as input to its traps) is **documented as a future option only — not built** (WDR-4 §5).

## Scope

### In Scope
- Land **Trap 6 (CWV-as-Design-Debt)** + **Trap 7 (AI-Slop Composition)** into the live `web_design` pack (Stanley-gated per Standing Rules 6/7).
- **Three trap hardenings** + `source_diversity` citation remediation (Trap 3 a11y coverage-ceiling + SC 2.5.8 + semantic-heading; Trap 4 earned-credibility/naturalness; Trap 5 precision fix).
- Stand up **5 correction candidates** (`C-029+` provisional) as `graduation_candidate` with `≥2-vault` evidence rows — held in the spec / a consumer local store, **never** the canonical jsonl.
- **ADR-002 §3 minor-bump wrapper sweep** for wrappers carrying `web_design` (SiteForge + VideoForge + CanvasForge; wga + LPWhitepaper if applicable) at III `v0.3.0 → v0.4.0`.
- Pack re-score against the ADR-007 §3 6-axis rubric (expect composite ≥4.00 hold; `source_diversity` 3→4/5).

### Out of Scope
- **No III audit runtime**; no audit-execution ADR; the ingest seam stays documented-only.
- **No graduation fires** — new corrections stay candidates (graduate at a later natural-frequency gate per operator decision). Canonical md5 mutates **only** if/when a future graduation ceremony runs — not in this campaign.
- **No SiteForge gate changes** — mechanical layer is SiteForge's.
- **No new ISS-pack work** (orthogonal surface).
- **Not Campaign E** — generalized writing-III stays reserved/gated on `LiteratureForge.aDNA` forge-BUILD.

### Subsumes
| Plan/Mission | Status at Subsumption | Tasks Absorbed By |
|-------------|----------------------|-------------------|
| [[plan_web_design_deep_review_charter]] | completed (planning mission) | P0 (becomes the charter-authoring record + source-of-record) |

## Phases & Missions (dependency-ordered; phase gates are human gates)

### Phase 0: Adoption & Setup
| Mission | Title | Sessions | Dependencies | Status |
|---------|-------|----------|-------------|--------|
| F0 | Adopt the 4 `wdr_*` artifacts + this charter as source-of-record; ratify codename + scope; confirm Q2 final trap set (Trap 6 + Trap 7 in; motion/responsive deferred) | 0.5 | — | ✅ **completed 2026-05-25** |

**Phase exit gate**: ✅ charter ratified (status→active; codename "Operation Tell"); ✅ trap set confirmed (Trap 6 CWV-as-Design-Debt + Trap 7 AI-Slop Composition in; motion + responsive deferred as 1-vault candidates per the planning-mission AAR); ✅ canonical md5 baseline recorded `5adb0dfa38d9224649c3b2cba83852ae` (28 entries) at session startup; ✅ 4 `wdr_*` artifacts + this charter adopted as source-of-record.

### Phase 1: Pack Revision (core)
| Mission | Title | Sessions | Dependencies | Status |
|---------|-------|----------|-------------|--------|
| F1 | Land Trap 6 (CWV-as-Design-Debt) + Trap 7 (AI-Slop Composition) into the live pack (full pack-format entries per WDR-4 §2) | 1 | F0 | ✅ **completed 2026-05-25** |
| F2 | Trap hardenings + `source_diversity` citation remediation (Trap 3/4/5 per WDR-4 §3); re-score pack against ADR-007 §3 rubric | 1 | F1 | ✅ **completed 2026-05-25** |

**Phase exit gate**: ✅ pack updated (7 traps: Trap 6 + Trap 7 landed; Trap 3 hardened with coverage-ceiling + SC 2.5.8 + semantic-heading; Trap 4 earned-credibility/naturalness; Trap 5 precision fix token-vs-semantic-data) + re-scored; ✅ composite ≥4.00 holds (4.00 → **4.17**); ✅ `source_diversity` ≥4 (3 → **4**; capped at 4 — academic source-type absent per rubric §4); ✅ ≥6 external citations (14 citation URLs across 8 source clusters) + ADR-002/ADR-003/skill_iii_review cross-refs; ✅ **Stanley ratified the pack edit 2026-05-25** (Standing Rule 6, Decision Point 3). Hard invariant held: canonical jsonl md5 `5adb0dfa38d9224649c3b2cba83852ae` (28 entries) invariant — zero graduation, candidates-only posture preserved.

### Phase 2: Correction Candidates
| Mission | Title | Sessions | Dependencies | Status |
|---------|-------|----------|-------------|--------|
| F3 | Author the 5 correction candidates (`C-029+`) with `≥2-vault` evidence rows into the spec / consumer local store; verify canonical jsonl md5 **invariant** (`5adb0dfa…`); zero graduation | 0.5–1 | F2 | ✅ **completed 2026-05-26** |

**Phase exit gate**: ✅ candidates recorded in `what/artifacts/f3_correction_candidates.md` (operator chose Markdown candidate ledger over a staging `.jsonl`, 2026-05-26) — 5 schema-conformant `graduation_candidate:true` rows (C-029…C-033 prov.); ✅ `md5 iii_corrections_canonical.jsonl` == `5adb0dfa38d9224649c3b2cba83852ae`; ✅ `wc -l` == 28 (unchanged). Honest-evidence posture held: only C-029 is genuinely 2-vault (SS+wga); C-030…C-033 are 1-vault and recorded as awaiting a 2nd vault (C-032 explicitly refuted on wga+CC). 3 reinforced existing corrections (C-012/C-015/C-016) noted as future re-graduation inputs — no action.

### Phase 3: Consumer-Wrapper Sweep
| Mission | Title | Sessions | Dependencies | Status |
|---------|-------|----------|-------------|--------|
| F4 | III minor bump `v0.3.0 → v0.4.0`; ADR-002 §3 minor-bump review for `web_design`-carrying wrappers (SiteForge + VideoForge + CanvasForge; wga + LPWhitepaper as applicable); each consumer re-pins explicitly | 1–2 | F2 | ✅ **completed 2026-05-26** |

**Phase exit gate**: ✅ III minor-bumped `v0.3.0 → v0.4.0` (declared in `MANIFEST.md` Version + Release-History row at III commit `f49e821`; annotated `v0.4.0` git tag deferred to F6). ✅ ADR-002 §3 minor-bump review fired at all minor-policy consumers. **4 `web_design` carriers** (by `packs_used`) reviewed + re-pinned to `v0.4.0` / `pinned_at_commit f49e821` / `lattice_version 1.2.5`: **VideoForge** (commit `3cc3191`) + **CanvasForge** (commit `97654e3`) + **wga** (commit `3bbfb7a`) committed in their own repos; **SiteForge** edited but **left UNCOMMITTED** (in-flight ISS-campaign migration dirtied `iii/CLAUDE.md` with an unrelated SIS→ISS rename — operator folds the F4 pin into their ISS commit; counts reviewed+re-pinned for DG-F crit 7 per operator decision). **LPWhitepaper** (no `web_design` in `packs_used`): review fired, **explicitly deferred** — stays `v0.3.0`, additive-only bump is backward-compatible, re-pins at its own cadence. **lattice-labs/iii** `v0.1.0` **TRACKED-DEFERRED** (workspace genesis-first migration discipline; untouched). No active consumer broken; no file moves so no forward stubs needed (Standing Rule 2 n/a); `local_extensions` untouched in every wrapper (out of minor-bump-review scope). Canonical jsonl md5 `5adb0dfa38d9224649c3b2cba83852ae` (28 entries) INVARIANT; lattice yaml at `1.2.5`; zero new III ADR; no `v0.4.0` tag (F6); local commits only, no push.

### Phase 4: Validation, Decision Gate & Close
| Mission | Title | Sessions | Dependencies | Status |
|---------|-------|----------|-------------|--------|
| F5 | Validation — confirm new traps catch the WDR-3 residue findings (re-inspect the gap-ledger cases against the revised pack; inspection-grade, no live runs) | 1 | F1–F4 | ✅ **completed 2026-05-26** |
| F6 | DG-F decision gate + Campaign AAR + annotated `v0.4.0` tag | 0.5 | F5 | ✅ **completed 2026-05-27** |

**Phase exit gate**: ✅ F5 validation artifact `what/artifacts/f5_validation_wdr3_residue.md` authored (inspection-grade; **8 of 8** in-scope WDR-3 residue findings CAUGHT with exact pack-line citations; 2 deferred residue findings — motion-absence + responsive-neglect — confirmed on-record per F0 + C-032; ContextCommons negative control confirms **zero** new false positives on a clean site); ✅ DG-F criterion 10 satisfied; ✅ F6 DG-F scorecard artifact `what/artifacts/f6_dg_f_scorecard.md` authored (10 sections mirroring `md_b6_dg_d_scorecard.md`; 11-criterion table all ✅; hard-invariant ledger pre/post; boundary-discipline coherence assertion); ✅ DP5 GO ratified 2026-05-27; ✅ DG-F 11/11; ✅ annotated `v0.4.0` git tag cut at close-commit SHA (MD-B6-style contract-surface enumeration; predecessor `v0.3.0` at MD-B6 close 2026-05-25); ✅ Campaign AAR populated inline (see §Campaign AAR); ✅ canonical jsonl md5 `5adb0dfa38d9224649c3b2cba83852ae` (28 entries) INVARIANT (held across all 7 Campaign F missions F0..F6); zero-regression CONFIRMED end-to-end.

## Decision Points
| # | When | Decision | Status |
|---|------|----------|--------|
| 1 | P0 entry | Open the campaign (this draft → active) | ✅ approved 2026-05-25 (operator gate) |
| 2 | F0 | Confirm final trap set + codename | ✅ confirmed 2026-05-25 (Trap 6 + Trap 7; "Operation Tell") |
| 3 | F2 close | Ratify live pack edit (Standing Rule 6/7) | pending (this session — DP3 human gate) |
| 4 | F4 | Approve III minor bump v0.3.0→v0.4.0 + each consumer re-pin | ✅ approved 2026-05-26 (operator gate; LPWhitepaper deferred + SiteForge edited-uncommitted dispositions ratified) |
| 5 | F6 | DG-F GO/NO-GO | ✅ GO 2026-05-27 (operator gate; DG-F 11/11; v0.4.0 tag cut) |

## Decision Gate DG-F (binary; ALL must hold to close)
1. Trap 6 (CWV-as-Design-Debt) landed in live pack with full pack-format entry. 
2. Trap 7 (AI-Slop Composition) landed with coverage-seam rationale cited.
3. Trap 3/4/5 hardened; `source_diversity` axis ≥4 (was 3); composite ≥4.00 holds.
4. ≥6 distinct external citations present in the pack (the `source_diversity` remediation).
5. 5 correction candidates recorded with `≥2-vault` evidence; **zero** appended to canonical.
6. Canonical jsonl md5 == `5adb0dfa38d9224649c3b2cba83852ae` (28 entries) — **invariant**.
7. ADR-002 §3 wrapper sweep complete; every `web_design`-carrying wrapper reviewed + re-pinned or explicitly deferred; no active consumer broken.
8. III minor-bumped `v0.3.0 → v0.4.0`; CHANGELOG entry present.
9. Audit-signal-ingest seam documented (not built); no III audit runtime introduced; no audit-execution ADR.
10. Validation (F5) confirms the revised pack catches the WDR-3 residue cases. — ✅ **F5 2026-05-26** (`what/artifacts/f5_validation_wdr3_residue.md`: 8/8 in-scope residue CAUGHT + 2 deferred on-record + CC negative control).
11. AAR populated; `v0.4.0` annotated tag cut; STATE.md + this charter `status: completed`. — ✅ **F6 2026-05-27** (`what/artifacts/f6_dg_f_scorecard.md`: 11/11 ✅; charter frontmatter `status: active → completed` + `phase: 4 → 5` + `dg_f_closed: 2026-05-27` + `release_tag: "v0.4.0"`; AAR populated inline below; annotated `v0.4.0` tag cut at close-commit SHA — MD-B6-style contract-surface enumeration body).

## Risk Register
| Risk | Severity | Mitigation |
|------|----------|------------|
| New traps over-fit to 3 already-good sites | High | External SOTA grounding (WDR-1); candidates-only posture; thin traps deferred until cross-vault frequency |
| False positives (Trap 5 on legit data colors; AI-slop on bespoke sites) | High | Trap 5 precision fix (data-vs-token distinction); Trap 7 framed as silhouette+content judgment, not silhouette alone |
| Boundary creep — III drifts toward owning audits | Critical | Ingest seam documented-only; no runtime; no audit-execution ADR; DG-F criterion 9 enforces |
| Premature graduation mutates canonical md5 | High | Candidates-only; DG-F criteria 5+6 assert md5 invariant; graduation deferred to later gate |
| Wrapper sweep breaks an active consumer | Medium | ADR-002 §3 minor-bump review; explicit per-consumer re-pin; forward-stub discipline (Standing Rule 2) |
| `graduation_yield` axis still reads low post-revision | Low | Known MB4-2 artifact resolved at MD-B6 (ADR-007 §3.6); not a content gap — note, don't chase |

## Verification Strategy
- **Per-mission**: SITREP + lightweight AAR; deliverables validated; `git status` clean.
- **Hard-invariant check (every mission touching corrections)**: `md5 what/context/core_domain_packs/iii_corrections_canonical.jsonl` == `5adb0dfa38d9224649c3b2cba83852ae`.
- **Pack re-score**: ADR-007 §3 6-axis rubric run on the revised pack (F2).
- **Wrapper integrity**: each consumer `iii/CLAUDE.md` resolves + re-pins; minor-bump review recorded (F4).
- **Validation**: WDR-3 residue cases re-checked against revised pack (F5, inspection-grade).

## Timeline
| Phase | Missions | Sessions |
|-------|----------|----------|
| P0 Adoption | F0 | 0.5 |
| P1 Pack Revision | F1–F2 | 2 |
| P2 Corrections | F3 | 0.5–1 |
| P3 Wrapper Sweep | F4 | 1–2 |
| P4 Validation+DG | F5–F6 | 1.5 |
| **Total** | **7 missions (F0–F6)** | **5–7 sessions** |

## Notes
- Modeled on the Campaign D charter skeleton + `how/templates/template_campaign.md`; mirrors the MD-X2 charter-draft structure.
- The single most defensible content change is the **Trap 5 precision fix** (wga ecoregion evidence); the highest-*value* change is **Trap 7 (AI-Slop Composition)** per III's anti-slop mandate, justified by the Brand Gate 8 manual-only coverage seam rather than prevalence.
- Relates to (does not block) Campaign E — the new web traps + corrections become inputs the SiteForge `iii/` wrapper consumes; the "III-tuned artifacts get better" thesis is shared.

## Completion Summary

Campaign F ("Operation Tell") closed end-to-end at `v0.4.0` on 2026-05-27 with DG-F 11/11 GO. The `web_design` pack gained **Trap 6 (CWV-as-Design-Debt)** + **Trap 7 (AI-Slop Composition)**; Trap 3/4/5 were hardened (coverage-ceiling + SC 2.5.8 + semantic-heading on Trap 3; earned-credibility on Trap 4; precision fix token-vs-semantic-data on Trap 5); `source_diversity` axis lifted 3→4 via 14 citation URLs across 8 source clusters; composite re-scored **4.00 → 4.17**. 5 correction candidates **C-029..C-033** recorded in `what/artifacts/f3_correction_candidates.md` (candidates-only — canonical jsonl md5 `5adb0dfa38d9224649c3b2cba83852ae` (28 entries) **INVARIANT** end-to-end). ADR-002 §3 minor-bump wrapper sweep re-pinned 3 `web_design` carriers committed (VideoForge `3cc3191` + CanvasForge `97654e3` + wga `3bbfb7a`) + SiteForge edited-uncommitted (folds into in-flight ISS commit) + LPWhitepaper explicitly deferred (no `web_design`). **Boundary held**: no III audit runtime; Trap 6 reads existing Lighthouse JSON, never re-runs; no audit-execution ADR (count stays 7); ingest seam documented-only. F5 inspection-grade validation: **8/8** in-scope WDR-3 residue findings caught + 2 deferred on-record + ContextCommons negative control clean. Production pin advances `v0.3.0 → v0.4.0`; annotated tag cut at close-commit SHA.

## Campaign AAR

*Populated inline at F6 close per Campaign A+B+C+D precedent. See `how/templates/template_aar_lightweight.md`.*

- **Worked**:
  - The **candidates-only posture** (operator decision 2026-05-25) preserved canonical jsonl md5 `5adb0dfa38d9224649c3b2cba83852ae` across all 7 missions (F0..F6) — verified at every mission close per the Verification Strategy invariant check. Hard-invariant discipline carried over cleanly from Campaign D.
  - **ADR-002 §3 minor-bump review** absorbed the wrapper sweep cleanly across 4 carriers; per-wrapper mechanical edit shape was consistent (5-field flip + frontmatter refresh + tag flip) and replicated MD-A4's v0.2→v0.3 sweep with no rollback.
  - **Boundary discipline held under operator pressure**: Trap 6 (CWV-as-Design-Debt) reads existing SiteForge Lighthouse JSON + attributes the breach but **never re-runs** (pack L127/L131/L165). F1+F2+F5 ran zero builds, zero audits — Campaign F bought reach without surface-area growth.
  - **F5 inspection-grade validation** paralleled MD-A5 / MD-B5 cleanly without needing a build/audit run; 8/8 in-scope residue findings caught with exact pack-line citations; ContextCommons negative control confirms zero new false positives on a clean site.
  - **Codename + scope confirmation at F0** + the 4 WDR artifacts as source-of-record kept Campaign F evidence-disciplined rather than aspirational; thin candidates correctly deferred (motion-absence refuted on wga+CC; responsive-neglect 1-vault mild).
  - **Consumption-only ADR discipline** maintained — zero new III ADRs across all 7 missions (final count stays 7); mirrors the 9-of-11 Campaign D ratio. The pack revision is content-only; ADR-002/003/007 already governed the contract surface F4 + F3 + F2 exercised.
  - **DG-F scorecard pattern from MD-B6** translated cleanly to a single-track campaign — 10-section shape, 11-criterion table, hard-invariant ledger pre/post, boundary-discipline coherence assertion. Audit anchor + close ceremony in one session per Campaign B+C+D precedent.

- **Didn't**:
  - **Graduation ceremony deferred** — zero canonical-frequency hits among C-030..C-033 (all 1-vault); only C-029 (`image_component_omits_dimensions`) is genuinely 2-vault (SS+wga) but ADR-003 §3 graduation still requires consumer-vault proposal step from own local store, not authored research. Canonical learning store grows zero entries in Campaign F.
  - **SiteForge couldn't commit its re-pin** at F4 — in-flight ISS migration dirtied `iii/CLAUDE.md` with an unrelated SIS→ISS rename; operator folds the F4 pin into the ISS commit. SiteForge counts reviewed+re-pinned for DG-F crit 7 (DP4 ratification 2026-05-26) but its `iii/CLAUDE.md` remains edited-uncommitted at F6 close — out-of-vault audit asymmetry that resolves when ISS commit lands.
  - **LPWhitepaper's wrapper review fired but re-pin deferred** — no `web_design` in `packs_used`; backward-compatible additive bump; LPWhitepaper stays at v0.3.0 and re-pins at own cadence (likely post-LiteratureForge migration chain per MD-X2 §6).
  - Only **3 trap-hardenings + 1 axis remediation** (`source_diversity` 3→4, not 5) — academic source-type absent in the rubric §4 caps the axis at 4 absent peer-reviewed citations. Lifting to 5 would require citing arXiv/ACM/Springer accessibility/CWV research; deferred as not load-bearing for the campaign.
  - **F4 mission ran without a dedicated session file** — the F4 work landed in two commits (`f49e821` v0.4.0 declare + `4cf72144` close bookkeeping) without intervening session ceremony in `how/sessions/history/2026-05/`. One-off audit-trail asymmetry; flagged forward to future close-ceremony hygiene (Follow-up item).
  - **Ingest seam not built** — `wdr_web_design_pack_revision_spec.md` §5 explicitly documents the audit-signal-ingest seam as a future option only; Campaign F documents what it would look like and explicitly defers construction (intentional per charter §Boundary posture, not a miss).

- **Finding**:
  WDR-3's "3 already-high-quality, already-III-reviewed sites" empirical corpus (wga / ScienceStanley / ContextCommons) produced **thinner residue than a naïve hypothesis predicted**. The right scoping move was **candidates-only + thin-trap deferral**, not aspirational pack expansion. Two new traps and three hardenings were well-justified; five candidates were the right ceiling. **The single highest-value content change (Trap 7 AI-Slop Composition) was justified by the *Brand Gate 8 manual-only coverage seam*, not by sample prevalence** — SiteForge's 10-gate framework formally hands manual judgment to a human at Gate 8, but the "structural AI-slop silhouette" pattern is a learnable trap *if* it's framed as silhouette+content judgment together rather than silhouette alone. **Coverage-driven trap authorship is a viable pattern when the seam is well-documented**; Campaign F established this as a precedent for future pack revisions.

- **Change**:
  Pack-revision campaigns inherit a clean recipe — codify as a template at `how/templates/template_pack_revision_campaign.md` (future follow-up if multiple pack revisions queue):
  1. **Recon WDR artifacts as source-of-record** (internal coverage map + external SOTA dossier + empirical gap ledger + revision spec + cross-vault architecture note where applicable) — Campaign F's `plan_web_design_deep_review_charter` planning mission produced 5 such artifacts before scope was locked.
  2. **Trap landings + hardenings + axis remediation in one pack-edit mission** (Campaign F F1+F2 split this into 2 missions; future campaigns may consolidate where pack delta is smaller).
  3. **Candidates-only correction recording** (`what/artifacts/<campaign>_correction_candidates.md` Markdown ledger; canonical jsonl untouched).
  4. **ADR-002 §3 minor-bump wrapper sweep** at the version-declare commit SHA (`pinned_at_commit f49e821` for Campaign F).
  5. **Inspection-grade validation paralleling MD-B5/F5** — re-walk the residue/gap-ledger against the revised pack with exact pack-line citations, with a negative-control site to confirm zero new false positives.
  6. **DG-class close ceremony per `skill_session_close_ceremony.md`** + 10-section scorecard artifact mirroring `md_b6_dg_d_scorecard.md`.
  This recipe is the natural template for the post-DG-D standalone `vault_maintenance` pack revision (carried forward from MD-B4) and for Campaign E's eventual generalized writing-III pack work.

- **Follow-up**:
  - **(a)** Graduation ceremony for **C-029** (`image_component_omits_dimensions`; 2-vault SS+wga) at next natural-frequency gate — triggered when a consumer vault re-observes the pattern from its own local RLHF store and proposes via ADR-008 `learning_store_graduation` cross_vault_request.
  - **(b)** **C-030..C-033** await 2nd-vault evidence; C-032 (motion-absence) REFUTED on wga+CC and is unlikely to graduate; C-030/C-031/C-033 await organic 2nd-vault observation.
  - **(c)** **`context_iii_vault_maintenance.md` pack revision** (post-DG-D standalone from MD-B4 `source_diversity = 2` finding; carried forward from MD-B6 §8 — apply the Campaign F recipe at next operator gate).
  - **(d)** **SiteForge folds F4 pin into ISS commit** at SiteForge's own cadence; resolves the edited-uncommitted audit asymmetry.
  - **(e)** **LPWhitepaper.aDNA wrapper re-pin** at own cadence, likely concurrent with Campaign E (post-LiteratureForge migration chain).
  - **(f)** **Motion/interaction-absence + responsive-breakpoint candidate traps** stay deferred until cross-vault frequency emerges (operator gate).
  - **(g)** **F4 audit-trail asymmetry** — flag forward to future close-ceremony hygiene: even bookkeeping-only commits (`v0.4.0` declare + close bookkeeping) benefit from a lightweight session file. Consider a `template_session_bookkeeping.md` for sub-1-hour mission slices that ship commits without full SITREP.
  - **(h)** **Audit-signal-ingest seam construction** is v0.5+ territory (future Platform.aDNA integration); Campaign F documented what it would look like — not in immediate roadmap.
  - **(i)** **Campaign E (generalized writing-III)** stays forward-seeded; opens when `LiteratureForge.aDNA` reaches its forge-BUILD phase per MD-X2 §6 — re-points LPWhitepaper `federation_ref` at that point.
  - **(j)** **Push to remote** — operator-discretionary per workspace standing rules; III.aDNA sits 5 commits ahead of `origin/main` post-F6 (F3 candidates + F4 declare + F4 close + F5 validation + F6 close).
