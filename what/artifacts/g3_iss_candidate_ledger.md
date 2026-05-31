---
artifact_class: iss_candidate_ledger
type: artifact
title: "Campaign G (Operation Atrium) — G3 ISS-surface candidate ledger (ISS-C6/C8/C9 provisional; candidates-only) + DP-4 graduation disposition"
mission: campaign_g_consolidation G3
campaign: campaign_g_consolidation
created: 2026-05-30
updated: 2026-05-30
last_edited_by: agent_argus
status: candidate_ledger
boundary_posture: boundary_preserving
graduation_posture: "candidates only — ISS-trap axes LAND INTO THE PACK (not the canonical jsonl) when live residue actualizes; DP-4 C-029 disposition = candidates-only (no consumer proposal)"
hard_invariant: "NOT a pack edit and NOT the canonical store. Zero append to iii_corrections_canonical.jsonl; canonical md5 5adb0dfa38d9224649c3b2cba83852ae (28 entries) INVARIANT. Zero edit to context_iii_iss_surfaces.md (the candidates stay candidates). These axes carry landing_candidate:true and live HERE until a future a11y/operational re-inspection actualizes live residue and promotes the axis into the pack as a full trap."
source_of_record: [g0_iss_pack_revision_spec, g0_iss_empirical_gap_ledger, context_iii_iss_surfaces]
tags: [artifact, campaign_g, operation_atrium, iss_surfaces, iss_candidate_ledger, candidates_only, canonical_md5_invariant, dp_4, conditional_graduation, adr_003, adr_007, adr_008]
---

# G3 — ISS-Surface Candidate Ledger (CANDIDATES ONLY) + DP-4 Disposition

> **What this is**: the three ISS-surface trap *axes* that were specified in the G0 pack-revision
> spec (`g0_iss_pack_revision_spec.md` §3) but **not landed** into `context_iii_iss_surfaces.md`
> at G1 — recorded as `landing_candidate: true` rows in a standalone, auditable ledger. They are
> the held a11y axes (Candidate 6 SR-under-glass, Candidate 8 touch-target) plus the operational
> Candidate 9 (gate-path erosion). The landed pack already carries them compactly (§"Candidate
> Axes", lines 131-137); G3 promotes that note to the full `f3`-shape ledger DG-G crit 5 requires.
>
> **Critical distinction from `f3_correction_candidates.md`** (read this before reasoning about
> graduation): the `f3` rows are **learning-store correction candidates** whose promotion target
> is the canonical `iii_corrections_canonical.jsonl` (via an ADR-003 §3 ceremony from a consumer's
> *local* store). The rows in *this* ledger are **ISS trap-AXIS candidates** whose promotion
> target is the **pack** (`context_iii_iss_surfaces.md`) — they "land" as new full Traps when
> live residue actualizes during a future review, not via canonical-jsonl graduation. They carry
> **no `C-0xx` ID** (that sequence belongs to the canonical correction store); they use
> `ISS-C<n>` provisional IDs keyed to the `g0_iss_pack_revision_spec.md` §3 axis numbers.
>
> **Hard invariant**: neither the canonical `iii_corrections_canonical.jsonl` **nor** the landed
> `context_iii_iss_surfaces.md` pack is touched by this ledger. Canonical md5 stays
> `5adb0dfa38d9224649c3b2cba83852ae`; `wc -l` stays `28`. The pack stays at its G1 6-trap /
> composite-4.00 state. (DG-G criteria 5 + 6 + 10.)
>
> **§DP-4** (below the candidate ledger) records the separate **C-029 graduation disposition** —
> the one place in Campaign G where the canonical store *could* rotate. It does not: no consumer
> proposal has arrived.

## Evidence-discipline note (read before trusting the `frequency` field)

The ISS-adopter corpus is small and young — 3 live wrappers, only 1 (MoleculeForge) with a
completed `.output.json`, the richest rendered corpus (ZenZachary's 14 P1 gates) carrying *zero*
operator responses yet (`g0_iss_empirical_gap_ledger.md` §2). Against that corpus, the **G1 a11y
re-inspection** of ZenZachary's rendered gates (`p1_1r` / `p1_1a`) found that the SiteForge
primitives library **already covers the standard a11y surface** — `:focus-visible` siblings
present (≥ `:hover` count), decoration `aria-hidden`, `prefers-reduced-motion` + `max-width:600px`
responsive rules, `sr-only` legends present. So the two a11y axes (6, 8) caught **no live
residue**: they are **latent**, actualizing only if a vault adds *template-local* custom
interactive elements that bypass the primitives contract. The operational axis (9) found **no
erosion at all** (MoleculeForge archive hygiene intact) → zero vaults.

Each `frequency` value below is the honest count of distinct vaults with **live** residue: it is
**0 for all three** today. These are candidates *precisely because* the substrate currently
covers the surface — they are **substrate-grounded but empirically-latent**, the same candidate
bar `f3` applied to the non-landed motion-trap (C-032). The substrate citations make each axis
*authorable on sight* the moment residue appears; the empirical ledger only sets the landing
*priority*. (Per `g0_iss_empirical_gap_ledger.md` §4 + the landed pack's candidate-axes §.)

Schema note (candidate-ledger only): `landed:false` + `landing_candidate:true` mark axis-level
(pack-landing) candidacy; there is no `accepted`/`graduated` field because these axes do not
traverse the ADR-003 correction-graduation lifecycle — they land into the pack, not the canonical
store.

## The 3 candidates

### ISS-C6 (prov.) — `screen_reader_semantics_under_glassmorphic_skin` · ISS-Trap-axis 6 (a11y) · **0-vault live / latent (substrate-grounded)**

```json
{"id":"ISS-C6-prov","axis":"iss_surfaces","pattern":"screen_reader_semantics_under_glassmorphic_skin","description":"Decorative glass layers (PRIM-12/14/15 decoration_svg/bg_grid/top_glow + backdrop-blur) are prioritized while keyboard-focus + SR semantics are left at defaults — confidence radios without a C-D9 confidence_tooltips SR-parity legend, :focus-visible gaps on custom interactive elements, decoration not aria-hidden. Excludes SR/keyboard operators from the decision.","example":"A glass-heavy gate where confidence radios lack the C-D9 SR legend and decorative SVG is not hidden from the a11y tree.","source_review":"g0_iss_empirical_gap_ledger §3 (EGL 'not yet observed') + g1 a11y re-inspection (ZenZachary p1_1r/p1_1a)","source_finding":"substrate-evidenced (primitives anti-patterns L33-35, C-D9, C-D6-1); G1 a11y re-inspection found library covers the standard surface → live residue NOT confirmed (latent only)","frequency":0,"landed":false,"landing_candidate":true,"created":"2026-05-30"}
```
- **Evidence**: substrate-grounded — primitives anti-patterns L33-35, C-D9 `confidence_tooltips`, C-D6-1 no-hover-only, WCAG 2.2 (accessible-name / focus). G1 a11y re-inspection of ZenZachary `p1_1r`/`p1_1a` rendered gates: `:focus-visible` siblings present, decoration `aria-hidden`, `sr-only` legends present → **no live residue**.
- **Vaults**: **0 live** (latent). Actualizes only on template-local custom interactive elements that bypass the primitives contract. (The one near-residue observed — confidence radios without polarity semantics — is owned by **landed Trap 7**, not here.)
- **Disposition**: **held as candidate.** Lands into `context_iii_iss_surfaces.md` as a full Trap when a vault adds template-local custom elements actualizing the residue (focused a11y re-inspection at G5 or a later review).

### ISS-C8 (prov.) — `mobile_responsive_touch_target_discipline` · ISS-Trap-axis 8 (a11y) · **0-vault live / latent (substrate-grounded)**

```json
{"id":"ISS-C8-prov","axis":"iss_surfaces","pattern":"mobile_responsive_touch_target_discipline","description":"New interactive elements added to a gate's template <style> don't mirror the C-D6-1 contract — sub-44px tap targets, 2-col grids without a ≤640px single-column companion, hover-only affordances, text containers without overflow-wrap. Excludes touch/mobile operators.","example":"A custom .axis-row 2-col grid with no @media (max-width:640px) single-col rule; a custom button under 44px at mobile.","source_review":"g0_iss_empirical_gap_ledger §3 (EGL 'not yet observed') + g1 a11y re-inspection","source_finding":"substrate-evidenced (C-D6-1, AD-9, WCAG 2.2 SC 2.5.8); G1 re-inspection found the library-rendered surface honors ≥44px targets + a max-width:600px companion → live residue NOT confirmed (latent to template-local additions)","frequency":0,"landed":false,"landing_candidate":true,"created":"2026-05-30"}
```
- **Evidence**: substrate-grounded — C-D6-1, AD-9, WCAG 2.2 SC 2.5.8 (24px floor; substrate ≥44px exceeds it). G1 re-inspection: library-rendered surface honors ≥44px targets + a `max-width:600px` companion → **no live residue**.
- **Vaults**: **0 live** (latent). Residue is specific to template-local interactive additions (primitives.css covers the standard set).
- **Disposition**: **held as candidate.** Sibling of ISS-C6; lands when a template-local custom element violates the C-D6-1 contract on a live rendered gate.

### ISS-C9 (prov.) — `gate_path_discipline_erosion` · ISS-Trap-axis 9 (operational) · **0-vault (no erosion observed)**

```json
{"id":"ISS-C9-prov","axis":"iss_surfaces","pattern":"gate_path_discipline_erosion","description":"The AD-6 4-file contract (<gate_id>.{html,pending,output.json,draft.json} + archive/<gate_id>/) + two-file sentinel discipline isn't honored — orphaned .pending after .output.json lands, un-archived completed gates, slug-invalid gate_id under D8 flags. Operational hygiene, not decision quality.","example":"A .pending left after .output.json lands; completed gates never moved to archive/.","source_review":"g0_iss_empirical_gap_ledger §3 (#9 no erosion) + g0_iss_pack_revision_spec §3 ISS-Trap 9","source_finding":"substrate-grounded (AD-6, skill_watch_iss D7-C69); MoleculeForge archive hygiene intact → NO erosion observed","frequency":0,"landed":false,"landing_candidate":true,"created":"2026-05-30"}
```
- **Evidence**: substrate-grounded — AD-6 4-file contract, `skill_watch_iss` D7-C69. MoleculeForge archive hygiene **intact** at G0 recon → no erosion.
- **Vaults**: **0** (no residue of any strength; this is the honest absence, not latency).
- **Disposition**: **first-defer.** Land as a *lighter "operational" trap* only if erosion appears as the adopter community accrues gates; distinct in kind from the decision-quality traps (Severity Low-Medium). Lowest landing priority of the three.

## Summary

| Prov. ID | Pattern | ISS-Trap axis | Vaults (live) | `frequency` | Strength | Disposition |
|---|---|---|---|---|---|---|
| ISS-C6 | `screen_reader_semantics_under_glassmorphic_skin` | 6 (a11y) | 0 | 0 | latent (substrate-grounded) | hold; land on template-local a11y residue |
| ISS-C8 | `mobile_responsive_touch_target_discipline` | 8 (a11y) | 0 | 0 | latent (substrate-grounded) | hold; land on template-local touch residue |
| ISS-C9 | `gate_path_discipline_erosion` | 9 (operational) | 0 | 0 | no erosion observed | first-defer; lighter operational trap |

All three are **0-vault live** — none clears a landing bar today; that is the correct candidate
state. The pack's 6 landed traps {1,2,3,4,5,7} (composite 4.00) already cover every L3 residue
*actually observed* in the G0 empirical pass (next section).

## Reinforced landed traps (NO ACTION — the residue that IS covered)

The G0 empirical gap ledger (`g0_iss_empirical_gap_ledger.md` §3) surfaced six live/latent L3
residues (EGL-1..6); **every one maps to a trap already landed at G1**, so they reinforce the
landed pack rather than motivate a new candidate. Recorded here as future re-inspection inputs
only; **G3 takes no action** and the pack is untouched:

- **EGL-1** RLHF capture incomplete (`rlhf_reviewer_persona` NULL; `axis_scores` null) → **landed Trap 4** (RLHF-completeness). MoleculeForge `p6_1`; live, 1-vault.
- **EGL-2** semantic outcome labels unexplained (`approve/amend/discuss` undecoded) → **landed Traps 1 / 5** (decision-frame / option-set). ZenZachary `p1_1r`/`p1_1a`; live, 1-vault.
- **EGL-3** confidence-scale polarity not surfaced (1-5 direction unstated) → **landed Trap 7** (confidence under-signaling). MoleculeForge `p6_1` + ZenZachary; live, 1-vault + latent.
- **EGL-4** persona-voice drift risk (generator voice vs techno-cyber-monk brief) → **landed Trap 2** (persona/voice drift). ZenZachary; latent, 1-vault.
- **EGL-5** confidence omitted on simpler gates (rank-only adaptive but unsignaled) → **landed Traps 7 / 1**. ZenZachary; live, 1-vault.
- **EGL-6** axis-ranker optionality unsignaled (`axis_scores` nullable; required-or-bonus?) → **landed Traps 4 / 1**. ZenZachary `p1_1r`; live, 1-vault.

(Multi-vault confirmation for these accrues as ZenZachary's 14 gates collect `.output.json`
responses and MoleculeForge/WilhelmAI add gates — `g0_iss_empirical_gap_ledger.md` §4.)

## §DP-4 — C-029 graduation disposition (the sole non-invariant gate; resolved INVARIANT)

DP-4 is the **only** point in Campaign G where the canonical learning store could rotate. The
charter (§6 DP-4, R2) made the firing **conditional**: graduate **C-029**
(`image_component_omits_dimensions`, the 2-vault SS+wga `web_design` candidate from
`f3_correction_candidates.md`) via an ADR-003 §3 ceremony **iff** a consumer vault re-observed
the pattern from its own local store and proposed it over the ADR-008
`learning_store_graduation` transport — **else** candidates-only continuation.

**Verified on-disk (2026-05-30) — no proposal has arrived:**

| Check | Source | Result |
|---|---|---|
| Aggregation index entries for C-029 | `who/coordination/.aggregation/graduation_proposals_index.jsonl` | **none** — only 2 VideoForge seed records (C-027 / C-028, MD-B4 retroactive seeds) |
| Inbound `cross_vault_request` `learning_store_graduation` for C-029 | `who/coordination/` sweep | **none** from any consumer |
| MoleculeForge local store (likeliest re-observer w/ outputs) | `MoleculeForge.aDNA/iss/…` local jsonl | **0 bytes** — no ACCUMULATE writes yet |
| VisualDNA (named "next C-029 slot" in the lite-ack reply) | wrapper status | **no wrapper shipped** — pre-v1.0-GA |

**Disposition: CANDIDATES-ONLY.** No ADR-008 proposal exists, so the ADR-003 §3 ceremony does
not fire. **Canonical `iii_corrections_canonical.jsonl` md5 stays `5adb0dfa38d9224649c3b2cba83852ae`;
`wc -l` stays `28`.** C-029 remains a `graduation_candidate:true` row in
`f3_correction_candidates.md`, awaiting a future natural-frequency gate when a consumer vault
re-observes the pattern from its own store. This satisfies **DG-G crit 6** on the
canonical-INVARIANT branch (per charter: either branch satisfies the criterion) and is the
charter R2-expected outcome (T4 conditional, never block-closing). Operator ratifies at G3 close.

## Cross-references

- Source-of-record: [[g0_iss_pack_revision_spec]] §3 (9 candidate axes) / §4 (recommended landing set) + [[g0_iss_empirical_gap_ledger]] §3 (EGL-1..6 + "not yet observed" a11y/operational) / §4 (evidence-discipline).
- Landed pack (where {1,2,3,4,5,7} landed at G1; carries the compact candidate-axes note this ledger expands): `what/context/core_domain_packs/context_iii_iss_surfaces.md` (§"Candidate Axes", lines 131-137).
- Governance: `what/decisions/adr_003_learning_store_ownership.md` §3/§3.6 (correction graduation ceremony — governs §DP-4) · `what/decisions/adr_007_adaptive_improvement_loop.md` §1 (CorrectionLifecycle `GRADUATION_CANDIDATE` stage) · `what/decisions/adr_008_cross_vault_aggregation.md` (cross-vault `learning_store_graduation` transport + aggregation index — the §DP-4 inbound channel).
- DP-4 evidence: `who/coordination/.aggregation/graduation_proposals_index.jsonl` (2 VideoForge seeds; zero C-029) · `f3_correction_candidates.md` C-029 (the candidate that did NOT graduate).
- Canonical store (UNTOUCHED): `what/context/core_domain_packs/iii_corrections_canonical.jsonl` (md5 `5adb0dfa38d9224649c3b2cba83852ae`, 28 entries).
- Shape precedent: `what/artifacts/f3_correction_candidates.md` (Campaign F candidates-only ledger).
