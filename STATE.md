---
type: state
created: 2026-05-07
updated: 2026-06-29
last_edited_by: agent_argus
status: active
persona: argus
pattern_category: framework
last_session: session_stanley_20260629_iii_adna_campaign_h_touchstone_planning
tags: [state, governance, iii, argus, framework, campaign_h_draft, iii_v0_5_0_tagged]
---

# III.aDNA — Operational State

> **Project**: III.aDNA (Inspect / Introspect / Improve) · **Category**: Framework.aDNA · **Persona**: Argus.
> Lean cold-start snapshot — current state only. **Full history → [[STATE_history]]** (dated direction blocks, closed-campaign mission queues + DG gate criteria, "PRIOR — Campaign …" stack, release narratives; relocated verbatim under finding ii-1, SO-7). On campaign/phase close, update THIS file; append the prior block to `STATE_history.md`.

## ⏭ Current Phase (read first)

**Campaign H ("Operation Touchstone") — 📝 DRAFT, awaiting DP-1.** Graduate the **III-campaign pattern** — the strategic Construct→Review→Improve scope tier piloted by aDNA.aDNA's Operation Looking Glass — into canonical III.aDNA capability on a `v0.5.0→v0.6.0` minor bump: a new **`skill_iii_campaign`** (anchor) + the **`representation_coherence`** pack (canonical-conditional) + the **`claim_tracer`** persona (candidate) + the doctrines + III-core improvements from the pilot's primitive-gaps. **H0 authored 2026-06-29** (Argus ack + planning mission + DRAFT charter + 3 `h0_*` graduation specs); **opens only at the DP-1 operator gate** (Standing Rule 7). Boundary: **graduation-inbound** (the pilot's artifacts stay read-only in aDNA.aDNA — no cross-vault writes); canonical jsonl **INVARIANT** (the pack/persona graduate *as* pack/persona, not as learning-store corrections). Handoff acked → `who/coordination/coord_2026_06_29_argus_ack_iii_campaign_handoff.md`. Charter: `how/campaigns/campaign_h_iii_campaign_pattern/` (`status: draft`); planning mission: `how/missions/plan_campaign_h_iii_campaign_pattern.md`.

---

**Predecessor — Campaign G ("Operation Atrium") — ✅ CLOSED 2026-06-23 — DG-G 11/11; `v0.5.0` annotated tag CUT.** Boundary-preserving consolidation push anchored on net-new III coverage of **ISS operator-decision-gate** quality. III stayed semantic end-to-end (no ISS/audit runtime — SiteForge owns audit execution + library). (Campaign E — generalized writing-III — remains forward-seeded, gated on LiteratureForge.aDNA forge-BUILD.)

- **G1–G4 ✅** — ISS pack (6 traps; composite 4.00) + learning_store deepening (sd 3→4; composite 4.00) + audit-ingest seam (lattice 1.2.5→**1.2.6**) + ISS candidate ledger (ISS-C6/C8/C9) + DP-4 candidates-only + III `v0.4.1→v0.5.0` DECLARED (MANIFEST `0f06aa6`) + ADR-002 §3 sweep (VideoForge `ab5b178` + CanvasForge `3ebd55a` + wga `cfc5d7e` @ v0.5.0) + ISS-adopter invite fired.
- **G5 ✅ — VERDICT GO** (`how/campaigns/campaign_g_consolidation/missions/g5_validation_20260623.md`) — 6/6 recommended L3 residue axes caught across **3 of 3** live adopters (ZenZachary + Molecules + WilhelmAI; ≥2-of-3 bar exceeded). Corpus grew since G0: **Trap 4** (RLHF-completeness — `rlhf_reviewer_persona` absent though top-level `persona` known) upgraded 1-vault→**3-vault**; **Trap 7** (confidence — none on an IRREVERSIBLE `lh0` security-model signoff + charter ratification) → **2-vault**; honest new **Trap 1** observation (`decision_significance` in HTML but dropped from output JSON). Held a11y candidates 6/8/9 **negative-control clean** (zero new FPs). `zero_regression_confirmed: true`.
- **G6 ✅ — DG-G 11/11** (`how/campaigns/campaign_g_consolidation/missions/g6_dg_g_scorecard.md`; 10-section, 11-criterion) + AAR inline + **annotated `v0.5.0` tag CUT at the G6 close commit** (F4/v0.4.0 precedent — declared at G4 `0f06aa6`, tagged at the DG-G close) + full cascade (STATE + root CLAUDE.md + MANIFEST already carried from G4 + workspace-router production-pin v0.4.0→v0.5.0). Local tag only — push operator-gated.

Charter: `how/campaigns/campaign_g_consolidation/` (`status: completed`).

## 📊 Campaigns

| Campaign | Goal | Status |
|----------|------|--------|
| A: Genesis + Foundation | Bootstrap vault, migrate content, modularize, airlock v0.1.0 | ✅ **CLOSED 2026-05-08** — DG-A 9/9 (`v0.1.0` tag `1628793`) |
| B: Ecosystem Federation | Wire `iii/` consumer wrappers across consumer vaults | ✅ **CLOSED 2026-05-12** — DG-B 9/9 (6 wrappers live) |
| C: Airlock Standard v0.2 | Cross-vault request surface + propagate airlock pattern | ✅ **CLOSED 2026-05-20** — DG-C 9/9 (`v0.2.0` tag `246124d`/`5cd210e`) |
| D: Federation-Aware Airlock v0.3 + RLHF/Adaptive Loop | D1 federation-aware airlock v0.3 + D2 RLHF/adaptive-improvement loop | ✅ **CLOSED 2026-05-25** — DG-D 11/11 (`v0.3.0` annotated tag) |
| F: Web-Design Deep-Review ("Operation Tell") | Boundary-preserving deep-review/improve of the `web_design` pack | ✅ **CLOSED 2026-05-27** — DG-F 11/11 (`v0.4.0` annotated tag; composite 4.00→4.17) |
| G: Consolidation & ISS-Surface Pack ("Operation Atrium") | v0.4.1→v0.5.0; net-new ISS operator-decision-gate coverage | ✅ **CLOSED 2026-06-23** — DG-G 11/11 (`v0.5.0` annotated tag at G6 close commit; G5 validation GO — 6/6 residue axes across 3/3 adopters; negative control clean) |
| H: Graduate the III-Campaign Pattern ("Operation Touchstone") | v0.5.0→v0.6.0; graduate `skill_iii_campaign` + `representation_coherence` pack + `claim_tracer` persona (from the Looking Glass pilot) | 📝 **DRAFT 2026-06-29** — H0 authored; opens at DP-1 |
| E: Generalized writing-III | Re-points LPWhitepaper `federation_ref` | ⏸ **forward-seeded** — opens when LiteratureForge.aDNA reaches its forge-BUILD phase (MD-X2 §6) |

## 🎯 Current invariants / pin

- **Production pin**: `v0.5.0` **TAGGED** at the G6 DG-G close commit 2026-06-23 (declared at G4 MANIFEST commit `0f06aa6`; annotated tag cut at the close commit per F4/v0.4.0 precedent — NOT at the declaration commit); predecessor `v0.4.0` tag `7500c19`. Local tag only — push operator-gated.
- **Canonical learning store**: `what/context/core_domain_packs/iii_corrections_canonical.jsonl` — **28 entries**, md5 `5adb0dfa38d9224649c3b2cba83852ae`, **INVARIANT since MD-B2 (2026-05-21)**.
- **III ADR count = 7** (000 + 001 + 002 + 003 + 005 + 007 + 008); ADR-009 REJECTED at Campaign G.
- **Lattice oracle**: `lattice_iii_verification_oracle.lattice.yaml` v**1.2.6**.
- **Active consumer wrappers**: VideoForge · CanvasForge · wga re-pinned **@ v0.5.0** (`ab5b178`/`3ebd55a`/`cfc5d7e`); SiteForge (F4-residue) + LPWhitepaper (no `web_design`) + lattice-labs tracked-deferred per own cadence.
- **Core domain packs** (`what/context/core_domain_packs/`): `inspect_procedures` · `introspect_checks` · `learning_store` · `web_design` (7 traps; composite 4.17) · `whitepaper_communication` · `canvas_visual` · `vault_maintenance` · `iss_surfaces` (6 traps; composite 4.00).

## ⛔ Blockers

None blocking. **Campaign H ("Operation Touchstone") is DRAFT — awaiting the DP-1 operator gate** to open (Standing Rule 7); H0 authored 2026-06-29 (no canonical mutation — production pin `v0.5.0`, canonical jsonl md5 `5adb0dfa…`/28, ADR count 7 all UNCHANGED). Campaign G CLOSED end-to-end (DG-G 11/11; `v0.5.0` tagged). Operator-gated tails: `git push` of the v0.5.0 commits + tag (local-only) **and** the new Campaign H H0 commit; workspace-router III-row pin (Berthier handles).

## 👁 Watch

- **Campaign H DP-1** — the DRAFT charter (`how/campaigns/campaign_h_iii_campaign_pattern/`) opens to `active` only at the operator gate; all H1+ work (graduate the `representation_coherence` pack [canonical-conditional] + `claim_tracer` persona, build `skill_iii_campaign`, the `v0.6.0` bump) is gated. **DP-2** (reviewer-bench: establish `who/reviewers/` vs map onto the reviewer-orchestra) + **DP-3** (scope-ladder: build atop `skill_iii_review`) are the open structural decisions. Rosetta→Argus handoff acked (`who/coordination/coord_2026_06_29_argus_ack_iii_campaign_handoff.md`); the source memo's ack checkboxes await Rosetta-side reconciliation in aDNA.aDNA.
- **`v0.5.0` push** — local commits + annotated tag await operator `git push` per workspace standing rules (HEAD will sit ahead of `origin/main`).
- **ISS candidates** — `ISS-C6`/`C8`/`C9` (G3 ledger; substrate-grounded, latent) land into the ISS pack (NOT the canonical jsonl) when template-local a11y residue actualizes; G5 negative-control re-confirmed all three held correctly on the richer corpus. Trap 2 (persona-voice) live actualization still watched (EGL-4 latent).
- **Graduation carry** — C-029 (2-vault SS+wga) at next natural-frequency gate; C-030..C-033 await 2nd-vault evidence; G-era correction candidates remain candidates-only (canonical md5 invariant).
- **`vault_maintenance` pack revision** — standing from MD-B4 §8.
- **Git.aDNA P6 Wave 2 (2026-06-22)** — flipped `aDNA-Network/III.aDNA` GitHub-public, class P-released (visibility-only; `origin` unchanged). Git/CI-CD doctrine federates via `git/` (broker Git.aDNA).
- **Consumer re-pins** — SiteForge folds F4 pin at own cadence; LPWhitepaper re-pins at own cadence (likely concurrent with Campaign E).

## 🏛 Lineage

III originated in Zeta-aDNA `campaign_zeta_genesis` M00; evolved through `campaign_iii_evolution` (COMPLETE, 5 missions); externalized to III.aDNA 2026-05-07. Five campaigns shipped end-to-end (A·B·C·D·F); Campaign G in flight. Full version-by-version history → [[STATE_history]].
