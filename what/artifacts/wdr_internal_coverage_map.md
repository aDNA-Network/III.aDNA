---
artifact_class: web_design_coverage_map
type: artifact
title: "WDR Deliverable 2 — Internal lattice web-review coverage map (III × SiteForge × peers)"
mission: plan_web_design_deep_review_charter
designs_campaign: campaign_web_design_deep_review
created: 2026-05-25
updated: 2026-05-25
last_edited_by: agent_argus
status: research_complete
evidence_method: inspection_grade
boundary_posture: boundary_preserving
tags: [artifact, web_design, coverage_map, siteforge, federation, boundary, reuse_before_invent]
---

# WDR-2 — Internal Web-Review Coverage Map

> **Purpose**: map current web-review coverage across the lattice before proposing anything new (reuse-before-invent). Answers: *what is covered, by whom, at which layer, and where are the gaps?* This is the internal counterpart to the external dossier (WDR-1) and the spatial frame the empirical gap ledger (WDR-3) fills with real findings.

## §1 The three review layers (canonical, from the web pack itself)

The `web_design` pack's own "Relationship to Other Checks" table already defines the layering. Restated:

| Layer | Owner | Mechanism | Validates |
|-------|-------|-----------|-----------|
| **Mechanical** | **SiteForge** (Playwright + Lighthouse + axe) | 10 quality gates, pass/fail thresholds | Build succeeds, scores, meta present, responsive renders, zero axe violations |
| **Semantic** | **III** (`web_design` pack + corrections) | 5 traps + 6 web corrections, judgment-based | Content authenticity, design coherence, token discipline, *real* a11y |
| **Voice** | **SiteForge** (5-voice registry) + III Voice Critic | 5 reviewer voices, multi-voice scoring | Register compliance, anti-slop, entity-specific language |

**Boundary statement (operator-locked)**: Mechanical stays in SiteForge. Semantic is III's. Voice is a SiteForge *consumer-side composition* that orchestrates III's pack findings. The campaign improves the **Semantic** layer only.

## §2 III semantic layer — current inventory (the surface under review)

**5 traps** (`context_iii_domain_packs_web_design.md`, composite 4.00; weak axes `source_diversity=3`, `graduation_yield=1`):

| # | Trap | Severity default | Catches | Robot blind to it? |
|---|------|------------------|---------|--------------------|
| 1 | Visual Hierarchy | Medium | equal-weight sections, no heading/whitespace progression | yes (Gate 8 manual only) |
| 2 | Template Voice | High | entity-substitution-test failures; interchangeable copy | yes (Voice Critic partial) |
| 3 | Accessibility Theater | Medium | semantically-wrong ARIA, present-but-useless alt | **partial** — axe Gate 4 catches ~30% by criterion |
| 4 | SEO Stuffing | Medium | keyword-salad meta/headings; reads like a query | partial — Gate 6 checks presence only |
| 5 | Design Token Drift | High | raw hex/values bypassing CSS custom properties | yes (no mechanical token gate) |

**6 web corrections** (canonical jsonl; md5 `5adb0dfa…`):

| ID | Pattern | Graduated? |
|----|---------|-----------|
| C-012 | `brand_color_dual_role` | no |
| C-013 | `nested_interactive_elements` | no |
| C-014 | `missing_structured_data_on_generated_pages` | no |
| C-015 | `hardcoded_entity_strings` | no |
| C-016 | `inline_data_instead_of_collection` | no |
| C-027 | `producer_consumer_pair_unwired` | **core** |

**Reader profile**: "web standards practitioner" — semantic HTML, progressive enhancement, design-system consistency, content authenticity; anti-slop applies to content *and* code.

## §3 SiteForge mechanical layer — the 10 gates (what III must NOT duplicate)

From `SiteForge.aDNA/what/artifacts/sf_m02_quality_gate_framework.md`:

| Gate | Name | Tool | Threshold |
|------|------|------|-----------|
| 1 | Builds | `astro build` | exit 0 |
| 2 | Types | `astro check` | zero type errors |
| 3 | **Lighthouse** | Playwright + playwright-lighthouse | **Perf ≥95 · A11y ≥95 · Best-Practices ≥95 · SEO =100** |
| 4 | **Accessibility** | @axe-core/playwright | **zero WCAG 2.1 AA violations** |
| 5 | Content | Zod schema (build-time) | zero validation errors |
| 6 | **Meta (SEO)** | Playwright head inspection | all required meta present; OG 200; desc 50–160 chars |
| 7 | **Security / CSP** | Playwright + grep | CSP header present; no key patterns in dist/ |
| 8 | **Brand** | Playwright screenshots (manual review) | entity-specific visual match (human) |
| 9 | **Responsive** | Playwright viewport test | no horizontal overflow @ 320/768/1024/1440 |
| 10 | Docs | bash README check | required sections, ≥50 words each |

**Key boundary facts**: Gate 4 = WCAG **2.1** AA (axe-auto only); Gate 8 = **manual** human screenshot review (no automated composition/slop detection); Gate 3 SEO threshold = **100** (strict, presence-oriented). These three gates define exactly where the semantic residue accumulates — see §6.

## §4 Voice layer — SiteForge 5-voice registry (consumer-side orchestration)

From `SiteForge.aDNA/what/context/siteforge/siteforge_reviewers.yaml` (orchestration triggers when `web_design` pack loads during III DISPATCH; cross-voice escalation threshold = 3 voices on same section):

| Voice | ID | Focus |
|-------|-----|-------|
| Voice Critic | VCRIT | anti-slop doctrine, register, banned-phrase scan, entity-specific voice |
| Design Reviewer | DESIGN | visual hierarchy, component reuse, token discipline, whitespace |
| UX Reviewer | UX | navigation coherence, CTA clarity, journey continuity, IA |
| SEO Specialist | SEO | meta quality, JSON-LD accuracy, heading hierarchy, keyword naturalness |
| Brand Alignment | BRAND | brand-color consistency, typography, messaging, visual identity |

This registry is `kind: reviewer_registry` per ADR-002 §1a — consumer-side, **never absorbed into III core**. III owns the *traps the voices apply*; SiteForge owns *which voices apply them*.

## §5 Adjacent / orthogonal coverage

- **ISS pack** (`SiteForge.aDNA/iii/.../context_iii_domain_packs_iss.md`) — **orthogonal**, NOT web_design. Scope = interactive operator-input surfaces (gates, forms, wizards, dashboards), 6-axis rubric + 6 traps (I-1..I-6). Local-first at SiteForge, awaiting Phase-3 graduation. Out of scope for this campaign (different surface: operator UIs, not public pages).
- **VideoForge `iii/`** — *imports* `web_design` for title-card/thumbnail/caption UI register (shared surface). Any web_design revision triggers a minor-bump review here (ADR-002 §3).
- **CanvasForge `iii/`** — *imports* `web_design` for deck title slides / comic chapter pages / marketing-canvas UI register (shared surface). Same minor-bump review obligation.
- **Federation pin**: SiteForge `iii/CLAUDE.md` pins III `version: "0.3.0"`, `version_policy: minor`, `pinned_at_commit: a309ad4`, `packs_used` includes `context_iii_domain_packs_web_design`. 5 active wrappers total at v0.3.0/a309ad4.

## §6 Coverage matrix — design concern × who covers it × gap

The synthesis. **"neither"** rows are the campaign's target residue.

| Design concern | Mechanical (SiteForge) | Semantic (III) | Voice | Residue / gap |
|---|---|---|---|---|
| Build / types | Gate 1, 2 | — | — | covered |
| Layout shift (CLS) | Gate 3/7 (number) | — | — | **no III trap maps the CLS number → design decision** → CWV-as-design-debt |
| Hero weight / LCP | Gate 3 (number) | — | — | **no III trap** → CWV-as-design-debt |
| Over-hydration / INP | (none) | — | — | **fully uncovered** → CWV-as-design-debt + motion candidate |
| Visual hierarchy | Gate 8 (manual) | **Trap 1** ✓ | DESIGN | covered (semantic) |
| Generic AI composition (hero+3-card+blob) | Gate 8 (manual screenshot only) | — | VCRIT (copy only) | **biggest gap** → AI-slop-hero (NEW) |
| Template / hollow copy | — | **Trap 2** ✓ | VCRIT | covered (semantic) |
| ARIA / alt *meaning* | Gate 4 (~30% by criterion) | **Trap 3** ✓ (under-cited) | — | partial; harden Trap 3 + add SC 2.5.8 target size |
| Heading *logic*, link-in-context, focus order | Gate 4 (partial) | (loosely Trap 3) | — | **thin** → Trap 3 expansion |
| Meta / schema presence | Gate 6 ✓ | — | SEO | covered (mechanical) |
| Meta / schema *naturalness* + earned credibility | — | **Trap 4** ✓ + C-014 | SEO | covered but under-cited → harden Trap 4 |
| Token discipline | (none) | **Trap 5** ✓ + C-012 | DESIGN/BRAND | covered (semantic) |
| Inline-data vs content-collection | — | C-016 | — | covered (correction) |
| Nested interactive / hardcoded entity strings | Gate 4 partial | C-013, C-015 | — | covered (corrections) |
| Responsive overflow | Gate 9 ✓ | — | — | covered (mechanical) — but *breakpoint design intent* uncovered → responsive-breakpoint-neglect candidate |
| Motion / interaction design | (none) | — | — | **fully uncovered** → motion/interaction-design-absence candidate |
| Brand visual match | Gate 8 (manual) | — | BRAND | covered (manual + voice) |

## §7 Findings for downstream deliverables

1. **5 trap candidates are spatially justified** by genuine "neither" rows: **AI-slop-hero** (top — only manual Gate 8 touches it), **CWV-as-design-debt** (3 uncovered CWV→design links), **responsive-breakpoint-neglect**, **motion/interaction-design-absence**. (Final set chosen by WDR-3 empirical evidence per operator Q2 deferral.)
2. **2 existing traps need hardening, not replacement**: Trap 3 (Accessibility Theater — add the ~30%/~57% coverage-ceiling citations + SC 2.5.8) and Trap 4 (SEO Stuffing — add earned-credibility / naturalness dimension). This *is* the `source_diversity` remediation — adding ADR/external cites to traps that currently have none.
3. **No duplication risk** if the campaign stays semantic: every mechanical concern (CLS/LCP number, axe rules, meta presence, responsive overflow) is already gated by SiteForge. III's job is the *design-decision interpretation* of those signals, never re-measuring them.
4. **2 cross-consumer wrappers (VideoForge, CanvasForge) import `web_design`** → any pack revision is a 3-wrapper-minimum minor-bump sweep (SiteForge + VideoForge + CanvasForge), plus wga + LPWhitepaper if they carry the pack — confirmed at WDR-6.

## Cross-references
- Pairs with: [[wdr_external_sota_dossier]] (external residue map §6), [[wdr_empirical_gap_ledger]] (fills the "neither" rows with real findings), [[wdr_web_design_pack_revision_spec]] (acts on §7).
- Sources: `context_iii_domain_packs_web_design.md`; `SiteForge.aDNA/what/artifacts/sf_m02_quality_gate_framework.md`; `SiteForge.aDNA/what/context/siteforge/siteforge_reviewers.yaml`; `SiteForge.aDNA/iii/CLAUDE.md`; `what/decisions/adr_002_consumer_federation_contract.md`.
