---
artifact_class: correction_candidate_ledger
type: artifact
title: "Campaign F (Operation Tell) — F3 correction-candidate ledger (C-029..C-033 provisional; candidates-only)"
mission: campaign_web_design_deep_review F3
campaign: campaign_web_design_deep_review
created: 2026-05-26
updated: 2026-05-26
last_edited_by: agent_argus
status: candidate_ledger
boundary_posture: boundary_preserving
graduation_posture: "candidates only — graduate at a later natural-frequency gate (operator decision 2026-05-25)"
hard_invariant: "NOT the canonical store. Zero append to iii_corrections_canonical.jsonl; canonical md5 5adb0dfa38d9224649c3b2cba83852ae (28 entries) INVARIANT. These rows carry graduation_candidate:true and live HERE, never in the canonical jsonl, until a future ADR-003 §3 ceremony promotes a re-observed pattern from a consumer local store."
source_of_record: [wdr_empirical_gap_ledger, wdr_web_design_pack_revision_spec]
tags: [artifact, campaign_f, operation_tell, web_design, correction_candidates, candidates_only, canonical_md5_invariant, adr_003, adr_007]
---

# F3 — Correction-Candidate Ledger (CANDIDATES ONLY)

> **What this is**: the five web-design correction candidates surfaced by the WDR-3 empirical
> gap ledger, recorded as `graduation_candidate: true` rows **outside** the canonical learning
> store. Per operator decision (2026-05-25) Campaign F is candidates-only — **none of these
> graduate in-campaign**. They wait at the `GRADUATION_CANDIDATE` stage (ADR-007 §1) until a
> future natural-frequency gate, at which point a consumer vault that re-observes the pattern
> proposes it through the ADR-003 §3 ceremony from its own local store.
>
> **Hard invariant**: the canonical `iii_corrections_canonical.jsonl` is **not** touched.
> md5 stays `5adb0dfa38d9224649c3b2cba83852ae`; `wc -l` stays `28`. (DG-F criteria 5 + 6.)
>
> **Provisional IDs** `C-029..C-033` are **placeholders** for cross-referencing only. Real IDs
> are assigned at a future graduation ceremony — they may differ, and they are *not* reserved in
> the canonical sequence.

## Evidence-discipline note (read before trusting the `frequency` field)

The empirical corpus (WDR-3) is three already-high-quality, already-III-reviewed sites, so the
residue is thin. **Only C-029 genuinely reaches 2-vault** (ScienceStanley strong + wga weak).
The other four are **1-vault** (ScienceStanley) — they are candidates *precisely because* they
await a second independent vault before any ADR-003 §3 promotion. Each `frequency` value below
is the honest count of distinct vaults that observed the pattern; the ledger does not claim
2-vault where only one exists. (Per ADR-003 §3: graduation needs `frequency ≥ 3` across ≥2
sessions + `acceptance ≥ 0.80`; none clear that bar today, which is the correct candidate state.)

Schema deviation (intentional, candidate-ledger only): `accepted` is set to `null` because none
of these has ever been *proposed* to a user in a live review — there is no acceptance signal yet.
A canonical entry would carry a boolean; a pre-proposal candidate honestly carries `null`.

## The 5 candidates

### C-029 (prov.) — `image_component_omits_dimensions` · Trap 6 (CWV-as-Design-Debt) · **2-vault (strongest)**

```json
{"id":"C-029-prov","trap":"web_design","pattern":"image_component_omits_dimensions","description":"A shared/reusable image component ships without intrinsic width/height and with loading=\"lazy\", so an LCP-class element lazy-loads and CLS rises. The defect is a component-design decision that compounds across every page using the component — not a one-off perf glitch.","example":"ScienceStanley ContentImage.astro:19 sets loading=\"lazy\" with no width/height on the LCP hero image (hero at index.astro:36); baseline CLS 0.168. wga renders an eager full-bleed hero photo (Hero.astro:20 loading=\"eager\") driving home LCP 2.9s — same design-debt class, weaker.","source_review":"campaign_f_wdr3_gap_ledger","source_finding":"WDR-3 §2 ContentImage-no-dims (strong) + WDR-3 §1 wga-eager-hero (weak)","frequency":2,"accepted":null,"graduated":false,"graduated_to":null,"graduation_candidate":true,"created":"2026-05-26"}
```
- **Evidence**: ScienceStanley `ContentImage.astro:19` + `index.astro:36` (CLS 0.168) — strong; wga `Hero.astro:20` (LCP 2.9s) — weak.
- **Vaults**: 2 (SS + wga).
- **Disposition**: strongest candidate; **defer** to a later ADR-003 §3 gate (needs `frequency ≥ 3` across ≥2 sessions — currently 2 vaults, no accepted-rate signal yet).

### C-030 (prov.) — `semantic_data_value_no_shared_source` · Trap 5 (Design Token Drift, precision sub-case) · 1-vault

```json
{"id":"C-030-prov","trap":"web_design","pattern":"semantic_data_value_no_shared_source","description":"A semantic data value (e.g. a legend/category color) is legitimately a literal, but it is duplicated across pages with no single shared source-of-truth — producing inconsistent values for the same logical entity. Distinct from raw token-drift (which wants var(--token)): the fix here is one shared data map, not tokenization. This is the Trap 5 precision sub-case that prevents false-positives on legitimate data literals.","example":"wga ecoregion colors: Central Valley #8B6914 (atlas.astro:12) vs #6B5010 (stories.astro:12); Sierra #6B7280 vs #4B5563 — three separate hex tables across atlas.astro / stories.astro / StoryCard.astro, with no shared ecoregion→color map.","source_review":"campaign_f_wdr3_gap_ledger","source_finding":"WDR-3 §1 same-ecoregion-different-hex (Trap 5 precision gap)","frequency":1,"accepted":null,"graduated":false,"graduated_to":null,"graduation_candidate":true,"created":"2026-05-26"}
```
- **Evidence**: wga `atlas.astro:12,22` vs `stories.astro:12` + `StoryCard.astro:13,15`.
- **Vaults**: 1 (wga) — strong on the one site.
- **Disposition**: **defer pending a 2nd vault.** (The Trap 5 *detection-method* precision fix already landed in the pack at F2; this candidate is the correction-store counterpart that graduates when independently re-observed.)

### C-031 (prov.) — `lcp_element_lazy_loaded` · Trap 6 (CWV-as-Design-Debt) · 1-vault

```json
{"id":"C-031-prov","trap":"web_design","pattern":"lcp_element_lazy_loaded","description":"The Largest Contentful Paint element is lazy-loaded (loading=\"lazy\" or below-fold hydration), delaying LCP. This is a design/component decision about render priority, not a runtime perf glitch — the LCP element should load eagerly / be preloaded.","example":"ScienceStanley hero image at index.astro:36 is rendered via ContentImage, which forces loading=\"lazy\" (ContentImage.astro:19) — so the LCP element defers. (Closely related to C-029; recorded separately because the lazy-LCP signal can occur without a shared image component.)","source_review":"campaign_f_wdr3_gap_ledger","source_finding":"WDR-3 §2 ContentImage lazy LCP","frequency":1,"accepted":null,"graduated":false,"graduated_to":null,"graduation_candidate":true,"created":"2026-05-26"}
```
- **Evidence**: ScienceStanley `index.astro:36` via `ContentImage.astro:19`.
- **Vaults**: 1 (SS).
- **Disposition**: **defer.** Narrower sibling of C-029 (the lazy-LCP signal standalone); graduates if observed independently of the shared-component case.

### C-032 (prov.) — `view_transitions_absent_on_multipage_flow` · motion candidate-trap (NOT a landed pack trap) · 1-vault, refuted on 2

```json
{"id":"C-032-prov","trap":"web_design","pattern":"view_transitions_absent_on_multipage_flow","description":"A multi-page site has no view transitions and unconsidered interaction states across a navigation flow where they would materially improve perceived continuity — an interaction-design absence. Maps to the deferred 'motion/interaction-design-absence' candidate trap (not landed in the pack at F1; motion was deferred per the F0 trap-set decision).","example":"ScienceStanley ships zero astro:transitions site-wide (grep view-transition = 0 hits); only a .reveal fade-up at global.css:310. Explicitly REFUTED on wga and ContextCommons — both ship <ClientRouter/> view transitions.","source_review":"campaign_f_wdr3_gap_ledger","source_finding":"WDR-3 §2 no-view-transitions; refuted §1 (wga) + §3 (CC)","frequency":1,"accepted":null,"graduated":false,"graduated_to":null,"graduation_candidate":true,"created":"2026-05-26"}
```
- **Evidence**: ScienceStanley `global.css:310` (no `astro:transitions`); refuted by wga + ContextCommons (`<ClientRouter/>` present).
- **Vaults**: 1 (SS); **explicitly refuted on 2 sites.**
- **Disposition**: **defer (thinnest).** Maps to the deferred *motion candidate-trap*, not Trap 6/7. The 2-site refutation is recorded so a future reviewer does not over-weight the single positive.

### C-033 (prov.) — `webfont_reflow_no_font_display` · Trap 6 (CWV-as-Design-Debt) · 1-vault

```json
{"id":"C-033-prov","trap":"web_design","pattern":"webfont_reflow_no_font_display","description":"Webfonts are imported without font-display / preload, so a font swap reflows layout and raises CLS. This is a base-layout design decision (how fonts are loaded), distinct from any single component.","example":"ScienceStanley baseline CLS 0.168 from Inter font reflow — bare @fontsource imports with no preload or font-display in BaseLayout.astro:5-6.","source_review":"campaign_f_wdr3_gap_ledger","source_finding":"WDR-3 §2 CLS 0.168 Inter reflow","frequency":1,"accepted":null,"graduated":false,"graduated_to":null,"graduation_candidate":true,"created":"2026-05-26"}
```
- **Evidence**: ScienceStanley `BaseLayout.astro:5-6` (bare `@fontsource`); baseline CLS 0.168.
- **Vaults**: 1 (SS).
- **Disposition**: **defer.** Font-loading sibling of the CWV-as-design-debt family; graduates on independent re-observation.

## Summary

| Prov. ID | Pattern | Trap | Vaults | `frequency` | Disposition |
|---|---|---|---|---|---|
| C-029 | `image_component_omits_dimensions` | 6 | SS + wga | 2 | strongest; defer to later §3 gate |
| C-030 | `semantic_data_value_no_shared_source` | 5 | wga | 1 | defer pending 2nd vault |
| C-031 | `lcp_element_lazy_loaded` | 6 | SS | 1 | defer |
| C-032 | `view_transitions_absent_on_multipage_flow` | motion candidate-trap | SS (refuted ×2) | 1 | defer; thinnest |
| C-033 | `webfont_reflow_no_font_display` | 6 | SS | 1 | defer |

## Reinforced existing corrections (NO ACTION — future re-graduation-review inputs)

The corpus gave three *already-canonical* corrections fresh ≥2-vault provenance. They are
recorded here as inputs for a future re-graduation-review only; **F3 takes no action** on them
and the canonical store is untouched (per WDR-4 §4 / WDR-6 §3.5):

- **C-012** `brand_color_dual_role` — wga (raw hex) + ScienceStanley (42 `rgba` literals).
- **C-015** `hardcoded_entity_strings` — wga + ContextCommons (27 inline entity strings).
- **C-016** `inline_data_instead_of_collection` — wga (4 inline arrays) + ContextCommons (`faqItems`).

## Cross-references

- Source-of-record: [[wdr_empirical_gap_ledger]] §4 (cross-site provenance) + [[wdr_web_design_pack_revision_spec]] §4 (candidate table).
- Governance: `what/decisions/adr_003_learning_store_ownership.md` §3/§3.6 (graduation ceremony) · `what/decisions/adr_007_adaptive_improvement_loop.md` §1 (CorrectionLifecycle `GRADUATION_CANDIDATE` stage).
- Live pack (where Trap 6/7 + the Trap 5 precision fix landed at F1/F2): `what/context/core_domain_packs/context_iii_domain_packs_web_design.md`.
- Canonical store (UNTOUCHED): `what/context/core_domain_packs/iii_corrections_canonical.jsonl` (md5 `5adb0dfa38d9224649c3b2cba83852ae`, 28 entries).
