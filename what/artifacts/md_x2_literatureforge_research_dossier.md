---
type: artifact
artifact_class: research_dossier
title: "MD-X2 — LiteratureForge.aDNA Research Dossier (external SOTA + internal pattern-mine + LPWhitepaper pilot-seed)"
mission: MD-X2
campaign: campaign_d_federation_adaptive_loop
created: 2026-05-25
updated: 2026-05-25
last_edited_by: agent_argus
status: complete
produces_for_vault: LiteratureForge.aDNA
tags: [artifact, research_dossier, md_x2, literatureforge, agentic_writing, rlhf, multi_voice_review, pattern_mine, lpwhitepaper_seed]
---

# MD-X2 Research Dossier — LiteratureForge.aDNA

> Consolidates O1 (external SOTA), O2 (internal lattice pattern-mine), O3 (LPWhitepaper pilot-seed) per the MD-X2 mission card §3. Feeds the LiteratureForge genesis-campaign charter draft + cross-vault architecture note. Citations in §2 were web-verified; one date-uncertainty flagged inline.

## §1 Purpose

LiteratureForge.aDNA is a planned **Forge.aDNA** that composes high-quality written artifacts across many genres (blog, article, research paper, book, short story, whitepaper, executive summary, grant proposal, …) using LLM agents with human+agent-in-the-loop review/improvement cycles. Each genre is a composable **writing submodule** carrying genre-specific quality knowledge. This dossier grounds that architecture in (a) external research, (b) reusable mechanisms already proven inside the Lattice ecosystem, and (c) the LPWhitepaper.aDNA seed corpus that becomes the first (whitepaper) submodule. **Reuse before invent** is the governing discipline.

---

## §2 External SOTA (O1)

### 2.1 RLHF-style content-improvement loops
The dominant inference-time pattern is **iterative self-critique-then-revise** (no weight updates). **Self-Refine** (Madaan et al., 2023) — generate → multi-aspect self-feedback → revise — improves human-preferred quality ~20% absolute. **Reflexion** (Shinn et al., NeurIPS 2023) stores feedback as *verbal self-reflection* in an episodic memory buffer conditioning later attempts. For preference tuning, **RLAIF / Constitutional AI** swaps human labels for model critiques against an explicit principle set; **DPO**-style optimization is increasingly fed by the model's own critique/refine pairs.
- Self-Refine — https://arxiv.org/abs/2303.17651
- Reflexion — https://arxiv.org/abs/2303.11366

### 2.2 Multi-agent critique / debate for writing
Recurring architecture: **role-based generator–critic pipelines** (writer / critic / editor / fact-checker) and **multi-agent debate (MAD)**. **Debate-to-Write** (COLING 2025) is the most writing-specific — persona-driven agents debate to build a high-level plan before drafting. The **actor-critic** pair (proposer + error-finder) is the simplest robust building block. *Caution (ICLR 2025):* MAD does not always beat a strong single agent and costs more compute — reserve debate for planning/critique where divergent perspective genuinely helps.
- Debate-to-Write — https://aclanthology.org/2025.coling-main.314.pdf
- MAD scaling challenges — https://d2jud02ci9yv69.cloudfront.net/2025-04-28-mad-159/blog/mad/

### 2.3 Retrieval-grounded fact-checking + citation vetting
Leading paradigm decomposes text into **atomic claims** verified against retrieved evidence. **FActScore** (Min et al., EMNLP 2023) scores per-atomic-fact precision. **RARR** (Gao et al., ACL 2023) is the canonical post-hoc loop: per-claim verification questions → retrieve evidence → *agreement gate* → edit only unsupported claims while preserving phrasing, emitting an attribution report. 2024–25 work shows **claim-level NLI beats sentence-level**, and generation-time grounding beats post-hoc citation attachment.
- FActScore — https://arxiv.org/abs/2305.14251 *(widely-cited; verify exact arXiv ID before print use)*
- RARR — https://arxiv.org/abs/2210.08726

### 2.4 Genre / format-conditioned generation
Consensus for long structured output is **plan-then-write / outline-then-generate**, often hierarchical. **STORM** (Shao et al., NAACL 2024) simulates multi-perspective question-asking against sources to synthesize an outline, then writes a cited article (beats outline-RAG on organization +25%, coverage +10%). **Re3** (Yang et al., EMNLP 2022) established Plan → Draft → Rewrite → Edit recursive reprompting for long stories; **DOC** and dynamic hierarchical outlining extend it with memory. Shared lesson: an explicit, inspectable **outline/plan stage** is what makes genre structure, length, and register controllable.
- STORM — https://arxiv.org/abs/2402.14207
- Re3 — https://arxiv.org/abs/2210.06774

### 2.5 Iterative human-feedback rework routing
Production systems frame revision as a **gated state machine**: a draft advances only when it clears a scored threshold; otherwise it routes back to the *specific* stage tied to the failing dimension. Per-aspect scoring (organization, faithfulness, register) routes to the right fixer rather than blanket rewriting; gates are explicit numeric thresholds with a **bounded retry budget** to prevent infinite refinement.
- Self-Refine (feedback/gate) — https://arxiv.org/abs/2303.17651
- RARR (agreement-gate routing) — https://arxiv.org/abs/2210.08726

### 2.6 Design implications (external → architecture)
1. **Mandate an explicit outline/plan stage** before drafting; each genre submodule emits an inspectable plan encoding required structure/register/length.
2. **Carry a per-genre reward rubric** — named scorable axes (coverage, faithfulness, register-fit, structural-conformance) measured against *genre*, not a global default.
3. **Revision = gated state machine**, not open-ended looping; failing axes route to a specific rework stage with a bounded retry budget.
4. **Atomic-claim fact verification** (FActScore/RARR): decompose → retrieve → agreement-gate → edit only unsupported claims; prefer generation-time grounding.
5. **Role-based agents are the default**; reserve full debate for planning/critique.
6. **Per-project verbal-reflection / correction memory** (Reflexion) persists reviewer feedback across rounds — the natural seam to a learning store (→ III).
7. **Self/AI-critique for routine gains; humans at genre-defined decision gates** (judgment, not mechanical fixes).
8. **Scores + gate decisions are first-class logged artifacts** read by the router, the human reviewer, and downstream RLHF/DPO collection.

---

## §3 Internal lattice pattern-mine (O2)

Four ecosystem systems already do staged, multi-agent, human+agent-in-the-loop content generation/review. Their reusable mechanisms:

### 3.1 SpeechForge.aDNA — closest staged-writing analog
**8-stage Greene pipeline**: 01 Raw Material → 02 Authentic Core → 03 Accordion (structure) → 04 Four Languages → 05 Instrument (voice calibration) → 06 Sixty Mirrors (60-Metrics scoring) → 07 Conversation → 08 The Forge (final package). **3 human gates**: post-03 (authentic core + structure), post-05 (voice/register fit), post-06 (composite ≥0.75 empathy + technique). **Rework discipline**: per-stage delta optimization (a single metric lift compounds); early-stage rework blocks later stages; 04–05 rework loops back without touching 01–03. Stage I/O is sequential brief→package.

### 3.2 CanvasForge.aDNA — multi-voice review + courier-check
**5 reviewer voices (VR1–VR5)**: Visual Critic, Narrative Reviewer, Accessibility Reviewer, Comic Consistency (comics only), Template-Voice Detector. **Verdict**: 3 independent passes of the 5-voice stack → median per voice → composite = mean of voices. **Ship gate**: ≥0.75 composite; 0.60–0.74 → weakest-first rework; <0.60 → hold/escalate. **Courier-check**: structure (hierarchy/flow/audience-fit) is verified *before* any visual polish — fail structure → held, no polish attempted. **Character invariance** enforced mid-pipeline before final render.

### 3.3 VideoForge.aDNA — ship-gate + federation pattern
**4 voices**: VCRIT (narrative/pacing/technical), VIDEO_QUALITY (codec/artifact), PLATFORM (platform register fit), BRAND (identity coherence). **Ship gate**: composite ≥0.75 + per-voice parity floors from the M16 baseline (VCRIT ≥0.93, QUALITY ≥0.90, PLATFORM ≥0.92, BRAND ≥0.88). **bridge_pack federation**: consumes III via `federation_ref` pinned at III.aDNA v0.3.0; local 10-trap canvas-visual bridge pack (local wins on name conflict per ADR-003 §2); writes corrections to a vault-local learning store, never canonical.

### 3.4 III.aDNA adaptive-improvement loop — the quality substrate
**5 phases**: INSPECT (≥1 of 4 modalities) → INTROSPECT (Check 2g corrections_pattern_match) → IMPROVE (severity calibration → ImprovementTable) → HUMAN_REVIEW (accept/reject/defer/accept-with-mod → captures rlhf_signal_type) → ACCUMULATE (writes consumer-local store; fires lifecycle transitions; emits graduation_signals at frequency ≥3 AND acceptance ≥80%). **Six-axis pack rubric (ADR-007 §3)**: signal_density, actionability, coverage_uniformity, source_diversity, cross_topic_coherence, graduation_yield (mean composite; any axis ≤2 forces pack revision). **CorrectionLifecycle 8-state machine**: SIGNAL_CAPTURED → CORRECTION_TRACKED → GRADUATION_CANDIDATE → GRADUATION_PROPOSED → GRADUATION_RATIFIED → PACK_DELTA_LANDED, plus terminal PRUNED / ARCHIVED. **RLHF signal channel (ADR-005)**: 3 required fields (rlhf_signal_type, rlhf_session_id, rlhf_captured_at) + 4 optional (modification_delta, reviewer_persona, severity_calibration, cross_session_link); `rlhf_consumer_namespace.<vault>.*` stays vault-local and NEVER enters canonical. **8 modules**: dispatch + inspect_{text,code,visual,data} + introspect + improve + accumulate.

### 3.5 Common patterns to generalize
1. **Staged pipeline with structure-first gating** — structure validated before polish; human gates at fixed progressive points; no auto-advance.
2. **Multi-voice / multi-axis scoring** — N independent voices → median/mean composite → **≥0.75 ship gate** + optional per-voice floors.
3. **Rework routing = weakest-first, bounded** — fix the failing axis, not the whole artifact; threshold gates the loop.
4. **Learning-store accumulation → graduation ceremony** — local writes; pull-up graduation at frequency ≥3 + acceptance ≥80%; canonical md5 rotates on graduation.
5. **Federation-wrapper discipline** — `federation_ref` pin + local bridge pack (local wins on conflict) + vault-local learning store; consumer never edits upstream.
6. **Human gates cluster at structure-validation (early) + quality-validation (late).**

---

## §4 LPWhitepaper pilot-seed (O3)

LPWhitepaper.aDNA already encodes production writing-quality knowledge — the seed for the **whitepaper submodule** and for generalizing the writing-III.

### 4.1 Genre quality knowledge — `whitepaper_communication` pack
9 traps across 3 dimensions (canonical at `III.aDNA/what/context/core_domain_packs/context_iii_whitepaper_communication.md`):
- **Communication clarity**: WP-AUDIENCE-01 (audience-register mismatch), WP-JARGON-01 (unglossaried jargon), WP-ARC-01 (narrative-arc gap).
- **Visual design**: WP-FIGTEXT-01 (figure-text inconsistency), WP-FIGLOAD-01 (overcrowded figure), WP-FIGREF-01 (orphan figure).
- **IP rigor**: WP-GCLAIM-01 (claim-strength mismatch), WP-PRIOR-01 (prior-art differentiation gap), WP-PATENT-01 (patent-language precision).
- **5 cross-trap escalation rules** (e.g. GCLAIM+PATENT → Critical; PRIOR+GCLAIM → Critical).

### 4.2 Review-cycle mechanism — `campaign_iii_deep_review` (25 cycles)
Each cycle runs DISPATCH → INSPECT (4 reader-persona voices F-TECH/F-INV/F-PAT/F-VIS in parallel) → INTROSPECT (cross-voice escalation: 2 voices same location → +1 severity; 3+ → Critical) → IMPROVE (per-finding remediation with effort estimate + change language; NOT applied directly — accumulated into a change spec) → ACCUMULATE (local learning store; graduation at frequency ≥3 across ≥2 sessions + acceptance ≥80%). 25 cycles → 10 missions → 4 phases (DG0/DG1/DG2/DG3 human gates). Output: a severity-tiered, effort-estimated, protected-content-aware **change spec** (`cycle_25_v210_change_spec.md`).

### 4.3 Format spec — LaTeX/TikZ + language standards
`whitepaper.tex` (3,307 lines, 17 TikZ figures) + `lattice-styles.tex` (constrained color palette + predefined node styles: institution/process/decision/datastore/external/mathnode/contract) + `mathematical_language_standards.md` (6 rules: label analogies; name-it-for-what-it-is; axiom-check before naming; two-layer operational+precise; empirical over theoretical; convention vs. structure) + glossary/appendix structure + protected-content boundaries. This is the whitepaper submodule's **format spec** — format that carries implicit quality assumptions.

### 4.4 Current federation relationship (what LiteratureForge will reshape)
`LPWhitepaper.aDNA/iii/CLAUDE.md` is the **first 6/7-pack consumer**, pinned at III.aDNA v0.3.0 / commit `a309ad4`, packs: inspect_procedures + introspect_checks + learning_store + whitepaper_communication + canvas_visual + vault_maintenance; local extension = vault-local learning store. Two in-flight campaigns: `campaign_iii_deep_review` (25-cycle, awaiting DG2) + `campaign_sharing_prep`. **These must be preserved during any migration.**

### 4.5 What seeds the whitepaper submodule
| Submodule component | Seed artifact |
|---|---|
| (a) Genre quality knowledge / traps | `context_iii_whitepaper_communication.md` (9 traps + 5 escalations) |
| (b) Review-cycle mechanism + reviewer personas | `campaign_iii_deep_review` + `voice_definitions.md` (F-TECH/F-INV/F-PAT/F-VIS) + `cycle_25_*` change-spec format |
| (c) Format spec | `whitepaper.tex` + `lattice-styles.tex` + `mathematical_language_standards.md` + glossary/appendix structure |

---

## §5 Synthesis — design directives for LiteratureForge (Phase B distilled)

These directives reconcile §2/§3/§4 into the architecture the charter will ratify.

**D1 — Genre submodule = {trap-pack + reviewer-voice-set + format-spec + reward-rubric}.** Mirrors the III domain-pack + the LPWhitepaper voice-set + the LaTeX format spec, bound together per genre. The whitepaper submodule is the worked first instance (its three seed artifacts map 1:1).

**D2 — 5-step submodule-creation lifecycle** (from the mission §2, grounded in research): (1) **Ingest + interview** — gather genre writing samples + interview user + ingest reference sources; (2) **Synthesize genre context graph** — distill quality dimensions + register patterns + fact-discipline + the **format spec**; (3) **Multi-step generation pipeline** — brief → outline/plan (mandatory inspectable stage, per STORM/Re3) → draft → revision, conditioned on the genre graph; (4) **Human+agent review/improvement cycles** — multi-voice scoring + weakest-first rework routing + RLHF signal capture (the III generalization); (5) **Format-bound output** — conforms to the genre format spec.

**D3 — Generation pipeline = staged, structure-first** (SpeechForge stage-modularity + VideoForge artifact-per-stage + STORM plan stage). Each stage emits a checkpointed artifact; the **outline/plan stage is mandatory and inspectable**.

**D4 — Review = multi-voice scored ship-gate** (CanvasForge median-of-N + VideoForge per-voice floors + III six-axis): per-genre reviewer voices → composite ≥ ship threshold (default 0.75, genre-tunable) → weakest-first bounded rework; structure-validation gate early + quality-validation gate late; humans at decision gates only.

**D5 — Fact-discipline is a first-class stage** (FActScore/RARR): atomic-claim decomposition → retrieval → agreement-gate → edit-only-unsupported, surfaced as a scored axis. Genre-tunable (a short story needs little; a research paper needs much).

**D6 — Quality loop is III, not bespoke.** LiteratureForge owns the *generation* pipeline + genre submodules + format specs; **III owns the *review/improvement* machinery** (INSPECT→INTROSPECT→IMPROVE→HUMAN_REVIEW→ACCUMULATE + the learning store + graduation). LiteratureForge consumes a **generalized writing-III** (future III Campaign E) via `federation_ref` — exactly the VideoForge/CanvasForge bridge_pack pattern. This is the writing-III boundary (mission §9 Q5): genre traps + format specs are LiteratureForge-owned writing packs; the general review-loop + learning store stay canonical III.

**D7 — Per-project correction memory → learning store.** Reviewer feedback persists per project (Reflexion-style) and feeds the III learning store; cross-genre patterns graduate via the ADR-003 ceremony.

**D8 — Scores/gates/RLHF signals are logged artifacts** read by the rework router + human reviewer + downstream RLHF collection (ADR-005 signal channel).

---

## §6 Provenance
Authored at MD-X2 execution session `session_stanley_20260525_iii_adna_md_x2_literatureforge_genesis_execution`. Research via 3 parallel subagents (external WebSearch SOTA; internal Explore pattern-mine of SpeechForge/CanvasForge/VideoForge/III; LPWhitepaper Explore seed-mine). No code, no learning-store touch — canonical md5 `5adb0dfa38d9224649c3b2cba83852ae` invariant.
