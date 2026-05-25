---
artifact_class: web_design_gap_ledger
type: artifact
title: "WDR Deliverable 3 — Empirical gap ledger (inspection-grade) across wga + ScienceStanley + ContextCommons"
mission: plan_web_design_deep_review_charter
designs_campaign: campaign_web_design_deep_review
created: 2026-05-25
updated: 2026-05-25
last_edited_by: agent_argus
status: research_complete
evidence_method: inspection_grade
boundary_posture: boundary_preserving
empirical_weighting: "wga + ScienceStanley deep; ContextCommons confirmatory"
tags: [artifact, web_design, gap_ledger, empirical, inspection_grade, wga, sciencestanley, contextcommons, negative_evidence]
---

# WDR-3 — Empirical Gap Ledger (inspection-grade)

> **Method**: read existing on-disk reports + source only (NO builds, NO live audit runs). Each finding bucketed: **machine-caught** (a SiteForge gate flags it) · **III-trap-caught** (an existing trap/correction covers it) · **neither-caught** (the residue — the campaign's target). Weighted to wga + ScienceStanley; ContextCommons confirmatory.
>
> **Card correction**: the planning card §7 stated ScienceStanley has "~44 Playwright tests." **Actual = 310 `test(` cases across 39 spec files** (interaction 102, code-quality 54, a11y 52, design-tokens 35, voice 33, seo 25, performance 5, responsive 3, visual 1). Corrected here and propagated to WDR-4/5/6.

## §0 Headline finding (shapes everything downstream)

**All three sites are already high-quality** — they have been built and reviewed under the existing III + SiteForge stack. This is itself the most important empirical result: the design-residue candidates are **thinner than a naïve hypothesis would predict**, and several are *refuted* on these sites. That does **not** weaken the campaign — it sharpens it:
- The strongest new trap by evidence is **CWV-as-design-debt** (2-vault: ScienceStanley strong + wga weak).
- The **AI-slop-hero** trap is justified by a *coverage seam* (Brand Gate 8 is manual-screenshot-only — AI-slop judgment is explicitly un-automated) **more than by prevalence** on these well-tended sites. That is a legitimate and honest basis: III needs the trap precisely because no robot owns it, even where current sites happen to pass.
- The highest-value *refinement* is a **Trap 5 precision fix** (wga): distinguish "raw value that should be a token" from "semantic data value needing a single shared source-of-truth" — the current trap would **false-positive** legitimate legend data while missing the real bug.

## §1 wga.aDNA (deep) — Astro 6.1.3 + Tailwind 4.2.2; 8 pages; 8 LH reports + 11 gate JSONs + 42 screenshots on disk

**Real Lighthouse / CWV** (from `lh-*.json`, 2026-04-14):

| Page | Perf | A11y | BP | SEO | LCP | CLS | TBT |
|------|------|------|----|----|-----|-----|-----|
| home (`lh-home.json`) | 0.94 | 1.0 | 1.0 | 1.0 | **2.9s (0.79)** | 0 | 0ms |
| partners | 1.0 | 1.0 | 1.0 | 1.0 | 1.2s | 0 | 0ms |
| atlas | 1.0 | 1.0 | 1.0 | 1.0 | 1.2s | 0 | 0ms |

Zero `client:*` directives site-wide (fully static → TBT 0); view transitions present (`ClientRouter` in `BaseLayout.astro:2`); scroll-reveal/count-up gated on `prefers-reduced-motion`.

| Finding | Bucket | Evidence | Maps to |
|---|---|---|---|
| Home LCP 2.9s from eager full-bleed hero photo | machine | `lh-home.json` LCP 0.79; `Hero.astro:20` `loading="eager"` | CWV / **CWV-as-design-debt** (weak) |
| Inline data arrays vs content collection | III-trap | `atlas.astro:9`, `curriculum.astro:10`, `data-portal.astro:17`, `stories.astro:9` | **C-016** |
| Raw hex ecoregion colors via inline `style` | III-trap (with FP caveat) | `atlas.astro:12-37`, `stories.astro:12-22`, `StoryCard.astro:13-16` | Trap 5 / C-012 |
| **Same ecoregion → different hex across pages** (Central Valley `#8B6914` atlas vs `#6B5010` stories; Sierra `#6B7280` vs `#4B5563`) | **neither** | `atlas.astro:12,22` vs `stories.astro:12` + `StoryCard.astro:13,15` | **NEW: semantic-token-drift / cross-page data inconsistency** |
| Trap-5 would false-positive the legend hex (legit data) while missing the real bug (no shared ecoregion→color map) | **neither** | three separate hex tables, no shared source | **Trap 5 precision gap** |
| Brand Gate 8 is "review"-only — AI-slop / "no stock imagery" judgment explicitly un-automated | **neither** | `gate_08_brand.json` `"status":"review"` | **machine-defers-to-human seam → AI-slop-hero rationale** |

**Residue verdict (honest)**: AI-slop-hero **ABSENT** (bespoke hand-drawn California SVG hero, distinct per-card copy); motion-absence **ABSENT** (view transitions + considered reduced-motion); CWV-as-design-debt **WEAK-POSITIVE** (home LCP 2.9s). The genuine novel residue is the **Trap-5 precision gap** + the **machine-defers-to-human seam**.

## §2 ScienceStanley.aDNA (deep) — Astro 6.1.3; 7 pages + 2 dynamic collections; 6 typed collections; 310 tests / 39 specs; 8 LH baselines + analysis docs

**Real Lighthouse / CWV** (`tests/lighthouse/baseline-index.json`, LH 13.1.0, dev-server):

| Metric | Value | Score |
|---|---|---|
| Performance | 0.55 | — (dev-inflated; `BASELINE_ANALYSIS.md` est. prod 80–85) |
| Accessibility | 1.00 | — |
| SEO | 0.92 | — (one non-descriptive "Learn more" link) |
| Best Practices | 1.00 | — |
| LCP | 7.7s | 0.03 (dev-inflated) |
| **CLS** | **0.168** | 0.71 (**real prod issue** — Inter font reflow, no preload/`font-display`) |
| TBT | 0ms | 1.00 |

Zero `client:*` directives (`src/components/islands/` empty — fully static). No inline data (6 typed collections — C-016 satisfied). Person JSON-LD + full per-page meta (C-014 satisfied).

| Finding | Bucket | Evidence | Maps to |
|---|---|---|---|
| CLS 0.168 (>0.1) from Inter font reflow; no preload/`font-display` | machine + design | `baseline-index.json` cls 0.168; `BaseLayout.astro:5-6` bare `@fontsource` imports | CWV / **CWV-as-design-debt** |
| **`ContentImage` omits width/height + `loading="lazy"` on the LCP hero image** | **neither** → CWV | `ContentImage.astro:19` `loading="lazy"` no dims; hero `index.astro:36` | **CWV-as-design-debt (STRONG)** |
| 42 raw `rgba(...)` literals bypassing `--ss-` tokens | III-trap | `grep rgba( ` ×42 in `src/*.astro` (e.g. `Hero.astro:54`) | Trap 5 / C-012 |
| Flat heading nesting: section `<h2>` (`index.astro:46`) sibling to subordinate card `<h2>` (`ProjectCard.astro:31`) — scores 100 on axe | **neither** | sibling h2/h2; axe flags *skipped* levels not flat-nesting | **NEW: semantic-heading-hierarchy** (extends Trap 1/3) |
| No view transitions anywhere (`astro:transitions` = 0 hits); only `.reveal` fade-up | **neither** | `global.css:310`; grep view-transition = 0 | **motion/interaction-design-absence** |
| 3-card auto-fill grid + centered glass hero + nebula radial-gradient blob | partial **neither** | `global.css:155`; `Hero.astro:17,52` | **AI-slop-hero** (silhouette only — see verdict) |
| Only 768px+480px breakpoints; grid 3→1 with no 2-col tier | **neither** (mild) | `global.css:159-162` | **responsive-breakpoint-neglect** |

**Residue verdict (honest)**: **CWV-as-design-debt CONFIRMED STRONG** — the `ContentImage` component decision (no dims + lazy LCP) is the cleanest case of "machine sees a number (CLS 0.168), cannot attribute it to the design decision, and no existing trap names it." Motion-absence **CONFIRMED** (zero view transitions). AI-slop-hero **PARTIAL/HONEST-WEAK** — composition silhouette matches but content + dual Ghibli/pixel visual identity actively resist slop; I would *not* file a strong slop finding. responsive-neglect **mild-real**.

## §3 ContextCommons.aDNA (confirmatory) — Astro 6.1.3 + Tailwind 4.2.2; 10 pages

**Real Lighthouse / CWV** (`.lighthouseci/lhr-1778619538686.json`, 2026-05-12): Perf **1.0** · A11y **1.0** · SEO **1.0** · BP 0.96 (only `errors-in-console`). LCP **0.4s** · CLS **0** · TBT **0ms**. Hero preloaded + `fetchpriority="high"`; `<ClientRouter/>` view transitions; reduced-motion-gated entrance animations; zero hardcoded hex (all `var(--*)`).

| Residue candidate / trap | Verdict | Evidence |
|---|---|---|
| AI-slop-hero | **ABSENT** | asymmetric pixel-art `<picture>` hero, intentional grids |
| CWV-as-design-debt | **ABSENT** (strong negative) | LCP 0.4s, CLS 0, hero preloaded |
| motion-design-absence | **ABSENT** | `BaseLayout:58` `<ClientRouter/>`; hover/focus-visible states |
| responsive-neglect | INCONCLUSIVE (leans clean) | considered `md:` breakpoints; no overflow evidence on disk |
| Design Token Drift / C-012 | **ABSENT** | zero hardcoded hex |
| C-014 missing structured data | **ABSENT** | Org JSON-LD present |
| **C-015 hardcoded_entity_strings** | **PRESENT (mild)** | 27 inline entity strings ("Grand Rapids", "contextcommons.org") |
| **C-016 inline_data_instead_of_collection** | **PRESENT (mild)** | `partners.astro:10` + `about.astro:9` inline `faqItems` arrays |

**Confirmatory verdict**: ContextCommons is a high-quality build that **refutes all four primary design-residue candidates** and **independently corroborates only C-015 + C-016** (the data-hygiene corrections).

## §4 Cross-site synthesis → provenance & graduation eligibility

| Candidate / correction | wga | ScienceStanley | ContextCommons | ≥2-vault? | Disposition |
|---|---|---|---|---|---|
| **CWV-as-design-debt** (new trap) | weak (LCP 2.9s) | **strong** (ContentImage no-dims + lazy LCP + CLS 0.168) | absent | **YES (2)** | **Primary new-trap candidate** |
| **Trap-5 precision fix** (semantic-data vs token) | **strong** (ecoregion hex) | (rgba literals — different sub-case) | absent (tokens clean) | partial (1 strong) | **Trap-5 refinement** (not new trap) |
| **semantic-heading-hierarchy** (new sub-signal) | not evidenced | **present** (flat h2/h2) | absent | 1 | candidate (extends Trap 1/3) |
| **motion/interaction-design-absence** (new trap) | absent (has VT) | **present** (no VT) | absent (has VT) | 1 | candidate — thin; graduate later |
| **AI-slop-hero** (new trap) | absent | partial | absent | 0 strong | **seam-justified** (Gate 8 manual-only), not prevalence-justified |
| **responsive-breakpoint-neglect** (new trap) | not evidenced | mild | inconclusive | 1 | candidate — thin; graduate later |
| C-012 Design Token Drift | present | present (42 rgba) | absent | **YES (2)** | existing — reinforced |
| C-015 hardcoded_entity_strings | present | — | present (27) | **YES (2)** | existing — reinforced |
| C-016 inline_data_instead_of_collection | present (4) | satisfied | present (faqItems) | **YES (2)** | existing — reinforced |

## §5 Implications for WDR-4 (pack revision)

1. **CWV-as-design-debt is the one prevalence-AND-coverage-justified new trap** (2-vault, strong on SS). Lead with it.
2. **AI-slop-hero is justified by the coverage seam, not by these sites' sloppiness** — file it honestly as "Brand Gate 8 defers AI-slop composition judgment to humans; III owns the semantic/code-level tells." This is the highest-*value* trap (anti-slop mandate) but lowest-*prevalence* on the current (well-tended) corpus — exactly why it stays a **candidate**, not a within-campaign graduation (aligns with operator decision: candidates-only).
3. **Trap 5 needs a precision amendment** (the wga ecoregion case): a token-drift flag must distinguish raw-value-that-should-be-a-token (true positive) from semantic-data-value-needing-one-shared-source (different fix). This is the single most defensible *content* change — and it raises `source_diversity` by citing the empirical case + Astro idioms.
4. **motion-absence + responsive-neglect + semantic-heading-hierarchy** are real but **1-vault thin** → ship as candidates pending more cross-vault frequency (not graduated). This is precisely the candidates-only posture.
5. **Three existing corrections (C-012/C-015/C-016) gained fresh ≥2-vault provenance** — useful evidence rows for WDR-6's graduation-path discussion even though no graduation fires this campaign.

## Cross-references
- Fills the "neither" rows of [[wdr_internal_coverage_map]] §6; classified against [[wdr_external_sota_dossier]] §6 residue map.
- Feeds [[wdr_web_design_pack_revision_spec]] (trap candidates + provenance) and [[wdr_cross_vault_architecture_note]] (graduation path).
- Sources (read-only): `wga.aDNA/site/` (src + `lh-*.json` + `tests/evidence/`); `ScienceStanley.aDNA/site/` (src + `tests/lighthouse/`); `ContextCommons.aDNA/site/` (src + `.lighthouseci/`).
