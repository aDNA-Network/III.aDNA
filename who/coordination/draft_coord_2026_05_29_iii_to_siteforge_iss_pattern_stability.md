---
type: coordination
artifact_class: cross_vault_memo_draft
status: draft   # DRAFT — fires at G0 of Campaign G (after DP-1 open), NOT at planning-mission execution
from_vault: III.aDNA
from_persona: agent_argus
to_vault: SiteForge.aDNA
to_persona: agent_siteforge_native
created: 2026-05-29
campaign: campaign_g_consolidation
subject: "ISS pattern-stability confirm + commit-SHA pin + ISS-pack draft review invite + 2 adaptation-guide-doc candidates"
tags: [coordination, draft, campaign_g, operation_atrium, iii_to_siteforge, iss_pattern_stability]
---

# DRAFT coord — III → SiteForge: ISS pattern-stability + pack-review invite

> **DRAFT.** Fires at Campaign G **G0** after the operator opens the campaign (DP-1). Not sent at planning-mission execution.

## Context

III.aDNA is opening **Campaign G (Operation Atrium)** to author a net-new semantic review pack — `context_iii_iss_surfaces.md` — that reviews **operator-decision-gate (ISS) quality** at consumer-authoring time. This is the **L3 semantic layer** complementing your **L1 library contract** and **L2 AD-10 6-axis design ranker**; III stays semantic and **never** runs the ISS runtime — the pack reads gate output JSON (AD-7) + rendered HTML structure + wrapper config on disk.

## Asks

1. **Pattern-stability confirm + commit-SHA pin.** III pins the ISS contract surface at a SiteForge commit-SHA for Campaign G (R1 mitigation). Current snapshot: ADR-028/029 `accepted`; adaptation guides + `skill_create_iss` `active`; live wrappers pin `903f461` (D10) / `^0.3.0` (P5.M3). **Confirm the ISS substrate is pattern-stable through ~v0.5.0** (or name the next expected bump) so the pack reviews a stable contract.
2. **ISS-pack draft review at G1 close.** Invite SiteForge to review the landed `iss_surfaces` pack for boundary-fidelity (that III's traps describe the contract surface accurately and don't drift into L1/L2 territory).
3. **2 adaptation-guide-doc candidates** surfaced by the G0 empirical gap ledger — these are **SiteForge-side** (not III traps):
   - **G-W1** — `framework_orgvault_orggraph_guide.md` (and siblings) don't document *when* a vault author should set `decision_significance` (ROUTINE / LOAD_BEARING / IRREVERSIBLE). Consider an adaptation-guide passage.
   - **G-W2** — no guide states the confidence-capture rule ("rank-only gates skip confidence; multi-decision gates require it"). Consider documenting.

## Boundary note
III does not propose any change to the ISS runtime, library, or your AD-10 ranker. The pack is Markdown trap definitions. ADR-009 (ISS-Surface Artifact Class on the III side) was evaluated and pre-disposed REJECTED — ISS is a consumed pattern; no III-side structural ADR (same precedent as `web_design`).

## Cross-references
`g0_iss_pack_revision_spec.md`; `g0_iss_internal_coverage_map.md` (L1/L2/L3); `g0_cross_vault_architecture_note.md`; ADR-028/029.
