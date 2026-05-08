---
type: context_guide
topic: iii_domain_packs
subtopic: web_design
created: 2026-04-04
updated: 2026-05-08
token_estimate: 1500
last_edited_by: agent_stanley
tags: [context_guide, iii-review, traps, web-design, astro, siteforge]
banner: "who/assets/banners/banner_context_guide.jpg"
icon: layout
migration_provenance:
  previous_home: lattice-labs/what/context/iii_domain_packs/context_iii_domain_packs_web_design.md
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-2
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

### Trap 4: SEO Stuffing Trap

| Field | Value |
|-------|-------|
| **Description** | Keywords forced into headings, descriptions, or content unnaturally — optimizing for crawlers at the expense of human readability |
| **Example** | `<title>eDNA Environmental DNA Testing California DNA Sequencing Academy</title>` — keyword salad. Contrast: `<title>World Genome Academy - California eDNA Atlas</title>` — reads as a name |
| **Where It Hides** | `<title>` tags, `<meta name="description">`, JSON-LD description fields, h1/h2 headings, image alt text used as keyword dumps |
| **Severity Default** | Medium |

**Detection method**: Read each meta element and heading aloud. If it sounds like a search query rather than a sentence, it's stuffed. Check JSON-LD `description` fields against the same standard — they should read as prose. Cross-reference with the mechanical SEO gate (Gate 6) to distinguish: Gate 6 checks presence, this trap checks quality.

### Trap 5: Design Token Drift Trap

| Field | Value |
|-------|-------|
| **Description** | Hard-coded color, font, or spacing values where CSS custom properties should be used — creating maintenance debt and breaking theme consistency |
| **Example** | `fill="#7EC8A0"` in an SVG and `from-[#0a1a0f]` in a gradient — raw hex values that won't respond to dark mode toggle or brand changes. Should use `var(--brand-primary)` or `var(--color-surface)` |
| **Where It Hides** | Inline SVG fills and strokes, gradient definitions, box-shadow values, one-off spacing (`mt-6` instead of `mt-[var(--space-6)]`), z-index magic numbers |
| **Severity Default** | High |

**Detection method**: Search for raw hex colors (`#[0-9a-fA-F]{3,8}`), raw `rgb()`/`hsl()` values, and Tailwind utility values that bypass the custom property system (e.g., `text-green-600` instead of `text-[var(--brand-primary)]`). Exception: decorative elements like SVG map backgrounds may legitimately use fixed values if documented — flag but allow with justification.

## Reader Profile

Web design review targets artifacts produced by the SiteForge pipeline. The reviewer should evaluate as a **web standards practitioner** who cares about: semantic HTML, progressive enhancement, design system consistency, and content authenticity. The anti-slop doctrine (no generic AI aesthetics) applies to both content and code.

## Relationship to Other Checks

| Check Layer | Tool | What It Validates |
|-------------|------|-------------------|
| Mechanical | Playwright (Gate 4, 6, 8, 9) | Build succeeds, a11y score, meta present, responsive renders |
| Semantic | III + web_design pack | Content authenticity, design coherence, token discipline, real a11y |
| Voice | III + Voice Critic | Register compliance, anti-slop, entity-specific language |

The web_design traps complement the mechanical Playwright gates. Gate 4 (a11y) checks axe-core rules; Trap 3 catches semantic ARIA failures that axe misses. Gate 8 (brand) captures screenshots for human review; Trap 5 catches the code-level token drift that causes brand inconsistency.

## Related

- [[how/skills/skill_iii_review|III Review Skill]] — the review procedure that consumes this pack
- [[what/context/core_domain_packs/context_iii_inspect_procedures|INSPECT Procedures]] — Five Traps + modality procedures
- [[how/campaigns/campaign_siteforge/artifacts/sf_m05_voice_bible|Voice Bible]] — register definitions for voice compliance (consumer-side, lattice-labs)
- [[how/campaigns/campaign_siteforge/artifacts/sf_m02_quality_gate_framework|Quality Gate Framework]] — mechanical validation counterpart (consumer-side, lattice-labs)
