---
artifact_class: web_design_research_dossier
type: artifact
title: "WDR Deliverable 1 — External SOTA web/Astro design-evaluation dossier (framed for III's semantic layer)"
mission: plan_web_design_deep_review_charter
designs_campaign: campaign_web_design_deep_review
created: 2026-05-25
updated: 2026-05-25
last_edited_by: agent_argus
status: research_complete
evidence_method: external_research_cited
boundary_posture: boundary_preserving
tags: [artifact, web_design, research_dossier, sota, core_web_vitals, wcag22, astro6, anti_slop, source_diversity_remediation]
---

# WDR-1 — External SOTA Dossier (semantic-layer framing)

> **Purpose**: source-diverse external grounding for the future `campaign_web_design_deep_review`. Every finding is a *design-intent / authenticity* signal that a code-and-content reviewer can flag — **not** an audit procedure. Audit numbers (CWV, axe coverage) are treated as **proxies for design decisions**; the focus is what the robots provably miss. This dossier is also the **`source_diversity` remediation corpus** — the `web_design` pack scores `source_diversity=3` precisely because it carries no external citations; the citations here are the ones the pack revision (WDR-4) will fold in.

## §1 Core Web Vitals as a DESIGN signal (2026 thresholds)

- **"Good" thresholds (75th-percentile of real users):** LCP < 2.5s · INP < 200ms · CLS < 0.1. A page passes only if 75% of visits hit "good" — so a breach implies a **systemic design choice**, not a one-off glitch. [What Are the Core Web Vitals? (2026)](https://www.corewebvitals.io/core-web-vitals)
- **CLS ↔ layout-reservation discipline.** High CLS means space was not *reserved*: images/video/iframes/ad slots shipped without explicit width/height, fonts swapped without a metrics-matched fallback. CLS is the tell for "designed in a static mockup, never reasoned about load order." [Core Web Vitals 2026: INP/LCP/CLS](https://www.digitalapplied.com/blog/core-web-vitals-2026-inp-lcp-cls-optimization-guide)
- **LCP ↔ hero strategy / above-the-fold weight.** Slow LCP traces to the *hero design decision* — unoptimized hero image, un-preloaded webfont, render-blocking critical CSS. A heavy decorative hero is a choice LCP exposes. [Core Web Vitals 2026](https://www.digitalapplied.com/blog/core-web-vitals-2026-inp-lcp-cls-optimization-guide)
- **INP ↔ interaction design / main-thread budget.** INP is the most-failed vital in 2026 (~43% of sites miss 200ms); failure implies an *interaction designed without a main-thread budget* — over-eager hydration, long tasks, heavy DOM. [Most Important CWV Metrics in 2026 (NitroPack)](https://nitropack.io/blog/most-important-core-web-vitals-metrics/)
- **Passing all three is a design-maturity proxy** — correlates with ~24% lower bounce, i.e. the perceived-quality decisions a semantic reviewer cares about. [Core Web Vitals 2026 (senorIT)](https://senorit.de/en/blog/core-web-vitals-2026)

**III implication**: CWV numbers (which SiteForge already measures at Gate 3/Gate 7) are *inputs* to a design-debt judgment. A green Lighthouse can still hide a heavy-hero / over-hydrated design; a CWV breach is a pointer to a specific design decision III should name. → seeds **CWV-as-design-debt** trap candidate.

## §2 WCAG 2.2 semantic gaps beyond automated detection

- **Automated tools catch a minority.** axe-core detects ~57% of issues *by volume* — inflated because high-frequency machine-detectable defects (color contrast ≈ 30% of all errors) dominate the count. [Deque: Automated Testing Identifies 57%](https://www.deque.com/blog/automated-testing-study-identifies-57-percent-of-digital-accessibility-issues/)
- **By *success criterion* it's ~30%** — only ~15–16 of WCAG 2.1 AA's 50 criteria are meaningfully machine-testable; the rest are semantic. "No automated issues ≠ conformance." [Deque: Automated Accessibility Coverage Report](https://www.deque.com/automated-accessibility-coverage-report/)
- **Alt presence vs *meaning*.** A checker confirms `alt` exists; only a human/agent judges whether it conveys the image's *purpose in context* (decorative vs informative, redundant caption). [WebAIM WCAG 2 Checklist](https://webaim.org/standards/wcag/checklist)
- **Heading order present vs *logical*.** Tools verify headings nest numerically; they cannot judge whether the outline reflects real document structure. [WebAIM WCAG 2 Checklist](https://webaim.org/standards/wcag/checklist)
- **Link purpose in context & focus order.** "Click here" passes a presence check; semantic review confirms link text works *out of context* and that DOM/focus order matches reading order. [WebAIM WCAG 2 Checklist](https://webaim.org/standards/wcag/checklist)
- **WCAG 2.2 SC 2.5.8 Target Size (Minimum):** interactive targets ≥ 24×24 CSS px (or equivalent spacing) — a *layout-density* decision violated by tightly-packed AI card grids and icon rows. [WCAG 2.2 New Success Criteria (TestParty)](https://testparty.ai/blog/wcag-22-new-success-criteria)

**III implication**: this is the exact niche of the existing **Accessibility Theater** trap (Trap 3) — but the pack states it without the ~30%/~57% coverage-ceiling evidence that *justifies why a semantic a11y trap must exist alongside Gate 4*. WDR-4 cites Deque + WebAIM to harden Trap 3 and adds SC 2.5.8 (target size) as a new sub-signal.

## §3 Structured-data / SEO naturalness

- **The spam line is "value-add," not "AI-or-not."** Google: using gen-AI to produce pages "without adding value for users" is *scaled content abuse* — production method is irrelevant, helpfulness is the test. [Google: Gen-AI Content Guidance](https://developers.google.com/search/docs/fundamentals/using-gen-ai-content)
- **"Valid but low quality" = deceptive markup.** Google warns against structured data / page elements implying credibility the page has not earned — schema can validate yet still violate policy. [Google Search Spam Policies](https://developers.google.com/search/docs/essentials/spam-policies)
- **Scaled-content-abuse named:** doorway pages, mass-produced thin content, auto-generated articles without editorial oversight; reviewer tell = *templated repetition across near-identical pages*. [Google Search Spam Policies](https://developers.google.com/search/docs/essentials/spam-policies)
- **Markup must match visible content & feature policy** — review/rating schema not visible to users is the classic "valid-but-low-quality" flag. [Google: Gen-AI Content Guidance](https://developers.google.com/search/docs/fundamentals/using-gen-ai-content)

**III implication**: sharpens existing **SEO Stuffing** trap (Trap 4) and existing correction **C-014 `missing_structured_data_on_generated_pages`** — the trap currently checks "reads like a search query"; this adds the *"valid JSON-LD but unearned-credibility / not-visible-on-page"* dimension. SiteForge Gate 6 checks meta *presence*; this semantic layer checks *naturalness + earned credibility*.

## §4 Astro 6 idioms and their DESIGN implications

- **Islands / selective hydration ↔ INP budget.** Astro ships zero JS by default and hydrates only `client:*`; *over-hydrating* (everything `client:load`) re-imports the INP cost Astro exists to avoid. Misuse tell = interactivity directives on static content. [Islands architecture — Astro Docs](https://docs.astro.build/en/concepts/islands/)
- **Hydration directive = a UX priority decision.** `client:idle` vs `client:visible` vs `client:load` encodes *when* an element becomes interactive; `client:load` on below-the-fold widgets is a design-intent error taxing the main thread. [Islands architecture — Astro Docs](https://docs.astro.build/en/concepts/islands/)
- **`<Image>`/`<Picture>` ↔ CLS.** Built-in optimization auto-generates WebP/AVIF + responsive `srcset` + lazy-load; skipping the helper (or omitting dimensions) reintroduces the layout shift CLS penalizes. Missing dimensions = canonical CLS-by-design defect. [Astro Content Collections: 2026 Guide](https://inhaq.com/blog/getting-started-with-astro-content-collections)
- **View Transitions ↔ perceived continuity.** Cross-page app-like transitions without a client framework; *absence* on a multi-page flow is a perceived-quality gap, gratuitous transitions a polish-over-substance tell. [Astro Content Collections: 2026 Guide](https://inhaq.com/blog/getting-started-with-astro-content-collections)
- **Content collections ↔ schema as design contract.** Typed collections (with `image()` in schema) enforce content structure; bypassing collections for ad-hoc pages signals content modeled in markup rather than designed as data. [Astro Content Collections: 2026 Guide](https://inhaq.com/blog/getting-started-with-astro-content-collections)

**III implication**: directly extends existing corrections **C-016 `inline_data_instead_of_collection`** (now backed by the Astro-idiom rationale) and seeds **responsive-breakpoint-neglect** + **motion/interaction-design-absence** trap candidates. The over-hydration → INP link is the mechanism connecting §1 (CWV-as-design-debt) to Astro code review.

## §5 "AI-slop web aesthetic" tells (2026)

- **The signature stack:** centered hero (eyebrow + ~64pt vague headline + subhead + two CTAs) → three-up uniform feature cards (16px radius) → animated gradient blobs. The dominant slop fingerprint. [AI Slop Web Design Guide (925studios)](https://www.925studios.co/blog/ai-slop-web-design-guide)
- **Default typography + palette:** Inter 400 + system fallback with *no other type choice*, plus a purple→blue gradient reused for hero/CTA/accents — what a token-predictor reaches for. [Frontend-Design Plugin (Wiegold)](https://thomas-wiegold.com/blog/claude-code-frontend-design-plugin/)
- **Uniformity tell:** identical padding / radius / card heights across components — sameness *is* the diagnostic. [AI Slop Web Design Guide](https://www.925studios.co/blog/ai-slop-web-design-guide)
- **Hollow copy:** interchangeable headlines ("Build the future of work," "Your all-in-one platform") + cliché verbs *Empower / Elevate / Unlock*, "game-changer," "best-in-class" — fits a CRM, a dental office, or a scam equally. [AI Slop Web Design Guide](https://www.925studios.co/blog/ai-slop-web-design-guide) · [Less Like AI (Infomedia)](https://infomedia.com/blog/how-to-make-content-look-less-like-it-was-written-by-aimake-content-look-less-like-ai/)
- **Hedging & superlatives:** "may help you," "can potentially," uncontextualized "cutting-edge" — copy with no committed claim. [AI Slop Web Design Guide](https://www.925studios.co/blog/ai-slop-web-design-guide)
- **Stock-substitute imagery:** "diverse group around a laptop in an impossibly well-lit office," "abstract 3D blobs floating in space" standing in for real screenshots — authenticity gap. [AI Slop Web Design Guide](https://www.925studios.co/blog/ai-slop-web-design-guide)

**III implication**: this is III's anti-slop mandate made concrete and *nameable*. The existing **Template Voice** trap (Trap 2) catches the copy half ("Empower/Elevate/Unlock," entity-substitution test); there is **no trap for the structural/visual half** (centered-hero + 3-card + gradient-blob + uniformity). → seeds the **AI-slop-hero** (a.k.a. generic-composition) trap candidate — the single highest-value new trap, because it sits squarely in the "passes every robot gate, still reads as slop" residue.

## §6 Synthesis — the residue map (what robots miss → III's target)

| External signal | Robot that *partially* sees it | The residue III should own |
|---|---|---|
| CWV breach | Lighthouse Gate 3 / Gate 7 (number) | *Which design decision caused it* (hero weight, over-hydration, unreserved layout) — CWV-as-design-debt |
| a11y defect | axe Gate 4 (~30% by criterion) | alt *meaning*, heading *logic*, link-in-context, focus order, 2.5.8 target size — Accessibility Theater (hardened) |
| meta/schema presence | SEO Gate 6 (presence) | naturalness, earned credibility, templated repetition — SEO Stuffing (hardened) + C-014 |
| Astro idiom misuse | (none mechanical) | over-hydration, missing dims, inline-data, no view-transitions — C-016 + responsive/motion candidates |
| generic composition | Brand Gate 8 (manual screenshot only) | centered-hero + 3-card + gradient-blob + uniformity — **AI-slop-hero (NEW, top priority)** |

## Citation list (deduplicated)

1. [What Are the Core Web Vitals? (2026)](https://www.corewebvitals.io/core-web-vitals)
2. [Core Web Vitals 2026: INP, LCP & CLS (digitalapplied)](https://www.digitalapplied.com/blog/core-web-vitals-2026-inp-lcp-cls-optimization-guide)
3. [Most Important Core Web Vitals Metrics in 2026 (NitroPack)](https://nitropack.io/blog/most-important-core-web-vitals-metrics/)
4. [Core Web Vitals 2026 (senorIT)](https://senorit.de/en/blog/core-web-vitals-2026)
5. [Automated Testing Identifies 57% of Issues (Deque)](https://www.deque.com/blog/automated-testing-study-identifies-57-percent-of-digital-accessibility-issues/)
6. [The Automated Accessibility Coverage Report (Deque)](https://www.deque.com/automated-accessibility-coverage-report/)
7. [WebAIM's WCAG 2 Checklist](https://webaim.org/standards/wcag/checklist)
8. [WCAG 2.2 New Success Criteria (TestParty)](https://testparty.ai/blog/wcag-22-new-success-criteria)
9. [Google Search's Guidance on Generative AI Content](https://developers.google.com/search/docs/fundamentals/using-gen-ai-content)
10. [Spam Policies for Google Web Search](https://developers.google.com/search/docs/essentials/spam-policies)
11. [Islands architecture — Astro Docs](https://docs.astro.build/en/concepts/islands/)
12. [Astro Content Collections: 2026 Guide (inhaq)](https://inhaq.com/blog/getting-started-with-astro-content-collections)
13. [AI Slop Web Design Guide (925studios)](https://www.925studios.co/blog/ai-slop-web-design-guide)
14. [Frontend-Design Plugin (Wiegold)](https://thomas-wiegold.com/blog/claude-code-frontend-design-plugin/)
15. [How to Make Content Look Less Like AI (Infomedia)](https://infomedia.com/blog/how-to-make-content-look-less-like-it-was-written-by-ai)

## Cross-references
- Consumed by: [[wdr_web_design_pack_revision_spec]] (§4 trap candidates + source_diversity remediation), [[wdr_empirical_gap_ledger]] (residue classification).
- Source pack under review: `what/context/core_domain_packs/context_iii_domain_packs_web_design.md`.
