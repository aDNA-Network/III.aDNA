---
artifact_class: campaign_checkpoint
type: artifact
title: "Campaign G (Operation Atrium) — mid-campaign checkpoint + G3/G4 session AAR + forward-carry ledger (G1-G4 done; G5-G6 pending)"
campaign: campaign_g_consolidation
created: 2026-05-31
updated: 2026-05-31
last_edited_by: agent_argus
status: checkpoint                 # mid-campaign wind-down/handoff — NOT the G6 campaign AAR (charter §12/§13 stay reserved)
covers_missions: [G3, G4]
covers_sessions: [session_stanley_20260530_iii_adna_campaign_g_g3_iss_candidate_ledger, session_stanley_20260531_iii_adna_campaign_g_g4_minor_bump_wrapper_sweep]
dg_g_progress: "8/11"              # crit 1-8 + 10 MET; crit 9 (G5) + 11 (G6) PENDING
boundary_posture: boundary_preserving
hard_invariant: "Doc-only checkpoint. Mutates nothing operational. Canonical jsonl md5 5adb0dfa38d9224649c3b2cba83852ae (28 entries) INVARIANT across G1-G4; lattice 1.2.6; III ADR count 7 (ADR-009 REJECTED); III v0.5.0 DECLARED (G4) / annotated tag at G6."
tags: [artifact, campaign_g, operation_atrium, checkpoint, mid_campaign_aar, forward_carry_ledger, dg_g_8_of_11, g3_g4_session_aar, canonical_md5_invariant, read_first_at_g5]
---

# Campaign G ("Operation Atrium") — Mid-Campaign Checkpoint + G3/G4 AAR

## §1 Purpose

This is the **wind-down / handoff** after a single work session closed **G3** (ISS candidate ledger
+ DP-4) and **G4** (v0.5.0 minor bump + wrapper sweep) back-to-back. It exists so the **next
sessions start fresh with no findings dropped** — every gap surfaced this session is captured in
the §5 forward-carry ledger, tagged with its target phase.

> **Read this first at G5 start.** It is the consolidated handoff for the rest of Campaign G.

**This is NOT the G6 campaign AAR.** Charter §12 (Completion Summary) + §13 (Campaign AAR) stay
reserved for the G6 close per Campaign A-F precedent. This is a session-level checkpoint — a novel
artifact class for this vault (no prior mid-campaign wind-down existed), modelled on the
lightweight AAR shape (`how/templates/template_aar_lightweight.md`) at session scope.

## §2 Session arc

One work session, two mission closes, three III-side commits + three foreign-vault commits:

| Step | Commit | Landed |
|---|---|---|
| **G3 close** | `75d8b64` (2026-05-30) | `what/artifacts/g3_iss_candidate_ledger.md` (`f3`-shape; 3 trap-axis candidates **ISS-C6**/`C8`/`C9`, substrate-grounded/0-vault-live; land into the *pack*, not the canonical jsonl) + **DP-4 RESOLVED candidates-only** (no consumer C-029 proposal; aggregation index = 2 VideoForge seeds only; canonical md5 INVARIANT) |
| **G4 declaration** | `0f06aa6` (2026-05-31) | MANIFEST **v0.4.1 → v0.5.0 DECLARED** (Version prose + Release-History row; annotated tag DEFERRED to G6) |
| **G4 wrapper sweep** | VideoForge `ab5b178` · CanvasForge `3ebd55a` · wga `cfc5d7e` | 3 carriers re-pinned `iii/CLAUDE.md` @ v0.5.0 (`pinned_at_commit 0f06aa6` / lattice 1.2.6; `local_extensions` byte-identical; path-scoped commits) |
| **G4 close** | `fe33813` (2026-05-31) | Close bookkeeping (MANIFEST Active Consumers + STATE + charter + III CLAUDE.md) + **ISS-adopter opt-in/validation invite FIRED** (`coord_2026_05_31_iii_to_iss_adopters_validation_invite.md`) |

Campaign now at **phase G4**; **G5 + G6 pending**. DP-1..DP-4 cleared; DP-5/DP-6 (G6) pending.

## §3 AAR (G3 + G4 session)

- **Worked**: (1) Per-mission close-ceremony discipline held — both G3 and G4 closed clean with full SITREP + cascade. (2) DP-4 was resolved by *evidence* not assumption — the aggregation index was read on-disk and forced the candidates-only branch (canonical INVARIANT), exactly the charter R2-expected path. (3) The foreign-vault sweep ran safely: clean-tree pre-flight + **path-scoped commits** (`git add iii/CLAUDE.md`) meant wga's unrelated dirty file was never swept up. (4) The 2-commit declaration→sweep shape (declare v0.5.0 → consumers pin to that SHA → close bookkeeping) mirrored F4 cleanly. (5) Every cross-vault/outward-facing decision (DP-4 disposition, sweep mechanic, invite firing) went through an operator gate before action. (6) Hard invariants held end-to-end (canonical md5 / lattice / ADR count / boundary).
- **Didn't**: (1) STATE.md is now ~96K tokens with multi-thousand-token single lines — it can no longer be full-read; all edits were grep-targeted + surgical (workable but fragile). (2) The close-ceremony counter is internally inconsistent (see Finding). (3) The MANIFEST Active-Consumers table tracks only 6 of the 13 live `iii/` wrappers. (4) wga developed an unexplained mid-session working-tree change (`audit/scripts/01_login.ts`) that pre-flight had shown clean — cause unknown; left untouched.
- **Finding**: The ADR-002 §3 "wrapper sweep" as practiced (MD-A4 → F4 → G4) only ever touches the **MANIFEST-tracked 6** (lattice-labs, SiteForge, VideoForge, CanvasForge, wga, LPWhitepaper). But the live landscape is **13 `iii/` wrappers**; four *active* ones (ContextCommons v0.2.0, LatticeLabs.aDNA v0.2.0, MoleculeForge v0.3.0, ZenZachary `~0.3.0`) sit outside the tracked set and therefore drift further behind canonical with each minor bump. The sweep's scope discipline is sound, but the *tracked set* has not kept pace with wrapper proliferation — a governance-surface gap, not a correctness gap.
- **Change** (process, for G5+ and future campaigns): (a) keep the **clean-tree pre-flight + path-scoped foreign-vault commit** as the standing sweep safety rule (it earned its keep on wga this session); (b) at the next version-bump cascade (G6), reconcile the MANIFEST Active-Consumers table against the real 13-wrapper landscape so "swept the consumers" means the same as "swept the tracked table"; (c) treat the close-ceremony ordinal as derived-not-asserted — reconcile once at G6 rather than incrementing a contested counter each mission.
- **Follow-up**: enumerated in **§5** below (single consolidated ledger; supersedes the scattered per-session "Next Session Prompt" forward-carries in the G3/G4 session files).

## §4 DG-G progress ledger (8/11 — pre-seed for the G6 scorecard)

Criteria + phases per charter §7. The G6 `g6_dg_g_scorecard.md` (10-section, `f6_dg_f` shape) can lift this table.

| # | Criterion (abbrev.) | Phase | Status | Evidence |
|---|---|---|---|---|
| 1 | ISS pack landed ≥5 traps (full schema + substrate cite) | G1 | ✅ MET | `context_iii_iss_surfaces.md`; 6 traps {1,2,3,4,5,7} |
| 2 | Pack `quality_metric` composite ≥4.0 (ADR-007 §3) | G1 | ✅ MET | composite **4.00** |
| 3 | `learning_store` deepened; sd 3→4; composite 3.83→~4.00; canonical INVARIANT | G2 | ✅ MET | §Sources 5-cluster; composite **4.00** |
| 4 | Audit-ingest note + Trap-6 worked example; boundary held (no runtime/validator/ADR) | G2 | ✅ MET | `g0_audit_ingest_schema_note.md` v1.0.0 wired into `web_design` Trap-6 |
| 5 | ISS candidate ledger (≥3 candidates, honest labels) | G3 | ✅ MET | `g3_iss_candidate_ledger.md`; ISS-C6/C8/C9, 0-vault-live |
| 6 | DP-4 disposition resolved (either branch) | G3 | ✅ MET (INVARIANT branch) | candidates-only; canonical md5 `5adb0dfa…`/28 INVARIANT |
| 7 | III v0.4.1 → v0.5.0 minor bump | G4 | ✅ MET | MANIFEST `0f06aa6` |
| 8 | ADR-002 §3 wrapper sweep fired; carriers re-pinned or explicitly deferred | G4 | ✅ MET | 3 committed @ v0.5.0; rest deferred per own cadence; ISS-adopter invite fired |
| 9 | Inspection-grade validation; residues caught + negative control clean; `zero_regression_confirmed` | **G5** | ⏳ PENDING | deliverable `g5_validation_*.md` |
| 10 | Zero new III ADR (count 7); ADR-009 REJECTED recorded | G0/G6 | ✅ MET | ADR count 7 active (no adr_009 file); REJECTED at G0 |
| 11 | DG-G scorecard + AAR inline + annotated `v0.5.0` tag; cascade complete | **G6** | ⏳ PENDING | deliverable `g6_dg_g_scorecard.md` + §13 AAR + tag |

**8/11 met; 9 + 11 pending (both G5/G6 deliverables). GO → G5; no blockers.**

## §5 Consolidated open-items / forward-carry ledger

The single source of truth for "what's left." Supersedes the per-session forward-carries.

| # | Item | Target | Notes |
|---|---|---|---|
| 1 | Inspection-grade validation against the landed `iss_surfaces` pack | **G5** | ZenZachary (14 rendered gates — richest residue corpus) + MoleculeForge `p6_1` output-JSON + WilhelmAI config; **≥2 of 3**; each recommended residue caught w/ pack-line citation + negative control clean; produce `g5_validation_*.md` (5-section, MX-1/`md_b5` shape; `zero_regression_confirmed: true`). Satisfies DG-G crit 9. Consumes the G4-fired ISS-adopter invite. |
| 2 | DG-G gate + AAR + tag + cascade | **G6** | DG-G 11/11; `g6_dg_g_scorecard.md` (10-section, `f6_dg_f` shape — lift §4 above); populate charter §12 Completion Summary + §13 Campaign AAR; cut **annotated `v0.5.0` git tag** at close commit; full cascade = STATE.md + root III CLAUDE.md Campaign State + MANIFEST + **workspace-router `~/lattice/CLAUDE.md` production-pin** (the only place the production pin formally advances). |
| 3 | **Close-ceremony counter reconciliation** (logged finding) | **G6** | STATE.md labels G0-exec/G1/G2 all "19th"; charter/CLAUDE said G3="17th"; G3/G4 continued forward as 20th/21st. Audit the true count once during the G6 cascade (when STATE/charter/MANIFEST are all touched anyway) and normalize. No mid-campaign historical-label rewrite. |
| 4 | **MANIFEST Active-Consumers table undercount** (logged finding) | **G6** (or hygiene mission) | Table tracks 6 of 13 live `iii/` wrappers. At G6 cascade, consider adding the 4 active untracked carriers (ContextCommons / LatticeLabs.aDNA / MoleculeForge / ZenZachary) so the tracked set matches the swept reality. (Skeletons ScienceStanley/TaskForge + abandoned LatticeAgent stay out.) |
| 5 | **wga `audit/scripts/01_login.ts`** uncommitted change | **operator-action** | Appeared mid-session in `wga.aDNA` (not authored by this work); the G4 sweep was path-scoped to `iii/CLAUDE.md` and left it untouched/unstaged. Operator to review → commit or discard. |
| 6 | ISS-adopter `iss_surfaces` opt-in + re-pin | own cadence | Invite FIRED G4 (`coord_2026_05_31_…`). MoleculeForge / WilhelmAI (no `iii/` wrapper — ISS-only) / ZenZachary decide independently whether to add `iss_surfaces` to `packs_used` + re-pin; consumer-discretionary, no re-pin forced. |
| 7 | SiteForge F4-residue fold-in + v0.5.0 re-pin | own cadence | SiteForge `iii/CLAUDE.md` carries the edited-uncommitted F4 SIS→ISS rename; the operator folds the v0.5.0 pin into the in-flight ISS commit. Also the ISS substrate owner + natural primary `iss_surfaces` reviewer. |
| 8 | LPWhitepaper re-pin | own cadence (~Campaign E) | No `web_design`/`iss_surfaces` in `packs_used`; v0.5.0 is additive backward-compat; re-pins likely concurrent with Campaign E (post-LiteratureForge migration). |
| 9 | ContextCommons / LatticeLabs.aDNA re-pins | own cadence | Older pins (v0.2.0); multi-minor jump to v0.5.0 warrants own-cadence review. **LatticeLabs.aDNA two-step pin** has been pending since Campaign C DG-C — **overdue**; flag for the operator. |
| 10 | C-029 graduation (`image_component_omits_dimensions`) | post-campaign / natural-frequency gate | Stays a `graduation_candidate:true` row in `f3_correction_candidates.md`; fires only when a consumer vault re-observes from its own local store and proposes via ADR-008 `learning_store_graduation`. C-030..C-033 await 2nd-vault evidence. |
| 11 | Campaign E (generalized writing-III) | externally gated | Stays forward-seeded; opens only when LiteratureForge.aDNA reaches its forge-BUILD phase per MD-X2 §6. NOT subsumed by Campaign G. |
| 12 | Optional `git push origin main` | operator-discretionary | III.aDNA (G3+G4+wind-down commits) + the 3 swept consumer repos (VideoForge/CanvasForge/wga). Push is operator-discretionary per workspace standing rules. |

## §6 Hard-invariant ledger (G1 → G4)

| Invariant | G1 | G2 | G3 | G4 | Wind-down |
|---|---|---|---|---|---|
| Canonical jsonl md5 | `5adb0dfa…` | `5adb0dfa…` | `5adb0dfa…` | `5adb0dfa…` | `5adb0dfa…` (28) — **INVARIANT throughout** |
| Lattice oracle version | 1.2.5 | **1.2.5→1.2.6** (ISS registration) | 1.2.6 | 1.2.6 | 1.2.6 |
| III ADR count | 7 | 7 | 7 | 7 | 7 (ADR-009 REJECTED; no adr_009 file) |
| III version | v0.4.1 | v0.4.1 | v0.4.1 | **v0.4.1→v0.5.0 DECLARED** (tag G6) | v0.5.0 (declared) |
| Boundary | held | held | held | held | held (no III audit/ISS runtime; doc-only wind-down) |

Zero graduation across Campaign G; the canonical store never rotated (DP-4 candidates-only). The
sole intended mutation is the version bump (G4); everything else is additive/documentation.

## §7 Cross-references

- Charter: `how/campaigns/campaign_g_consolidation/campaign_g_consolidation.md` (§5 phases, §6 DPs, §7 DG-G; §12/§13 reserved for G6).
- Session logs: `how/sessions/history/2026-05/session_stanley_20260530_iii_adna_campaign_g_g3_iss_candidate_ledger.md` · `…20260531_…g4_minor_bump_wrapper_sweep.md`.
- G3 ledger: `what/artifacts/g3_iss_candidate_ledger.md`. Audit-ingest seam: `g0_audit_ingest_schema_note.md`. Recon: `g0_iss_*`.
- MANIFEST Release-History `v0.5.0` row + Active Consumers table: `MANIFEST.md`.
- Fired memo: `who/coordination/coord_2026_05_31_iii_to_iss_adopters_validation_invite.md`.
- Governance: `adr_002_consumer_federation_contract.md` §3 (sweep) · `adr_003_learning_store_ownership.md` §3/§3.6 (graduation) · `adr_007_adaptive_improvement_loop.md` §3 (quality rubric).
- AAR format precedent: Campaign F §13 (`campaign_web_design_deep_review.md`) · `how/templates/template_aar_lightweight.md`.
