---
type: artifact
artifact_class: graduation_spec
campaign_id: campaign_h_iii_campaign_pattern
mission_id: h0_charter
title: "H0 graduation spec — claim_tracer persona → candidate (+ the who/reviewers decision)"
created: 2026-06-29
updated: 2026-06-29
last_edited_by: agent_argus
status: active
target_track: T3
tags: [artifact, graduation_spec, campaign_h, claim_tracer, persona, reviewer, who_reviewers, dp_2]
---

# H0 Graduation Spec — `claim_tracer` persona (T3, lands H1)

> The adaptation plan to graduate the pilot's `claim_tracer` reviewer persona — the **fidelity** complement to the currency-owning Standard Archivist — into III.aDNA as a **candidate** reviewer. This spec also frames **DP-2**, the structural decision its landing forces.

## Source → Target

| | Path |
|---|---|
| **Source** (read-only; `scope: campaign_local`) | `aDNA.aDNA/how/campaigns/campaign_looking_glass/artifacts/personas/reviewer_claim_tracer.md` |
| **Target** | **DP-2-dependent** — see below |

## DP-2 — the reviewer-bench artifact class (the structural decision)

The source persona's `graduation_target` reads `who/reviewers/ (III.aDNA Reviewer Lens Pass)`. **But III.aDNA has no `who/reviewers/` directory** (`who/` = AGENTS.md · coordination · governance · team). III's native reviewer model is the **reviewer-orchestra voice registry** (`skill_iii_review` §Reviewer Orchestra — voices in a consumer-vault YAML: `voice_id`, `finding_prefix`, `check_criteria`, `severity_default`). The 16 reviewers the source references are **aDNA.aDNA's Rosetta-extension bench**, not III's.

Two landing options:

- **Option A (recommended) — establish a minimal `who/reviewers/` bench in III.** A standalone `who/reviewers/reviewer_claim_tracer.md` (mirroring aDNA.aDNA's `reviewer` entity class). Rationale: the *campaign-scale* pattern (`skill_iii_campaign`) runs a persona bench (the pilot's 15-lens decadal review); a graduated, cross-vault-referenced, prose-rich persona is far more legible as a reviewer .md than as a voice-YAML row; it gives III a home for future graduated personas. Cost: a net-new artifact-class folder (no structural ADR needed — ADR-010 pre-dispositioned).
- **Option B — map onto the reviewer-orchestra voice registry.** Encode `claim_tracer` as a `standard_voice` (e.g. `finding_prefix: F-TRACE`) consumed by `skill_iii_review` multi-voice mode. Rationale: stays inside III's existing model; no new artifact class. Cost: loses the prose (background, example finding, A1/A2 reasoning); voices are tuned for web_design multi-voice, not campaign-scale fidelity review.

**Recommendation: Option A.** Operator ratifies at DP-1/DP-2. If A: target = `III.aDNA/who/reviewers/reviewer_claim_tracer.md` + a `who/reviewers/AGENTS.md` establishing the class. If B: target = a voice row + the persona's prose preserved in the `representation_coherence` pack §persona.

## What graduates intact (either option)

- The **role**: holds the subject's bounded ground-truth slice; walks **claim → source → verification** (does the source exist · is it current · does it say this · are qualifiers/trade-offs kept).
- The **A1/A2 division of labour**: Claim-Tracer owns **A1** (correctness / no fabrication) as primary + the *semantic* slice of **A2** (is a cited source superseded?); the Standard Archivist owns mechanical **A2** currency; the Tier-1 gate is the floor under both. This is a graduating **doctrine** (also woven into the pack + skill).
- The **6 critique prompts** (traceability · source-accuracy · qualifier-preservation · source-currency · honest-trade-offs · owner-class split).
- The **primary ranker lens = `trust`** (fidelity *is* trust) + secondary `findability`.
- The **worked example finding** (the `/learn/comparisons/adna-vs-para` qualifier-strip + framing-drift → Critical).

## Adaptations H1 must make

1. **Frontmatter** — `scope: campaign_local` → `scope: canonical`; add `graduation_state: candidate`; replace `campaign:` + `graduation_target:` with `graduated_from` / `graduated_at`; `last_edited_by: agent_argus`.
2. **Re-home / reframe cross-vault references** — the source links the Standard Archivist + Content Strategist (aDNA.aDNA reviewers) and "16 who/reviewers." In III those don't exist:
   - Standard Archivist / Content Strategist → **explicit cross-vault provenance pointers** (`aDNA.aDNA/who/reviewers/...`), framed as "the complementary currency/pairing lenses in the pilot's bench"; do NOT graduate them (out of scope).
   - "of the 16 `who/reviewers`, none was a source-fidelity auditor" → reframe as the *pilot's* finding (provenance), not an III-local claim.
   - `[[how/campaigns/campaign_looking_glass/...]]` (pack, slice, scaffolding_spec, skill_decadal_aar) → re-point to III-local where the target graduates (the pack), else explicit `aDNA.aDNA/...` provenance pointers.
3. **Log the ranker-axis primitive gap** — the persona notes "the ranker has no native fidelity axis; this mapping [onto `trust`] is logged as an III-primitive gap for the terminal handoff." This is a **§5 primitive-gap** → fold into III core at **H4** (T5): either add a native `fidelity` ranker axis or canonize the trust-mapping.

## Verification (H1)

Persona at the DP-2-resolved path; frontmatter valid (`graduation_state: candidate`); the A1/A2 split + 6 prompts + ranker lens + worked example intact; no dangling wikilinks; if Option A, `who/reviewers/AGENTS.md` establishes the class; cross-vault pointers explicit and resolvable.
