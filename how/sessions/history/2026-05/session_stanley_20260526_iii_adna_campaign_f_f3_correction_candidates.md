---
type: session
created: 2026-05-26
updated: 2026-05-26
last_edited_by: agent_argus
tags: [session, campaign_f, operation_tell, web_design, correction_candidates, candidates_only, boundary_preserving, canonical_md5_invariant]
session_id: session_stanley_20260526_iii_adna_campaign_f_f3_correction_candidates
user: stanley
started: 2026-05-26T00:00:00Z
status: completed
intent: "Campaign F (Operation Tell) Phase 2 — Mission F3: author the 5 correction candidates (C-029..C-033 provisional) from WDR-4 §4 / WDR-3 §4 into a Markdown candidate ledger (what/artifacts/f3_correction_candidates.md), NEVER the canonical jsonl. Candidates-only (graduate at a later natural-frequency gate). Hard invariant: canonical jsonl md5 5adb0dfa38d9224649c3b2cba83852ae / 28 lines unchanged. Operator chose Markdown ledger over staging .jsonl (2026-05-26)."
executes_mission: "campaign_web_design_deep_review F3"
campaign: campaign_web_design_deep_review
operator_decisions:
  candidate_home: "Markdown candidate ledger (what/artifacts/f3_correction_candidates.md) — chosen over staging .jsonl 2026-05-26"
  graduation_posture: "candidates only — graduate later"
canonical_md5_baseline: "5adb0dfa38d9224649c3b2cba83852ae (28 entries) — recorded at startup"
canonical_md5_post: "5adb0dfa38d9224649c3b2cba83852ae (28 entries) — INVARIANT verified post"
completed: 2026-05-26T00:00:00Z
files_created:
  - how/sessions/active/session_stanley_20260526_iii_adna_campaign_f_f3_correction_candidates.md
  - what/artifacts/f3_correction_candidates.md
files_modified:
  - how/campaigns/campaign_web_design_deep_review/campaign_web_design_deep_review.md
  - STATE.md
---

## Activity Log

- Session started. Startup: `git pull` already up to date; canonical md5 baseline confirmed `5adb0dfa38d9224649c3b2cba83852ae` (28 entries); no other active sessions. Plan ratified in plan mode — F3 only this session; Markdown candidate ledger (operator choice over staging `.jsonl`).
- **F3** — authored `what/artifacts/f3_correction_candidates.md`: 5 schema-conformant candidate rows (C-029…C-033 prov.) each carrying `graduation_candidate:true` / `graduated:false` / `graduated_to:null`, with per-candidate Evidence + Disposition lines. Honest-evidence posture: only C-029 (`image_component_omits_dimensions`) is genuinely 2-vault (SS+wga); C-030 (`semantic_data_value_no_shared_source`, wga), C-031 (`lcp_element_lazy_loaded`, SS), C-032 (`view_transitions_absent_on_multipage_flow`, SS — refuted on wga+CC), C-033 (`webfont_reflow_no_font_display`, SS) are 1-vault awaiting a 2nd vault. `accepted:null` documented as an intentional pre-proposal candidate-ledger deviation. 3 reinforced existing corrections (C-012/C-015/C-016) recorded as future re-graduation inputs — no action.
- **Invariant** — `md5 iii_corrections_canonical.jsonl` == `5adb0dfa38d9224649c3b2cba83852ae`, `wc -l` == 28: verified before authoring AND after; all 5 candidate JSON blocks parse (python json.load) with `graduation_candidate=True` / `graduated=False`.
- **State cascade** — charter F3 row → ✅ completed + Phase-2 exit gate checked + `phase: 1 → 2` + `updated: 2026-05-26`; STATE.md frontmatter (`updated` prefix + `last_session`/`prior_session` + tags) + Current Phase lead paragraph (F3-complete prepend + DG-F criteria 5/6 green + Queue F4-next/DP4-gated). No router (`~/lattice/CLAUDE.md`) or III root CLAUDE.md edit — Campaign State row there already reads "F3 pending"→ left for F4/close-ceremony cascade (Standing Rule 7: router carries routing identity; campaign state lives in STATE.md/charter).

## SITREP

**Completed**: Campaign F Phase 2 / Mission F3 — 5 correction candidates (C-029…C-033 prov.) recorded in a Markdown candidate ledger; candidates-only; canonical jsonl untouched (md5 `5adb0dfa…` / 28 invariant). DG-F criteria 5 + 6 now green.
**In progress**: none (F3 done; session closing).
**Next up**: **F4 (Phase 3)** — III minor bump `v0.3.0 → v0.4.0` + ADR-002 §3 wrapper sweep across the 5 `web_design`-carrying wrappers (SiteForge primary; VideoForge/CanvasForge/LPWhitepaper may defer; lattice-labs untouched). Gated at **DP4** (operator approves the bump + each consumer's re-pin). Then F5 (validation vs WDR-3 residue) → F6 (DG-F + AAR + `v0.4.0` tag). All human-gated per Standing Rule 7.
**Blockers**: none.
**Files touched**: 1 artifact created (`f3_correction_candidates.md`) + 1 session created; charter + STATE.md modified. Canonical jsonl + consumer wrappers + ADRs + lattice yaml + pack file UNTOUCHED.

## AAR (lightweight)

- **Worked**: the candidate set was paste-ready from WDR-3 §4 / WDR-4 §4 — F3 was near-mechanical because the planning mission already banked the cross-site provenance. Recording candidates *outside* the canonical store made the md5-invariant check trivially true (the file was never opened for writing).
- **Didn't**: nothing blocked. The one judgment call surfaced honestly: the charter's "5 candidates with ≥2-vault evidence" overstates the corpus — only C-029 reaches 2-vault. Resolved by recording each candidate's *actual* provenance and an explicit "awaiting 2nd vault" disposition, rather than inflating `frequency`.
- **Finding**: a candidate ledger needs a place to be honest about *negative* evidence — C-032 is observed on SS but *refuted* on wga + CC. Capturing the refutation inline stops a future reviewer from over-weighting a single positive. The canonical schema has no field for "refuted-on," so the ledger carries it as prose.
- **Change**: when a charter's evidence-strength language (here "≥2-vault") is more optimistic than the empirical ledger supports, honor the ledger and note the gap in both the deliverable and the phase-exit-gate text — do not pad the metric to match the charter.
- **Follow-up**: F4 is the first consumer-facing propagation (minor bump + wrapper sweep) and is DP4-gated. The candidates here graduate only at a *later* natural-frequency gate, when a consumer vault re-observes a pattern from its own local store and proposes it via ADR-003 §3 — not in Campaign F.

## Next Session Prompt

Campaign F ("Operation Tell") is at **Phase 2 complete** in III.aDNA. F3 recorded the 5 correction candidates (C-029…C-033 prov.) in `what/artifacts/f3_correction_candidates.md` as `graduation_candidate:true` rows — candidates-only, canonical jsonl md5 `5adb0dfa38d9224649c3b2cba83852ae` (28 entries) INVARIANT. **Next mission F4 (Phase 3)**: bump III `v0.3.0 → v0.4.0` (CHANGELOG entry) and run the ADR-002 §3 minor-bump wrapper sweep across the 5 `web_design`-carrying consumer wrappers — SiteForge (primary; re-pin), wga (re-pin), VideoForge + CanvasForge + LPWhitepaper (review + re-pin or *defer* per consumer; UI-register / whitepaper surfaces may legitimately defer); lattice-labs/iii v0.1.0 stays TRACKED-DEFERRED. **F4 is gated at Decision Point 4** — operator approves the minor bump + each consumer re-pin before edits land; forward-stub discipline (Standing Rule 2), no active consumer breaks. Then F5 (validation: re-check the WDR-3 residue cases against the revised 7-trap pack, inspection-grade) → F6 (DG-F gate + AAR + annotated `v0.4.0` tag). Charter + DG-F 11-criterion gate at `how/campaigns/campaign_web_design_deep_review/`; the 5 `wdr_*` source-of-record artifacts are frozen. All phase advances are human gates (Standing Rule 7).
