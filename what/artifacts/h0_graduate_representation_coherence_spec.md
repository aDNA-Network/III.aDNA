---
type: artifact
artifact_class: graduation_spec
campaign_id: campaign_h_iii_campaign_pattern
mission_id: h0_charter
title: "H0 graduation spec — representation_coherence pack → canonical-conditional"
created: 2026-06-29
updated: 2026-06-29
last_edited_by: agent_argus
status: active
target_track: T2
tags: [artifact, graduation_spec, campaign_h, representation_coherence, pack, canonical_conditional]
---

# H0 Graduation Spec — `representation_coherence` pack (T2, lands H1)

> The adaptation plan to graduate the pilot's `representation_coherence` pack from a campaign-local draft (aDNA.aDNA) to a **canonical-conditional** III.aDNA core domain pack. This is a *spec* — H1 executes it. Q2 disposition: **graduate now, conditional/draft-with-evidence** (5/11 traps proven first-cycle, 0 false fires; 6 silent traps held candidate).

## Source → Target

| | Path |
|---|---|
| **Source** (read-only; stays `status: draft`) | `aDNA.aDNA/how/campaigns/campaign_looking_glass/artifacts/packs/pack_representation_coherence.md` |
| **Target** (canonical) | `III.aDNA/what/context/core_domain_packs/context_iii_representation_coherence.md` |

The source is **read-only ground truth** (Rule 10; pilot closed). H1 reads it, snapshots its content, and writes the adapted canonical copy. The source is NOT edited.

## What graduates intact

- The 4 dimensions / 11 traps: **A1 Claim-Source Traceability** (RC-TRACE-01 untraceable · RC-TRACE-02 source-mismatch · RC-FAB-01 fabricated-specificity); **A2 Currency vs Source** (RC-CURR-01 stale-vault-state · RC-CURR-02 superseded-source); **A3 Structural Fidelity** (RC-STRUCT-01 misrepresented-shape · RC-STRUCT-02 dangling-reference · RC-STRUCT-03 framing-drift); **A4 "Does Justice"** (RC-JUST-01 qualifier-stripping · RC-JUST-02 one-sided · RC-JUST-03 confidence/hype-drift).
- The **"does justice" 0–5 rubric** (currency & fidelity are floors, not averages).
- The **cross-trap escalation rules** table.
- The **per-trap Description/Example/Where-Hides/Severity/Detection** fields (already in source — matches the core-pack trap shape).

## Adaptations H1 must make

1. **Rename** `pack_representation_coherence.md` → `context_iii_representation_coherence.md` (the `context_iii_*` core-pack convention).
2. **Frontmatter** — conform to the core-pack convention (see `context_iii_inspect_procedures.md` / `context_iii_iss_surfaces.md`):
   - `type: context_guide`, `topic: iii_domain_packs`, `subtopic: representation_coherence` (source already carries these).
   - `status: draft` → `status: active`; add `graduation_state: conditional` + `graduation_cycles: 1` (toward the ≥3 / ≥80% bar).
   - Replace `campaign: campaign_looking_glass` + `graduation_target:` with `graduated_from: campaign_looking_glass (aDNA.aDNA pilot, 2026-06-28)` + `graduated_at: campaign_h_iii_campaign_pattern`.
   - `last_edited_by: agent_argus`; refresh `token_estimate`.
   - **Add the `quality_metric` block** (the one element the source lacks) — per ADR-007 §3 / `adr_007_v0`, the 6 axes (signal_density · actionability · coverage_uniformity · source_diversity · cross_topic_coherence · graduation_yield) + `composite` + `floor_check`. **H1 scores it for real** against the rubric; target **composite ≥4.0** (DG-H crit 4). Do not pre-fill scores here.
3. **Re-home wikilinks** — the source links are aDNA.aDNA-relative and break in III:
   - `[[../../../../who/reviewers/reviewer_standard_archivist|Standard Archivist]]` and `[[who/reviewers/reviewer_standard_archivist]]` → resolve per **DP-2**. The Standard Archivist stays in aDNA.aDNA (out of scope to graduate); keep as an **explicit cross-vault provenance pointer** (`aDNA.aDNA/who/reviewers/reviewer_standard_archivist.md`), not a wikilink that will dangle.
   - `[[how/campaigns/campaign_looking_glass/...]]` (instrumentation_log, site_backing_slice, campaign master) → convert to a §"Graduation provenance" pointer block (explicit `aDNA.aDNA/...` paths), since those targets do not exist in III.
   - `[[how/campaigns/campaign_looking_glass/artifacts/personas/reviewer_claim_tracer]]` → re-point to the III-local `claim_tracer` home (the DP-2-resolved path; see `h0_graduate_claim_tracer_spec.md`).
4. **Generalize the canonical reference** — the source's "Canonical Reference" §pins the Looking Glass `site_backing_slice` as ground truth. Reframe to the **general** rule (the subject's bounded ground-truth slice, carved by the three-ring method per `skill_iii_campaign`), with the Looking Glass slice cited as the worked example. The two standing rules (Ring-A read-only; a drift is fixable on either side) graduate as doctrine.
5. **Add §"Graduation provenance + conditional state"** — the honest fire-table.

## The honest trap fire-table (H1 populates from the pilot record — do NOT transcribe from memory)

The Q2 disposition rests on *which* 5 traps fired. **H1 must derive the exact fired-vs-silent split** by reading the pilot's canonical records — **do not fabricate or guess** (that would trip RC-FAB-01, the very trap being graduated):

- `aDNA.aDNA/how/campaigns/campaign_looking_glass/missions/artifacts/findings_register.md` (the 25 scored findings — map each to its RC-* trap)
- `aDNA.aDNA/how/campaigns/campaign_looking_glass/artifacts/instrumentation_log.md` Log 2 (the canonical reusable-vs-bespoke disposition record)

Render a table: each of the 11 traps → `fired (n findings, examples)` | `silent (clean-subject true-negative)`, with the 5 fired carrying first-cycle evidence and the 6 silent labeled `candidate` (needs cross-artifact catch-evidence at H3). The marquee currency inversion (B1/B2 — *the mirror more current than its source*) maps to A2 (RC-CURR-01/02) and is the load-bearing fired evidence.

## Graduation criterion + path to canonical-full

- Bar: each trap fires in **≥3 cycles** with **≥80% acceptance**. Pilot = cycle 1. H3 supplies cycle 2 (a 2nd artifact type). Cycle 3 accrues as the pattern is used post-campaign.
- **Conditional graduation is not block-closing** (DG-H crit 4 + R1). The pack lands canonical with `graduation_state: conditional`; firing candidate traps lift to canonical at H3 with evidence; the rest accrue over time.
- Canonical jsonl md5 `5adb0dfa…`/28 **INVARIANT** — this pack graduates as a *pack*, not as learning-store corrections; **zero graduation fires** in the ADR-003 sense.

## Verification (H1)

Pack at the target path; frontmatter valid + `quality_metric` scored ≥4.0; 11 traps intact; no dangling wikilinks (every link resolves to a real III target or is an explicit cross-vault path); §provenance + fire-table present + sourced from the pilot record; `core_domain_packs/` count 8→9; canonical jsonl md5 unchanged.
