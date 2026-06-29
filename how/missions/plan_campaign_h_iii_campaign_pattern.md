---
plan_id: plan_campaign_h_iii_campaign_pattern
type: plan
title: "Campaign H — Graduate the III-Campaign Pattern (Operation Touchstone) — campaign charter authoring (planning mission)"
owner: stanley
status: completed   # EXECUTED 2026-06-29 — charter draft + 3 H0 graduation specs + ack memo authored; charter awaits DP-1 operator gate
mission_class: campaign_planning_research
campaign: null   # standalone — designs a not-yet-opened campaign (no III campaign is currently open; Campaign G closed 2026-06-23)
designs_campaign: campaign_h_iii_campaign_pattern   # proposed slug; proposed letter Campaign H (G "Operation Atrium" closed; E reserved for generalized writing-III gated on LiteratureForge forge-BUILD)
codename: "Operation Touchstone"                     # confirmed by operator 2026-06-29 (Q1); sibling-callback to G "Operation Atrium" / F "Operation Tell" / pilot "Looking Glass" — the assay-stone that tells true from false
disposition: standalone_planning
created: 2026-06-29
updated: 2026-06-29
last_edited_by: agent_argus
authored_at_session: session_stanley_20260629_iii_adna_campaign_h_touchstone_planning
executed_at_session: session_stanley_20260629_iii_adna_campaign_h_touchstone_planning   # authored + executed same session (operator scope: plan + draft charter, stop at DP-1)
deliverables_landed: 5   # ack memo + planning mission (this card) + draft charter + 3 H0 specs (one combined count below)
designs_campaign_charter_at: how/campaigns/campaign_h_iii_campaign_pattern/campaign_h_iii_campaign_pattern.md   # created at execution per Campaign G precedent
predecessor_pilot: campaign_looking_glass   # aDNA.aDNA (Rosetta) — the pilot that proved + extracted the pattern
boundary_posture: graduation_inbound   # source artifacts stay campaign-local in aDNA.aDNA (status draft); III.aDNA receives canonical-adapted copies. No aDNA.aDNA writes (Rule 10).
evidence_method: inspection_grade   # recon = read the handoff memo + pattern_packet + source pack/persona + the Campaign-G charter/planning precedents; no live runs, no canonical landing
sequencing: "standalone; executes phase-by-phase on the operator gate (DP-1). NOT Campaign E (generalized writing-III stays gated on LiteratureForge.aDNA forge-BUILD). Anchor priority within Campaign H = the reusable skill_iii_campaign (H2) — the pack + persona graduate to support it."
forward_seeds: "the conditional pack reaches the ≥3-cycle bar via H3's cross-artifact test cycles; ISS-style adopter consumption of the new pattern (an iii/ wrapper declaring skill_iii_campaign + packs_used: [representation_coherence]); the primitive-gap fixes (§5 of the packet) become III-core improvements at H4"
tags: [plan, campaign_planning, campaign_h, operation_touchstone, iii_campaign_pattern, graduation, skill_iii_campaign, representation_coherence, claim_tracer, v0_6_0_target, boundary_graduation_inbound, inspection_grade, standalone_planning, completed]
---

# Campaign H — Graduate the III-Campaign Pattern ("Operation Touchstone") — Campaign-Planning Mission

> **Status note**: This planning-mission card was authored **and executed** in one session (2026-06-29), per the operator's locked scope (*plan + draft charter, stop at DP-1*). It produced the **DRAFT charter** for Campaign H plus three H0 graduation specs. It does **not** open or charter the campaign — opening is the **DP-1 operator gate** (Standing Rule 7). When the operator ratifies DP-1, this card becomes Campaign H's P0 charter-authoring record. The substantive graduation (landing canonical artifacts, building the skill) is gated work in phases H1+.

## §1 Context & Why

Operation Looking Glass — the **pilot of the "III campaign" pattern** — closed in aDNA.aDNA (Rosetta) on 2026-06-28 (M0–M5, ~2 days), with a currency-sweep follow-on wound down 2026-06-29. Its charter's terminal charge was to hand the pattern to III.aDNA to graduate. That handoff arrived as [[../../../aDNA.aDNA/who/coordination/coord_2026_06_28_rosetta_to_argus_iii_campaign_handoff|the Rosetta→Argus memo]] (acked this session: [[../../who/coordination/coord_2026_06_29_argus_ack_iii_campaign_handoff|Argus ack]]).

What the pilot proved (from [[../../../aDNA.aDNA/how/campaigns/campaign_looking_glass/artifacts/pattern_packet|the pattern packet]]):

- **The "build the III" (Construct) phase is real and unmodeled.** An III *campaign* = **(build the III) → (run the III) → (improve from it)**; the scope ladder is **cycle (tactical) → decadal (operational) → campaign (strategic)**. III's existing primitives — the consumer-side `skill_iii_cycle` / `skill_decadal_aar` (which live in aDNA.aDNA) and III.aDNA's own `skill_iii_review` — model the *run*, not the *build*.
- **Tier-1 gates caught 0 of 25 findings** — they are a regression floor, not a discovery instrument. All 25 findings came from the agent (T2) and persona (T3) tiers.
- **Paired-subject review is load-bearing** — the marquee result (*the mirror was more current than its source*) is invisible to a single-subject cycle.
- **Improve grows the instrument** — re-measure went 302 → 304 (a fix shipped with the gate that had missed it).

III.aDNA is the framework's canonical home and the right vault to graduate this into reusable, network-wide form: a `skill_iii_campaign` skill, the `representation_coherence` pack, the `claim_tracer` persona, the doctrines, and — fed back into III's own function/context/process — the primitive-gaps the pilot exposed.

**Boundary posture (graduation-inbound).** The source artifacts stay campaign-local in aDNA.aDNA (`status: draft`); III.aDNA receives canonical-adapted copies. No writes into aDNA.aDNA (Rule 10) — the pilot's records are read-only ground truth. Consumption-only ADR discipline holds: Campaign H targets **0 new III ADRs** (the pattern is a *skill* + *pack* + *persona*, not a structural contract change), with one explicit candidate (ADR-010 "III-Campaign Scope Tier" / a `who/reviewers/` artifact class) evaluated and dispositioned at H0/DP-1 — see §9.

## §2 The Campaign Being Planned — Vision (what this mission charters)

A **graduation campaign** that promotes the proven III-campaign pattern from a campaign-local pilot artifact to canonical III.aDNA capability, on a clean **`v0.5.0` → `v0.6.0`** minor bump. Five things land, in dependency order:

1. **`skill_iii_campaign` (ANCHOR)** — a new III.aDNA-canonical skill modeling the three-phase pattern (Construct → Review[Inspect · Introspect/Plan] → Improve[Act · Extract]) and the steps the tactical cycle lacks: subject-bounding, paired-subject review, the Plan/Disposition phase, calibration-vs-ratified-ADR, and instrument-repair-in-Improve.
2. **`representation_coherence` pack** — graduated **canonical-conditional** (Q2): 5 proven traps carry first-cycle evidence, 6 silent traps held as honest candidates pending cross-artifact catch-evidence; the "does justice" rubric + cross-trap escalation graduate intact.
3. **`claim_tracer` persona** — graduated **candidate**; the fidelity complement to the currency-owning Standard Archivist. Its landing forces a structural decision (§9 DP): III has no `who/reviewers/` bench and its native model is the reviewer-orchestra voice registry.
4. **The doctrines** — A1/A2 fidelity↔currency split · three-way owner-class split · fix-side decision procedure · flag-not-fix · calibration-vs-ADR · the 3-tier measurement model — graduate as reusable doctrine, woven into the skill and pack.
5. **III-core improvements** — fold the packet's §5 primitive-gaps + §4 measurement-model lessons back into III's function/context/process (e.g., the "regression-floor vs discovery-instrument" framing into `inspect_procedures`; the calibration-vs-ADR step adjacency to `introspect_checks`).

The thesis: after Campaign H, any vault can run an *III campaign* — not just an III cycle — by federating III.aDNA's `skill_iii_campaign` + `representation_coherence` pack via an `iii/` wrapper, and the pattern that made the aDNA website faithful to its vault becomes reusable for any "is this artifact faithful to its source?" review (README · deck · whitepaper · generated summary).

## §3 Objectives (what the executing campaign produces, by phase)

- **O1 (H1) — Graduate the scaffolding.** Land `context_iii_representation_coherence.md` canonical-conditional (rename + `quality_metric` block + re-homed wikilinks + honest trap fire-table); land `claim_tracer` as a candidate reviewer at the location DP-1 resolves; canonical jsonl INVARIANT.
- **O2 (H1) — Resolve the reviewer-bench question.** Decide whether III establishes a `who/reviewers/` artifact class (mirroring aDNA.aDNA's Rosetta extension) or maps `claim_tracer` onto the reviewer-orchestra voice model — and how the pack/persona's cross-vault references to aDNA.aDNA reviewers (Standard Archivist, Content Strategist) resolve.
- **O3 (H2) — Spec + build `skill_iii_campaign`.** The anchor: author the skill against `template_skill.md` / `skill_iii_review.md` shape, modeling the Construct phase + the five missing steps; resolve the scope-ladder coherence question (the skill is III-canonical, but `skill_iii_cycle`/`skill_decadal_aar` are aDNA.aDNA-local — build atop `skill_iii_review` as the canonical tactical primitive, acknowledge the consumer-side analogs).
- **O4 (H3) — Test it (cross-artifact cycle).** Apply the pattern (or its Review phase) to a *second* artifact type to supply the pack cycles 2–3 toward the ≥3-cycle / ≥80% bar, and to validate the skill end-to-end. Candidate subjects: a README, a deck, or a vault↔its-own-docs pairing. Lift confirmed-firing candidate traps from candidate → canonical.
- **O5 (H4) — Improve III core + bump + sweep.** Fold §5 primitive-gaps + §4 measurement lessons into III's function/context/process; declare **`v0.6.0`** (MANIFEST + Release-History); run the ADR-002 §3 wrapper sweep (consumers reviewing on minor bump re-pin `federation_ref`).
- **O6 (H5) — Closeout.** DG-H gate + AAR + annotated `v0.6.0` tag + cascade; author the federation note (how a consumer adopts `skill_iii_campaign` + `representation_coherence` via an `iii/` wrapper).

## §4 Method / Phases (for the executing campaign — drafted in the charter)

| Phase | Mission | Produces | Human gate |
|-------|---------|----------|------------|
| **H0 — Charter & ack** | this planning-mission execution | ack memo + planning mission + DRAFT charter + 3 H0 specs | **DP-1** (open Campaign H) |
| **H1 — Graduate scaffolding** | land pack (conditional) + persona (candidate) | `context_iii_representation_coherence.md` + the persona at its resolved home | **DP-2** (reviewer-bench decision) |
| **H2 — Build the skill (ANCHOR)** | author + dry-validate `skill_iii_campaign` | `how/skills/skill_iii_campaign.md` | DP-3 (skill shape ratified) |
| **H3 — Test (cross-artifact)** | run the pattern on a 2nd artifact type | validation record; candidate→canonical trap lifts | — |
| **H4 — Improve III + bump + sweep** | fold gaps into core; `v0.6.0`; wrapper sweep | core-pack edits + MANIFEST + re-pins | DP-4 (bump + sweep scope) |
| **H5 — Closeout** | DG-H + AAR + tag + federation note | scorecard + tag + `iii/` adoption note | DP-5 (DG-H GO) |

Recon for this planning mission was inspection-grade: read the handoff memo + pattern packet + source `pack_representation_coherence` + source `reviewer_claim_tracer` + the Campaign-G planning-mission/charter precedents + `skill_iii_review`. No live runs, no canonical landing.

## §5 Deliverables (this planning-mission execution — H0)

1. **Argus ack memo** — [[../../who/coordination/coord_2026_06_29_argus_ack_iii_campaign_handoff|coord_2026_06_29_argus_ack_iii_campaign_handoff]] (receipt + Q1/Q2 answers).
2. **This planning-mission card** — `how/missions/plan_campaign_h_iii_campaign_pattern.md`.
3. **DRAFT charter** — [[../campaigns/campaign_h_iii_campaign_pattern/campaign_h_iii_campaign_pattern|campaign_h_iii_campaign_pattern.md]] (`status: draft`; opens at DP-1).
4. **Graduation spec — pack** — `what/artifacts/h0_graduate_representation_coherence_spec.md`.
5. **Graduation spec — persona** — `what/artifacts/h0_graduate_claim_tracer_spec.md`.
6. **Build spec — skill** — `what/artifacts/h0_skill_iii_campaign_spec.md`.

## §6 Out of Scope (for this planning mission)

- **No canonical landing** — the pack/persona/skill are *specs* (H0); the actual canonical files land in the execution campaign after DP-1 (H1/H2).
- **No `who/reviewers/` created** — its existence is a DP-1/DP-2 decision; this card only *surfaces* it.
- **No learning-store mutation** — canonical jsonl md5 `5adb0dfa…`/28 INVARIANT; zero graduation fires.
- **No `v0.6.0` bump / no wrapper sweep / no annotated tag** — H4/H5 work.
- **No new III ADR** — the architecture note is a proposal; ADR-010 candidate is pre-dispositioned (§9) and ratified at DP-1.
- **No writes into aDNA.aDNA** (Rule 10) — the pilot's artifacts are read-only ground truth; the source memo's ack checkboxes are Rosetta's to reconcile.
- **Does NOT open or charter the campaign** — produces the charter *draft* for the DP-1 operator gate.
- **NOT Campaign E** — generalized writing-III stays gated on LiteratureForge forge-BUILD; Campaign H does not subsume it.

## §7 Reuse Map (read these; cited in the charter)

- **The graduation material** (aDNA.aDNA, read-only): `how/campaigns/campaign_looking_glass/artifacts/pattern_packet.md` (§1 pattern · §3 doctrines · §4 measurement lessons · §5 primitive gaps · §7 dispositions); `.../artifacts/packs/pack_representation_coherence.md` (11 traps / 4 dims / rubric); `.../artifacts/personas/reviewer_claim_tracer.md`; `.../artifacts/instrumentation_log.md` (the raw extraction record); `.../missions/artifacts/campaign_looking_glass_aar.md`.
- **III structural precedents** (this lane): `how/missions/plan_campaign_g_consolidation_charter.md` (PRIMARY card template — this card mirrors its §-shape); `how/campaigns/campaign_g_consolidation/campaign_g_consolidation.md` (charter template — phases/DPs/DG/risk/timeline); `what/artifacts/g6_dg_g_scorecard.md` (DG scorecard shape); `what/context/core_domain_packs/context_iii_iss_surfaces.md` (the freshest pack graduated *via a campaign* — the conditional-with-honest-candidates precedent for the pack).
- **III skill + pack + contract shape**: `how/skills/skill_iii_review.md` (canonical skill template + `federation:` block); `what/context/core_domain_packs/context_iii_inspect_procedures.md` (pack `quality_metric` block + trap-table shape); `what/decisions/adr_002_consumer_federation_contract.md` (minor-bump wrapper sweep); `what/decisions/adr_007_adaptive_improvement_loop.md` §3 (six-axis rubric for scoring the graduated pack).
- **Cross-vault primitives the skill builds on/references**: aDNA.aDNA `how/skills/skill_iii_cycle.md` + `skill_decadal_aar.md` (the consumer-side tactical/operational analogs — the scope-ladder coherence question, §9).
- **Templates**: `how/templates/template_skill.md`, `template_campaign.md`, `template_campaign_mission.md`, `template_coordination.md`, `template_aar_lightweight.md`.

## §8 Exit Gate

This planning mission (this session) closes when: the ack memo + this card + the DRAFT charter + the 3 H0 specs are authored and committed with valid frontmatter, all §7 paths verified, and the hard invariants hold (canonical jsonl md5 `5adb0dfa…`/28 INVARIANT; production pin `v0.5.0`; no `core_domain_packs/` add; no `who/reviewers/` created). At that point the operator decides DP-1 (open Campaign H). ✅ **MET** this session.

## §9 Decisions & Open Questions (the DPs the charter carries)

1. **Codename / slug** — **RESOLVED** (Q1): Campaign H, slug `campaign_h_iii_campaign_pattern`, codename **Operation Touchstone** (operator-confirmed 2026-06-29).
2. **Pack graduation timing** — **RESOLVED** (Q2): graduate `representation_coherence` **canonical-conditional** now (5/11 proven traps with evidence; 6 honest candidates), per the Campaign-G ISS-pack precedent. Ratified at DP-1.
3. **Reviewer-bench artifact class** (DP-2, the structural call) — does III establish a `who/reviewers/` bench (mirroring aDNA.aDNA's Rosetta extension) for `claim_tracer`, or map it onto the reviewer-orchestra voice registry (`skill_iii_review` §Reviewer Orchestra)? **Recommendation: establish a minimal `who/reviewers/`** — the campaign-scale pattern uses a persona bench (the pilot's 15-lens decadal review), and a standalone reviewer .md is more legible than a voice-YAML row for a graduated, cross-vault-referenced persona. Operator decides at DP-1/DP-2. *(This is the lone ADR-010-adjacent candidate; pre-dispositioned: a new artifact-class folder needs no structural ADR — same precedent as `web_design` packing an external class with no ADR. Final III ADR count stays 7.)*
4. **Scope-ladder coherence** (DP-3) — `skill_iii_campaign` is III-canonical, but `skill_iii_cycle`/`skill_decadal_aar` are aDNA.aDNA-local. **Recommendation: build `skill_iii_campaign` atop `skill_iii_review`** (the III-canonical tactical primitive) and acknowledge the aDNA.aDNA cycle/decadal skills as consumer-side analogs — do **not** pull them upstream this campaign (out of scope; revisit if a second consumer needs them).
5. **Cross-artifact test subject** (H3) — which 2nd artifact type supplies cycles 2–3? Recommendation: a README or a deck with vault-factual claims (the pack's stated generalization targets); operator/Argus pick at H3.
6. **Version target** — `v0.6.0` clean minor bump (new canonical skill + pack + persona = minor, backward-compatible). Confirm at DP-1.

## §10 AAR (lightweight — this planning mission)

- **Worked**: the pilot's pattern_packet was a near-turnkey graduation brief — §5 primitive-gaps mapped 1:1 onto the skill's missing steps, and §7 dispositions pre-sorted graduate-vs-template-vs-bespoke; the Campaign-G planning-mission/charter pair gave an exact house-style template, so authoring was mostly adaptation.
- **Didn't / surprised**: two structural gaps the packet didn't flag surfaced at recon — III has **no `who/reviewers/`** bench (the persona's `graduation_target` assumes one), and the **scope-ladder is split across vaults** (cycle/decadal in aDNA.aDNA, campaign skill landing in III.aDNA). Both became charter DPs rather than silent assumptions.
- **Finding**: a graduation campaign is structurally *unlike* a content campaign (F/G) — its "subject" is a pattern, its "traps" are the primitive-gaps, and its Construct phase already happened (in the pilot). The charter inverts the usual recon→build order: recon is *reading the pilot's instrumentation*, not surveying a fresh domain.
- **Change (next time)**: when graduating an artifact *across* vaults, verify the target vault's artifact-class inventory at recon (the `who/reviewers/` gap would have been a silent H1 blocker otherwise).
- **Follow-up**: DP-1 ratification opens Campaign H; H1 resolves the reviewer-bench decision; the source memo's ack checkboxes await Rosetta-side reconciliation (1-line, in aDNA.aDNA).

## Notes

Authored as a standalone planning mission, structurally modeled on `plan_campaign_g_consolidation_charter.md`. Three operator decisions are locked: **graduation-inbound boundary** (source stays in aDNA.aDNA; III gets canonical-adapted copies; no cross-vault writes), **plan + draft charter, stop at DP-1** (execution is gated per Standing Rule 7), **inspection-grade** (read the pilot's records; no live runs). The card is `status: completed` (authored + executed same session); executing the *campaign* it designs begins at the DP-1 operator gate.
