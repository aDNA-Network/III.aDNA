---
type: context_guide
topic: iii_domain_packs
subtopic: learning_store
created: 2026-04-03
updated: 2026-05-30
context_version: "1.2"
token_estimate: ~1900
last_edited_by: agent_argus
tags: [context, iii-review, learning-store, corrections, calibration, vaas, rlhf, adaptive_loop, adr_005, adr_007]
banner: "who/assets/banners/banner_context_guide.jpg"
icon: database
fair:
  keywords: [iii-review, learning-store, corrections, calibration, graduation, rlhf]
  license: Apache-2.0
migration_provenance:
  previous_home: lattice-labs/what/context/iii_domain_packs/context_iii_learning_store.md
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-2
quality_metric:
  rubric_version: "adr_007_v0"
  scored_at: 2026-05-20
  scored_by: agent_argus
  scoring_mission: campaign_d_federation_adaptive_loop MD-B1
  re_scored_at: 2026-05-30
  re_scored_by: agent_argus
  re_scoring_mission: campaign_g_consolidation G2
  signal_density: 4
  actionability: 4
  coverage_uniformity: 4
  source_diversity: 4
  cross_topic_coherence: 5
  graduation_yield: 3
  composite: 4.00
  floor_check: passed
  prior_score:
    scored_at: 2026-05-20
    scoring_mission: campaign_d_federation_adaptive_loop MD-B1
    source_diversity: 3
    composite: 3.83
  notes: "G2 (Campaign G T3) citation-hardening re-score: source_diversity 3→4 after formalizing the ~21 embedded cross-refs into a structured §Sources & Citations section (5 internal source clusters; principled-4 reflexive-pack ceiling, mirroring MX-1 vault_maintenance + web_design F2). composite 3.83→4.00 = (4+4+4+4+5+3)/6 = 24/6. All other axes HELD — citation-add only; no trap/detection redefinition (matches MX-1 scope discipline). graduation_yield held at 3 (deliberately NOT collapsed to the gy=1 'measurement artifact' ADR-007 §3.6 assigns trap-packs with zero PACK_DELTA_LANDED records): learning_store is the loop-meta substrate pack that OPERATES the graduation machinery, not a graduation TARGET — its gy reflects the real graduation throughput it governs (5 canonical graduations C-001/C-002/C-005/C-027/C-028 flowed through this pack's CorrectionLifecycle), genuinely mid-strength. Original MD-B1 exemplar note: first per-pack score under ADR-007 §3; pack is loop-meta (governs the learning-store substrate the rubric reads)."
---

# III Learning Store — Schema & Protocol

## Purpose

The learning store accumulates knowledge across III reviews, enabling the system to recognize recurring failure patterns, calibrate severity judgments, and graduate validated patterns into domain pack traps. Inspired by the VaaS corrections list, which reduced rejection rates 7x (73.4% → 10.56%) through accumulated knowledge.

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    III REVIEW CYCLE                       │
│                                                           │
│  ┌──────────┐    ┌───────────┐    ┌──────────┐          │
│  │  INSPECT  │───▶│ INTROSPECT│───▶│ IMPROVE  │          │
│  └────┬─────┘    └─────┬─────┘    └────┬─────┘          │
│       │                │               │                  │
│  inject            pattern           accumulate           │
│  corrections       match             new corrections      │
│       │                │               │                  │
│  ┌────▼────────────────▼───────────────▼─────┐           │
│  │         iii_corrections.jsonl               │           │
│  │  (accumulated patterns across all reviews)  │           │
│  └──────────────────┬────────────────────────┘           │
│                     │                                     │
│                 graduate                                  │
│                 (freq ≥ 3, accept ≥ 80%)                  │
│                     │                                     │
│                     ▼                                     │
│             Domain Pack Traps                             │
│             (permanent, curated)                           │
└─────────────────────────────────────────────────────────┘
```

## Corrections JSONL Schema

**Canonical location**: `III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl` (upstream — graduated entries only after ADR-003 ceremony).

**Per-consumer location**: `<consumer_vault>/iii/what/context/<vault>_iii_corrections_local.jsonl` (downstream — operational accumulation per consumer).

Each line is one JSON object representing a recurring failure pattern:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | yes | Unique ID: `C-NNN` |
| `trap` | string | yes | Which trap this triggers: `confidence`, `completeness`, `boundary`, `analogy`, `level`, or domain-specific |
| `pattern` | string | yes | Machine-readable pattern name (snake_case) |
| `description` | string | yes | Human-readable description of the failure pattern |
| `example` | string | yes | Concrete example from a real review |
| `source_review` | string | yes | Which review produced this correction |
| `source_finding` | string | yes | Finding ID that spawned it (e.g., `F-TEXT-004`) |
| `frequency` | int | yes | How many times observed across reviews |
| `accepted` | bool | yes | Whether user accepted the improvement when proposed |
| `graduated` | bool | yes | Whether promoted to a domain pack trap |
| `graduated_to` | string\|null | yes | Which domain pack absorbed it, or null |
| `created` | string | yes | ISO date of first observation |
| `rlhf_signal_type` | string (enum) | no (required-min when capturing RLHF; ADR-005 §2) | `accept` \| `reject` \| `defer` \| `accept_with_modification` |
| `rlhf_session_id` | string | no (required-min when capturing RLHF; ADR-005 §2) | Originating review session id |
| `rlhf_captured_at` | string (ISO 8601 UTC) | no (required-min when capturing RLHF; ADR-005 §2) | Capture instant |
| `rlhf_modification_delta` | string \| object | no (optional-open; ADR-005 §2) | Diff or rationale when `rlhf_signal_type: accept_with_modification` |
| `rlhf_reviewer_persona` | string | no (optional-open; ADR-005 §2) | Multi-voice persona id |
| `rlhf_severity_calibration` | float | no (optional-open; ADR-005 §2) | 0.0–1.0 reviewer confidence |
| `rlhf_cross_session_link` | string | no (optional-open; ADR-005 §2) | Prior occurrence pointer |
| `rlhf_consumer_namespace.<vault>.<field>` | any | no (consumer namespace; ADR-005 §3) | Vault-specific signals (e.g., VideoForge's `last_updated`) |

### Example Entry

```json
{"id":"C-001","trap":"confidence","pattern":"projective_claim_as_fact","description":"Cost/performance figures stated in present tense when they are projections, not measured results","example":"88% cost reduction stated as achieved when it's a target from UCLA deployment proposal","source_review":"iii_validation_report","source_finding":"F-TEXT-004","frequency":3,"accepted":true,"graduated":false,"graduated_to":null,"created":"2026-04-02"}
```

## Lifecycle Protocol

> Per ADR-007 §1, the per-pattern CorrectionLifecycle has six progression stages (SIGNAL_CAPTURED → CORRECTION_TRACKED → GRADUATION_CANDIDATE → GRADUATION_PROPOSED → GRADUATION_RATIFIED → PACK_DELTA_LANDED) plus two terminal-non-graduated states (PRUNED, ARCHIVED). The four steps below execute the transitions. See `what/artifacts/iii_adaptive_improvement_loop_spec.md` §5 for the full state machine reference.

### 1. Injection (at INSPECT start) — read-side feedback edge of the loop

Before beginning INSPECT, read the canonical store (`iii_corrections_canonical.jsonl`) AND the consumer-vault local store (if present). For each non-graduated correction:
- Add it to the active check list alongside the loaded domain pack traps
- Prioritize by frequency (higher frequency = check earlier)
- Report to user: "Loaded N corrections from learning store"

(This step does not fire CorrectionLifecycle transitions; it provides the substrate `module_iii_introspect` Check 2g reads against. Matches inside Check 2g fire CORRECTION_TRACKED transitions.)

### 2. Accumulation (at IMPROVE end) — fires SIGNAL_CAPTURED / CORRECTION_TRACKED / GRADUATION_CANDIDATE

After completing the IMPROVE phase, evaluate each finding:
- **Is this a recurring pattern** (not a one-off factual error)? If yes, check if it matches an existing correction (canonical OR local).
  - **Match found**: Increment `frequency` in the consumer-vault local store. Update `accepted` based on user's decision. Append additive `rlhf_*` fields per ADR-005 §2 (required-min: `rlhf_signal_type`, `rlhf_session_id`, `rlhf_captured_at`). Canonical store is read-only outside the graduation ceremony. → fires **CORRECTION_TRACKED** transition.
  - **No match**: Append a new correction entry to the consumer-vault local store with `frequency: 1` and the three ADR-005 §2 required-min `rlhf_*` fields. → fires **SIGNAL_CAPTURED** transition.
- After all per-finding writes: any entry with `frequency >= 3 AND acceptance_rate >= 80%` → fires **GRADUATION_CANDIDATE** transition; surfaced in `accumulate_report.graduation_signals`.
- **One-off errors** (e.g., a specific port number wrong) do NOT become corrections. Only patterns that could recur in other documents qualify.

### 3. Graduation (periodic, ADR-003 §3 ceremony) — fires GRADUATION_PROPOSED → GRADUATION_RATIFIED → PACK_DELTA_LANDED

During OODA cycles or after every 5-10 reviews:
- Scan local-store corrections where `frequency >= 3` AND acceptance rate >= 80% (matches CorrectionLifecycle GRADUATION_CANDIDATE entries)
- Propose graduation via cross-vault request memo to III.aDNA → fires **GRADUATION_PROPOSED** transition
- Stanley + Argus co-ratification: PR to upstream III.aDNA + canonical jsonl append + md5 rotation → fires **GRADUATION_RATIFIED** transition
- Trap definition written into relevant domain pack → fires **PACK_DELTA_LANDED** transition
- Graduated corrections stay in the JSONL as historical record but are excluded from injection (the domain pack trap supersedes them)

### 3.5 Per-pack quality metric (ADR-007 §3 six-axis rubric)

Per ADR-007 §3, each canonical domain pack at `what/context/core_domain_packs/` SHOULD carry a `quality_metric:` frontmatter block scoring six axes (signal_density / actionability / coverage_uniformity / source_diversity / cross_topic_coherence / graduation_yield) on a 1–5 scale. Composite = arithmetic mean. Floor rule: any axis ≤2 triggers pack revision. See `iii_adaptive_improvement_loop_spec.md` §4 for the consumer-facing reference and §8.3 for the worked exemplar (this pack's own quality_metric block, MD-B1 close). MD-B4 scores remaining 6 packs against this exemplar.

### 4. Pruning (periodic) — terminal-non-graduated PRUNED state

- Corrections with acceptance < 50% over 5+ observations → review for removal (may be false positive patterns) → enters **PRUNED** terminal state per ADR-007 §1
- If JSONL exceeds ~50 entries / ~2000 tokens → graduate high-frequency items and archive low-frequency ones to `iii_corrections_archive.jsonl` (same directory, full record preserved) → archived entries enter **ARCHIVED** terminal state per ADR-007 §1

## Token Efficiency

| Corrections Count | Approximate Tokens | Budget Impact |
|-------------------|-------------------|---------------|
| 10 | ~250 | Negligible — within one domain pack budget |
| 25 | ~650 | Moderate — equivalent to half a domain pack |
| 50 | ~1300 | Cap — graduate or prune before exceeding |

## VaaS Integration Mapping

The III learning store parallels VaaS's corrections list. Both are non-parametric knowledge stores that reshape quality through accumulated evidence rather than model retraining.

| VaaS Concept | III Learning Store Analog |
|---|---|
| Corrections list (JSONL, 0→14→20+ items across waves) | `iii_corrections_canonical.jsonl` (seeded from validation report, grows per graduation ceremony) |
| Injection at L1+L2 (biases generation before verification) | Injection at INSPECT start (biases review toward known patterns) |
| Error pattern accumulation across gene review waves | Pattern accumulation across document review sessions |
| Frequency tracking + wave-over-wave rejection rate | Frequency tracking + acceptance rate calibration |
| Non-parametric: no retraining, just pattern storage | Non-parametric: no prompt rewriting, just checklist expansion |
| Fleet-wide sharing via federated corrections | Federation via canonical/local split + graduation ceremony (ADR-003) |

### III as Semantic Verification Oracle

III can serve as a verification module in VaaS-style pipelines. The mapping:

| VaaS Pipeline Role | III Implementation |
|---|---|
| **Input** | Document (markdown, YAML, code, image) |
| **Verification operation** | INSPECT + INTROSPECT phases |
| **Output** | `VerificationResult` with `ReviewState` |
| **Oracle type** | Semantic (reasoning quality) vs VaaS's factual (citation accuracy) |
| **Corrections integration** | Learning store injected at review start, accumulated at review end |

**ReviewState** (analog to VaaS `VerificationState`):

```
UNREVIEWED (0) < INSPECTED (1) < INTROSPECTED (2) < IMPROVED (3)
```

This forms a bounded chain lattice with `sup(a,b) = max(a,b)`, mirroring VaaS's `VerificationState`. The lattice YAML composition is defined at `what/lattices/lattice_iii_verification_oracle.lattice.yaml`.

## Self-Review Protocol

[conjectural] The learning store should be periodically III-reviewed. First executed during campaign_iii_evolution M04 (2026-04-03). No automated trigger exists yet — run manually during OODA cycles or after every 10 reviews:
- Check corrections for false positives (patterns that don't actually recur)
- Check for stale entries (patterns from documents that have been rewritten)
- Verify graduation candidates are correctly identified
- Source review: `self_review` — tracked in corrections JSONL

## Sources & Citations

Internal grounding for the schema + lifecycle protocol + quality-metric machinery this pack defines (the `source_diversity` corpus). `learning_store` is a **reflexive, loop-meta pack**: its subject IS the III learning-store substrate — the corrections store, the CorrectionLifecycle, and the rubric that scores every pack (including this one). Its citation set is therefore correctly internal-source-heavy. External knowledge-management / organizational-memory literature was evaluated and deferred at G2 (2026-05-30) as non-load-bearing — the same principled-4 ceiling MX-1 set for `context_iii_vault_maintenance.md` and F2 set for `context_iii_domain_packs_web_design.md`. (Until G2 these citations lived embedded inline — ~21 cross-refs scattered through the body; T3 formalizes them into the structure below without adding or redefining any trap/protocol step.)

### Internal contracts (III canonical ADRs)

1. **`what/decisions/adr_003_learning_store_ownership.md`** §3 — the graduation ceremony (frequency ≥ 3 + acceptance ≥ 80%; canonical-vs-consumer-local store boundary: consumer edits never push automatically, canonical receives entries only via the ceremony) + §3.6 (≥2-vault elevated-scrutiny queue at cross-fork frequency ≥50). Grounds: § Lifecycle Protocol step 3 (Graduation), § Corrections JSONL Schema (canonical-vs-per-consumer location split), the canonical/local read split in step 1 (Injection).
2. **`what/decisions/adr_005_rlhf_signal_channel.md`** §2 (required-min `rlhf_signal_type`/`rlhf_session_id`/`rlhf_captured_at` + optional-open `rlhf_*` fields) + §3 (`rlhf_consumer_namespace.<vault>.<field>` consumer namespace). Grounds: § Corrections JSONL Schema (all `rlhf_*` rows), § Lifecycle Protocol step 2 (Accumulation — the additive `rlhf_*` append).
3. **`what/decisions/adr_007_adaptive_improvement_loop.md`** §1 (the six-stage CorrectionLifecycle state machine + two terminal-non-graduated states) + §3 (the six-axis per-pack rubric this pack's own `quality_metric` is scored against) + §3.6 amendment (MD-B6 2026-05-25 — graduation_yield credits packs by PACK_DELTA_LANDED stage marker + graduation-memo `pack_delta_target`, not by floor-matching `graduated_to: "core"`). Grounds: § Lifecycle Protocol (every transition annotation), § 3.5 Per-pack quality metric, this pack's own `quality_metric` frontmatter (incl. the graduation_yield-held-at-3 rationale).
4. **`what/decisions/adr_008_cross_vault_aggregation.md`** — the cross-vault RLHF aggregation contract (boundary-crossing field set + ≥2-vault aggregation feeding ADR-003 §3.6; transport = `learning_store_graduation` cross_vault_request over the v0.3 airlock §4.3). Grounds: § Corrections JSONL Schema (cross-vault provenance fields), the federation framing of the canonical/local split.

### Internal procedures (III canonical skills)

5. **`how/skills/skill_iii_review.md`** — the orchestration that reads this store at INSPECT start and writes it at IMPROVE end; **Step 3b ACCUMULATE** is the read/write edge of the adaptive loop this pack specifies. Grounds: § Lifecycle Protocol steps 1 (Injection) + 2 (Accumulation), § Architecture diagram (inject → accumulate → graduate cycle).
6. **`how/skills/skill_session_close_ceremony.md`** — the cascade-bookkeeping discipline under which graduation md5-rotations + score-changes are committed (STATE.md + charter + pack in the SAME commit). Grounds: § Lifecycle Protocol step 3 (graduation as a ceremony, not an ad-hoc edit), the canonical-store md5-rotation discipline.
7. **`how/skills/skill_aggregation_index_intake.md`** — the ADR-008 §3.2 intake ceremony (one record per received `learning_store_graduation` proposal, keyed by normalized (trap, pattern) + originating_vault; receiver-side, read-only against canonical). Grounds: § Lifecycle Protocol step 3 (the upstream-receive half of graduation), § Corrections JSONL Schema (cross-vault proposal provenance).

### Cross-campaign precedent

8. **Campaign D MD-B1** (`what/artifacts/iii_adaptive_improvement_loop_spec.md` provenance) — the first per-pack `quality_metric` score under ADR-007 §3; **this pack is the scoring exemplar** the other six canonical packs were scored against. Grounds: § 3.5 Per-pack quality metric, this pack's frontmatter `prior_score`.
9. **Campaign D MD-B2** — first canonical jsonl md5 rotation (`dde2cbd…` → `5adb0dfa…`) under the graduation ceremony; the rotation-with-provenance discipline. Grounds: § Lifecycle Protocol step 3 (md5 rotation), § Token Efficiency (cap-and-graduate).
10. **Campaign D MD-B4** (`what/artifacts/md_b4_7_pack_pilot_report.md`) — the 7-pack scoring pilot that surfaced the graduation_yield measurement-artifact finding (resolved at MD-B6 via ADR-007 §3.6); the empirical basis for this pack's graduation_yield-held-at-3 note. Grounds: § 3.5 Per-pack quality metric, frontmatter `notes`.
11. **Campaign D MD-B6** (`what/artifacts/md_b6_dg_d_scorecard.md`) + **MX-1** (`what/artifacts/mx1_validation_self_review.md`) — the DG scorecard shape + the citation-hardening recipe (structured §Sources, principled-4 ceiling) this very section replays. Grounds: § Sources preamble, the principled-4 ceiling rationale below.

### The canonical artifact itself

12. **`what/context/core_domain_packs/iii_corrections_canonical.jsonl`** — the 28-entry upstream store (md5 `5adb0dfa38d9224649c3b2cba83852ae`; 5 graduated: C-001/C-002/C-005/C-027/C-028) this schema documents. The pack and its artifact are co-located by design. Grounds: § Corrections JSONL Schema (every field row), § Example Entry.
13. **`what/artifacts/iii_adaptive_improvement_loop_spec.md`** §5 — the full CorrectionLifecycle state-machine reference the § Lifecycle Protocol four steps execute against. Grounds: § Lifecycle Protocol preamble + every transition annotation.

### Citation ceiling rationale (principled-4)

The `source_diversity` rubric (`adr_007_adaptive_improvement_loop.md` §3 + `how/skills/skill_context_quality_audit.md` source-diversity definition) reserves the **5**-score for source sets that include peer-reviewed academic literature alongside vendor/practitioner/empirical sources. `learning_store` is a **reflexive, loop-meta pack** — its subjects ARE III's own corrections substrate, ADRs, and lifecycle machinery; the academically-recognized literature for "non-parametric correction stores" (the VaaS corrections-list lineage is already cited in § VaaS Integration Mapping; broader active-learning / RLHF literature sits behind ADR-005's own grounding) is either already embedded or at the wrong abstraction layer for an aDNA-internal schema doc. At G2 the operator-approved scope was citation-formalization, not external-citation-chasing: padding a reflexive schema pack with general active-learning papers would *reduce* signal density, not increase load-bearing source diversity. The principled ceiling is **4**, mirroring MX-1 `vault_maintenance` and F2 `web_design` (both capped at 4 absent load-bearing academic citations).

## Related

- [[how/skills/skill_iii_review|III Review Skill]] — the procedure that reads and writes corrections
- `what/context/core_domain_packs/` — graduation target for high-frequency corrections (the canonical domain packs live in this directory; see the per-pack file naming `context_iii_<domain>.md`)
- `what/context/vaas_review/context_vaas_technical_review` (VaaS Technical Review) — corrections-list design pattern (Section 2.3). **Unresolvable in-vault** (predecessor `lattice-labs` artifact; not migrated to III.aDNA — link de-wikified to a plain path to avoid a dangling reference; restore as a wikilink if/when the source is migrated).
- [[what/lattices/lattice_iii_verification_oracle|III Verification Oracle Lattice]] — composable VaaS node definition
