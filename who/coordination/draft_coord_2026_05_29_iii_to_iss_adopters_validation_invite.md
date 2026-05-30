---
type: coordination
artifact_class: cross_vault_memo_draft
status: draft
from_vault: III.aDNA
from_persona: agent_argus
to_vault: [MoleculeForge.aDNA, WilhelmAI.aDNA, ZenZachary.aDNA]
to_persona: [franklin, hygieia, pygmalion]
created: 2026-05-29
campaign: campaign_g_consolidation
subject: "ISS-adopter community: §G5 validation invite + opt-in iss_surfaces packs_used consultation + RLHF-completeness preview"
tags: [coordination, draft, campaign_g, operation_atrium, iii_to_iss_adopters, multi_recipient, validation_invite]
---

# DRAFT coord — III → ISS adopters (MoleculeForge + WilhelmAI + ZenZachary)

> **DRAFT.** Multi-recipient. Fires at Campaign G **G0** after DP-1. Not sent at planning-mission execution.

## Context

III.aDNA is opening **Campaign G (Operation Atrium)** to author `context_iii_iss_surfaces.md` — a semantic review pack for operator-decision-gate quality. You are the live ISS-adopter community (verified 2026-05-29): **MoleculeForge** (1 completed `.output.json`), **WilhelmAI** (config exemplar), **ZenZachary** (14 rendered gates — the richest corpus). The pack reviews the gates you author; III never runs the ISS runtime.

## Asks

1. **§G5 validation invite.** At Campaign G's validation phase (G5), III will inspection-grade-review your authored gates against the landed pack — to confirm each trap catches real residue without false positives. **No action needed from you now**; this is a courtesy heads-up that your `iss/` gates are the validation corpus. (ZenZachary: your 14 gates are the primary rendered-gate corpus; MoleculeForge: your `p6_1` output is the JSON exemplar.)
2. **Opt-in `iss_surfaces` consultation.** At G4 (v0.5.0 minor bump), you may add `iss_surfaces` to your `iii/` wrapper's `packs_used` to have III review your gate quality. **Decision for you:** adopting `iss/` (authoring gates) and reviewing gate quality (`iii/iss_surfaces`) are **two separate decisions** — you can do either, both, or neither at your own cadence (additive minor bump, backward-compatible per F4 precedent). No re-pin is forced.
3. **RLHF-completeness preview.** The pack's strongest trap (ISS-Trap 4, RLHF signal-capture completeness) was motivated partly by your gate outputs — e.g. `rlhf_reviewer_persona` NULL though persona is known; confidence-scale polarity unstated. Capturing these makes your gate outputs richer RLHF signal (the CF→Future-Cross-Coupling-ISS-RLHF-III path). Optional preview of the trap's detection cues available on request.

## Boundary note
III reviews the contract surface (output JSON + rendered HTML + wrapper config) on disk; it does not render, re-render, or score your gates via runtime. ADR-009 REJECTED — no new III ADR.

## Cross-references
`g0_iss_empirical_gap_ledger.md` (your gates' residue); `g0_iss_pack_revision_spec.md` (ISS-Trap 4); `g0_cross_vault_architecture_note.md` §2/§4; ADR-002 §3 (sweep); ADR-005 / ADR-028 AD-7 (RLHF schema).
