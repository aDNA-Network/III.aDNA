---
plan_id: plan_literatureforge_genesis_planning
type: plan
title: "MD-X2 — LiteratureForge.aDNA Genesis-Planning Mission (research + genesis-charter authoring)"
owner: stanley
status: completed
executed_at_session: session_stanley_20260525_iii_adna_md_x2_literatureforge_genesis_execution
closed: 2026-05-25
outcome: "EXECUTED through fork + genesis Phase 0 per operator decision. Research dossier + genesis charter draft + cross-vault architecture note authored; LiteratureForge.aDNA forked (commit dede876, branch main, persona Thoth); Operation Scriptorium genesis campaign live (P0 closed; P1 Resume-Here)."
mission_class: genesis_planning_research
campaign: campaign_d_federation_adaptive_loop
mission: MD-X2
disposition: standalone_interstitial   # mirrors MD-X1; does NOT extend the DG-D gate (stays 11-criterion)
created: 2026-05-25
updated: 2026-05-25
last_edited_by: agent_argus
authored_at_session: session_stanley_20260525_iii_adna_md_x2_literatureforge_genesis_planning_authoring
sequencing: "executed NEXT in Campaign D, before MD-B5; MD-B5 LPWhitepaper-as-pilot scope held/reframed pending this mission's architecture output"
produces_for_vault: LiteratureForge.aDNA   # the genesis-campaign charter draft is carried INTO the new vault at fork
forward_seeds: "III.aDNA Campaign E — generalized writing-III (builds the layer LiteratureForge consumes; re-points LPWhitepaper federation_ref)"
tags: [plan, md_x2, standalone_interstitial, literatureforge, genesis_planning, forge_aDNA, agentic_writing, human_agent_in_the_loop, rlhf_content_generation, cross_vault_architecture, planned]
---

# MD-X2 — LiteratureForge.aDNA Genesis-Planning Mission

> **Status note**: This is a *planning-mission card* authored at session `session_stanley_20260525_..._md_x2_...` (2026-05-25). It scopes what the **future executing session** will research and produce. It is NOT executed yet. It does NOT fork `LiteratureForge.aDNA`. Per operator decision it is a **standalone-interstitial** mission in Campaign D (mirrors MD-X1) — it does **not** extend the DG-D gate — and it runs **next, before MD-B5**.

## §1 Context & Why

The operator intends to **evolve the LPWhitepaper.aDNA writing system into a general-purpose `LiteratureForge.aDNA`** — an aDNA-native, agentic, context-graph-driven system for composing high-quality / high-spec writing across many document genres. The whitepaper is no longer the end; it becomes the **pilot / case study** for a reusable writing forge.

This reshapes a three-vault relationship:

- **`LiteratureForge.aDNA`** (NEW — **Forge.aDNA** category): the writing-composition forge. Produces written artifacts for consumer vaults via the standard `<forge>/` wrapper + `federation_ref` pull pattern. Owns the genre submodules, the multi-step generation pipeline, the review/improvement loop, and per-genre format specs.
- **`III.aDNA`** (EXISTING — **Framework.aDNA**): provides the quality-improvement layer. A **generalized "writing III"** (future III.aDNA Campaign E) supersedes LPWhitepaper's bespoke `whitepaper_communication`-centric III usage — a domain-general writing-quality loop LiteratureForge consumes.
- **`LPWhitepaper.aDNA`** (EXISTING): the **pilot**. Its `whitepaper_communication` 9-trap pack + 25-cycle `campaign_iii_deep_review` are the seed corpus for the first LiteratureForge genre submodule (whitepaper genre) and for generalizing the writing-III.

**Why this precedes MD-B5**: MD-B5 (≥3-vault validation) names LPWhitepaper as the natural Day-1 target. But LPWhitepaper's III relationship is about to change (it becomes a LiteratureForge consumer). Reframing MD-B5's LPWhitepaper scope pending this mission's architecture output avoids validating a configuration we're about to migrate.

## §2 LiteratureForge.aDNA — Vision (the system MD-X2 plans to charter)

A **modular, agentic, multi-step writing-composition forge**. Intended to write a broad range of genres: blog posts, articles, research papers, books, short stories, white papers, executive summaries, grant proposals, and more.

**Genre submodule pattern** — each writing genre (or sub-genre) is a composable **writing submodule** carrying the genre-specific knowledge needed to drive high-quality output. Example: a **BioML-writing submodule** that writes about biological machine learning using the right register and terminology, vets facts/research/citations, and runs persona-driven review/improvement cycles.

**Submodule-creation lifecycle** (the mission must design this):
1. **Ingest + interact** — gather writing samples for the genre; interview the user; ingest reference context/sources.
2. **Synthesize the genre context graph** — distill the writing-quality dimensions, language/register patterns, fact-discipline + knowledge patterns, and **format spec** (e.g., a grant must be 3–5 pages with named sections) that this genre requires.
3. **Multi-step generation pipeline** — modular stages from brief → draft → revision, conditioned on the genre context graph.
4. **Human + agent-in-the-loop review/improvement cycles** — persona-driven critique + scoring + rework routing, surfacing feedback for both human and agent improvement loops (the III generalization).
5. **Format-bound output** — output conforms to the genre's format spec.

**The whitepaper genre** is the worked first instance (pilot): LPWhitepaper's accumulated review knowledge becomes the whitepaper submodule's context graph.

## §3 Objectives (what the executing session produces)

- **O1 — External SOTA research**: best practices for agentic, modular, human+agent-in-the-loop text generation / adjustment / review / improvement. Cover: RLHF-style content-improvement loops; multi-agent critique/debate for writing; retrieval-grounded fact-checking + citation vetting; genre/format-conditioned generation; iterative human-feedback rework routing. Produce a concise findings dossier with citations.
- **O2 — Internal lattice pattern-mine** (reuse before invent): extract the reusable mechanisms from prior agentic modular-pipeline RLHF content work in this ecosystem —
  - **SpeechForge.aDNA** — 8-stage Greene-methodology pipeline + 3 human gates + rework discipline + Voice-Bible compliance scoring (the *closest staged-writing-pipeline analog*).
  - **CanvasForge.aDNA** — VR1–VR5 voice-register protocol + multi-voice review (`skill_canvas_iii_review.md`, 5 voices) + courier-check-before-delivery + character invariance.
  - **VideoForge.aDNA** — multi-voice III ship-gate (≥0.75 score; VCRIT + quality + platform + brand voices) + `bridge_pack` federation pattern.
  - **III.aDNA adaptive-improvement loop** — `what/artifacts/iii_adaptive_improvement_loop_spec.md` (v0.3, INSPECT→INTROSPECT→IMPROVE→HUMAN_REVIEW→ACCUMULATE) + ADR-005 (RLHF signal channel + `rlhf_consumer_namespace` boundary) + ADR-007 (six-axis quality rubric + CorrectionLifecycle 8-state machine) + the 8 composable modules at `what/modules/`.
- **O3 — LPWhitepaper pilot-seed mine**: extract the writing-quality knowledge already encoded — the `whitepaper_communication` pack (9 traps across 3 dimensions: communication clarity / visual design / IP rigor; 5 cross-trap escalation rules) + the 25-cycle `campaign_iii_deep_review` human/agent review mechanism + any style/format/structural templates. This is the seed for the whitepaper genre submodule and for the writing-III generalization.
- **O4 — LiteratureForge.aDNA genesis-campaign charter draft**: a full **P0–P5 genesis-planning arc** per the canonical Forge.aDNA genesis pattern (P0 charter+persona+scope-lock → P1 source inventory → P2 pattern extraction → P3 SOTA research + design specs → P4 architecture ADRs → P5 execution-campaign charter), with ~20–30 mission stubs. Authored so it can be **carried into `LiteratureForge.aDNA` at fork** and executed from inside that vault.
- **O5 — Cross-vault architecture note**: the LiteratureForge ⇄ generalized-writing-III ⇄ LPWhitepaper-pilot topology. Federation_ref + graft direction; how LiteratureForge consumes the writing-III; what "replace the III layer on LPWhitepaper" concretely migrates (generalize `whitepaper_communication` → a LiteratureForge-owned writing pack; re-point LPWhitepaper's `iii/CLAUDE.md` `federation_ref`; preserve the in-flight deep-review campaign during transition). This note is a **proposal**, not a ratified ADR (ADRs land in the genesis campaign + the future III Campaign E).
- **O6 — Forward-seed the III.aDNA "general writing III" campaign** (future **Campaign E**): scope the III.aDNA work that, *after* LiteratureForge is built, generalizes the writing-quality loop and re-points LPWhitepaper. A stub/charter outline only — not chartered at MD-X2.

## §4 Method (for the executing session)

Phased, with a parallel research opening:

- **Phase A — Research (parallel)**: (A1) external SOTA dossier per O1; (A2) internal pattern-mine per O2 — read the SpeechForge/CanvasForge/VideoForge CLAUDE.md + review skills + the III adaptive-loop spec + ADR-005/-007 + the 8 modules; (A3) LPWhitepaper seed-mine per O3.
- **Phase B — Synthesis**: design (B1) the genre-submodule architecture + the 5-step submodule-creation lifecycle; (B2) the multi-step generation pipeline-stage model (reusing SpeechForge stage-modularity + VideoForge artifact-per-stage); (B3) the human+agent-in-the-loop review/improvement-cycle model (reusing CanvasForge multi-voice + VideoForge ship-gate + III adaptive loop); (B4) the per-genre format-spec contract.
- **Phase C — Charter authoring**: produce O4 (genesis charter draft) + O5 (cross-vault note) + O6 (Campaign E seed) + the MD-B5 reframe note.

The executing session SHOULD use Explore/Plan subagents for the research phases and SHOULD reach operator alignment (persona, genre taxonomy) via AskUserQuestion before finalizing the charter draft.

## §5 Deliverables

1. Research dossier (external SOTA + internal pattern-mine + LPWhitepaper seed) — `what/artifacts/md_x2_literatureforge_research_dossier.md` (or similar).
2. **LiteratureForge.aDNA genesis-campaign charter draft** (P0–P5; ~20–30 mission stubs) — the primary output; carried into the vault at fork.
3. Cross-vault architecture note (LiteratureForge ⇄ writing-III ⇄ LPWhitepaper-pilot).
4. Genre-submodule spec sketch + the 5-step submodule-creation lifecycle + a worked example (whitepaper genre, and/or the BioML-writing genre).
5. MD-B5 reframe note (how LPWhitepaper-as-pilot changes MD-B5's ≥3-vault validation scope).
6. Campaign E seed (future III.aDNA generalized-writing-III campaign outline).

## §6 Out of Scope (for MD-X2)

- **No fork of `LiteratureForge.aDNA`** at MD-X2. The fork (via `.adna/how/skills/skill_project_fork.md`) + the genesis-campaign **execution** run from *inside* the new vault, **after** MD-X2 and after operator approval.
- **No code**, no artifact generation, no genesis ratification.
- **No III ADR authored by MD-X2** — the cross-vault note (O5) is a proposal; ADRs are ratified in the LiteratureForge genesis campaign and the future III Campaign E.
- **No consumer-wrapper / learning-store / lattice-yaml changes.**
- **No MD-B5 / MD-B6 work** (those remain queued; MD-B5 scope reframed pending O5).

## §7 Reuse Map (read these; cite in the charter draft)

- Forge genesis: `aDNA.aDNA/what/specs/spec_forge_ecosystem.md`; `.adna/how/skills/skill_project_fork.md`; the canonical P0–P5 genesis-planning arc (precedent: `MoleculeForge.aDNA/how/campaigns/campaign_moleculeforge_genesis*/`, `ContextCompass.aDNA/how/campaigns/campaign_contextcompass_genesis_planning/`).
- Agentic writing/review-loop patterns: `SpeechForge.aDNA/CLAUDE.md` (8-stage + 3 gates + Voice-Bible — closest analog); `CanvasForge.aDNA/CLAUDE.md` + `CanvasForge.aDNA/how/skills/skill_canvas_iii_review.md` (VR1–VR5 + 5-voice review); `VideoForge.aDNA/CLAUDE.md` (≥0.75 multi-voice III ship-gate + bridge_pack).
- III adaptive-loop substrate (what the writing-III generalizes): `what/artifacts/iii_adaptive_improvement_loop_spec.md`; `what/decisions/adr_005_rlhf_signal_channel.md`; `what/decisions/adr_007_adaptive_improvement_loop.md`; `what/modules/module_iii_*.md` (8 modules).
- LPWhitepaper pilot seed: `LPWhitepaper.aDNA/iii/CLAUDE.md` (6/7-pack wrapper); `what/context/core_domain_packs/context_iii_whitepaper_communication.md` (9-trap / 3-dimension pack); LPWhitepaper's `campaign_iii_deep_review` (25-cycle) + `campaign_sharing_prep`.
- Mission/close conventions: `how/templates/template_mission.md`; `how/skills/skill_session_close_ceremony.md`.

## §8 Exit Gate

MD-X2 (when executed) closes when: the LiteratureForge genesis-campaign charter draft is complete + the cross-vault architecture note is complete + the operator approves forking `LiteratureForge.aDNA`. At that point the operator forks the vault and the genesis campaign executes from inside it; III.aDNA returns later for the generalized-writing-III work (Campaign E).

## §9 Open Questions for the Executing Session

1. **Persona** for LiteratureForge.aDNA (decided at the new vault's P0 — e.g., a writing/rhetoric archetype). Not picked now.
2. **Genre taxonomy lock** — which genres are Day-1 submodules vs deferred; the sub-genre granularity (e.g., is "BioML paper" a sub-genre of "research paper" or its own submodule?).
3. **Where the genesis campaign physically lives** — confirmed inside `LiteratureForge.aDNA` after fork; MD-X2 produces the draft that is carried in.
4. **Transition consumption model** — does LPWhitepaper consume LiteratureForge-only, or both LiteratureForge + III.aDNA, during the migration window? (Federation is composable per airlock spec §6.)
5. **Writing-III boundary** — what stays in III.aDNA canonical (general review-loop machinery) vs what becomes a LiteratureForge-owned writing pack (genre traps + format specs).

## Notes

Authored as a standalone-interstitial planning mission per operator decision (2026-05-25). Non-gating for DG-D (stays 11-criterion / 9 satisfied). The MD-X1 carve-out is the structural precedent.

## AAR

- **Worked**: Three parallel research streams (external SOTA via WebSearch; internal Explore pattern-mine of SpeechForge/CanvasForge/VideoForge/III; LPWhitepaper Explore seed-mine) gave a complete, grounded dossier in one pass. The operator-locked decisions (persona Thoth; 5 Day-1 genres; hybrid+promotion sub-genre model) let the charter + ADR-000 be authored concretely. SpeechForge/MoleculeForge were clean genesis structure templates.
- **Didn't**: Nothing blocking. Per operator decision the scope extended *beyond* the mission card's §6 ("no fork at MD-X2") — the operator explicitly chose "through fork + Phase 0", so the fork + genesis Phase 0 were executed in the same effort rather than deferred to a separate gate. Documented here for audit.
- **Finding**: The whitepaper submodule's three LPWhitepaper seed artifacts (whitepaper_communication pack + 25-cycle deep-review + LaTeX format spec) map 1:1 to the genre-submodule 4-part bundle `{trap-pack + reviewer-voice-set + format-spec + reward-rubric}` — the design is grounded in a real corpus, not invented. The writing-III boundary (D6) cleanly resolves mission §9 Q5.
- **Change**: O4's "P0–P5 genesis-planning arc" was realized as a 6-phase Operation Scriptorium charter; P5 produces a separate forge-BUILD execution campaign (*Operation Codex*), co-dependent with III.aDNA Campaign E.
- **Follow-up**: III.aDNA returns for **Campaign E** (generalized writing-III + LPWhitepaper migration); **MD-B5** (≥3-vault validation) reframed to use the 3 stable v0.3 consumers, deferring LPWhitepaper to its post-migration chain (cross-vault note §5); then **MD-B6** DG-D gate + tag. DG-D unchanged 9/11 (MD-X2 non-gating).

## Deliverables produced (execution)

- `III.aDNA/what/artifacts/md_x2_literatureforge_research_dossier.md` (D1)
- `III.aDNA/what/artifacts/md_x2_literatureforge_genesis_charter_draft.md` (D2 + D4 embedded Design Foundation)
- `III.aDNA/what/artifacts/md_x2_cross_vault_architecture_note.md` (D3 + D5 MD-B5 reframe §5 + D6 Campaign E seed §6)
- `LiteratureForge.aDNA/` forked (commit `dede876`, branch `main`) — genesis Phase 0 closed; Operation Scriptorium live; 3 MD-X2 artifacts carried in.
