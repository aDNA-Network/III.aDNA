---
type: context_guide
topic: iii_domain_packs
subtopic: learning_store
created: 2026-04-03
updated: 2026-05-20
context_version: "1.1"
token_estimate: ~1700
last_edited_by: agent_argus
tags: [context, iii-review, learning-store, corrections, calibration, vaas, rlhf, adaptive_loop, adr_005, adr_007]
banner: "who/assets/banners/banner_context_guide.jpg"
icon: database
migration_provenance:
  previous_home: lattice-labs/what/context/iii_domain_packs/context_iii_learning_store.md
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-2
quality_metric:
  rubric_version: "adr_007_v0"
  scored_at: 2026-05-20
  scored_by: agent_argus
  scoring_mission: campaign_d_federation_adaptive_loop MD-B1
  signal_density: 4
  actionability: 4
  coverage_uniformity: 4
  source_diversity: 3
  cross_topic_coherence: 5
  graduation_yield: 3
  composite: 3.83
  floor_check: passed
  notes: "First per-pack quality score under ADR-007 §3 six-axis rubric (MD-B1 exemplar). Pack is loop-meta: it governs the learning-store substrate the rubric reads. MD-B4 will score remaining 6 canonical packs against this exemplar."
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

## Related

- [[how/skills/skill_iii_review|III Review Skill]] — the procedure that reads and writes corrections
- `what/context/core_domain_packs/` — graduation target for high-frequency corrections (the canonical domain packs live in this directory; see the per-pack file naming `context_iii_<domain>.md`)
- [[what/context/vaas_review/context_vaas_technical_review|VaaS Technical Review]] — corrections list design pattern (Section 2.3)
- [[what/lattices/lattice_iii_verification_oracle|III Verification Oracle Lattice]] — composable VaaS node definition
