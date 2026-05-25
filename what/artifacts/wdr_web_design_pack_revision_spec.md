---
artifact_class: web_design_pack_revision_spec
type: artifact
title: "WDR Deliverable 4 — web_design pack-revision SPEC + new-trap/correction candidates (proposal only)"
mission: plan_web_design_deep_review_charter
designs_campaign: campaign_web_design_deep_review
created: 2026-05-25
updated: 2026-05-25
last_edited_by: agent_argus
status: proposal
boundary_posture: boundary_preserving
hard_invariant: "PROPOSAL ONLY — no live edit to context_iii_domain_packs_web_design.md; no jsonl append; canonical md5 5adb0dfa38d9224649c3b2cba83852ae invariant; zero graduation fires"
graduation_posture: "candidates only — graduate later (operator decision 2026-05-25)"
tags: [artifact, web_design, pack_revision, spec, proposal, trap_candidates, correction_candidates, source_diversity_remediation, audit_signal_ingest]
---

# WDR-4 — `web_design` Pack-Revision Spec (PROPOSAL)

> **Hard boundary**: this is a *specification* for the future `campaign_web_design_deep_review`. It edits **nothing**. The live `context_iii_domain_packs_web_design.md` is untouched; the canonical jsonl stays at md5 `5adb0dfa…` (28 entries); **zero graduations fire**. All traps/corrections below are **candidates** the execution campaign lands after the charter is approved. Per operator decision, new corrections ship as `≥2-vault`-evidenced candidates and graduate at a *later* natural-frequency gate, not within the campaign.

## §1 What the evidence supports (and what it doesn't)

The empirical corpus (WDR-3) is three *already-high-quality* sites — so the revision is **evidence-disciplined, not aspirational**. Three changes are well-supported; two new traps are thin-but-real (candidates); existing traps need citation-hardening (the `source_diversity` fix). Summary of dispositions:

| Change | Type | Evidence strength | Disposition |
|--------|------|-------------------|-------------|
| **Trap 6: CWV-as-Design-Debt** | NEW trap | 2-vault (SS strong + wga weak) | **propose for inclusion** |
| **Trap 7: AI-Slop Composition** | NEW trap | seam-justified (Gate 8 manual-only); prevalence low | **propose for inclusion** (highest value, anti-slop mandate) |
| Trap 3 hardening (a11y coverage ceiling + SC 2.5.8 + semantic-heading) | REFINE | external (Deque/WebAIM) + 1-vault (SS) | **propose** |
| Trap 4 hardening (earned-credibility / naturalness) | REFINE | external (Google spam policy) | **propose** |
| Trap 5 **precision fix** (semantic-data vs token) | REFINE | 1-vault strong (wga ecoregion) | **propose** (most defensible content change) |
| `source_diversity` remediation (add citations) | METADATA | the 15 WDR-1 citations | **propose** (raises axis 3→4/5) |
| motion/interaction-design-absence | NEW trap (thin) | 1-vault (SS) | **candidate only** — defer |
| responsive-breakpoint-neglect | NEW trap (thin) | 1-vault mild (SS) | **candidate only** — defer |

## §2 New trap candidates (pack-format; ready to paste-adapt at execution)

### Trap 6 (proposed): Core-Web-Vitals-as-Design-Debt

| Field | Value |
|-------|-------|
| **Description** | A Core Web Vital (CLS/LCP/INP) breach that traces to a *design or component decision* — not a one-off perf glitch. The number is SiteForge's (Gate 3/7); the **design attribution** is III's. |
| **Example** | A reusable image component ships `loading="lazy"` with **no intrinsic `width`/`height`**, relying on CSS `aspect-ratio` — so the LCP hero lazy-loads and CLS rises (ScienceStanley `ContentImage.astro:19`, CLS 0.168). Or webfonts imported without `font-display`/preload, causing reflow CLS. |
| **Where It Hides** | Shared image/media components; hero components with eager-vs-lazy mismatches; bare `@fontsource` imports in the base layout; any `client:load` on below-the-fold content (INP). |
| **Severity Default** | High (design-debt compounds across every page using the component) |

**Detection method**: read the existing Lighthouse JSON (SiteForge's `lh-*.json` / baselines) for sub-1.0 CWV audit scores; then trace each to its source — `grep` image components for missing `width`/`height` + `loading` attribute mismatches on LCP elements; check font imports for `font-display`/preload; check `client:*` directives on static/below-fold content. **Do not re-run the audit** — read the number, attribute the cause. Cross-reference Gate 3/Gate 7 (mechanical) vs this trap (design attribution). [CWV thresholds: LCP<2.5s · INP<200ms · CLS<0.1 — WDR-1 §1]

### Trap 7 (proposed): AI-Slop Composition

| Field | Value |
|-------|-------|
| **Description** | Generic AI-generated *structural/visual* composition — the centered-hero + uniform 3-card grid + gradient-blob silhouette, uniform padding/radius, default Inter-400 + purple gradient. Complements (does not duplicate) the **Template Voice** trap, which catches the *copy* half ("Empower/Elevate/Unlock"); this catches the *layout* half. |
| **Example** | Centered glass hero + nebula radial-gradient blob + `auto-fill minmax(300px,1fr)` 3-card grid with identical radius/padding — passing every Lighthouse gate while reading as template (the silhouette ScienceStanley partially exhibits; the composition Brand Gate 8 punts to human eyes). |
| **Where It Hides** | Landing-page heroes; feature-card sections; any section a generator would emit by default. Hides *behind green gates* — this is the canonical "passes the robots, still reads as slop" residue. |
| **Severity Default** | Medium (High when combined with Template Voice copy on the same section) |

**Detection method**: scan page structure for the slop signature — centered hero (eyebrow + large vague headline + two CTAs) immediately followed by a 3-up uniform card grid + decorative gradient blob; check for default-only typography (single Inter weight, no type system) and purple/blue gradient reuse across hero+CTA+accent. **Coverage rationale (cite in pack)**: SiteForge Brand Gate 8 is screenshot+human-review only — AI-slop composition judgment is explicitly *un-automated*, so III owns the semantic/code-level tells. [WDR-1 §5; WDR-3 §1 gate_08 "review" status]

### Candidate traps (DEFER — 1-vault thin; ship as candidates, graduate later)

- **motion/interaction-design-absence** — no view transitions / unconsidered interaction states on a multi-page flow (ScienceStanley: zero `astro:transitions`; but wga + CC both *have* `<ClientRouter/>`). 1-vault → candidate.
- **responsive-breakpoint-neglect** — layout that doesn't overflow (Gate 9 green) but degrades poorly at a breakpoint, e.g. a 3→1 column jump with no intermediate tier (ScienceStanley, 2 breakpoints sitewide). 1-vault mild → candidate.

## §3 Existing-trap hardening (the `source_diversity` remediation)

The pack scores `source_diversity=3` *because it carries no ADR/skill/external citations*. These refinements add them — raising the axis without inventing content.

- **Trap 3 (Accessibility Theater) — harden**:
  - Add the coverage-ceiling evidence: automated tools catch ~57% of issues *by volume* / ~30% *by success criterion* — this is *why* a semantic a11y trap must exist alongside Gate 4. [Deque; WebAIM — WDR-1 §2]
  - Add **WCAG 2.2 SC 2.5.8 Target Size (≥24×24px)** as a sub-signal (layout-density; violated by tight AI card/icon grids).
  - Add **semantic-heading-hierarchy** sub-signal: flat nesting (section `<h2>` sibling to subordinate card `<h2>`) scores 100 on axe yet is a real hierarchy defect (ScienceStanley `index.astro:46` vs `ProjectCard.astro:31`). (Could alternatively extend Trap 1 Visual Hierarchy — execution-campaign decision.)
- **Trap 4 (SEO Stuffing) — harden**: add the *earned-credibility / naturalness* dimension — "valid JSON-LD but unearned-credibility or not-visible-on-page" is spam-policy-relevant even when schema validates. [Google Search spam policy + gen-AI guidance — WDR-1 §3]. Reinforces existing **C-014**.
- **Trap 5 (Design Token Drift) — PRECISION FIX (most defensible change)**: the current trap flags any raw hex as drift. The wga ecoregion case (same region → different hex across `atlas.astro`/`stories.astro`/`StoryCard.astro`) shows two distinct defects the current trap conflates:
  1. *raw value that should be a token* (true drift → use `var(--…)`); vs
  2. *semantic data value that should have one shared source-of-truth* (a legend color is legitimately a literal, but must come from a single shared map — the bug is duplication/inconsistency, not the literal itself).
  Amend the detection method to distinguish these and add the "no shared source-of-truth for semantic data values" sub-signal. Prevents false positives on legitimate legend/data colors while catching the real cross-page inconsistency. Cite the Astro content-collections idiom (data-as-collection) [WDR-1 §4].

**`source_diversity` target**: add ≥6 distinct external citations (Deque, WebAIM, Google spam policy, Astro docs, CWV sources, 925studios) + cross-references to ADR-002/ADR-003 + `skill_iii_review.md`. Projected axis lift: **3 → 4 or 5**. (`graduation_yield=1` is the separate MB4-2 measurement artifact resolved at MD-B6 via ADR-007 §3.6 — *not* a content gap; out of scope for pack content but noted.)

## §4 Correction candidates (CANDIDATES — NOT appended; canonical md5 invariant)

Provisional IDs `C-029+` are **placeholders**, assigned only at a future graduation ceremony. These are spec rows, not jsonl entries.

| Provisional ID | Pattern | Trap | ≥2-vault evidence | Graduation posture |
|---|---|---|---|---|
| C-029 (prov.) | `image_component_omits_dimensions` (shared image component lacks `width`/`height`; lazy-loads LCP element) | Trap 6 | SS `ContentImage.astro:19` (strong) + wga eager-hero (weak) | candidate — strongest; defer |
| C-030 (prov.) | `semantic_data_value_no_shared_source` (same logical value → different literals across pages) | Trap 5 | wga ecoregion (strong, 1-vault) | candidate — defer pending 2nd vault |
| C-031 (prov.) | `lcp_element_lazy_loaded` | Trap 6 | SS (1-vault) | candidate — defer |
| C-032 (prov.) | `view_transitions_absent_on_multipage_flow` | motion candidate | SS (1-vault) | candidate — defer (wga/CC refute) |
| C-033 (prov.) | `webfont_reflow_no_font_display` | Trap 6 | SS CLS 0.168 (1-vault) | candidate — defer |

**Reinforced existing corrections (fresh ≥2-vault provenance, no action needed now)**: C-012 `brand_color_dual_role` (wga+SS), C-015 `hardcoded_entity_strings` (wga+CC), C-016 `inline_data_instead_of_collection` (wga+CC). Recorded for WDR-6's graduation-path discussion.

## §5 Audit-signal-ingest note (DOCUMENTED OPTION ONLY — not built)

Per operator decision (boundary-preserving; document-only), this records *how* III would consume SiteForge audit signals as **input** to Trap 6, without ever running an audit:

- **Read seam**: SiteForge already writes `lh-*.json` (Lighthouse), `tests/evidence/gates/gate_04_a11y.json` (axe), and per-gate evidence to disk. Trap 6's detection method **reads these existing artifacts** for sub-threshold CWV/a11y scores, then performs the *semantic attribution* (code trace to the design decision). III never invokes Lighthouse/axe/Playwright.
- **Contract shape (proposal)**: a thin optional `audit_signal_ref` in the SiteForge `iii/` wrapper pointing at the site's report dir + report-freshness note; III treats stale/absent reports as "no signal" (advisory downgrade), never as a failure. Mirrors the airlock v0.3 `assumes_draft` advisory-downgrade discipline.
- **Explicitly NOT in this campaign**: no `audit_signal_ref` field added to ADR-002; no III runtime; no ingest module. This paragraph is the documented seam for a *future* decision — nothing is built.

## §6 What the execution campaign does with this spec
1. Land Trap 6 + Trap 7 into `context_iii_domain_packs_web_design.md` (live edit — that campaign's scope, Stanley-gated per Standing Rule 6/7).
2. Apply the three trap hardenings + `source_diversity` citations; re-score the pack (expect composite ≥4.00 hold, source_diversity 3→4/5).
3. Hold the 5 correction candidates as `graduation_candidate: true` in the *spec* / a consumer local store — **not** the canonical jsonl — until a later natural-frequency gate.
4. Trigger the ADR-002 §3 minor-bump wrapper sweep (see WDR-6).

## Cross-references
- Synthesizes [[wdr_external_sota_dossier]] + [[wdr_internal_coverage_map]] + [[wdr_empirical_gap_ledger]].
- Graduation path + wrapper sweep detailed in [[wdr_cross_vault_architecture_note]].
- Live pack (untouched): `what/context/core_domain_packs/context_iii_domain_packs_web_design.md`. Contracts: `what/decisions/adr_002_consumer_federation_contract.md` §3, `what/decisions/adr_003_learning_store_ownership.md` §3/§3.6.
