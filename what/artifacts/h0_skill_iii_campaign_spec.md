---
type: artifact
artifact_class: build_spec
campaign_id: campaign_h_iii_campaign_pattern
mission_id: h0_charter
title: "H0 build spec — skill_iii_campaign (the anchor)"
created: 2026-06-29
updated: 2026-06-29
last_edited_by: agent_argus
status: active
target_track: T1
tags: [artifact, build_spec, campaign_h, skill_iii_campaign, anchor, construct_review_improve]
---

# H0 Build Spec — `skill_iii_campaign` (T1, the anchor — lands H2)

> The design for the new III-canonical skill that models the **III-campaign pattern** at strategic scope. This is a *spec* — H2 authors + dry-validates the skill. The skill is the reason the pack + persona graduate.

## Target

`III.aDNA/how/skills/skill_iii_campaign.md` — authored against the `skill_iii_review.md` shape + `template_skill.md`.

## Scope-ladder placement (DP-3)

**cycle (tactical) → decadal (operational) → campaign (strategic).** `skill_iii_cycle` (7-step single-artifact) + `skill_decadal_aar` (10-cycle + ranker) live in **aDNA.aDNA** (consumer-side operationalizations). The III-canonical tactical primitive is **`skill_iii_review`** (DISPATCH → INSPECT → INTROSPECT → IMPROVE). **Recommendation (DP-3): `skill_iii_campaign` builds atop `skill_iii_review`** as its per-pass primitive, and *acknowledges* `skill_iii_cycle`/`skill_decadal_aar` as the consumer-side analogs — they are NOT pulled upstream this campaign (out of scope). The campaign skill's Review phase invokes `skill_iii_review` (or its consumer-side cycle analog) per pass.

## Frontmatter (model on `skill_iii_review`)

`type: skill` · `lattice_type: skill` · `skill_type: agent` · `status: active` · `category: review` (or `orchestration`) · `trigger:` ("When applying III at *campaign* scale across two coupled subjects — e.g. an artifact and the source it represents — and/or when an III run must also build the packs/personas/measurement that drive it") · `fair:` (license Apache-2.0) · `federation:` (`discoverable: true`, `source_instance: III.aDNA`, `version_policy: minor`).

## Procedure — the three phases (each modeling steps `skill_iii_cycle` lacks)

### Phase 1 — CONSTRUCT (*build the III*) — the novel step

The whole of Part 1 is unmodeled by the cycle/decadal primitives. Steps:

1. **Subject-bounding** (missing-step #2) — carve the subject slice via the **three-ring owner-class method**: **Ring-A** read-only (e.g. pt19-/upstream-owned data — flag-only, never hand-fix) · **Ring-B** curated (the in-scope editable surface) · **Ring-C** constants + ground-truth (the source the artifact answers to). *Which ring a finding lands in determines fix-side.*
2. **Build/select the instruments** — choose existing core packs + author any net-new pack (the pilot built `representation_coherence`); select/author the personas (the pilot debuted `claim_tracer`); verify the **measurement model covers the subject's object-types** (pilot lesson: `compliance_checker.py` didn't cover the extension-prose slice → re-spec).
3. **Capture machine baselines** — the **3-tier model**: **T1** machine (regression floor; read-only ground-truth diff; *never* mutates the source) · **T2** agent (semantic discovery) · **T3** persona (craft/fidelity). Snapshot T1 baselines + thresholds; the agent/persona baseline *is* the opening Inspect.
4. **Gate DP-Construct** — "the III is good enough to drive the review."

### Phase 2 — REVIEW (*run the III*)

5. **Inspect** — run `skill_iii_review` (per modality) + the selected packs + personas across both subjects. **Paired-subject review** (missing-step #3): two subjects reviewed *together*, where a drift is fixable on either side and *deciding which side* is itself a finding. (Tier-1 is a regression floor — budget T2/T3 for discovery; gates alone are measurement theatre.)
6. **Introspect + Plan/Disposition** (missing-step #4 — the cycle jumps findings→fixes; this is its own phase):
   - **Calibrate vs ratified ADRs** (missing-step #5) — reconcile each finding against ratified design choices (ADRs, ethos dials) before ranking; else the III over-reports deliberate design as drift.
   - **Rank** by **(severity + fidelity-impact) ÷ effort**; **bound** the lean fix-set; **flag-not-fix** for out-of-slice drift (bound the subject — don't expand scope).
   - **Decide fix-side** per finding: `website/artifact-owned` (fix the artifact) · `source/ground-truth-owned` (fix the *source* the artifact faithfully mirrors) · `escalate` (Ring-A read-only → route to the owner). *Recording which side is a finding.*
7. **Gate DP-Plan** — "the ranked, fix-sided plan is approved."

### Phase 3 — IMPROVE (*act + extract*)

8. **Act** — apply the validated fixes; **re-measure**.
9. **Instrument-repair** (missing-step #6) — the Improve phase **repairs the instrument that missed a finding**: ship the fix *with* the gate that should have caught it (the pilot's C-027 closed loop; re-measure grew 302→304). Improve adds coverage, not just content.
10. **Extract** — produce the **pattern packet** (reusable scaffolding + doctrines + measurement lessons + primitive-gaps + dispositions) + the **terminal handoff** to the framework owner.
11. **Gate phase-exit + terminal handoff.**

## Doctrines to weave in (T4 — graduate as reusable)

A1/A2 fidelity↔currency split · three-way owner-class split · fix-side decision procedure · flag-not-fix · calibration-vs-ratified-ADR · the 3-tier measurement model. Each appears at the step above where it operates; cross-link the `representation_coherence` pack + `claim_tracer` persona as the worked instances.

## Sections (skill shape)

Overview · When to Use (campaign-scale; paired-subject; build-the-III) · Prerequisites (a subject + its source; a framework vault to hand off to) · Procedure (the 3 phases above) · Outputs (subject-boundary · baselines · findings register · ranked fix-sided plan · re-measure delta · **pattern packet** + handoff) · Depth (a single Review pass = an III *cycle*; the full Construct→Review→Improve = an III *campaign*) · Integration Points (`skill_iii_review` as the per-pass primitive; the consumer-side `skill_iii_cycle`/`skill_decadal_aar` analogs; the `iii/` wrapper + `federation_ref` for consumer adoption) · Related · Provenance (Operation Looking Glass pilot, 2026-06-28).

## Dry-validation (H2, DG-H crit 2)

Apply the skill **retro to the pilot's M0–M5 record** (read `aDNA.aDNA/how/campaigns/campaign_looking_glass/`): confirm each phase/step maps onto a real pilot mission (M1=Construct, M2 Inspect, M3 Introspect+Plan, M4 Improve+re-measure, M5 Extract). A step that *doesn't* map to a pilot mission is either over-engineered (cut it) or a genuine generalization (justify it). The skill must reproduce the pilot's phase structure.

## Federation note (H5)

A consumer adopts the pattern via an `iii/` wrapper declaring `federation_ref: { source_vault: III.aDNA, source_skill: how/skills/skill_iii_campaign.md, version, version_policy: minor, packs_used: [..., representation_coherence] }` — per the standard intake (`adr_002_consumer_federation_contract.md`).
