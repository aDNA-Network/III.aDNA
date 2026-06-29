---
campaign_id: campaign_h_iii_campaign_pattern
type: campaign
title: "Campaign H — Graduate the III-Campaign Pattern (Operation Touchstone)"
codename: "Operation Touchstone"
owner: stanley
status: draft        # DRAFT — opens to `active` only at the DP-1 operator gate (Standing Rule 7)
phase: H0
mission_count: 6     # H0..H5
estimated_sessions: 6
estimation_class: calibrated_against_campaign_g
priority: high
created: 2026-06-29
updated: 2026-06-29
last_edited_by: agent_argus
authored_by_mission: plan_campaign_h_iii_campaign_pattern
predecessor: campaign_g_consolidation          # Campaign G (Operation Atrium); v0.5.0
predecessor_pilot: campaign_looking_glass       # aDNA.aDNA (Rosetta) — proved + extracted the pattern
boundary_posture: graduation_inbound            # source artifacts stay campaign-local in aDNA.aDNA (draft); III.aDNA receives canonical-adapted copies; no aDNA.aDNA writes (Rule 10)
graduation_posture: canonical_conditional       # representation_coherence pack graduates canonical-conditional (5/11 proven + 6 honest candidates); NOT block-closed on reaching ≥3 cycles
version_target: "0.6.0"                          # clean minor bump (new canonical skill + pack + persona; backward-compatible)
adr_posture: zero_new_iii_adr                    # ADR-010 (III-campaign scope tier / who/reviewers artifact class) pre-dispositioned no-structural-ADR; final III ADR count stays 7
learning_store_posture: invariant               # canonical jsonl md5 5adb0dfa…/28 INVARIANT — the pack/persona graduate as pack/persona, NOT as learning-store corrections; zero graduation fires
tags: [campaign, campaign_h, operation_touchstone, iii_campaign_pattern, graduation, skill_iii_campaign, representation_coherence, claim_tracer, v0_6_0, boundary_graduation_inbound, zero_new_iii_adr, draft]
---

# Campaign H — Graduate the III-Campaign Pattern ("Operation Touchstone")

> **DRAFT charter** authored at `plan_campaign_h_iii_campaign_pattern` execution (2026-06-29). Opens to `status: active` only at the **DP-1 operator gate**. Models on the Campaign G charter (`campaign_g_consolidation.md`), which extended the Campaign F/D skeleton.

## §1 Goal

Promote the proven **III-campaign pattern** — the strategic, instrumented application of III across two coupled subjects, piloted by Operation Looking Glass — from a campaign-local aDNA.aDNA artifact to **canonical III.aDNA capability**, on a clean **`v0.5.0` → `v0.6.0`** minor bump. After Campaign H, any vault can run an *III campaign* (not just an III cycle) by federating `skill_iii_campaign` + the `representation_coherence` pack via an `iii/` wrapper. Thesis: the pattern that made the aDNA website faithful to its vault becomes a reusable instrument for any "is this artifact faithful to its source?" review — README, deck, whitepaper, generated summary.

## §2 Context

Campaign G (Operation Atrium) closed at DG-G 11/11 (`v0.5.0`, 2026-06-23). No III campaign is open. The graduation material arrived as the [[../../../who/coordination/coord_2026_06_29_argus_ack_iii_campaign_handoff|Rosetta→Argus handoff]] (acked 2026-06-29) from the closed Looking Glass pilot. The consolidated brief is [[../../../../aDNA.aDNA/how/campaigns/campaign_looking_glass/artifacts/pattern_packet|the pattern packet]].

**What the pilot proved** (the graduation evidence base):
- The **"build the III" (Construct) phase** is real and unmodeled by III's tactical/operational primitives. Scope ladder: **cycle → decadal → campaign**.
- **Tier-1 gates caught 0 of 25 findings** — a regression floor, not a discovery instrument; all findings came from the agent (T2) + persona (T3) tiers.
- **Paired-subject review is load-bearing** — the marquee result (*the mirror more current than its source*) is invisible to a single-subject cycle.
- **Improve grows the instrument** — re-measure 302 → 304 (a fix shipped with the gate that missed it).

**Two structural facts surfaced at H0 recon** (verified against the filesystem; surfaced not silently carried):
1. **III has no `who/reviewers/` bench.** The source persona's `graduation_target` assumes one; III's native model is the reviewer-orchestra voice registry (`skill_iii_review` §Reviewer Orchestra). The reviewer-bench artifact-class decision is **DP-2**.
2. **The scope ladder is split across vaults.** `skill_iii_cycle` + `skill_decadal_aar` live in aDNA.aDNA (consumer-side); the new `skill_iii_campaign` lands in III.aDNA (canonical). The skill builds atop `skill_iii_review` (the III-canonical tactical primitive) — **DP-3**.

## §3 Boundary posture (operator-locked)

**Graduation-inbound.** The pilot's artifacts stay campaign-local in aDNA.aDNA (`status: draft`); III.aDNA receives **canonical-adapted copies** (rename to convention, add `quality_metric`, re-home wikilinks). **No writes into aDNA.aDNA** (Rule 10) — the pilot's records are read-only ground truth; the source memo's ack checkboxes are Rosetta's to reconcile. **Consumption-only ADR discipline**: Campaign H targets **0 new III ADRs**; the one candidate (ADR-010 — III-campaign scope tier / `who/reviewers/` artifact class) is **pre-dispositioned at H0**: a new skill + pack + persona + artifact-class folder needs no structural ADR (same precedent as `web_design` packing an external artifact class with no III-side ADR). Operator may override at DP-1. **Learning store INVARIANT**: the pack/persona graduate *as* pack/persona — **not** as learning-store corrections — so canonical jsonl md5 `5adb0dfa…`/28 stays INVARIANT; zero graduation fires.

## §4 Scope

### IN (5 graduation tracks)
| Track | Scope | Lands |
|---|---|---|
| **T1 — `skill_iii_campaign` (ANCHOR)** | New III-canonical skill modeling Construct → Review[Inspect · Introspect/Plan] → Improve[Act · Extract] + the 5 missing steps (subject-bounding · paired-subject · Plan/Disposition · calibration-vs-ADR · instrument-repair) | H2 |
| **T2 — `representation_coherence` pack** | Graduate **canonical-conditional**: rename → `context_iii_representation_coherence.md`; add `quality_metric` (target composite ≥4.0); 5 proven traps carry evidence, 6 silent traps held candidate; rubric + escalation intact | H1 |
| **T3 — `claim_tracer` persona** | Graduate **candidate** at the DP-2-resolved home; the fidelity complement to the currency-owning Standard Archivist; resolve cross-vault references | H1 |
| **T4 — the doctrines** | A1/A2 split · three-way owner-class · fix-side procedure · flag-not-fix · calibration-vs-ADR · 3-tier measurement — woven into the skill + pack as reusable doctrine | H2 |
| **T5 — III-core improvements** | Fold §5 primitive-gaps + §4 measurement lessons into III's function/context/process (`inspect_procedures` "regression-floor vs discovery-instrument"; `introspect_checks` calibration-vs-ADR adjacency) | H4 |

### OUT
- **Campaign E** (generalized writing-III) — hard-gated on LiteratureForge forge-BUILD; NOT subsumed.
- **Pulling `skill_iii_cycle` / `skill_decadal_aar` upstream** — they stay aDNA.aDNA-local; the skill references them as consumer-side analogs (revisit only if a 2nd consumer needs them).
- **Graduating the Looking Glass gates as runtime** — the claim-trace/currency gates graduate **as-template** (shape, not selectors); III builds no gate runtime (boundary).
- **The `site_backing_slice`** — bespoke to the pilot; only its three-ring subject-bounding *method* graduates (into the skill).
- **Re-homing the Standard Archivist / Content Strategist** — those are aDNA.aDNA's Rosetta reviewer bench; out of scope (cross-vault references kept explicit).

### Subsumes (the handoff's asks)
graduate `skill_iii_campaign` (→ T1); graduate the pack (→ T2); graduate the persona (→ T3); the doctrines (→ T4); review/improve III from pilot learnings (→ T5).

## §5 Phases & Missions

### Phase H0 — Charter & Ack
| Mission | Title | Sessions | Deps | Status |
|---|---|---|---|---|
| H0 | Ack handoff + author planning mission + DRAFT charter + 3 H0 graduation specs | 1 | — | ✅ authored 2026-06-29 (this planning-mission execution); ratify at DP-1 |

**Exit gate:** ack memo + planning mission + DRAFT charter + 3 `h0_*` specs authored; hard invariants hold; DP-1 (open) cleared with operator. ✅ **MET** (authoring); DP-1 pending.

### Phase H1 — Graduate the Scaffolding
| Mission | Title | Sessions | Deps | Status |
|---|---|---|---|---|
| H1 | Land `context_iii_representation_coherence.md` (canonical-conditional) + `claim_tracer` (candidate) at the DP-2 home; re-home wikilinks; `quality_metric` score | 1–2 | H0/DP-1 | ⬜ planned |

**Exit gate:** pack landed (11 traps; 5 with first-cycle evidence + 6 honest candidates; `quality_metric` composite ≥4.0; rubric + escalation intact); persona landed at its resolved home; cross-vault references resolved; **DP-2 reviewer-bench decision ratified**; canonical jsonl INVARIANT; pack count 8→9.

### Phase H2 — Build the Skill (ANCHOR)
| Mission | Title | Sessions | Deps | Status |
|---|---|---|---|---|
| H2 | Author + dry-validate `skill_iii_campaign` (Construct→Review→Improve + 5 missing steps + 6 doctrines woven in) | 1–2 | H1 | ⬜ planned |

**Exit gate:** `how/skills/skill_iii_campaign.md` authored against `skill_iii_review` shape + `federation:` block; the Construct phase + the 5 missing steps modeled; the 6 doctrines (T4) woven in; **DP-3 scope-ladder coherence ratified** (built atop `skill_iii_review`); dry-validation against the pilot's M0–M5 record (the skill, applied retro to Looking Glass, reproduces its phase structure).

### Phase H3 — Test (cross-artifact cycle)
| Mission | Title | Sessions | Deps | Status |
|---|---|---|---|---|
| H3 | Apply the pattern's Review phase to a 2nd artifact type; lift confirmed candidate traps | 1 | H2 | ⬜ planned |

**Exit gate:** a 2nd-artifact validation record (README / deck / vault↔docs pairing) supplying pack cycle 2 (toward the ≥3-cycle bar); each firing candidate trap lifted candidate→canonical with evidence; negative control clean (no new false positives); the skill runs end-to-end on a non-pilot subject.

### Phase H4 — Improve III Core + Bump + Sweep
| Mission | Title | Sessions | Deps | Status |
|---|---|---|---|---|
| H4 | Fold §5 gaps + §4 lessons into core packs; `v0.5.0→v0.6.0`; ADR-002 §3 wrapper sweep | 1 | H3 | ⬜ planned |

**Exit gate:** III-core edits landed (the primitive-gaps/measurement-lessons woven into `inspect_procedures` / `introspect_checks` as appropriate, content-only); `v0.6.0` declared (MANIFEST Version + Release-History row); ADR-002 §3 review fired at minor-policy consumers; carriers adopting `representation_coherence` re-pin (or defer per own cadence); canonical jsonl INVARIANT; zero new III ADR.

### Phase H5 — Closeout
| Mission | Title | Sessions | Deps | Status |
|---|---|---|---|---|
| H5 | DG-H gate + AAR + annotated `v0.6.0` tag + cascade + federation note | 1 | H4 | ⬜ planned |

**Exit gate:** DG-H all-pass; `h5_dg_h_scorecard.md` (10-section, `g6` shape); AAR inline; federation note (how a consumer adopts `skill_iii_campaign` + `representation_coherence` via an `iii/` wrapper + `federation_ref`); cascade (STATE.md + MANIFEST + workspace router production-pin); annotated `v0.6.0` tag at close commit; `status: completed`.

## §6 Decision Points

| # | When | Decision | Status |
|---|---|---|---|
| DP-1 | H0 close | **Open Campaign H** (charter draft → active) + confirm Q1 codename + Q2 conditional-graduation + ADR-010 no-structural-ADR disposition + `v0.6.0` target | ⬜ **PENDING** — operator gate |
| DP-2 | H1 | **Reviewer-bench artifact class** — establish `who/reviewers/` in III (recommended) vs map `claim_tracer` onto the reviewer-orchestra voice registry | ⬜ pending |
| DP-3 | H2 | **Scope-ladder coherence** — build `skill_iii_campaign` atop `skill_iii_review` (recommended); cycle/decadal stay aDNA.aDNA-local | ⬜ pending |
| DP-4 | H4 | **Bump + sweep scope** — `v0.6.0`; which carriers re-pin vs defer | ⬜ pending |
| DP-5 | H5 | **DG-H GO/NO-GO** + cut annotated `v0.6.0` tag | ⬜ pending |

## §7 Decision Gate DG-H (11 criteria — all must pass)

1. `skill_iii_campaign` authored (Construct → Review[Inspect·Introspect/Plan] → Improve[Act·Extract]) modeling the **5 steps** `skill_iii_cycle` lacks (subject-bounding · paired-subject · Plan/Disposition · calibration-vs-ADR · instrument-repair). *(H2)*
2. `skill_iii_campaign` dry-validated against the pilot's M0–M5 record (reproduces the phase structure retro). *(H2)*
3. `context_iii_representation_coherence.md` landed canonical-conditional (11 traps; rename + re-homed wikilinks; rubric + escalation intact). *(H1)*
4. Pack `quality_metric` scored against ADR-007 §3; composite **≥4.0**; honest trap fire-table (5 proven / 6 candidate). *(H1)*
5. `claim_tracer` landed at the DP-2-resolved home; the A1/A2 fidelity↔currency split documented; cross-vault references resolved. *(H1)*
6. The **6 doctrines** (A1/A2 · three-way owner-class · fix-side · flag-not-fix · calibration-vs-ADR · 3-tier measurement) captured in the skill + pack. *(H2)*
7. Cross-artifact test (H3): pack cycle 2 supplied on a non-pilot artifact; confirmed candidate traps lifted; negative control clean. *(H3)*
8. III-core improvements folded (§5 gaps + §4 lessons) into the relevant core packs, content-only. *(H4)*
9. `v0.5.0 → v0.6.0` minor bump (MANIFEST + Release-History) + ADR-002 §3 wrapper sweep fired. *(H4)*
10. **Zero new III ADR** (count stays 7); ADR-010 disposition recorded; **canonical jsonl md5 `5adb0dfa…`/28 INVARIANT** (zero graduation fires). *(H0/H5)*
11. DG-H scorecard (10-section) + AAR inline + annotated `v0.6.0` tag + federation note + cascade complete. *(H5)*

## §8 Risk Register

| Risk | Severity | Mitigation |
|---|---|---|
| **R1 — conditional pack never reaches ≥3 cycles** (H3 supplies only cycle 2) | Medium | Canonical-**conditional** is explicitly OK (Campaign-G ISS precedent); full graduation of the 6 candidates is **NOT block-closing**; honest candidate labels; remaining cycles accrue as the pattern is used post-campaign |
| **R2 — `who/reviewers/` is a net-new III artifact class** | Medium | DP-2 gate; minimal bench (one reviewer); ADR-010 pre-dispositioned no-structural-ADR; fallback = reviewer-orchestra voice row |
| **R3 — scope-ladder split across vaults** (cycle/decadal in aDNA.aDNA) | Medium | Build atop `skill_iii_review` (DP-3); do NOT pull cycle/decadal upstream; acknowledge as consumer-side analogs |
| **R4 — cross-vault reference rot** (pack/persona cite aDNA.aDNA reviewers) | Low | Re-home wikilinks to III-local where the target graduates; keep explicit cross-vault pointers (Standard Archivist) where it doesn't; verify at H1 |
| **R5 — source-artifact drift** (sources are `status: draft` in aDNA.aDNA) | Low | Pilot is CLOSED (low drift risk); snapshot/read-pin source content at H1; the canonical copy is the graduation, not a live mirror |
| **R6 — graduation scope creep** (tempting to also land gates-as-runtime, cycle/decadal) | Medium | Gates graduate **as-template** only; cycle/decadal OUT; the `site_backing_slice` is bespoke (only its method graduates) |

## §9 Verification Strategy

| Level | Check | Gate? |
|---|---|---|
| Per-mission | Hard-invariant snapshot pre+post (jsonl md5+count, production pin, ADR count, no `who/reviewers/` until DP-2) | yes |
| Per-phase | Phase-exit gate assertion (each phase §5) | yes |
| Skill | Dry-validation against the pilot's M0–M5 record (DG-H crit 2) | yes |
| Pack | Cross-artifact cycle 2 + negative control (DG-H crit 7) | yes |
| Boundary | No gate runtime / no cross-vault writes / no new ADR — asserted in DG-H scorecard | yes (crit 10) |
| Canonical | jsonl md5 INVARIANT (no graduation fires) | yes (crit 10) |

## §10 Timeline

| Phase | Missions | Sessions |
|---|---|---|
| H0 | charter + ack + specs | 1 (done 2026-06-29) |
| H1 | graduate pack + persona | 1–2 |
| H2 | build skill (anchor) | 1–2 |
| H3 | cross-artifact test | 1 |
| H4 | III-core improve + bump + sweep | 1 |
| H5 | DG-H + tag + federation | 1 |
| **Total** | **6 missions** | **~6–8 sessions** |

## §11 Notes

Structurally models on `campaign_g_consolidation.md` (Campaign G), which extended Campaign F/D. Phases H0..H5 (one fewer than G's H0..H6 — H folds validation into H3 and closeout into H5). DG-H mirrors DG-G's 11-criterion shape; the H5 scorecard will mirror `g6_dg_g_scorecard.md`. Three operator decisions are locked: **graduation-inbound boundary**, **canonical-conditional pack graduation**, **`v0.6.0`**. ADR-010 pre-dispositioned no-structural-ADR. This campaign carries **no campaign-local `CLAUDE.md`** (Campaign G precedent — the charter is authoritative); its standing orders are: (1) phase gates are human gates (Standing Rule 7); (2) no writes into aDNA.aDNA (Rule 10) — the pilot is read-only ground truth; (3) canonical jsonl INVARIANT — the pack/persona graduate as pack/persona, never as learning-store corrections; (4) every mission gets an AAR. The H0 artifacts (`h0_*` + this charter + the planning mission + the ack memo) are this campaign's P0 record.

## §12 Completion Summary

*Placeholder — populated at H5 (DG-H close).*

## §13 Campaign AAR

*Placeholder — populated inline at the DG-H scorecard (H5) and restated here per Campaign A–G precedent.*
