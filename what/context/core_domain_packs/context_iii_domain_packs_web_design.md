---
type: context_guide
topic: iii_domain_packs
subtopic: web_design
created: 2026-04-04
updated: 2026-05-25
token_estimate: 2600
last_edited_by: agent_argus
tags: [context_guide, iii-review, traps, web-design, astro, siteforge, cwv_design_debt, ai_slop_composition, campaign_f]
banner: "who/assets/banners/banner_context_guide.jpg"
icon: layout
migration_provenance:
  previous_home: lattice-labs/what/context/iii_domain_packs/context_iii_domain_packs_web_design.md
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-2
quality_metric:
  rubric_version: "adr_007_v0"
  scored_at: 2026-05-25
  scored_by: agent_argus
  scoring_mission: campaign_web_design_deep_review F2
  signal_density: 5
  actionability: 5
  coverage_uniformity: 5
  source_diversity: 4
  cross_topic_coherence: 5
  graduation_yield: 1
  composite: 4.17
  floor_check: triggered
  floor_axes: [graduation_yield]
  prior_score:
    scored_at: 2026-05-23
    scoring_mission: campaign_d_federation_adaptive_loop MD-B4
    source_diversity: 3
    composite: 4.00
  notes: "Campaign F (Operation Tell) F2 re-score after pack revision. Now seven traps (Trap 6 CWV-as-Design-Debt + Trap 7 AI-Slop Composition added at F1) each carrying description / example / where it hides / severity default / detection method — signal density + actionability + coverage uniformity hold at 5. source_diversity 3→4: the F2 remediation added 8 external citation clusters (Astro/Google official docs, WCAG 2.2 SC 2.5.8, Deque + WebAIM practitioner coverage studies, 925studios anti-slop, CWV sources) + internal contract cross-refs (ADR-002/ADR-003/skill_iii_review) + first-hand empirical evidence (3-site gap ledger). Capped at 4 not 5 because academic/research source-type is absent (rubric §4: 5 requires vendor+academic+practitioner+empirical). composite 24/6=4.00 → 25/6=4.17. graduation_yield stays 1 = the known MB4-2 measurement artifact resolved at MD-B6 via ADR-007 §3.6 (credit packs by PACK_DELTA_LANDED stage marker, not by floor-matching graduated_to:\"core\"; web_design carries zero PACK_DELTA_LANDED ceremony records because Campaign F lands candidates-only — zero graduation fires). The floor trigger is the measurement artifact, NOT pack content — not a revision candidate."
---

# III Domain Pack: Web Design

Domain-specific trap library for reviewing generated websites, Astro components, content collections, and web design artifacts. Extends the [[what/context/core_domain_packs/context_iii_inspect_procedures|Five Traps]] with web-specific failure modes.

## When to Load

Load alongside `core` when reviewing:
- Astro pages, layouts, and components (`*.astro`)
- Web utility modules (`*.ts` in site projects)
- CSS and design token files
- Content collection data and schemas
- SiteForge campaign artifacts

## Web Design Traps

### Trap 1: Visual Hierarchy Trap

| Field | Value |
|-------|-------|
| **Description** | Content sections lacking clear visual weight differentiation — equal-weight elements stacked without heading size progression, whitespace variation, or emphasis signals |
| **Example** | Three card grid sections in a row, each with identical h3 + paragraph + button, no size or spacing variation to indicate relative importance |
| **Where It Hides** | Landing pages with many sections, feature grids, partner/team listings where every item gets identical treatment regardless of priority |
| **Severity Default** | Medium |

**Detection method**: Scan page structure for consecutive sections. Flag when 3+ sections use identical heading level + layout pattern without any differentiating visual signal (background change, size shift, spacing increase).

### Trap 2: Template Voice Trap

| Field | Value |
|-------|-------|
| **Description** | Content that sounds interchangeable — detectable by the entity-name substitution test: swap the entity name and check if the copy still reads naturally for any organization |
| **Example** | "We are committed to innovation and excellence in our field" — true of literally any org. Contrast: "We connect schools, farms, and ocean stations through portable DNA sequencing" — only true of WGA |
| **Where It Hides** | Hero taglines, about page intros, CTA sections, meta descriptions, footer blurbs. Any text written to fill a template slot rather than convey entity-specific meaning |
| **Severity Default** | High |

**Detection method**: For each content block, mentally replace the entity name with a competitor or unrelated org. If the copy still works, it's template voice. Cross-reference the Voice Bible register constraints — `confident_clarity` requires first-person plural with concrete, action-oriented language.

### Trap 3: Accessibility Theater Trap

| Field | Value |
|-------|-------|
| **Description** | ARIA attributes present but semantically wrong or meaningless — creating the appearance of accessibility without the substance |
| **Example** | `aria-label="Section"` on a `<section>` (redundant, tells screen reader nothing), `alt="image"` on an `<img>` (describes element type, not content), `role="button"` on a `<div>` without keyboard handlers |
| **Where It Hides** | Decorative SVGs missing `aria-hidden="true"`, icon buttons with `aria-label` that describes the icon rather than the action, form inputs with labels that repeat the placeholder |
| **Severity Default** | Medium |

**Detection method**: For each ARIA attribute, ask: "Does this convey information a sighted user gets visually that a screen reader user would otherwise miss?" If no, it's theater. Correct patterns: `aria-label="World Genome Academy home"` on logo link (good — conveys destination), `aria-hidden="true"` on decorative SVG (good — hides noise).

**Why a semantic a11y trap must exist alongside Gate 4** (coverage-ceiling evidence): automated tooling (axe-core) detects only ~57% of accessibility issues *by volume* — and that figure is inflated by high-frequency machine-detectable defects like color contrast (~30% of all errors). *By success criterion* the coverage is ~30% (only ~15–16 of WCAG 2.1 AA's 50 criteria are meaningfully machine-testable). "No automated issues ≠ conformance." Gate 4's green is necessary, not sufficient; this trap owns the semantic remainder. [Deque coverage study; WebAIM WCAG 2 Checklist]

**Sub-signals** (added Campaign F / F2):
- **WCAG 2.2 SC 2.5.8 Target Size (Minimum)** — interactive targets ≥ 24×24 CSS px (or equivalent spacing). A *layout-density* decision violated by tightly-packed AI card grids and icon rows; axe does not reliably flag it.
- **Semantic heading hierarchy** — a flat outline (a section `<h2>` sibling to a subordinate card `<h2>`) scores 100 on axe yet is a real hierarchy defect (ScienceStanley `index.astro:46` vs `ProjectCard.astro:31`). Tools verify headings nest numerically; only semantic review judges whether the outline reflects real document structure. (May alternatively be read under Trap 1 Visual Hierarchy.)
- **Alt *meaning* / link-in-context / focus order** — a checker confirms `alt` exists and links are present; only an agent judges whether alt conveys the image's *purpose in context*, whether link text ("click here") works *out of context*, and whether DOM/focus order matches reading order.

### Trap 4: SEO Stuffing Trap

| Field | Value |
|-------|-------|
| **Description** | Keywords forced into headings, descriptions, or content unnaturally — optimizing for crawlers at the expense of human readability |
| **Example** | `<title>eDNA Environmental DNA Testing California DNA Sequencing Academy</title>` — keyword salad. Contrast: `<title>World Genome Academy - California eDNA Atlas</title>` — reads as a name |
| **Where It Hides** | `<title>` tags, `<meta name="description">`, JSON-LD description fields, h1/h2 headings, image alt text used as keyword dumps |
| **Severity Default** | Medium |

**Detection method**: Read each meta element and heading aloud. If it sounds like a search query rather than a sentence, it's stuffed. Check JSON-LD `description` fields against the same standard — they should read as prose. Cross-reference with the mechanical SEO gate (Gate 6) to distinguish: Gate 6 checks presence, this trap checks quality.

**Earned-credibility / naturalness sub-signal** (added Campaign F / F2): the spam line is "value-add," not "AI-or-not" — Google treats production method as irrelevant and helpfulness as the test. Two new tells beyond "reads like a query":
- **Valid-but-unearned markup** — structured data / page elements that imply credibility the page has not earned (review/rating schema, `Organization`/`Person` claims) validate against the schema yet still violate spam policy when the credibility is not substantiated on-page.
- **Markup not visible on the page** — JSON-LD asserting content (ratings, authorship, FAQs) absent from the rendered page is the classic "valid-but-low-quality" deceptive-markup flag.
Reinforces existing correction **C-014** (`missing_structured_data_on_generated_pages`) — C-014 catches *absence*; this catches *present-but-unearned*. [Google Search spam policies + gen-AI content guidance]

### Trap 5: Design Token Drift Trap

| Field | Value |
|-------|-------|
| **Description** | Hard-coded color, font, or spacing values where CSS custom properties should be used — creating maintenance debt and breaking theme consistency |
| **Example** | `fill="#7EC8A0"` in an SVG and `from-[#0a1a0f]` in a gradient — raw hex values that won't respond to dark mode toggle or brand changes. Should use `var(--brand-primary)` or `var(--color-surface)` |
| **Where It Hides** | Inline SVG fills and strokes, gradient definitions, box-shadow values, one-off spacing (`mt-6` instead of `mt-[var(--space-6)]`), z-index magic numbers |
| **Severity Default** | High |

**Detection method**: Search for raw hex colors (`#[0-9a-fA-F]{3,8}`), raw `rgb()`/`hsl()` values, and Tailwind utility values that bypass the custom property system (e.g., `text-green-600` instead of `text-[var(--brand-primary)]`). Exception: decorative elements like SVG map backgrounds may legitimately use fixed values if documented — flag but allow with justification.

> **Precision sub-signal — semantic-data value vs token drift** (added Campaign F / F2): the raw-hex scan conflates two distinct defects. Distinguish (1) a *raw value that should be a token* (true drift → replace with `var(--…)`) from (2) a *semantic data value that is legitimately a literal but lacks a single shared source-of-truth* (e.g. a map legend color: the literal is fine, the bug is the same logical value rendered as different literals across pages). Case: the wga ecoregion color appears as different hex in `atlas.astro`, `stories.astro`, and `StoryCard.astro` — flagging the literal as "drift" would be a **false positive**; the real defect is cross-page inconsistency from no shared source. **Sub-signal**: "same logical value → divergent literals across files, no shared source-of-truth." Remedy = one shared map (the Astro content-collections data-as-collection idiom), not necessarily a CSS var. This prevents false positives on legitimate legend/data colors while catching the inconsistency the bare scan misses.

### Trap 6: Core-Web-Vitals-as-Design-Debt Trap

| Field | Value |
|-------|-------|
| **Description** | A Core Web Vital (CLS/LCP/INP) breach that traces to a *design or component decision* — not a one-off perf glitch. The number is SiteForge's (Gate 3/Gate 7, mechanical); the **design attribution** is III's. A green-ish Lighthouse can still hide a heavy-hero / over-hydrated / unreserved-layout design choice that compounds across every page using the component. |
| **Example** | A reusable image component ships `loading="lazy"` with **no intrinsic `width`/`height`**, relying on CSS `aspect-ratio` — so the LCP hero lazy-loads and CLS rises (ScienceStanley `ContentImage.astro:19`, CLS 0.168). Or webfonts imported (`@fontsource`) without `font-display`/preload, causing reflow CLS. Or `client:load` on a below-the-fold widget, taxing INP. |
| **Where It Hides** | Shared image/media components; hero components with eager-vs-lazy mismatches; bare `@fontsource` imports in the base layout; any `client:load` on static or below-the-fold content (INP). |
| **Severity Default** | High (design-debt compounds across every page using the component) |

**Detection method**: Read the **existing** Lighthouse JSON that SiteForge already writes (`lh-*.json` / baselines) for sub-1.0 CWV audit scores — **do not re-run the audit** (audit execution is SiteForge's; III reads the number and attributes the cause). Then trace each breach to its source: `grep` image components for missing `width`/`height` + `loading` mismatches on LCP elements; check font imports for `font-display`/preload; check `client:*` directives on static/below-fold content. Cross-reference Gate 3/Gate 7 (mechanical) vs this trap (design attribution). CWV "good" thresholds (75th-percentile of real users): **LCP < 2.5s · INP < 200ms · CLS < 0.1** — a breach implies a systemic design choice, not a glitch. **Field map + reproducible worked example**: `what/artifacts/g0_audit_ingest_schema_note.md` (Campaign G T2) — the exact Lighthouse `report.json` keys III reads (`categories.*.score`, `audits['cumulative-layout-shift'|'largest-contentful-paint'|'total-blocking-time'].numericValue`, design-attribution diagnostics) and a replayable CLS-breach → `ContentImage.astro` attribution trace; the read-only ingest seam (III parses an existing file, never instantiates Lighthouse).

### Trap 7: AI-Slop Composition Trap

| Field | Value |
|-------|-------|
| **Description** | Generic AI-generated *structural/visual* composition — the centered-hero + uniform 3-card grid + gradient-blob silhouette, uniform padding/radius, default Inter-400 + purple→blue gradient reused everywhere. **Complements (does not duplicate) the Template Voice trap**: Trap 2 catches the *copy* half ("Empower / Elevate / Unlock," entity-substitution test); this catches the *layout* half. The canonical "passes every robot gate, still reads as slop" residue. |
| **Example** | Centered glass hero (eyebrow + large vague headline + two CTAs) + nebula radial-gradient blob + `auto-fill minmax(300px,1fr)` 3-card grid with identical radius/padding — passing every Lighthouse gate while reading as template (the silhouette ScienceStanley partially exhibits). Reinforced by default-only typography (single Inter weight, no type system) and a purple/blue gradient reused across hero + CTA + accent. |
| **Where It Hides** | Landing-page heroes; feature-card sections; any section a generator would emit by default. Hides *behind green gates* — Lighthouse/axe/CWV all pass. |
| **Severity Default** | Medium (High when combined with Template Voice copy on the same section) |

**Detection method**: Scan page structure for the slop signature — a centered hero (eyebrow + large vague headline + two CTAs) immediately followed by a 3-up uniform card grid + a decorative gradient blob; check for default-only typography (single Inter weight, no committed type system) and a single purple/blue gradient reused across hero + CTA + accent. Judge **silhouette + content together**, not silhouette alone (a bespoke site may legitimately use a hero + cards). **Coverage rationale**: SiteForge **Brand Gate 8 is screenshot + human-review only** — AI-slop *composition* judgment is explicitly *un-automated*, the work the mechanical layer defers to human eyes. III owns the semantic/code-level tells so the deferral has a home in the agentic review.

## Reader Profile

Web design review targets artifacts produced by the SiteForge pipeline. The reviewer should evaluate as a **web standards practitioner** who cares about: semantic HTML, progressive enhancement, design system consistency, and content authenticity. The anti-slop doctrine (no generic AI aesthetics) applies to both content and code.

## Relationship to Other Checks

| Check Layer | Tool | What It Validates |
|-------------|------|-------------------|
| Mechanical | Playwright (Gate 4, 6, 8, 9) | Build succeeds, a11y score, meta present, responsive renders |
| Semantic | III + web_design pack | Content authenticity, design coherence, token discipline, real a11y |
| Voice | III + Voice Critic | Register compliance, anti-slop, entity-specific language |

The web_design traps complement the mechanical Playwright gates — they own the *residue* the robots provably miss:

| Mechanical gate (SiteForge) | What it measures | III trap that owns the residue |
|---|---|---|
| Gate 4 (axe, ~30% of WCAG SC) | a11y rule violations | Trap 3 — semantic ARIA, alt *meaning*, heading *logic*, 2.5.8 target size |
| Gate 6 (SEO presence) | meta/schema present | Trap 4 — naturalness + earned credibility |
| Gate 8 (brand, screenshot+human only) | visual brand, manual | **Trap 7 — AI-slop composition** (the explicitly un-automated layer) |
| Gate 3 / Gate 7 (Lighthouse / CWV number) | LCP·INP·CLS scores | **Trap 6 — design attribution of the breach** |

III never *runs* these audits (that is SiteForge's mechanical layer); Trap 6 *reads* the Lighthouse/axe numbers SiteForge already produces and attributes them to the design decision. The boundary is firm: **mechanical execution stays in SiteForge; semantic attribution is III's.**

## Sources & Citations

External grounding for the traps above (the `source_diversity` corpus — full annotated dossier at `what/artifacts/wdr_external_sota_dossier.md`):

1. Core Web Vitals thresholds & design-signal framing — https://www.corewebvitals.io/core-web-vitals · https://www.digitalapplied.com/blog/core-web-vitals-2026-inp-lcp-cls-optimization-guide · https://nitropack.io/blog/most-important-core-web-vitals-metrics/ (Traps 6)
2. Automated a11y coverage ceiling (~57% by volume / ~30% by success criterion) — https://www.deque.com/blog/automated-testing-study-identifies-57-percent-of-digital-accessibility-issues/ · https://www.deque.com/automated-accessibility-coverage-report/ (Trap 3)
3. WCAG 2 semantic checklist (alt meaning, heading logic, link-in-context, focus order) — https://webaim.org/standards/wcag/checklist (Trap 3)
4. WCAG 2.2 SC 2.5.8 Target Size (≥24×24px) — https://testparty.ai/blog/wcag-22-new-success-criteria (Trap 3)
5. Google Search spam policies + gen-AI content guidance (value-add test, unearned/invisible markup) — https://developers.google.com/search/docs/essentials/spam-policies · https://developers.google.com/search/docs/fundamentals/using-gen-ai-content (Trap 4)
6. Astro islands / hydration directives as UX-priority decisions — https://docs.astro.build/en/concepts/islands/ (Trap 6)
7. Astro content-collections (data-as-collection idiom; image schema → CLS) — https://inhaq.com/blog/getting-started-with-astro-content-collections (Traps 5 & 6)
8. AI-slop web-aesthetic tells (centered-hero + 3-card + gradient-blob; default Inter+purple) — https://www.925studios.co/blog/ai-slop-web-design-guide · https://thomas-wiegold.com/blog/claude-code-frontend-design-plugin/ · https://infomedia.com/blog/how-to-make-content-look-less-like-it-was-written-by-ai (Trap 7)

**Contract cross-references**: `what/decisions/adr_002_consumer_federation_contract.md` §3 (minor-bump wrapper review that propagates pack revisions to consumers) · `what/decisions/adr_003_learning_store_ownership.md` §3/§3.6 (graduation gate — frequency ≥3, acceptance ≥80%, ≥2-vault for elevated scrutiny — governs when web corrections graduate) · `how/skills/skill_iii_review.md` (the review procedure that loads this pack + triggers the multi-voice web orchestration).

## Related

- [[how/skills/skill_iii_review|III Review Skill]] — the review procedure that consumes this pack
- [[what/context/core_domain_packs/context_iii_inspect_procedures|INSPECT Procedures]] — Five Traps + modality procedures
- [[how/campaigns/campaign_siteforge/artifacts/sf_m05_voice_bible|Voice Bible]] — register definitions for voice compliance (consumer-side, lattice-labs)
- [[how/campaigns/campaign_siteforge/artifacts/sf_m02_quality_gate_framework|Quality Gate Framework]] — mechanical validation counterpart (consumer-side, lattice-labs)
- `what/artifacts/g0_audit_ingest_schema_note.md` — Trap-6 audit-signal-ingest field map (Lighthouse/axe key → design-attribution) + canonical worked example; the read-only seam by which this pack reads SiteForge's audit JSON without re-running an audit (boundary held — no III audit runtime)
