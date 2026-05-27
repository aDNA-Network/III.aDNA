---
campaign_id: campaign_web_design_deep_review
type: campaign
title: "Campaign F — Web-Design Deep-Review & Improve (boundary-preserving)"
codename: "Operation Tell"
owner: stanley
status: active
phase: 3
campaign_letter: F
phase_count: 5
mission_count: 9
estimated_sessions: "5-8"
calibrated_sessions: "4-6"
estimation_class: governance-tight
priority: medium
created: 2026-05-25
updated: 2026-05-26
opened: 2026-05-25
last_edited_by: agent_argus
authored_by_mission: plan_web_design_deep_review_charter
predecessor: campaign_d_federation_adaptive_loop
boundary_posture: boundary_preserving
graduation_posture: "candidates only — graduate later (operator decision 2026-05-25)"
tags: [campaign, active, web_design, siteforge, boundary_preserving, anti_slop, campaign_f, operation_tell, phase_3, f4_complete, v0_4_0_declared]
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
| F5 | Validation — confirm new traps catch the WDR-3 residue findings (re-inspect the gap-ledger cases against the revised pack; inspection-grade, no live runs) | 1 | F1–F4 | planned |
| F6 | DG-F decision gate + Campaign AAR + annotated `v0.4.0` tag | 0.5 | F5 | planned |

**Phase exit gate**: DG-F all-green; AAR populated; tag cut; STATE.md updated.

## Decision Points
| # | When | Decision | Status |
|---|------|----------|--------|
| 1 | P0 entry | Open the campaign (this draft → active) | ✅ approved 2026-05-25 (operator gate) |
| 2 | F0 | Confirm final trap set + codename | ✅ confirmed 2026-05-25 (Trap 6 + Trap 7; "Operation Tell") |
| 3 | F2 close | Ratify live pack edit (Standing Rule 6/7) | pending (this session — DP3 human gate) |
| 4 | F4 | Approve III minor bump v0.3.0→v0.4.0 + each consumer re-pin | ✅ approved 2026-05-26 (operator gate; LPWhitepaper deferred + SiteForge edited-uncommitted dispositions ratified) |
| 5 | F6 | DG-F GO/NO-GO | pending |

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
10. Validation (F5) confirms the revised pack catches the WDR-3 residue cases.
11. AAR populated; `v0.4.0` annotated tag cut; STATE.md + this charter `status: completed`.

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
*Fill out when setting `status: completed`.*

## Campaign AAR
*Mandatory before `status: completed`. See `how/templates/template_aar_lightweight.md`.*
- **Worked**: _(pending execution)_
- **Didn't**: _(pending)_
- **Finding**: _(pending)_
- **Change**: _(pending)_
- **Follow-up**: _(pending)_
