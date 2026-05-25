---
artifact_class: genesis_campaign_charter_draft
type: artifact
title: "MD-X2 deliverable — LiteratureForge.aDNA genesis-campaign charter DRAFT (Operation Scriptorium; P0–P5; carried into the vault at fork)"
mission: MD-X2
campaign: campaign_d_federation_adaptive_loop
produces_for_vault: LiteratureForge.aDNA
destined_path: "LiteratureForge.aDNA/how/campaigns/campaign_literatureforge_genesis/campaign_literatureforge_genesis.md"
created: 2026-05-25
updated: 2026-05-25
last_edited_by: agent_argus
status: draft
operator_decisions:
  persona: thoth
  category: forge
  day1_genres: [whitepaper_pilot, research_paper, grant_proposal, blog_article, executive_summary]
  sub_genre_model: "hybrid + promotion rule"
tags: [artifact, charter_draft, md_x2, literatureforge, operation_scriptorium, thoth, forge_aDNA, genesis_planning]
---

# DRAFT — Operation Scriptorium (LiteratureForge.aDNA genesis-planning campaign)

> **Draft status**: authored at III.aDNA MD-X2 as a deliverable, to be **carried into `LiteratureForge.aDNA` at fork** as `how/campaigns/campaign_literatureforge_genesis/campaign_literatureforge_genesis.md` (frontmatter + relative paths re-homed to vault-native at fork). Persona (Thoth), category (Forge.aDNA), Day-1 genre taxonomy, and sub-genre model are operator-locked (2026-05-25). The research that feeds P1–P3 is substantially pre-completed at `III.aDNA/what/artifacts/md_x2_literatureforge_research_dossier.md` (carried in alongside).

## Codename & persona

**Operation Scriptorium** — the scriptorium is where scribes both *compose* and *preserve* the written word. **Persona: Thoth** — ibis-headed Egyptian god of writing, scribes, wisdom, and measure; recorder in the Hall of Ma'at who weighs every heart against the feather of truth. Doctrine: *the scribe who both composes and judges; every word weighed against Ma'at (truth/balance); records that endure.* This binds generation (compose) + review/fact-discipline (weigh against truth) + format-bound durable output (records that endure) into one identity.

## WHO / WHAT / HOW

- **WHO** — Thoth (this vault). Coordinates with: III.aDNA (Argus — quality-improvement substrate; LiteratureForge consumes a generalized writing-III via `federation_ref`); LPWhitepaper.aDNA (pilot / first consumer + whitepaper-submodule seed corpus); aDNA.aDNA (standard owner courtesy); SiteForge.aDNA (canonical Forge.aDNA pattern source); SpeechForge.aDNA + CanvasForge.aDNA + VideoForge.aDNA (peer Forges — staged-pipeline + multi-voice-review pattern donors); ScienceStanley.aDNA (plausible Day-1 consumer — `literatureforge/` wrapper for brand content genres).
- **WHAT** — a standalone **Forge.aDNA** that composes high-quality written artifacts across many genres via composable **genre submodules**, with a multi-step generation pipeline + human+agent-in-the-loop review/improvement cycles. Owns: genre submodules, the generation pipeline, per-genre format specs, and the genre-specific writing packs. Does **not** own the review-loop machinery or learning store — those are consumed from III.
- **HOW** — a 6-phase (P0–P5) dependency-ordered **genesis-planning** campaign, human-gated. P0 (Setup) is CLOSED at fork. P1–P5 are seeded as `planned` stubs; entry is operator-discretionary. P5 produces the **execution-campaign charter** (the campaign that actually builds the forge).

## Goal

When Operation Scriptorium completes, **LiteratureForge.aDNA holds a ratified design + an execution-campaign charter** for an agentic writing-composition forge: the genre-submodule contract (`{trap-pack + reviewer-voice-set + format-spec + reward-rubric}`) is specified; the 5-step submodule-creation lifecycle is documented; the multi-step generation pipeline-stage model + the multi-voice scored ship-gate review model + the atomic-claim fact-discipline stage are designed; the writing-III federation boundary with III.aDNA is defined; the whitepaper submodule (seeded from LPWhitepaper) is the worked first instance; the 5 Day-1 genre submodules (whitepaper, research paper, grant proposal, blog/article, executive summary) have spec sketches; and the execution campaign is chartered and ready for operator gate.

## Context

LiteratureForge is the generalization of LPWhitepaper.aDNA's bespoke writing system into a reusable Forge.aDNA. The whitepaper stops being the end and becomes the **pilot / case study**. Pre-research (external SOTA + internal lattice pattern-mine + LPWhitepaper seed) was completed at III.aDNA MD-X2 (carried-in dossier). This vault was forked directly from `.adna/` (not via cascading-genesis); III.aDNA returns later for the generalized-writing-III work (Campaign E — see the carried-in cross-vault architecture note).

## Design Foundation (the genre-submodule architecture this campaign ratifies)

> Satisfies MD-X2 deliverable 4 (genre-submodule spec sketch + 5-step lifecycle + worked example). Distilled from the research dossier §5 directives D1–D8.

### A genre submodule = a 4-part bundle
A **genre submodule** carries the genre-specific knowledge to drive high-quality output, as four composable parts:
1. **Trap-pack** — genre quality traps (mirrors an III domain pack). *Whitepaper seed:* the 9-trap / 3-dimension `whitepaper_communication` pack.
2. **Reviewer-voice-set** — the reader personas that score a draft. *Whitepaper seed:* F-TECH / F-INV / F-PAT / F-VIS.
3. **Format-spec** — the format-bound output contract (structure, register, length, named sections, asset conventions). *Whitepaper seed:* LaTeX/TikZ + `lattice-styles.tex` + `mathematical_language_standards.md` + glossary/appendix structure.
4. **Reward-rubric** — the named scorable axes + ship threshold for this genre (default composite ≥0.75, genre-tunable; optional per-voice floors).

### The 5-step submodule-creation lifecycle
1. **Ingest + interview** — gather genre writing samples; interview the user; ingest reference sources.
2. **Synthesize the genre context graph** — distill quality dimensions, register patterns, fact-discipline norms, and the format spec.
3. **Multi-step generation pipeline** — brief → **outline/plan (mandatory, inspectable)** → draft → revision, conditioned on the genre graph (STORM/Re3 plan-then-write).
4. **Human + agent-in-the-loop review/improvement cycles** — multi-voice scoring + weakest-first bounded rework routing + RLHF signal capture (the III generalization).
5. **Format-bound output** — output conforms to the genre format spec.

### Sub-genre model (operator-locked: hybrid + promotion rule)
A specialized variant (e.g. a **BioML research paper**) is by default **parent submodule + a domain overlay** (register/terminology/citation-norm deltas). It is **promoted to its own first-class submodule** only when divergence is large. **Promotion criterion (to be ratified at P4):** promote when the overlay would override ≥X% of the parent's trap-pack/format-spec OR introduces a distinct reviewer-voice-set — mirroring III's local-pack-graduates-to-canonical discipline (frequency/divergence-gated, not ad hoc).

### Generation pipeline + review model (reused, not invented)
- **Pipeline** (P2/P3): staged, structure-first, artifact-per-stage — SpeechForge stage-modularity + STORM mandatory plan stage + VideoForge checkpointed artifacts.
- **Review** (P2/P3): multi-voice scored ship-gate — CanvasForge median-of-N + VideoForge per-voice floors + III INSPECT→INTROSPECT→IMPROVE→HUMAN_REVIEW→ACCUMULATE. Structure-validation gate early, quality-validation gate late; humans at decision gates only.
- **Fact-discipline** (P2/P3): atomic-claim decomposition → retrieval → agreement-gate → edit-only-unsupported (FActScore/RARR), surfaced as a scored, genre-tunable axis.

## Scope

### In scope
- Genesis of LiteratureForge.aDNA structure + identity + governance + persona (P0 — CLOSED at fork by `mission_setup_00_fork_and_bootstrap`).
- P1–P5 (operator-gated): source inventory (ingest carried-in research), pattern extraction (vault-native pipeline/review/fact-discipline specs), design specs (genre-submodule contract + format-spec contract + whitepaper worked example + 4 other Day-1 genre sketches), architecture ADRs, and the execution-campaign charter.

### Out of scope (for the genesis-planning campaign)
- Building the actual forge runtime / writing any submodule code or pipeline implementation — that is the **execution campaign** (chartered at P5).
- Authoring the generalized writing-III itself — that is **III.aDNA Campaign E** (LiteratureForge consumes it; see cross-vault note).
- Re-pointing LPWhitepaper's `iii/` federation_ref or touching its in-flight deep-review campaign — migration is staged in the cross-vault note as a proposal; ratified later.
- Re-architecting the canonical Forge.aDNA pattern spec (`SiteForge.aDNA/what/artifacts/sf_forge_pattern_spec.md`) — inherit.

## Binary success criteria (campaign exits only when ALL hold)
1. ADR-000 accepted; persona_thoth ratified; CLAUDE.md / MANIFEST / STATE / CHANGELOG at current `.adna/` standard — ✅ at P0 close.
2. Operation Scriptorium charter committed; `mission_setup_00` closed; P0 ✅ — ✅ at P0 close.
3. P1 source inventory ingests the carried-in MD-X2 dossier + LPWhitepaper seed as the vault's source-of-record.
4. P2 pattern-extraction specs (pipeline-stage model + review-loop model + fact-discipline model) are vault-native + binding.
5. P3 genre-submodule contract + 5-step lifecycle + per-genre format-spec contract + whitepaper worked example + 4 Day-1 genre sketches authored.
6. P4 architecture ADRs ratified (genre-submodule architecture; generation-pipeline architecture; writing-III federation contract; format-spec contract; sub-genre hybrid+promotion model).
7. P5 execution-campaign charter authored + Day-1 consumer-wrapper plan (LPWhitepaper pilot + SS `literatureforge/`) drafted.
8. Cross-vault federation boundary with III.aDNA documented (writing-III consumption); coord memo to III/Argus + aDNA standard owner filed.
9. LiteratureForge.aDNA operationally self-governing; execution campaign ready for operator gate.

## Campaign abort / replan procedure
NO-GO at any phase gate → **freeze** (no further mutations), **diagnose** (root-cause memo in this campaign dir), **re-plan or abandon** (`status: abandoned`, never delete — SO-7). P0 abort is not applicable (complete at fork). P1+ abort leaves the vault P0-complete and stable.

## Phases & Missions (dependency-ordered; phase gates are human gates; ~22 missions)

| Phase | Missions | Exit gate (human) |
|-------|----------|-------------------|
| **P0 Setup** | `mission_setup_00_fork_and_bootstrap` (✅ CLOSED at fork) — fork from `.adna/`; identity quintet (MANIFEST/STATE/CLAUDE/CHANGELOG, `agent_thoth` markers); `persona_thoth` authored; ADR-000 accepted; this charter committed; MD-X2 dossier + cross-vault note carried in. | Vault forked; identity + persona + ADR-000 + charter + carried-in research landed; P1 unblocked (operator-discretion entry) |
| **P1 Source Inventory** | `p1_01_ingest_md_x2_dossier` (adopt carried-in dossier as vault source-of-record) · `p1_02_lpwhitepaper_seed_intake` (whitepaper_communication pack + deep-review mechanism + LaTeX format spec) · `p1_03_genre_corpus_gathering` (writing samples + interview protocol for the 5 Day-1 genres — lifecycle step 1) · `p1_04_synthesis` (deps p1_01..03) | Source inventory filed under `what/sources/`; P2 stubs authored |
| **P2 Pattern Extraction** | `p2_01_pipeline_stage_model` (multi-step generation pipeline; SpeechForge + STORM + VideoForge) · `p2_02_review_loop_model` (multi-voice scored ship-gate; CanvasForge + VideoForge + III adaptive loop) · `p2_03_fact_discipline_model` (atomic-claim verification; FActScore/RARR) · `p2_04_synthesis` | Vault-native pattern specs binding; P3 stubs authored |
| **P3 Design Specs** | `p3_01_genre_submodule_spec` (the 4-part bundle + 5-step lifecycle) · `p3_02_format_spec_contract` (per-genre format-bound output contract) · `p3_03_whitepaper_submodule_worked_example` (the pilot, seeded from LPWhitepaper) · `p3_04_day1_genre_specs` (research paper [+ BioML overlay] + grant + blog/article + exec summary sketches) · `p3_05_synthesis` | Design specs complete; P4 ADR stubs authored |
| **P4 Architecture ADRs** | `p4_01_adr_genre_submodule_architecture` · `p4_02_adr_generation_pipeline_architecture` · `p4_03_adr_writing_iii_federation_contract` (bridge_pack consumption of generalized writing-III; the boundary) · `p4_04_adr_format_spec_contract` · `p4_05_adr_sub_genre_model` (hybrid + promotion rule + criterion) | ADRs ratified; P5 stubs authored |
| **P5 Execution-Campaign Charter** | `p5_01_execution_campaign_charter` (charter the forge-BUILD campaign — provisional codename *Operation Codex*) · `p5_02_consumer_wrapper_plan` (LPWhitepaper pilot consumer + SS `literatureforge/`) · `p5_03_synthesis_genesis_close` (AAR; genesis close; execution campaign ready for operator gate) | Execution campaign chartered + consumer-wrapper plan drafted; genesis-planning campaign closes; operator gate to execution |

**P1 entry trigger**: LiteratureForge operator decision — not auto-advanced by fork. Plausible triggers: demand for a first real authored artifact in any Day-1 genre; III.aDNA Campaign E readiness (so the writing-III boundary can be exercised); a consumer vault (SS / LPWhitepaper) requesting generation.

## Carried-in artifacts (placed at fork)
- `what/sources/md_x2_literatureforge_research_dossier.md` (the III.aDNA MD-X2 dossier; vault source-of-record for P1).
- `what/context/md_x2_cross_vault_architecture_note.md` (LiteratureForge ⇄ writing-III ⇄ LPWhitepaper; migration proposal + MD-B5 reframe + Campaign E seed).
