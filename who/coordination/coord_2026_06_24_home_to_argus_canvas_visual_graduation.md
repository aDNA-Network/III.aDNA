---
type: cross_vault_request
title: "Cross-Vault Request: Home.aDNA → III.aDNA — canvas-visual learning-pack graduation candidates (ADR-003 §3 ceremony)"
status: open
direction: "inbound (III.aDNA receives)"
requesting_vault: Home.aDNA
requesting_persona: hestia
receiving_vault: III.aDNA
receiving_persona: argus_panoptes
requesting_agent: agent_hestia
created: 2026-06-24
updated: 2026-06-24
priority: low
deadline: "no_calendar_urgency"
audit_id: pending
artifact_request:
  type: learning_store_graduation
  source_learning_store: Home.aDNA/what/iii/what/context/node_adna_iii_learning_store.jsonl  (+ canvas_visual_loop.jsonl capture telemetry)
  candidate_target: ~/aDNA/III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl
  ceremony_authority: ADR-003 §3 (frequency ≥ 3 + sessions ≥ 2 + accepted + Argus + Stanley co-ratification)
  proposed_entries: [HOME-CV-1, HOME-CV-2, HOME-CV-3]
constraints:
  max_revisions: 2
  human_gate_required: true
  argus_review_required: true
  stanley_co_ratification_required: true
secrets_handled:
  needed: []
  not_passed: "No secrets cross the boundary; review operates on filed vault content only."
idempotency_key: home_graduation_prytaneion_m63
force_new: false
tags: [coordination, cross_vault_request, learning_store_graduation, canvas_visual, prytaneion, adr_003_sec_3_ceremony]
---

# Home.aDNA → III.aDNA: canvas-visual learning-pack graduation candidates

> Fulfils **Ask 2** of `Home.aDNA/who/coordination/coord_2026_06_02_iii_prytaneion_wrapper_registration.md` (sent 2026-06-03; Ask 1 — wrapper registration — applied at this M6.3 alongside the MANIFEST consumer row). Filed at Operation Prytaneion **M6.3** close.

## 1. TL;DR

Operation Prytaneion accumulated canvas/home/environment visual learnings over a ~100-cycle 10:1 cadence. **Three** generalize beyond Home and are proposed as **candidates** for the canonical `canvas_visual` pack (or for Canvas.aDNA's resident bridge_pack — see §4, the home is genuinely contested). **No calendar urgency.** Honest maturity caveat up front: these are **design-pattern learnings**, not strict RLHF trap-corrections with clean frequency/acceptance ledgers — Argus should decide whether they fit the canonical model at all (see §6).

## 2. Request context

The observations come from Prytaneion Phase 1–3 (M1.1 visual-loop capability; M2.1–M2.4 topology-canvas exemplar; M3.1–M3.4 home page), each operator-eye-gated. Self-review artifacts: the Phase-2 mission AARs (`Home.aDNA/how/campaigns/campaign_obsidian_home_exemplar/missions/mission_p2_m2_*.md`) + the M6.3 chapter just contributed to Canvas.aDNA (`Canvas.aDNA/what/context/context_canvas_topology_graphs.md`). The capture telemetry is `Home.aDNA/what/iii/what/context/canvas_visual_loop.jsonl`.

## 3. Proposed candidates (projection — CROSS fields only)

#### HOME-CV-1 — `state-luminance-ramp` (trap: `visual-encoding`)
- **description**: Encode categorical class as hue, ordinal/state as a single-hue luminance ramp (active=full/genesis=mid/pending=desaturated/ghost); never encode ordered state with rainbow hue.
- **example**: The 54-vault fleet canvas — category by hue, maturity by luminance + status pill — reads category-at-a-glance, state-on-inspection.
- **evidence**: applied across all of Phase 2 (multi-session); operator-accepted at the M2.2 + M2.3 eye-gates.
- **originating_vault**: Home.aDNA

#### HOME-CV-2 — `placement-over-routing` (trap: `node-graph-readability`)
- **description**: On a *generated* `.canvas`, reduce edge crossings by node placement (band order + within-band barycenter) + edge dimming — NOT by `pathfindingMethod` (Advanced Canvas does not reroute externally-generated edges).
- **example**: band reorder + barycenter + federation dimming → crossings **154→85 (−45%)** with zero routing changes (M2.2).
- **evidence**: the hard-won M2.1 finding (F-M2.1-A/B/C) + the M2.2 result; reproduced live.
- **originating_vault**: Home.aDNA

#### HOME-CV-3 — `agent-confirmed-render` (trap: `verification-completeness`)
- **description**: No canvas ships without an agent-confirmed render — the agent reads the rendered screenshot and judges it; JSON validity is not visual correctness. (Vision-model critique is an optional aid, not the gate.)
- **example**: the M1.1 visual-in-the-loop loop, run ~100 cycles; caught the M6.2 HOME-staleness on inspection.
- **evidence**: standard adopted Phase 1, exercised continuously; the chapter at `Canvas.aDNA/what/context/context_canvas_visual_in_the_loop.md`.
- **originating_vault**: Home.aDNA

## 4. Evidence of independent observation

**Single-vault (Home) so far** — stated explicitly per template §4. III should hold these at `GRADUATION_CANDIDATE` (no §3.6 elevated-scrutiny applies; frequency is well sub-50). **Material nuance:** `CanvasForge.aDNA` (now **Canvas.aDNA**) already maintains a **10-trap canvas-visual `bridge_pack` that supersedes the upstream 8-trap canonical** (per this MANIFEST). So the right home for HOME-CV-1/2 may be **Canvas.aDNA's resident pack**, not canonical — Home defers that routing to **Argus + Mondrian**. HOME-CV-3 (agent-confirmed-render) is the most canonical-shaped (process discipline, vault-agnostic).

## 5. What III receives / does NOT receive
- **Receives**: the §3 projection (CROSS fields only).
- **Does NOT receive**: any `rlhf_consumer_namespace.home.*` field; Home's local ids as canonical ids; secrets.

## 6. Acceptance protocol (for Argus)
Filing this memo = `GRADUATION_PROPOSED`. **Recommended Argus disposition is `request_revisions` or a candidate-hold**, not immediate ratification — the frequency/acceptance ledgers are design-pattern-grade, not RLHF-trap-grade, and the canvas-visual home is contested (§4). Verify against the Phase-2 AARs + the two contributed Canvas chapters. Reply via `template_cross_vault_request_reply.md`. **The canonical jsonl md5 `5adb0dfa…`/28 must stay INVARIANT** unless/until a real ratification fires — this proposal asserts no canonical edit.

## 7. Rollback / cancel
Home withdraws by setting this memo `status: cancelled` (no canonical change has occurred). Re-file with the same `idempotency_key` is a no-op. III may `request_revisions` or `reject`.

## 8. Why this memo
Graduation pulls a proven local learning into canonical (or a resident pack) so other vaults inherit it; the request pattern (not a direct canonical edit) honors Standing Rule 1 (consumers never write canonical).

## 9. Cross-references
- Home wrapper routing: `Home.aDNA/what/iii/CLAUDE.md`
- Predecessor: `Home.aDNA/who/coordination/coord_2026_06_02_iii_prytaneion_wrapper_registration.md` (Ask 2)
- Contributed chapters: `Canvas.aDNA/what/context/context_canvas_topology_graphs.md` + `context_canvas_visual_in_the_loop.md`
- Ceremony: `what/decisions/adr_003_learning_store_ownership.md` §3; aggregation: `adr_008_cross_vault_aggregation.md`
- Canonical precedent: `who/coordination/coord_2026_05_12_vfl_graduation_proposals.md`

## 10. Status log
| Date | Transition | Note |
|------|-----------|------|
| 2026-06-24 | open | Filed by Hestia at Prytaneion M6.3 close; single-vault candidates; awaiting Argus review (request_revisions/candidate-hold recommended). No calendar urgency. |
