---
type: state
created: 2026-05-07
updated: 2026-06-22
last_edited_by: agent_argus
status: active
persona: argus
pattern_category: framework
last_session: session_stanley_20260531_iii_adna_campaign_g_g4_minor_bump_wrapper_sweep
tags: [state, governance, iii, argus, framework, campaign_g_open, iii_v0_5_0_declared]
---

# III.aDNA — Operational State

> **Project**: III.aDNA (Inspect / Introspect / Improve) · **Category**: Framework.aDNA · **Persona**: Argus.
> Lean cold-start snapshot — current state only. **Full history → [[STATE_history]]** (dated direction blocks, closed-campaign mission queues + DG gate criteria, "PRIOR — Campaign …" stack, release narratives; relocated verbatim under finding ii-1, SO-7). On campaign/phase close, update THIS file; append the prior block to `STATE_history.md`.

## ⏭ Current Phase (read first)

**Campaign G ("Operation Atrium") — OPEN. G1–G4 CLOSED; G5 + G6 PENDING.** Boundary-preserving consolidation push anchored on net-new III coverage of **ISS operator-decision-gate** quality. III stays semantic (no ISS/audit runtime — SiteForge owns audit execution + library).

- **G1 ✅** — `context_iii_iss_surfaces.md` ISS pack live (6 traps {1,2,3,4,5,7}; a11y Traps 6/8 held as candidates; composite **4.00**).
- **G2 ✅** — `learning_store` deepened (`source_diversity` 3→4; composite 4.00) + audit-ingest seam wired into `web_design` Trap-6 + lattice oracle **1.2.5→1.2.6** (ISS pack registered).
- **G3 ✅** — ISS candidate ledger `what/artifacts/g3_iss_candidate_ledger.md` (trap-axis candidates `ISS-C6`/`C8`/`C9`; land into the pack, not the canonical jsonl) + DP-4 candidates-only.
- **G4 ✅** — III **`v0.4.1 → v0.5.0` minor bump DECLARED** (MANIFEST commit `0f06aa6`; **annotated tag DEFERRED to G6** per the F4/v0.4.0 precedent) + ADR-002 §3 wrapper sweep (VideoForge `ab5b178` + CanvasForge `3ebd55a` + wga `cfc5d7e` re-pinned @ v0.5.0) + ISS-adopter validation invite FIRED (`coord_2026_05_31_iii_to_iss_adopters_validation_invite.md`).
- **G5 PENDING** — inspection-grade validation on live ISS adopters (ZenZachary + MoleculeForge + WilhelmAI; ≥2 of 3).
- **G6 PENDING** — DG-G 11/11 + AAR + **annotated `v0.5.0` tag** + full cascade (incl. the workspace-router production-pin).

Charter: `how/campaigns/campaign_g_consolidation/`.

## 📊 Campaigns

| Campaign | Goal | Status |
|----------|------|--------|
| A: Genesis + Foundation | Bootstrap vault, migrate content, modularize, airlock v0.1.0 | ✅ **CLOSED 2026-05-08** — DG-A 9/9 (`v0.1.0` tag `1628793`) |
| B: Ecosystem Federation | Wire `iii/` consumer wrappers across consumer vaults | ✅ **CLOSED 2026-05-12** — DG-B 9/9 (6 wrappers live) |
| C: Airlock Standard v0.2 | Cross-vault request surface + propagate airlock pattern | ✅ **CLOSED 2026-05-20** — DG-C 9/9 (`v0.2.0` tag `246124d`/`5cd210e`) |
| D: Federation-Aware Airlock v0.3 + RLHF/Adaptive Loop | D1 federation-aware airlock v0.3 + D2 RLHF/adaptive-improvement loop | ✅ **CLOSED 2026-05-25** — DG-D 11/11 (`v0.3.0` annotated tag) |
| F: Web-Design Deep-Review ("Operation Tell") | Boundary-preserving deep-review/improve of the `web_design` pack | ✅ **CLOSED 2026-05-27** — DG-F 11/11 (`v0.4.0` annotated tag; composite 4.00→4.17) |
| G: Consolidation & ISS-Surface Pack ("Operation Atrium") | v0.4.1→v0.5.0; net-new ISS operator-decision-gate coverage | 🔄 **OPEN** — G1–G4 closed; **G5 + G6 pending** (see Current Phase) |
| E: Generalized writing-III | Re-points LPWhitepaper `federation_ref` | ⏸ **forward-seeded** — opens when LiteratureForge.aDNA reaches its forge-BUILD phase (MD-X2 §6) |

## 🎯 Current invariants / pin

- **Production pin**: `v0.5.0` **DECLARED** at G4 (MANIFEST commit `0f06aa6`); predecessor `v0.4.1` (MX-1, 2026-05-28). **Annotated `v0.5.0` tag deferred to G6** — NOT yet cut.
- **Canonical learning store**: `what/context/core_domain_packs/iii_corrections_canonical.jsonl` — **28 entries**, md5 `5adb0dfa38d9224649c3b2cba83852ae`, **INVARIANT since MD-B2 (2026-05-21)**.
- **III ADR count = 7** (000 + 001 + 002 + 003 + 005 + 007 + 008); ADR-009 REJECTED at Campaign G.
- **Lattice oracle**: `lattice_iii_verification_oracle.lattice.yaml` v**1.2.6**.
- **Active consumer wrappers**: VideoForge · CanvasForge · wga re-pinned **@ v0.5.0** (`ab5b178`/`3ebd55a`/`cfc5d7e`); SiteForge (F4-residue) + LPWhitepaper (no `web_design`) + lattice-labs tracked-deferred per own cadence.
- **Core domain packs** (`what/context/core_domain_packs/`): `inspect_procedures` · `introspect_checks` · `learning_store` · `web_design` (7 traps; composite 4.17) · `whitepaper_communication` · `canvas_visual` · `vault_maintenance` · `iss_surfaces` (6 traps; composite 4.00).

## ⛔ Blockers

None. Open work = G5 (live-ISS-adopter validation, ≥2 of 3) → G6 (DG-G + AAR + annotated `v0.5.0` tag + cascade). G5/G6 are human-gated phase advances (Standing Rule 7).

## 👁 Watch

- **G5/G6 carry** — annotated `v0.5.0` tag + DG-G + workspace-router production-pin cascade all land at G6 (deferred from G4 per F4/v0.4.0 precedent).
- **ISS candidates** — `ISS-C6`/`C8`/`C9` (G3 ledger; substrate-grounded, 0-vault-live) land into the ISS pack, NOT the canonical jsonl; a11y Traps 6/8 held as candidates (latent-only on live ZenZachary corpus).
- **Graduation carry** — C-029 (2-vault SS+wga) at next natural-frequency gate; C-030..C-033 await 2nd-vault evidence; G-era correction candidates remain candidates-only (canonical md5 invariant).
- **`vault_maintenance` pack revision** — standing from MD-B4 §8.
- **Git.aDNA P6 Wave 2 (2026-06-22)** — flipped `aDNA-Network/III.aDNA` GitHub-public, class P-released (visibility-only; `origin` unchanged). Git/CI-CD doctrine federates via `git/` (broker Git.aDNA).
- **Consumer re-pins** — SiteForge folds F4 pin at own cadence; LPWhitepaper re-pins at own cadence (likely concurrent with Campaign E).

## 🏛 Lineage

III originated in Zeta-aDNA `campaign_zeta_genesis` M00; evolved through `campaign_iii_evolution` (COMPLETE, 5 missions); externalized to III.aDNA 2026-05-07. Five campaigns shipped end-to-end (A·B·C·D·F); Campaign G in flight. Full version-by-version history → [[STATE_history]].
