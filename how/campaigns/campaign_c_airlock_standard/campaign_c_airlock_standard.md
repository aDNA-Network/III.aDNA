---
type: campaign
campaign_id: campaign_c_airlock_standard
title: "Campaign C: Airlock Standard v0.2 + Cross-Vault Request Patterns"
status: completed
created: 2026-05-08
updated: 2026-05-20   # MC-5 + DG-C CLOSE — v0.2 validation complete; Campaign C end-to-end 9/9
last_edited_by: agent_argus
phase: closed
mc4_closed: 2026-05-13   # disk-state close; retroactive git commit landed 2026-05-20 at c8ce621
mc4_5_closed: 2026-05-20   # alignment recon SINGLE-SESSION CLOSE; planning-class interstitial mission
mc4_git_retroactive_close_commit: c8ce621
mc5_closed: 2026-05-20   # VideoForge → CanvasForge re-exercised against v0.2; zero regression confirmed
dg_c_closed: 2026-05-20   # Campaign C complete end-to-end; AAR populated; charter status: completed
predecessor: campaign_a_iii_genesis
parallel_eligible_with: campaign_b_iii_federation
inbound_proposal: who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md
inbound_findings_count: 5
inbound_proposal_status: closed   # flipped absorbed → closed at MC-5 close 2026-05-20
mc5_validation_artifact: what/artifacts/mc5_validation_videoforge_canvasforge_v0_2.md
tags: [campaign, airlock, standard, v0_2, cross_vault_request, videoforge_findings, completed]
---

# Campaign C: Airlock Standard v0.2 + Cross-Vault Request Patterns

## Mission

Formalize the III.aDNA airlock pattern as a versioned standard. v0.1.0 (the reference implementation in `how/airlock/AIRLOCK.md`, shipped at MA-4) covers **inbound entry paths** — an external agent enters a vault and runs the loop. v0.2 closes the **cross-vault request** gap surfaced by VideoForge.aDNA when it commissioned the Carly + Herb deck from CanvasForge.aDNA: bidirectional, ephemeral two-agent handshakes for "I need you to do X for me" workflows. Author the canonical standard spec at `what/artifacts/iii_airlock_standard_spec.md`, define the request payload YAML schema, bump AIRLOCK.md to v0.2, define substrate-enforcement preflights, and validate the new standard against the same VideoForge → CanvasForge real-world commission.

## Predecessor

Campaign A: Genesis & Foundation — DG-A CLOSED 2026-05-08 9/9. AIRLOCK.md v0.1.0 reference implementation shipped at MA-4 (commit `1628793`). The v0.1.0 anti-regression section "What v0.1.0 does NOT cover" explicitly defers cross-vault request patterns to this campaign.

## Parallel Eligibility

Campaign B (Federation) and Campaign C (Airlock Standard v0.2) are **independently sequencable**. C is lower-priority since v0.1.0 is shipped and no consumer is blocked. B has a critical-path retiring move (MB-1) that takes precedence. C may run interleaved with B once MB-1 closes, or sequentially after Campaign B.

## Inbound Proposal

`who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md` (committed `1bf3074`). VideoForge.aDNA (Iris) exercised AIRLOCK.md v0.1.0 against a real cross-forge commission and surfaced **5 specific gaps**. The proposal lands as a coord memo because III.aDNA does not yet have a `proposed/` channel (per STATE.md MA-0 close 2026-05-07; ADR-005-style RLHF channel not wired). Findings:

1. **Gap 1 — No request-initiation ceremony.** Coord-memo fallback works ad-hoc but has no schema, acceptance protocol, or rejection protocol.
2. **Gap 2 — No handshake.** Wrappers are unidirectional pull-based; no acceptance/ETA/back-pressure surface.
3. **Gap 3 — No request payload schema.** Every memo invents its own structure.
4. **Gap 4 — No cross-vault secret delegation rules.** No handling when receiving vault's pipeline needs a secret the requesting vault holds (or vice versa).
5. **Gap 5 — No idempotency contract for duplicate requests.** Same request filed twice → both processed independently.

The VideoForge → CanvasForge memo at `~/aDNA/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` is the worked example v0.2 must accommodate.

## DG-C Criteria

- [x] **MC-1**: `what/artifacts/iii_airlock_standard_spec.md` authored — codifies entry paths (existing v0.1.0 content) + cross-vault request patterns (VideoForge findings); status `reference_implementation`; version `0.2.0` ✅ **2026-05-08**
- [x] **MC-2**: Request payload YAML schema canonicalized at `what/artifacts/iii_airlock_request_schema.yaml` — JSON Schema Draft 2020-12 in YAML form; validated mechanically (9/9 checks) + walked against VideoForge → CanvasForge worked example (correctly non-conformant per spec §5.2 — pre-v0.2 authoring; gap documented in schema header) ✅ **2026-05-08**
- [x] **MC-3**: `how/airlock/AIRLOCK.md` bumped v0.1.0 → v0.2.0; reply-comment template authored at `how/templates/template_cross_vault_request_reply.md` (handshake — Gap 2) ✅ **2026-05-10**
- [x] **MC-4**: Substrate-enforcement preflight rules documented for `secrets_handled` (Gap 4) + `idempotency_key` contract (Gap 5); receiver-side dedup logic specified ✅ **2026-05-13** (disk) / **2026-05-20** (git retroactive `c8ce621`) — `what/artifacts/iii_airlock_substrate_implementation.md` authored (490 lines, one artifact covering both gaps per MC-2/MC-3 separate-artifact precedent); §2 secrets_preflight (preflight script structure + bash/python sketches + error-message conventions + fully-specified audit-log JSONL schema with `event_type: secrets_preflight` discriminator + boundary-integrity rules + sample acceptance/rejection records coordinated with MC-5); §3 idempotency_key dedup (canonical archive-search filter predicate + 30-day window semantics with 4-tier clock-source fallback + cache structure with optional rebuildable `.idempotency_index.jsonl` + performance profile table with ≤200ms SLA + force_new override audit semantics + dual-purpose audit-log JSONL with `event_type: idempotency_check` discriminator + 3 sample records); §4 reply-comment template integration cites MC-3 template slots; §5 cross-refs. Spec §4.4 line 261 + §4.5 line 282 forward-references resolved patch-level (no version bump); spec §7.2 row 4 flipped ✅; §7.3 forward-refs list updated ("five resolved / one pending — MC-5 validation"); §8.4 cross-references gain substrate-implementation row. AIRLOCK.md line 258 patched (forward-reference resolved; no version bump on AIRLOCK.md — `0.2.0` stays). Documentation-grade per R3; no executable runtime in v0.2. No git tag bump (additive disposition matches MB-6/MB-7/MB-8 precedent); `v0.2.0` commit `246124d` annotated tag object `5cd210e` remain the production pin. Canonical jsonl md5 `dde2cbd88c0b45956fb22285a2a0f856` invariant preserved. (Git commit fired 7 days after disk close — 2026-05-13 → 2026-05-20 — due to closure-protocol drift; discovered at MC-4.5 precondition check; retroactive close in commit `c8ce621`. Pattern lesson → MC-4.5 AAR.)
- [x] **MC-4.5** (interstitial): Alignment recon — III state refresh + LL.aDNA Phase 3+4 + LN.aDNA pc_01 Phase A landscape survey + AIRLOCK.md 9-stub ecosystem inventory + MC-5 scope decision tree (BASELINE/EXPANDED/EXTENDED) + Campaign D candidate sketches (D1 federation-aware + D2 RLHF/adaptive + D3 activation kit) + 2 cross-vault coord memo drafts (deferred fire per D3=B default) ✅ **2026-05-20** — `what/artifacts/mc4_5_alignment_recon_dossier.md` authored (single comprehensive dossier per III's minimal-artifact convention; 8 §sections); MC-5 recommended scope = BASELINE (~0.75 sess; preserves Campaign C original DG-C arc; expansion routes to Campaign D fresh per D2=A default); planning-class single-session shape mirrors aDNA.aDNA M2.3.5 interstitial precedent.
- [x] **MC-5**: VideoForge → CanvasForge request re-exercised against v0.2 schema; deltas captured vs the v0.1.0 ad-hoc execution; zero regression confirmed ✅ **2026-05-20** — `what/artifacts/mc5_validation_videoforge_canvasforge_v0_2.md` authored (§1 Method · §2 Inputs · §3 Execution trace 11 steps · §4 Deltas vs v0.1.0 — 4 enumerable retrofit categories · §5.1 worked-example fidelity — 16/16 §5.1 coverage map rows conform + 2 additive deltas confirmed · §5.2 sample-record fidelity — 5/5 substrate-impl §2.5+§3.6 records schema-conformant + dimensionally coherent · §6 DG-C gate criteria check · §7 out-of-scope deferred-to-Campaign-D · §8 cross-refs). BASELINE scope per MC-4.5 §4 D1=A default executed verbatim. Documentation-grade validation by inspection per R3 (no executable validator; first executable enforcement = future Platform.aDNA). Zero regression confirmed: the 2026-05-08 worked example as authored remains valid evidence under v0.2 with 2 purely additive non-blocking deltas per spec §5.2. Pre-flight grep discharge (Campaign B AAR Change #1 process improvement): 0 unflipped checkboxes; 0 non-Status-Log `(pending)` markers post-AAR populate; frontmatter status matches body assertions.
- [x] Annotated git tag `v0.2.0` cut at MC-3 closure ✅ **2026-05-10** — no further tag at DG-C close (additive disposition matches MB-6/MB-7/MB-8/MC-4/DG-B precedent; operator decision at MC-5 plan ratification: no v0.2.1 cut; `v0.2.0` commit `246124d` / tag object `5cd210e` remain the production pin)
- [x] VideoForge proposal status flipped `open` → `accepted` → `absorbed` → `closed` in `coord_2026_05_08_airlock_v0_2_videoforge_findings.md` ✅ **2026-05-20** — Status Log row 207 reservation fired at MC-5 close; full lifecycle traced

## Mission Table

| Mission | Objective | Sessions | Status |
|---------|-----------|----------|--------|
| **MC-1** | Author `iii_airlock_standard_spec.md` (entry paths + cross-vault request patterns) | 1 | ✅ **COMPLETE 2026-05-08** |
| **MC-2** | Request payload YAML schema; canonical location; worked-example validation | 0.5 | ✅ **COMPLETE 2026-05-08** |
| **MC-3** | AIRLOCK.md v0.1.0 → v0.2.0; handshake reply-comment template | 1 | ✅ **COMPLETE 2026-05-10** |
| **MC-4** | Substrate enforcement: `secrets_handled` preflight (Gap 4) + `idempotency_key` (Gap 5) | 1 | ✅ **COMPLETE 2026-05-13** (disk) / **2026-05-20** (git `c8ce621` retroactive) |
| **MC-4.5** | Alignment recon (interstitial): III state refresh + LL+LN landscape survey + 9-stub AIRLOCK inventory + MC-5 scope decision tree + Campaign D candidate sketches + coord memo drafts | 1 | ✅ **COMPLETE 2026-05-20** — single-session planning-class; mirrors aDNA.aDNA M2.3.5; see `what/artifacts/mc4_5_alignment_recon_dossier.md` |
| **MC-5** | Re-exercise VideoForge → CanvasForge against v0.2; capture deltas; flip proposal status — BASELINE scope per MC-4.5 §4 (D1=A default) executed verbatim; documentation-grade validation by inspection; zero regression confirmed; 4 enumerable v0.2-vs-v0.1.0 retrofit deltas documented; 5/5 substrate-impl sample records schema-conformant; see `what/artifacts/mc5_validation_videoforge_canvasforge_v0_2.md` | 0.5 | ✅ **COMPLETE 2026-05-20** |

Sequence is the VideoForge proposal's "Suggested v0.2 Mission Sequencing" verbatim — Argus may reorder during execution; current order is recommendation, not contract.

## Critical Path

MC-1 (spec) → MC-2 (schema) → MC-3 (AIRLOCK.md v0.2 + reply-comment template) — these three are sequential. MC-4 (substrate enforcement) is gate-eligible but lower-bandwidth and could run parallel to MC-3. MC-5 (validation) is sequential after MC-3 + MC-4.

→ **DG-C gate** at MC-5 closure.

## Risk Register

| # | Risk | Sev | Mitigation |
|---|------|-----|------------|
| R1 | v0.2 schema misses a real-world pattern not yet exercised | MED | MC-5 validation against real worked example; future findings (Gap 6+) absorbed in v0.3 cycle |
| R2 | Consumer wrappers (Campaign B) ship before v0.2 → out-of-date AIRLOCK.md sections in consumer wrappers | LOW | Campaign B wrappers point at AIRLOCK.md by version pin; AIRLOCK.md bump triggers minor-bump review per ADR-002 §3 |
| R3 | Substrate-enforcement spec (MC-4) lacks an executable runtime to validate | MED | Spec is documentation-grade in v0.2; first executable enforcement lands in a future Platform.aDNA (e.g., RareHarness) integration |
| R4 | Schema bloat — request payload too heavyweight for trivial 2-forge requests | MED | YAML schema marks Gap 2/4/5 fields as optional; required minimum is Gap 3 core (type + spec_path + output_sink); MC-1 spec calls out lightweight vs full profiles |

## Phase Plan

| Phase | Name | Missions | Status |
|-------|------|----------|--------|
| **P1** | Standard authoring | MC-1, MC-2 | ✅ **COMPLETE 2026-05-08** — MC-1 ✅; MC-2 ✅ |
| **P2** | Reference implementation + substrate | MC-3, MC-4 | ✅ **COMPLETE 2026-05-13** (disk) / **2026-05-20** (git) — MC-3 ✅ 2026-05-10 (AIRLOCK.md v0.2.0 + reply-comment template + `v0.2.0` git tag); MC-4 ✅ 2026-05-13 disk / 2026-05-20 git retroactive (`iii_airlock_substrate_implementation.md` 490 lines + spec patches + AIRLOCK.md line-258 patch; no tag bump) |
| **P2.5** | Alignment recon (interstitial; pre-P3) | MC-4.5 | ✅ **COMPLETE 2026-05-20** — planning-class single-session; MC-5 scope = BASELINE recommended; Campaign D candidates surfaced; mirrors aDNA.aDNA M2.3.5 |
| **P3** | Validation + ship | MC-5 (BASELINE), DG-C gate | ✅ **COMPLETE 2026-05-20** — MC-5 ✅ (`what/artifacts/mc5_validation_videoforge_canvasforge_v0_2.md`; zero regression confirmed; 16/16 §5.1 + 5/5 substrate-impl sample records); DG-C gate ✅ (9/9 scorecard green; AAR populated inline; charter status: completed); `v0.2.0` commit `246124d` / tag object `5cd210e` remain the production pin (no v0.2.1 cut per additive-disposition precedent) |

## What v0.1.0 → v0.2 Promises

The MA-4 AIRLOCK.md v0.1.0 anti-regression section ("What v0.1.0 does NOT cover") makes three commitments to v0.2 that this campaign must honor:

1. **Cross-vault request patterns will land in v0.2 / Campaign C MC-1.** ✅ **DELIVERED 2026-05-08** — `what/artifacts/iii_airlock_standard_spec.md` v0.2.0 §4 codifies cross-vault request schema (initiation, handshake, payload, secrets, idempotency) absorbing all 5 VF gaps.
2. **Until v0.2 ships, cross-vault requests use the coord-memo fallback** with the canonical worked example pointed at the CanvasForge memo. ✅ **RETIRED 2026-05-10** — `how/airlock/AIRLOCK.md` v0.2.0 routes cross-vault requests via § Cross-Vault Requests; the spec §4 contract is now operational at the reference instance. The CanvasForge worked example remains the canonical evidence (additive-deltas conformance per spec §5.2).
3. **The VideoForge proposal will be cited and acknowledged in v0.2 spec authoring.** ✅ **DELIVERED 2026-05-08** — spec §8.2 + §8.3 acknowledge proposal in full; all 5 gaps absorbed (no gap deferred); proposal status flipped `open → accepted` at MC-1 close, `accepted → absorbed` at MC-3 close 2026-05-10.

## AAR
<!-- Template: how/templates/template_aar_lightweight.md — campaign-level (multi-line per field per Campaign A + Campaign B precedent; mission-level AARs stay one-line) -->

- **Worked**: VideoForge's inbound proposal (5 specific gaps from a real cross-forge commission) made Campaign C's contract surface concrete — every spec section maps to a worked-example fragment that justifies its existence; no speculative API. The 5-mission sequence (MC-1 spec / MC-2 schema / MC-3 reference instance + reply template / MC-4 substrate implementation / MC-5 validation) followed the inbound proposal's "Suggested v0.2 Mission Sequencing" verbatim, validating the contributor's structural intuition. Substrate-implementation §2.5 + §3.6 sample records pre-coordinated with MC-5's dimensions paid off cleanly — MC-5 validated dimensional coherence by inspection without any retrofit; the per-MC2/MC3 separate-artifact precedent extended cleanly to MC-4's single-artifact-for-both-gaps disposition. Pre-flight grep at MC-5 close (discharging Campaign B AAR Change #1) caught zero charter-hygiene drifts — the process improvement DG-B committed to applying at DG-C worked first-try. MC-4.5 interstitial (planning-class single-session mirroring aDNA.aDNA M2.3.5) was the right move when the post-MC-4 ecosystem grew faster than original Campaign C scope anticipated (9 AIRLOCK.md stubs vs Phase-1's 5-stub estimate; LL.aDNA Phase 3+4 + LN.aDNA pc_01 Phase A federation substrate landed concurrently) — without MC-4.5 we'd have either expanded MC-5 scope (Campaign C scope creep) or shipped MC-5 with stale landscape context (audit clarity hit). Single-dossier authoring at MC-4.5 (vs multi-artifact fragmentation) matched III's actual minimal-artifact convention. MC-5 BASELINE scope held: 0.75 sess budget, zero foreign-vault mutations, additive-disposition tag policy preserved (no v0.2.1 cut — matches the 5-of-last-6 closure precedent MB-6/MB-7/MB-8/MC-4/DG-B).
- **Didn't**: MC-4 closure-protocol drift — substrate-implementation work landed cleanly on disk 2026-05-13 but the close ceremony was skipped entirely (no Execution log populated, no SITREP, no `status: active → completed` flip, no `ended/closed_at`, no `active/→history/` move, no commit, no STATE.md refresh at the time). 7-day disk-vs-git drift accumulated before MC-4.5 precondition check exposed it on 2026-05-20T20:31Z. Cleanup forced a 2-commit pattern (MC-4 retroactive close `c8ce621` + MC-4.5 session close `8235837`) and added ~20-30 kT unplanned content-load to the MC-4.5 session. The closure-ceremony steps were listed in `how/sessions/AGENTS.md` as protocol but were not enforced anywhere — when work feels "done" at the last edit, the metadata + git + audit-trail propagation is exactly what's easiest to skip. Secondary observation: the inbound-proposal Status Log on `coord_2026_05_08_airlock_v0_2_videoforge_findings.md` reserved line 207 (`closed | Reserved for MC-5 validation`) at MC-3 close 2026-05-10 — that reservation hung for 10 days; not a bug (it fired correctly at MC-5 close) but a sign that reservation-line patterns work best with short fuse times.
- **Finding**: The airlock standard v0.2 is shape-stable across substantial growth in the surrounding ecosystem. Between MC-3 (2026-05-10) and DG-C (2026-05-20), 4 new vaults emerged (SpeechForge.aDNA + MoleculeForge.aDNA + LatticeAgent.aDNA + LatticeTerminal.aDNA — 9 AIRLOCK.md stubs now vs 5 at MC-3) and the federation substrate (LatticeNetwork.aDNA pc_01 Phase A Ed25519 sigs + ledger emit + admission ceremony + Phase B1 dual-substrate Tailscale+Nebula pluralism via ADR-014/-015) landed LIVE — none of this required v0.2 contract amendment. The `secrets_handled` env-presence check + `idempotency_key` archive-search dedup are **transport-agnostic application-layer concerns**; cryptographic identity binding + ledger observability + multi-substrate-network-identity awareness are Campaign D D1 territory (federation-aware airlock v0.3+). Spec R3 boundary (documentation-grade) held — first executable enforcement legitimately defers to a future Platform.aDNA integration. Second finding (load-bearing for cross-vault discipline): **session-file closure protocol drift is a generalizable risk** for any aDNA vault; mitigation is mechanical (7-step close ceremony codified as a skill) and the canonical exemplar is MC-4.5's own close execution.
- **Change**: (a) **DG-closure pre-flight sub-step** — discharge Campaign B AAR Change #1 at every future DG close: grep over campaign master for `^- \[ \]` (unflipped boxes) + `(pending)` non-Status-Log occurrences + frontmatter/body status consistency BEFORE the human authors the AAR. Validated working at MC-5 close (caught zero drifts first-try). (b) **Session-close-ceremony skill candidate** filed at `how/backlog/idea_skill_session_close_ceremony.md` as DG-C carry-forward — author at Campaign D planning or as a standalone post-DG-C interstitial. (c) **MC-4.5 interstitial pattern adoption** — planning-class single-session shape (3rd canonical instance after aDNA.aDNA M2.2 + M2.3.5) confirmed as the right move for "ecosystem grew faster than original scope anticipated" situations; future campaigns may use the pattern proactively at phase transitions when external landscape signals accumulate. (d) **Sample-record pre-coordination convention** validated at MC-5 — when a documentation-grade implementation artifact ships samples, dimensions should be pre-coordinated with the downstream validation mission's anticipated scope; saves retrofit and proves dimensional coherence by construction.
- **Follow-up**: (1) `skill_session_close_ceremony.md` authoring per `how/backlog/idea_skill_session_close_ceremony.md` — codify the 7-step close protocol (Execution log + SITREP + status flip + ended/closed_at + history move + commit + STATE.md refresh); candidate Campaign D entry or interstitial. (2) `lattice-labs/iii/` v0.1.0 → v0.2.0 minor-bump review per ADR-002 §3 (only consumer wrapper not at v0.2.0; operator-discretionary timing). (3) VFL-001 + VFL-002 graduation ceremony per `who/coordination/coord_2026_05_12_vfl_graduation_proposals.md` (status `open` 8 days; Argus + Stanley co-ratification pending). (4) 2 cross-vault coord memo drafts at `who/coordination/coord_2026_05_20_iii_to_ll_airlock_status_post_mc4_5.md` + `coord_2026_05_20_iii_to_ln_federation_substrate_intersect.md` — hold at `status: draft` until Campaign D planning per operator decision; fire as single coherent signal at Campaign D charter ratification. (5) 5 TEMPLATE-DH-B promotion-candidate assessment from LL.aDNA Phase 4 S22 (deferred at MC-4.5; doctrine substrate growing; revisit at Campaign D scoping). (6) 3 Campaign D candidates sketched at MC-4.5 §5.2: D1 federation-aware airlock v0.3 (LOAD-BEARING from LN.aDNA pc_01 Phase A federation substrate + Phase B1 dual-substrate ratification ADR-014/-015) + D2 RLHF + adaptive-improvement loop (operator's "modular agentic + adaptive + RLHF + improvement" framing) + D3 optional AIRLOCK activation kit (from 9-stub ecosystem discovery). Stanley signals on Campaign D charter.

## Cross-References

- Inbound proposal: `~/aDNA/III.aDNA/who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md`
- v0.1.0 reference implementation: `~/aDNA/III.aDNA/how/airlock/AIRLOCK.md`
- Worked example (cross-forge request): `~/aDNA/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md`
- Predecessor campaign: `~/aDNA/III.aDNA/how/campaigns/campaign_a_iii_genesis/campaign_a_iii_genesis.md`
- Sibling campaign (parallel-eligible): `~/aDNA/III.aDNA/how/campaigns/campaign_b_iii_federation/campaign_b_iii_federation.md`
- VideoForge ADR 003 (cross-graph entry contract; pattern source): `~/aDNA/VideoForge.aDNA/what/decisions/adr_003_cross_graph_entry_contract.md`
- VideoForge ADR 005 (RLHF channel; pattern source for `proposed/` mechanic): `~/aDNA/VideoForge.aDNA/what/decisions/adr_005_context_learning_rlhf.md`
- Forge canonical spec (entry-time pattern, federation-bound): `~/aDNA/SiteForge.aDNA/what/artifacts/sf_forge_pattern_spec.md`
