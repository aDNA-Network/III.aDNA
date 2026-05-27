---
type: mission_artifact
title: "Campaign F (Operation Tell) — F5 Validation: the revised 7-trap web_design pack catches the WDR-3 residue (inspection-grade)"
campaign: campaign_web_design_deep_review
mission: F5
mission_class: validation
status: complete
version: "0.4.0"   # matches the web_design pack revision under validation; F5 is validation evidence, not new contract
created: 2026-05-26
updated: 2026-05-26
last_edited_by: agent_argus
authoring_mission: campaign_web_design_deep_review F5
governed_by:
  - what/context/core_domain_packs/context_iii_domain_packs_web_design.md   # the revised 7-trap pack under validation (F1+F2)
  - what/decisions/adr_007_adaptive_improvement_loop.md                     # §3 six-axis rubric (pack scored 4.17 at F2)
  - what/decisions/adr_003_learning_store_ownership.md                      # §3/§3.6 graduation gate (candidates stay candidates)
source_of_record:
  - what/artifacts/wdr_empirical_gap_ledger.md          # WDR-3 — the residue ledger this validation re-checks
  - what/artifacts/wdr_web_design_pack_revision_spec.md # WDR-4 — the revision spec the pack implemented
  - what/artifacts/f3_correction_candidates.md          # F3 — the 5 candidates traced here
predecessor_validation: what/artifacts/md_b5_consumer_vault_validation.md   # validation-by-inspection shape reused (frontmatter + numbered sections + zero-regression table)
sites_re_inspected: [wga.aDNA, ScienceStanley.aDNA, ContextCommons.aDNA]
evidence_method: inspection_grade   # read existing on-disk reports + source + the revised pack; NO builds, NO live audit runs
non_runnable: true
boundary_posture: boundary_preserving   # III stays semantic; no audit runtime introduced
zero_regression_confirmed: true
canonical_jsonl_touched: false
consumer_wrappers_touched: []
new_iii_adrs_authored: []
canonical_md5_baseline: "5adb0dfa38d9224649c3b2cba83852ae (28 entries)"
satisfies: "DG-F criterion 10 — Validation (F5) confirms the revised pack catches the WDR-3 residue cases"
tags: [mission_artifact, f5, validation, campaign_f, operation_tell, web_design, wdr3_residue, inspection_grade, trap_5_precision, trap_6_cwv_design_debt, trap_7_ai_slop, semantic_heading, deferred_residue, negative_control, candidates_only, canonical_md5_invariant, zero_regression, dg_f_crit_10]
---

# F5 Validation — Revised `web_design` Pack vs the WDR-3 Residue

## §1 Purpose

F5 closes the loop opened by the planning mission's empirical recon. WDR-3
([[wdr_empirical_gap_ledger]]) bucketed every finding across the three built sites
(wga · ScienceStanley · ContextCommons) into **machine-caught** (a SiteForge gate flags it),
**III-trap-caught** (an existing trap/correction covers it), and **neither-caught** — the
*residue*, which was the campaign's whole target. F1+F2 then revised the pack to own that
residue. **F5 verifies the claim**: walk each "neither-caught" residue finding against the
**revised 7-trap pack** and confirm it is now either (a) caught — with the exact pack line
that owns it quoted — or (b) consciously, on-record **deferred** per the F0 trap-set decision.

This is the validation discipline used at MC-5 / MD-A5 / MD-B5: **documentation-grade by
inspection**. No builds, no live Lighthouse/axe runs. F5 reads the same on-disk reports + source
WDR-3 read, plus the revised pack, and confirms coverage. The first executable web-review run
lands when a SiteForge `iii/` consumer next reviews an Astro artifact under the v0.4.0 pin —
F5 proves the pack is *ready* for that, not that a runtime executed.

**Satisfies DG-F criterion 10.** F5 is the penultimate Campaign-F mission; F6 (DG-F GO/NO-GO
gate + AAR + annotated `v0.4.0` tag) is a separate, human-gated session (DP5).

## §2 Inputs

| Input | Path | Role |
|---|---|---|
| Revised pack (under validation) | `what/context/core_domain_packs/context_iii_domain_packs_web_design.md` | 7 traps (Trap 6+7 landed F1; Trap 3/4/5 hardened F2); composite 4.17 |
| WDR-3 gap ledger (source-of-record) | `what/artifacts/wdr_empirical_gap_ledger.md` | The residue buckets re-checked here (§1 wga, §2 SS, §3 CC, §4 synthesis) |
| WDR-4 revision spec | `what/artifacts/wdr_web_design_pack_revision_spec.md` | What the pack was supposed to implement (the contract F1/F2 met) |
| F3 candidate ledger | `what/artifacts/f3_correction_candidates.md` | C-029..C-033 — traced to residue rows in §8 |
| Canonical learning store | `what/context/core_domain_packs/iii_corrections_canonical.jsonl` | md5 `5adb0dfa38d9224649c3b2cba83852ae` / 28 entries — **invariant** (untouched by F5) |

## §3 Method

For each WDR-3 **neither-caught** row, this validation: (1) restates the residue finding + its
on-disk evidence (verbatim from WDR-3, no re-derivation); (2) names the revised-pack element
that now owns it; (3) **quotes the pack line** so coverage is checkable, not asserted; (4) marks
**CAUGHT** or **DEFERRED (on-record)**. A finding is only CAUGHT if the pack element names the
*same signal* — not merely an adjacent topic. Deferred residue is held to the same honesty bar:
it must be deferred *on the record* (F0 decision + a candidate-ledger row), not silently dropped.

## §4 Residue → revised-pack coverage matrix

The "neither-caught" residue from WDR-3 §1–§3, mapped to the revised pack. (Pack line numbers
are the current `context_iii_domain_packs_web_design.md`.)

| # | WDR-3 residue finding (the "neither" bucket) | Site(s) | Now owned by | Verdict |
|---|---|---|---|---|
| R1 | `ContentImage` omits width/height + `loading="lazy"` on LCP hero → CLS 0.168 | SS (strong) | **Trap 6** example (pack L127, verbatim `ContentImage.astro:19`) | ✅ CAUGHT |
| R2 | Webfont reflow — bare `@fontsource`, no `font-display`/preload → CLS | SS | **Trap 6** example + detection (pack L127, L131 "font imports for `font-display`/preload") | ✅ CAUGHT |
| R3 | Home eager full-bleed hero photo → LCP 2.9s (design-debt class, weaker) | wga | **Trap 6** (2nd-vault weak-positive; detection L131 LCP threshold <2.5s) | ✅ CAUGHT |
| R4 | Same ecoregion → divergent hex across pages, no shared source | wga | **Trap 5 precision sub-signal** (pack L120, verbatim wga ecoregion case) | ✅ CAUGHT |
| R5 | Trap-5 would false-positive the legit legend hex while missing the real bug | wga | **Trap 5 precision sub-signal** (L120 "would be a **false positive**") | ✅ CAUGHT (FP-prevented) |
| R6 | Brand Gate 8 review-only → AI-slop *composition* judgment un-automated | wga/SS | **Trap 7** coverage rationale (pack L142, "Brand Gate 8 is screenshot + human-review only") | ✅ CAUGHT (seam) |
| R7 | Centered glass hero + 3-card auto-fill grid + nebula gradient-blob silhouette | SS (partial) | **Trap 7** example + detection (pack L138, L142 "silhouette + content together") | ✅ CAUGHT (silhouette+content) |
| R8 | Flat heading nesting (section `<h2>` sibling to card `<h2>`) — axe scores 100 | SS | **Trap 3 semantic-heading sub-signal** (pack L90, verbatim `index.astro:46` vs `ProjectCard.astro:31`) | ✅ CAUGHT |
| R9 | No view transitions on a multi-page flow (motion/interaction absence) | SS (refuted ×2) | **DEFERRED** — motion candidate-trap, not landed per F0; ledger row **C-032** | ⏸ DEFERRED (on-record) |
| R10 | Responsive-breakpoint neglect (3→1, no 2-col tier; 768/480 only) | SS (mild) | **DEFERRED** — 1-vault thin; not landed, not even a candidate | ⏸ DEFERRED (on-record) |

**Tally: 8 of 8 in-scope residue findings CAUGHT; 2 deferred residue findings on-record.**
No residue finding is silently uncovered.

## §5 Per-trap validation walk (exact pack-line citations)

### Trap 6 (CWV-as-Design-Debt) — owns R1, R2, R3

The single most prevalence-AND-coverage-justified new trap (WDR-3 §4: 2-vault — SS strong,
wga weak). The pack landed it with the residue cases **as its worked example**:

- **R1 (SS `ContentImage` no-dims + lazy LCP):** pack L127 reads —
  *"A reusable image component ships `loading="lazy"` with **no intrinsic `width`/`height`** … so the LCP hero lazy-loads and CLS rises (ScienceStanley `ContentImage.astro:19`, CLS 0.168)."*
  This is the WDR-3 §2 "STRONG" residue case named verbatim. ✅
- **R2 (SS webfont reflow):** same example line continues — *"Or webfonts imported (`@fontsource`) without `font-display`/preload, causing reflow CLS"*; detection method L131 — *"check font imports for `font-display`/preload."* Matches WDR-3 §2 `BaseLayout.astro:5-6` (CLS 0.168 Inter reflow). ✅
- **R3 (wga eager hero LCP 2.9s):** Trap 6's detection (L131) keys on *sub-1.0 CWV audit scores* read from "the **existing** Lighthouse JSON SiteForge already writes" with the explicit threshold *"LCP < 2.5s"*. wga home LCP 2.9s (`lh-home.json` 0.79) breaches that threshold and traces to `Hero.astro:20 loading="eager"` full-bleed photo — the weaker 2nd-vault instance of the same design-debt class. ✅

**Boundary check (Trap 6 specifically):** L131 — *"Read the **existing** Lighthouse JSON … **do not re-run the audit** (audit execution is SiteForge's; III reads the number and attributes the cause)."* The trap is a *reader/attributor*, never an audit runtime. WDR-3's numbers (CLS 0.168, LCP 2.9s) are exactly the on-disk values F5 re-reads — nothing re-run.

### Trap 5 (Design Token Drift) precision sub-signal — owns R4, R5

WDR-3 §1's most defensible *content* finding: the bare hex-scan would conflate two defects and
**false-positive** legitimate legend data. The pack's F2 precision sub-signal (L120) resolves it:

- **R4 (divergent hex, no shared source):** L120 — *"a *semantic data value that is legitimately a literal but lacks a single shared source-of-truth* (e.g. a map legend color: the literal is fine, the bug is the same logical value rendered as different literals across pages)."* Then names the case: *"the wga ecoregion color appears as different hex in `atlas.astro`, `stories.astro`, and `StoryCard.astro`."* This is WDR-3 §1's Central-Valley/Sierra divergence verbatim. ✅
- **R5 (FP-prevention):** L120 — *"flagging the literal as 'drift' would be a **false positive**; the real defect is cross-page inconsistency from no shared source."* The trap now *prevents* the false positive WDR-3 warned the bare trap would emit, and redirects to the correct fix (one shared map / content-collection idiom, not a CSS var). ✅

### Trap 7 (AI-Slop Composition) — owns R6, R7

The highest-*value* trap (anti-slop mandate) — justified by the **coverage seam**, not by
prevalence on these well-tended sites (WDR-3 §4: 0 strong positives, seam-justified):

- **R6 (Gate-8 seam):** pack L142 — *"SiteForge **Brand Gate 8 is screenshot + human-review only** — AI-slop *composition* judgment is explicitly *un-automated* … III owns the semantic/code-level tells so the deferral has a home in the agentic review."* This is WDR-3 §1's `gate_08_brand.json "status":"review"` seam — the trap exists *because no robot owns it*. ✅
- **R7 (SS silhouette):** L138 example names *"Centered glass hero … + nebula radial-gradient blob + `auto-fill minmax(300px,1fr)` 3-card grid … (the silhouette ScienceStanley partially exhibits)."* Matches WDR-3 §2 (`global.css:155`, `Hero.astro:17,52`). Critically, L142 detection requires judging *"silhouette + content together, not silhouette alone (a bespoke site may legitimately use a hero + cards)"* — which is exactly why WDR-3 filed SS as **partial/honest-weak**, not a strong slop hit. The trap encodes that honesty. ✅

### Trap 3 (Accessibility Theater) semantic-heading sub-signal — owns R8

- **R8 (flat heading nesting):** pack L90 — *"a flat outline (a section `<h2>` sibling to a subordinate card `<h2>`) scores 100 on axe yet is a real hierarchy defect (ScienceStanley `index.astro:46` vs `ProjectCard.astro:31`)."* This is WDR-3 §2's residue case verbatim, including the "scores 100 on axe" coverage-gap framing. ✅
  *(The pack notes it "may alternatively be read under Trap 1 Visual Hierarchy" — either way the signal is owned.)*

## §6 Deferred-residue honesty ledger

Two residue findings are **not** caught by the revised pack — *by design*, decided at F0
(Trap 6 + Trap 7 in; motion + responsive deferred). F5's job is to confirm each deferral is
**on the record**, not silently dropped:

| Residue | Why deferred | On-record where | Correctness evidence |
|---|---|---|---|
| **R9 motion/interaction-design absence** (SS no view transitions) | 1-vault thin; F0 deferred motion from the landed trap set | F3 ledger **C-032** (`view_transitions_absent_on_multipage_flow`, `graduation_candidate:true`); WDR-3 §4 "candidate — thin; graduate later" | **Refuted on 2 of 3 sites** — wga + ContextCommons both ship `<ClientRouter/>` view transitions. The 2-site refutation is precisely why landing a motion trap now would over-fit a single positive. Deferral is the *correct* call. |
| **R10 responsive-breakpoint neglect** (SS 3→1, no 2-col tier) | 1-vault, mild; F0 did not even elevate it to a candidate | WDR-3 §2/§4 "candidate — thin; graduate later"; **no** C-0xx row (correctly below the candidate bar) | ContextCommons "INCONCLUSIVE (leans clean)"; wga not evidenced. Single mild positive — below even the candidate threshold. Recording it as residue-noted-but-not-landed is the honest disposition. |

This is the **candidates-only posture working as designed**: thin residue does not get a trap; it
gets a candidate row (or a note) and waits for cross-vault frequency. The pack did **not** inflate
coverage to claim these.

## §7 ContextCommons negative control (false-positive check)

WDR-3 §3 makes ContextCommons the campaign's **negative control**: a high-quality build that
**refutes all four primary design-residue candidates** (AI-slop-hero ABSENT, CWV-as-design-debt
ABSENT — LCP 0.4s/CLS 0/hero preloaded+`fetchpriority`, motion-absence ABSENT, token-drift ABSENT
— zero hardcoded hex). F5 confirms the revised pack **respects** this:

- **Trap 6** keys on *sub-1.0 CWV scores* — CC's 1.0/1.0/1.0 + LCP 0.4s + CLS 0 produce **no breach to attribute** → Trap 6 correctly stays silent. ✅ (no false positive)
- **Trap 7** requires *silhouette + content together*; CC's asymmetric pixel-art `<picture>` hero + intentional grids fail the silhouette test → correctly **not** flagged. ✅ (the L142 "judge together, not silhouette alone" clause is what prevents the FP)
- **Trap 5** precision sub-signal: CC has zero hardcoded hex → nothing to flag, and no legitimate-data-literal to false-positive. ✅
- CC independently corroborates only **C-015** (hardcoded entity strings) + **C-016** (inline data) — **existing** corrections, not new traps. The new F1/F2 surface adds **zero** false positives on the clean site.

This is the strongest single piece of evidence that the new traps are *judgments*, not
silhouette-matching false-positive machines: on a genuinely clean site, they stay quiet.

## §8 Candidate traceability (C-029..C-033 → residue rows)

Every F3 candidate traces to a residue row in §4, and each maps to a **landed** trap or the
**deferred** motion candidate-trap. None graduated (candidates-only; canonical untouched):

| Prov. ID | Pattern | Residue row | Maps to | Landed? |
|---|---|---|---|---|
| C-029 | `image_component_omits_dimensions` | R1 + R3 | Trap 6 | ✅ landed (2-vault) |
| C-030 | `semantic_data_value_no_shared_source` | R4 + R5 | Trap 5 precision sub-signal | ✅ landed (1-vault) |
| C-031 | `lcp_element_lazy_loaded` | R1 (lazy-LCP standalone) | Trap 6 | ✅ landed (1-vault) |
| C-032 | `view_transitions_absent_on_multipage_flow` | R9 | motion candidate-trap | ⏸ **not landed** (deferred) |
| C-033 | `webfont_reflow_no_font_display` | R2 | Trap 6 | ✅ landed (1-vault) |

4 of 5 candidates map to a landed trap (6 or 5); C-032 maps to the deliberately-deferred motion
candidate-trap. All 5 remain `graduation_candidate:true` in the F3 ledger — **zero** appended to
the canonical store.

## §9 Boundary re-assertion (DG-F criterion 9)

F5 confirms the campaign's boundary posture held through the pack revision:

1. **No III audit runtime.** Trap 6 (L131) and the relationship table (L165) both state III
   *reads* the Lighthouse/axe numbers SiteForge already produces and *attributes* them — *"mechanical
   execution stays in SiteForge; semantic attribution is III's."* No trap re-runs an audit.
2. **No audit-execution ADR authored** in Campaign F (ADR count stays 7).
3. **The audit-signal-ingest seam stays documented-only** — Trap 6's "read the existing JSON"
   detection is the documented seam; no ingest pipeline was built (WDR-4 §5).
4. F5 itself runs **no builds, no audits** — it re-reads the on-disk reports WDR-3 read.

## §10 Zero-regression / hard-invariant table

| Invariant | Pre-F5 | Post-F5 | Status |
|---|---|---|---|
| Canonical jsonl md5 | `5adb0dfa38d9224649c3b2cba83852ae` | `5adb0dfa38d9224649c3b2cba83852ae` | ✅ INVARIANT |
| Canonical jsonl `wc -l` | 28 | 28 | ✅ INVARIANT |
| Graduations fired | 0 | 0 | ✅ candidates-only |
| Consumer wrappers edited | 0 | 0 | ✅ none |
| New III ADRs | 7 total (000+001+002+003+005+007+008) | 7 total | ✅ no new ADR |
| Lattice yaml version | 1.2.5 | 1.2.5 | ✅ unchanged |
| Pack composite (ADR-007 §3) | 4.17 | 4.17 (not re-scored; F5 validates coverage, not score) | ✅ stable |
| III audit runtime | none | none | ✅ boundary held |
| Builds / live audits run by F5 | 0 | 0 | ✅ inspection-grade |

**Zero regression CONFIRMED. DG-F criterion 10 satisfied — the revised 7-trap `web_design`
pack catches all 8 in-scope WDR-3 residue findings (with exact pack-line citations); the 2
deferred residue findings (motion-absence, responsive-neglect) are on-record per the F0
trap-set decision + the C-032 candidate row; ContextCommons confirms zero new false positives
on a clean site.**

## §11 Cross-references

- **Validates**: `what/context/core_domain_packs/context_iii_domain_packs_web_design.md` (revised 7-trap pack, F1+F2).
- **Source-of-record**: [[wdr_empirical_gap_ledger]] §1–§4 (residue buckets) · [[wdr_web_design_pack_revision_spec]] (WDR-4 revision contract) · [[f3_correction_candidates]] (C-029..C-033).
- **Governance**: `what/decisions/adr_007_adaptive_improvement_loop.md` §3 (6-axis rubric) · `adr_003_learning_store_ownership.md` §3/§3.6 (graduation gate — why candidates stay candidates).
- **Charter / gate**: `how/campaigns/campaign_web_design_deep_review/campaign_web_design_deep_review.md` (DG-F criterion 10).
- **Precedent shape**: `what/artifacts/md_b5_consumer_vault_validation.md` (validation-by-inspection: frontmatter + numbered sections + zero-regression table).
- **Next**: F6 — DG-F GO/NO-GO gate (DP5) + Campaign AAR + annotated `v0.4.0` tag (separate, human-gated session).
