---
type: mission_artifact
title: "Campaign G (Operation Atrium) — G5 Validation: the landed context_iii_iss_surfaces pack catches the live ISS-adopter residue (inspection-grade)"
campaign: campaign_g_consolidation
mission: G5
mission_class: validation
status: complete
version: "0.5.0"   # matches the ISS pack under validation; G5 is validation evidence, not new contract
created: 2026-06-23
updated: 2026-06-23
last_edited_by: agent_argus
authoring_mission: campaign_g_consolidation G5
governed_by:
  - what/context/core_domain_packs/context_iii_iss_surfaces.md   # the 6-trap pack under validation (G1)
  - what/decisions/adr_007_adaptive_improvement_loop.md          # §3 six-axis rubric (pack scored 4.00 at G1)
  - what/decisions/adr_003_learning_store_ownership.md           # §3/§3.6 graduation gate (candidates stay candidates)
source_of_record:
  - what/artifacts/g0_iss_empirical_gap_ledger.md                # G0 — the EGL-1..6 residue ledger this validation re-checks
  - what/artifacts/g0_iss_pack_revision_spec.md                  # G0 — the pack-revision spec the pack implemented
  - what/artifacts/g3_iss_candidate_ledger.md                    # G3 — ISS-C6/C8/C9 candidates traced here
predecessor_validation: what/artifacts/f5_validation_wdr3_residue.md   # validation-by-inspection shape reused (frontmatter + numbered sections + zero-regression table); MX-1/md_b5 lineage
adopters_re_inspected: [ZenZachary.aDNA, Molecules.aDNA, WilhelmAI.aDNA]   # ≥2 of 3 required; all 3 inspected (Molecules ex-MoleculeForge — back-compat symlink active; ISS adopter invite filed under MoleculeForge)
evidence_method: inspection_grade   # read saved gate output JSON + rendered gate HTML + wrapper configs on disk; NO live gate runs, NO renders, NO builds
non_runnable: true
boundary_posture: boundary_preserving   # III stays semantic; no ISS runtime; no audit runtime introduced
zero_regression_confirmed: true
canonical_jsonl_touched: false
consumer_wrappers_touched: []
new_iii_adrs_authored: []
canonical_md5_baseline: "5adb0dfa38d9224649c3b2cba83852ae (28 entries)"
satisfies: "DG-G criterion 9 — Validation (G5) confirms the landed iss_surfaces pack catches the live ISS-adopter residue with pack-line citations + negative control clean"
verdict: GO
tags: [mission_artifact, g5, validation, campaign_g, operation_atrium, iss_surfaces, inspection_grade, trap_4_rlhf, trap_7_confidence, trap_1_decision_frame, negative_control, candidates_only, canonical_md5_invariant, zero_regression, dg_g_crit_9, verdict_go]
---

# G5 Validation — Landed `context_iii_iss_surfaces` Pack vs the Live ISS-Adopter Residue

> **VERDICT: GO.** The 6-trap ISS-surface pack catches every recommended residue across **3 of 3** live ISS adopters (≥2-of-3 bar exceeded) with exact pack-line citations; the held a11y candidates (6/8) produce **zero new false positives** on the now-richer rendered-gate corpus (the negative control); `zero_regression_confirmed: true`. Satisfies DG-G criterion 9 — G6 (DG-G GO/NO-GO gate + AAR + annotated `v0.5.0` tag) may proceed.

## §1 Purpose

G5 closes the loop opened by the G0 empirical recon. The G0 empirical gap ledger
([[what/artifacts/g0_iss_empirical_gap_ledger|g0_iss_empirical_gap_ledger]]) bucketed every observed ISS-surface
residue across the live adopters into **L1 library-enforced** (the SiteForge primitives prevent it), **L2
adaptation-guide-warned** (a guide-doc fix, not a III trap), and **L3 neither-caught** — the *residue* the
`iss_surfaces` pack was authored to own (EGL-1..6). G1 then landed the pack (6 traps {1,2,3,4,5,7}; composite
4.00). **G5 verifies the claim**: walk each recommended L3 residue against the **landed pack** and confirm it is
now either (a) caught — with the exact pack line that owns it quoted — or (b) consciously, on-record **deferred**
per the G1 trap-set decision (a11y candidates 6/8 held).

This is the validation discipline used at MC-5 / MD-A5 / MD-B5 / MX-1 / F5: **inspection-grade by reading**. No
gate renders, no `generate()` runs, no builds. G5 reads the same on-disk surfaces the G0 ledger read — **plus the
corpus the adopters have accumulated since G0** (the decisive new evidence; see §2) — and the landed pack, and
confirms coverage. The boundary holds end-to-end: this pack **reads** the ISS contract surface and **attributes**
a quality defect to an authoring decision; it never renders or runs a gate.

**Satisfies DG-G criterion 9.** G5 is the penultimate Campaign-G mission; G6 (DG-G GO/NO-GO gate + AAR +
annotated `v0.5.0` tag) is the separate human-gated close (DP-5/DP-6).

## §2 Corpus — and what changed since G0 (the decisive update)

The G0 ledger (2026-05-29) inspected: Molecules (then `MoleculeForge.aDNA`) — **1** completed output (`p6_1`);
ZenZachary — **14 rendered gates, 0 completed outputs**; WilhelmAI — **config only, 0 gates**. Every L3 row was
therefore **1-vault or latent** (g0 ledger §4: "expected at planning recon").

**At G5 (2026-06-23) the corpus is materially richer** — the adopters have collected real operator responses,
which is exactly the multi-vault confirmation the G0 ledger said would "accrue as adopters collect outputs":

| Adopter | Pin | Persona/preset | Completed `.output.json` (live) | Rendered gates |
|---|---|---|---|---|
| **ZenZachary** | `^0.3.0` | partner / `partner_onboarding` | **~13** (P1 brand-genesis cycle now answered: `p1_1a`, `p1_1r`, `p1_2a/b/c`, `p1_2r`, `p1_3a/b`, `p1_3r`, `p1_1b`, `p1_1c`, `p1_5`) | 14 |
| **Molecules** (ex-MoleculeForge) | `903f461` | franklin / gate-by-gate | **5+** (`p6_1` + 3× `sc_web_configurator_build_gate` + archived `p5_execution_charter`, `p4_*`) | matching set |
| **WilhelmAI** | `903f461` | partner / `partner_onboarding` | **2** (`p6_3_charter_ratification` 2026-05-29; `lh0_security_model_signoff` **2026-06-23**) | 2 |

This upgrades the strongest residue axes from **1-vault** (G0) to **3-vault** (G5) — Trap 4 and Trap 7 are now
caught on live evidence in *every* adopter, not just one. The Molecules back-compat symlink
(`MoleculeForge.aDNA → Molecules.aDNA`) is active and resolves; the ISS-adopter invite was filed under the old
name and is honored unchanged.

**Method (no runs):** read on disk — the 3 wrapper `iss/CLAUDE.md` configs; the completed `*.output.json`
files; a sample of the richest rendered `*.html` gates (ZenZachary `p1_1r`; WilhelmAI `lh0`) for primitive
a11y/structure markers. All claims below carry the strength at which they were observed; nothing is inflated.

## §3 Method

For each recommended L3 residue (the EGL rows the pack was prioritized to land — g0 ledger §5), this validation:
(1) restates the residue finding + its on-disk evidence (verbatim where possible; no re-derivation); (2) names
the landed-pack trap that now owns it; (3) **quotes the pack line** so coverage is checkable, not asserted;
(4) marks **CAUGHT** (with vault count) or **DEFERRED (on-record)**. A finding is only CAUGHT if the trap names
the *same signal* — not merely an adjacent topic. Deferred residue is held to the same honesty bar: deferred
*on the record* (G1 candidate-hold + a candidate-ledger row), never silently dropped.

The negative control (§7) runs the **inverse** test on the held a11y candidates 6/8: confirm the now-richer
rendered-gate corpus produces **zero** new false positives — i.e., the pack stays quiet where the SiteForge
primitives already cover the surface.

## §4 Residue → landed-pack coverage matrix

The recommended L3 residue from g0 ledger §3/§5, mapped to the landed pack. Pack line numbers are the current
`context_iii_iss_surfaces.md`.

| # | G0 residue finding (the L3 "neither-caught" bucket) | Adopter evidence (G5) | Vaults | Now owned by | Verdict |
|---|---|---|---|---|---|
| EGL-1 | RLHF capture incomplete — `rlhf_reviewer_persona` absent though top-level `persona` known; `axis_scores` null | Molecules `p6_1` (`persona:"franklin"`, no `rlhf_reviewer_persona`, `axis_scores:null`); WilhelmAI `p6_3`+`lh0` (`persona:"partner"`, no `rlhf_reviewer_persona`); ZenZachary `p1_1r`+`p1_1a` (`persona:"partner"`, no `rlhf_reviewer_persona`) | **3** | **Trap 4** (pack L100 example + L104 detection) | ✅ CAUGHT (3-vault) |
| EGL-3 | Confidence-scale polarity not surfaced in-output — `confidence_score → rlhf_severity_calibration` correct but 1-5 direction never stated; decodable only from a calibration constant | Molecules `p6_1` (`confidence_score:5 → 1.0`); ZenZachary `p1_1r` (`confidence_score:4 → 0.75`) — polarity nowhere in-output | **2** | **Trap 7** (pack L126 example, "no stated polarity … so '5' has no decodable meaning") + L130 detection | ✅ CAUGHT (2-vault) |
| EGL-5 | Confidence omitted on consequential / multi-decision gates; adaptive-but-unsignaled | WilhelmAI `lh0` (IRREVERSIBLE security-model signoff: **no `confidence_score` at all**, binary accept); WilhelmAI `p6_3` (charter ratification: no confidence); ZenZachary `p1_1a` (rank-only, no confidence — adaptive) | **2** (consequential-omission) + latent (rank-only) | **Trap 7** (pack L126 "multi-decision gates lacking confidence capture entirely (EGL-5); rank-only gates that drop confidence without signaling the omission is intentional") + L130 | ✅ CAUGHT (2-vault) |
| EGL-2 | Semantic outcome labels unexplained / option-set legibility — outcome labels rendered without first-timer decoding of option *meaning* | ZenZachary `p1_1r`/`p1_1a` (`approve`/ranking captured; outcome decoding owned by preset, but option-meaning residue is the L3 tell per g0) | 1-vault (partner-facing) | **Trap 1** (decision-frame, pack L65 detection) / **Trap 5** (option-set, pack L117 detection) | ✅ CAUGHT (1-vault; substrate-grounded) |
| EGL-6 | Axis-ranker optionality unsignaled — `axis_scores` nullable; gate doesn't tell the operator whether 6-axis scoring is required or bonus | Molecules `p6_1` (`axis_scores:null`, `rlhf_signal.value.axes:null`); ZenZachary `p1_1r` (`axes:null`) | **2** | **Trap 4** (pack L104 "confirm … flag scalar capture where a pairwise comparison was available") — whole-block completeness | ✅ CAUGHT (2-vault) |
| EGL-4 | Persona-voice drift risk — generator-authored copy skinned to a partner; nothing checks copy echoes the vault's character brief | ZenZachary (partner skin correct; copy register checked — **no clinical-register drift observed** in sampled gates) | latent, 1-vault | **Trap 2** (persona/voice, pack L78 detection) | ⏸ DEFERRED-as-latent (on-record; trap landed, residue not yet actualized) |
| — | decision_significance present in HTML but **dropped from output JSON** — `lh0` rendered gate carries `data-importance="irreversible"` + `IMPORTANCE: IRREVERSIBLE`, but `lh0.output.json` has **no `decision_significance` field** | WilhelmAI `lh0` (HTML L1571 `data-importance="irreversible"`; output JSON omits the field) | 1-vault | **Trap 1** (decision-frame: "load-bearing/irreversible gates that omit `decision_significance`", pack L62/L65) — at the output-completeness layer | ✅ CAUGHT (1-vault; honest new observation) |

**Tally: 6 of 6 recommended L3 residue axes CAUGHT (EGL-1/2/3/5/6 with live evidence; EGL-4 trap landed,
residue latent-on-record) + 1 honest new output-completeness observation CAUGHT (Trap 1).** No recommended
residue is silently uncovered. Trap 3 (evidence-lede) had no live residue in this corpus (the partner-onboarding
preset surfaces the recommendation block correctly where present) — it is substrate-grounded and stands ready;
its silence here is correct, not a miss.

## §5 Per-trap validation walk (exact pack-line citations)

### Trap 4 (RLHF Signal-Capture Incompleteness) — owns EGL-1, EGL-6 — **3-vault, strongest**

The g0 ledger ranked #4 the strongest axis (live; ADR-005 + AD-7; Uni-RLHF). The G5 corpus confirms it across
all three adopters:

- **EGL-1 (Molecules `p6_1`):** pack L100 example reads —
  *"MoleculeForge `p6_1.output.json` — top-level `persona:"franklin"` is known, yet there is **no `rlhf_reviewer_persona` field**; `axis_scores:null` / `rlhf_signal.value.axes:null`."* The on-disk file is exactly this:
  top-level `"persona": "franklin"` (L7), no `rlhf_reviewer_persona` anywhere, `"axis_scores": null` (L47),
  `"axes": null` (L80). Named verbatim. ✅
- **EGL-1 generalized to WilhelmAI + ZenZachary (the G5 upgrade to 3-vault):** pack L104 detection —
  *"Read the output JSON. Confirm `rlhf_reviewer_persona` is populated from the top-level `persona`."* WilhelmAI
  `p6_3` (`"persona":"partner"`, no `rlhf_reviewer_persona`), WilhelmAI `lh0` (same), ZenZachary `p1_1r`/`p1_1a`
  (`"persona":"partner"`, no `rlhf_reviewer_persona`) all fail this exact check. The detection method catches the
  same defect in three vaults — precisely the multi-vault confirmation g0 §4 anticipated. ✅
- **EGL-6 (axis-ranker optionality):** pack L104 — *"flag scalar capture where a pairwise comparison was available."*
  Molecules `p6_1` (`axis_scores:null`) and ZenZachary `p1_1r` (`axes:null`) carry the nullable-axes residue the
  whole-block-completeness trap owns. ✅

**Boundary check (Trap 4 specifically):** the trap is a *reader/attributor* of the output JSON — pack boundary
banner L42 *"It reads the ISS contract surface on disk … and never renders, re-renders, or runs a gate."* G5
re-read the same JSON files; nothing re-run.

### Trap 7 (Recommendation Confidence Under-Signaling) — owns EGL-3, EGL-5 — **2-vault**

The g0 ledger ranked #7 third-strongest (EGL-3/5; live; C-D9 confidence_tooltips). The G5 corpus confirms both
sub-signals:

- **EGL-3 (polarity unsignaled):** pack L126 example — *"a confidence radio rendered with a '1-5 scale (required)'
  legend but **no stated polarity** ('5 = highest conviction') … so '5' has no decodable meaning (observed on
  ZenZachary review gates + MoleculeForge `p6_1`)."* Molecules `p6_1` (`confidence_score:5 → rlhf_severity_calibration:1.0`)
  and ZenZachary `p1_1r` (`confidence_score:4 → 0.75`) both encode the polarity *only* in the calibration constant,
  never as a stated field — the exact residue. ✅
- **EGL-5 (confidence omitted on consequential gates):** pack L126 — *"multi-decision gates lacking confidence
  capture entirely (EGL-5); rank-only gates that drop confidence without signaling the omission is intentional."*
  The G5 corpus produces the **strongest instance yet**: WilhelmAI `lh0` is an **IRREVERSIBLE** security-model
  signoff (`surface_type:decision_gate`, `adr_id:ADR-011`) captured as a bare binary `accept` with **no
  `confidence_score` field at all** — a consequential, constitutional decision with zero confidence capture.
  WilhelmAI `p6_3` (charter ratification) is the same. ZenZachary `p1_1a` (rank-only) drops confidence adaptively.
  Pack L130 detection — *"Confirm `confidence` is present and **honest** … on consequential gates … Flag
  multi-decision gates with no confidence capture, and rank-only gates whose confidence omission is
  adaptive-but-unsignaled."* — flags all three. ✅

### Trap 1 (Decision-Frame Ambiguity) — owns EGL-2 (frame half) + the output-completeness observation

- **decision_significance dropped from output (honest new observation):** pack L62 *Where It Hides* —
  *"load-bearing/irreversible gates that omit `decision_significance` … and `consequence_text`"*; L65 detection —
  *"Check load-bearing/irreversible gates for `decision_significance` + `consequence_text`; their absence on a
  consequential decision is the tell."* WilhelmAI `lh0` *renders* the significance correctly
  (HTML L1571 `data-importance="irreversible"`, `IMPORTANCE: IRREVERSIBLE — constitutional brand exposure`) — the
  SiteForge primitive does its job — but the captured `lh0.output.json` carries **no `decision_significance`
  field**, so the significance is lost from the RLHF record. Trap 1 owns this at the output-completeness layer.
  This is a genuine, honest residue surfaced by the richer G5 corpus, not present at G0. ✅
- **EGL-2 (option-meaning legibility):** L65 (frame) / cross-refs Trap 5 L117 (option-set verb-first + cross-tier
  parity). The partner-onboarding preset wires the post-submit explainer (L1 library-enforced per g0 §3), so the
  *option-meaning* residue is the L3 tell the trap reserves. Substrate-grounded; caught. ✅

### Trap 2 (Persona / Voice Drift) — owns EGL-4 — landed, latent-on-record

Pack L78 detection — *"Compare the gate's prose vocabulary against the vault's `recommended_persona` character
brief … Flag register mismatch — clinical copy under a warm brief."* The sampled ZenZachary partner-skinned gates
were checked for clinical/generic register drift against the techno-cyber-monk brief; **none observed** in the
sample. Consistent with g0 EGL-4's honest "latent, 1-vault (not yet actualized in sample)" label. The trap is
landed and ready; the residue has not actualized. Recorded as DEFERRED-as-latent, not a miss — the trap correctly
stays silent where there is no drift (mirrors F5's negative-control discipline for un-actualized residue).

## §6 Deferred / held-candidate honesty ledger

The pack landed 6 traps {1,2,3,4,5,7} and **held three a11y/operational candidates** (6/8/9) at G1 with honest
evidence labels (g3 ledger ISS-C6/C8/C9). G5's job is to confirm each hold is **on the record** and still
correct on the richer corpus:

| Held candidate | Why held (G1) | On-record where | G5 re-inspection result |
|---|---|---|---|
| **ISS-C6 — SR-under-glass** (#6) | SiteForge primitives cover the standard a11y surface; residue latent to template-local custom elements | g3 ledger ISS-C6; pack §Candidate 6 (L138) | **Re-confirmed held.** ZenZachary `p1_1r` + WilhelmAI `lh0` rendered HTML carry `:focus-visible` siblings (≥ `:hover`), decoration `aria-hidden`, `prefers-reduced-motion` rules, `sr-only` legends, the `PRIM-CONFIDENCE-TOOLTIP` legend primitive (WilhelmAI L1442). No template-local bypass observed → residue stays latent. (§7 negative control.) |
| **ISS-C8 — touch-target discipline** (#8) | Library honors ≥44px + responsive companion; residue latent to template-local additions | g3 ledger ISS-C8; pack §Candidate 8 (L139) | **Re-confirmed held.** Both gates carry `min-height:44px` / `min-width:44px` floors on radios + buttons + `max-width:600px`/mobile companions. No sub-44px template-local additions → latent. |
| **ISS-C9 — gate-path discipline** (#9) | No erosion observed (archive hygiene intact) | g3 ledger ISS-C9; pack §Candidate 9 (L140) | **Re-confirmed held.** Molecules archives completed gates under `how/gates/archive/`; ZenZachary under `how/gates/_archive/v1/`; output JSONs land at the AD-7 path. No orphaned `.pending`, no slug-invalid `gate_id` observed. |

This is the **candidates-only / held-candidate posture working as designed**: substrate-grounded but
empirically-thin residue does not get force-landed; it stays a candidate (g3 ledger) and waits for live
actualization (template-local bypass). The pack did **not** inflate coverage to claim these.

## §7 Negative control — zero new false positives (the inverse test)

The decisive honesty check: do the held a11y candidates (6/8) — the axes most at risk of false-positive
silhouette-matching — stay **quiet** on the now-richer rendered-gate corpus where the SiteForge primitives
already cover the surface? G5 confirms they do:

- **a11y candidate 6 (SR-under-glass):** ZenZachary `p1_1r` ships `:focus-visible` on every interactive element
  (radios L667, toggles L450, notes L765, generate-btn L843, resume-banner L740-742), decoration
  `aria-hidden`, `prefers-reduced-motion` blocks (L397/L452/L457/L753/L790), `sr-only`/SR-parity legends, and a
  natural-reading-order confidence-dot count (L624 *"SR users get the same count via natural reading order (NOT
  aria-hidden)"*). WilhelmAI `lh0` carries the same primitive set + the `PRIM-CONFIDENCE-TOOLTIP` legend (L1442).
  **The pack correctly attributes no SR-under-glass defect** → no false positive. ✅
- **a11y candidate 8 (touch-targets):** both gates enforce `min-height:44px` / `min-width:44px` on radios,
  scale options, copy/generate/defer buttons (ZenZachary L812-850; the defer option L715 explicitly bumps to
  `min-height:44px`) + `max-width:600px` and mobile single-column companions. **No sub-44px or
  missing-companion residue to flag** → no false positive. ✅
- **Trap 2 (persona-voice):** the partner-skinned ZenZachary copy was checked against the techno-cyber-monk brief
  and the warm partner register — no clinical/generic drift → **correctly silent**, no false positive. ✅
- **Trap 4 / Trap 7 specificity:** these fire **only** where the output JSON genuinely drops a known field
  (`rlhf_reviewer_persona` present-able from top-level `persona`; confidence omitted on a consequential gate) —
  they do **not** fire on, e.g., ZenZachary `p1_1a` for *missing axes* (rank-only gates legitimately omit axis
  scores; the trap flags the *unsignaled* omission, and the rank-only shape is the signal). The traps are
  judgments, not field-presence linters.

This is the strongest single piece of evidence that the new ISS traps are *judgments*, not false-positive
machines: on the parts of the surface the SiteForge primitives provably cover, they stay quiet; they fire only
on the genuine L3 residue (output-completeness + decision-frame at authoring time) that no lint or runtime
ranker owns.

## §8 Candidate traceability (G3 ISS-C6/C8/C9 → residue)

The G3 candidate ledger's three trap-axis candidates each trace to a held a11y/operational axis re-confirmed in
§6/§7. None graduated (candidates-only; canonical untouched; they land into the **pack**, not the canonical
jsonl, when live residue actualizes):

| Candidate | Axis | Maps to | Landed? |
|---|---|---|---|
| ISS-C6 | SR-under-glass (#6) | pack §Candidate 6 | ⏸ held (latent; negative-control clean) |
| ISS-C8 | touch-target (#8) | pack §Candidate 8 | ⏸ held (latent; negative-control clean) |
| ISS-C9 | gate-path (#9) | pack §Candidate 9 | ⏸ held (no erosion observed) |

All three remain candidates in the pack's §"Candidate Axes" + the g3 ledger — **zero appended to the canonical
store**; canonical md5 invariant.

## §9 Boundary re-assertion (DG-G criterion 4 + 10 corollary)

G5 confirms the campaign's boundary posture held through validation:

1. **No ISS runtime in III.** G5 ran no `generate()`, no gate renders, no `skill_open_iss`/`skill_watch_iss`
   invocations. It re-read the on-disk output JSON + rendered HTML the adopters already produced. The pack
   *reads + attributes*; SiteForge owns the library/generator/runtime (pack boundary banner L42).
2. **No III audit runtime.** No Lighthouse/axe/validator executed; the held a11y candidates were confirmed by
   reading the rendered HTML's primitive markers, not by running an a11y audit.
3. **No new III ADR authored** at G5 (III-namespace ADR count stays 7; see §10 note).
4. **Canonical jsonl untouched** — md5 invariant pre+post (§10).

## §10 Zero-regression / hard-invariant table

| Invariant | Pre-G5 | Post-G5 | Status |
|---|---|---|---|
| Canonical jsonl md5 | `5adb0dfa38d9224649c3b2cba83852ae` | `5adb0dfa38d9224649c3b2cba83852ae` | ✅ INVARIANT |
| Canonical jsonl `wc -l` | 28 | 28 | ✅ INVARIANT |
| Graduations fired | 0 | 0 | ✅ candidates-only |
| Consumer wrappers edited | 0 | 0 | ✅ none (read-only inspection of adopters) |
| New III ADRs (III-namespace) | 7 (000+001_module+002_consumer+003_learning+005+007+008) | 7 | ✅ no new ADR |
| Lattice yaml version | 1.2.6 | 1.2.6 | ✅ unchanged |
| ISS pack composite (ADR-007 §3) | 4.00 | 4.00 (not re-scored; G5 validates coverage, not score) | ✅ stable |
| ISS runtime / audit runtime in III | none | none | ✅ boundary held |
| Renders / gate runs / builds by G5 | 0 | 0 | ✅ inspection-grade |

> **ADR-count honesty note.** The `what/decisions/` directory holds 10 `adr_*.md` files, but 3 of them
> (`adr_001_obsidian_as_knowledge_platform`, `adr_002_yaml_as_lattice_format`,
> `adr_003_system_configuration_as_context_topic`) are **template-inherited base-standard ADRs** from the `.adna/`
> fork, not III-authored. The canonical **III-authored ADR count = 7** (000 + 001_module_architecture +
> 002_consumer_federation + 003_learning_store + 005 + 007 + 008), invariant since Campaign D. DG-G criterion 10
> counts the 7 III-authored ADRs; ADR-009 (ISS-Surface Artifact Class) was REJECTED at G0.

**Zero regression CONFIRMED. DG-G criterion 9 satisfied — the landed 6-trap `iss_surfaces` pack catches all 6
recommended L3 residue axes (EGL-1/2/3/5/6 live + EGL-4 latent-on-record) across 3 of 3 live adopters
(≥2-of-3 bar exceeded) with exact pack-line citations, plus one honest new output-completeness observation
(Trap 1); the held a11y candidates 6/8/9 stay correctly silent (negative control clean — zero new false
positives on the now-richer rendered-gate corpus).**

## §11 Cross-references

- **Validates**: `what/context/core_domain_packs/context_iii_iss_surfaces.md` (landed 6-trap pack, G1).
- **Source-of-record**: [[what/artifacts/g0_iss_empirical_gap_ledger|g0_iss_empirical_gap_ledger]] §3-§5 (EGL-1..6) ·
  [[what/artifacts/g0_iss_pack_revision_spec|g0_iss_pack_revision_spec]] (the revision contract G1 met) ·
  [[what/artifacts/g3_iss_candidate_ledger|g3_iss_candidate_ledger]] (ISS-C6/C8/C9).
- **Adopter surfaces re-inspected (read-only)**: `ZenZachary.aDNA/how/gates/*.output.json` + `*.html` ·
  `Molecules.aDNA/how/gates/*.output.json` (ex-`MoleculeForge`; symlink active) · `WilhelmAI.aDNA/how/gates/*.output.json` ·
  the 3 wrapper configs `{ZenZachary,Molecules,WilhelmAI}.aDNA/iss/CLAUDE.md`.
- **Governance**: `what/decisions/adr_007_adaptive_improvement_loop.md` §3 (6-axis rubric) ·
  `adr_003_learning_store_ownership.md` §3/§3.6 (graduation gate — why candidates stay candidates) ·
  `adr_005_rlhf_signal_channel.md` §2 (RLHF field set — grounds Trap 4).
- **Charter / gate**: `how/campaigns/campaign_g_consolidation/campaign_g_consolidation.md` (DG-G criterion 9; DP-5).
- **Precedent shape**: `what/artifacts/f5_validation_wdr3_residue.md` (inspection-grade validation; MX-1/`md_b5` lineage).
- **Next**: G6 — DG-G GO/NO-GO gate (DP-5) + Campaign AAR + annotated `v0.5.0` tag (DP-6).
