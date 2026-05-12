---
type: context_guide
topic: iii_domain_packs
subtopic: learning_store
created: 2026-04-03
updated: 2026-05-08
context_version: "1.0"
token_estimate: ~1500
last_edited_by: agent_stanley
tags: [context, iii-review, learning-store, corrections, calibration, vaas]
banner: "who/assets/banners/banner_context_guide.jpg"
icon: database
migration_provenance:
  previous_home: lattice-labs/what/context/iii_domain_packs/context_iii_learning_store.md
  migrated: 2026-05-08
  migration_mission: campaign_a_iii_genesis MA-2
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

### Example Entry

```json
{"id":"C-001","trap":"confidence","pattern":"projective_claim_as_fact","description":"Cost/performance figures stated in present tense when they are projections, not measured results","example":"88% cost reduction stated as achieved when it's a target from UCLA deployment proposal","source_review":"iii_validation_report","source_finding":"F-TEXT-004","frequency":3,"accepted":true,"graduated":false,"graduated_to":null,"created":"2026-04-02"}
```

## Lifecycle Protocol

### 1. Injection (at INSPECT start)

Before beginning INSPECT, read the canonical store (`iii_corrections_canonical.jsonl`) AND the consumer-vault local store (if present). For each non-graduated correction:
- Add it to the active check list alongside the loaded domain pack traps
- Prioritize by frequency (higher frequency = check earlier)
- Report to user: "Loaded N corrections from learning store"

### 2. Accumulation (at IMPROVE end)

After completing the IMPROVE phase, evaluate each finding:
- **Is this a recurring pattern** (not a one-off factual error)? If yes, check if it matches an existing correction (canonical OR local).
  - **Match found**: Increment `frequency` in the consumer-vault local store. Update `accepted` based on user's decision. Canonical store is read-only outside the graduation ceremony.
  - **No match**: Append a new correction entry to the consumer-vault local store with `frequency: 1`.
- **One-off errors** (e.g., a specific port number wrong) do NOT become corrections. Only patterns that could recur in other documents qualify.

### 3. Graduation (periodic, ADR-003 ceremony)

During OODA cycles or after every 5-10 reviews:
- Scan local-store corrections where `frequency >= 3` AND acceptance rate >= 80%
- Propose graduation to the appropriate domain pack OR to the canonical store (Stanley approval required)
- On approval: PR to upstream III.aDNA. On merge: write formal trap definition in domain pack OR copy entry into canonical store with `graduated: true` and `graduated_to: "<pack_name>"`
- Graduated corrections stay in the JSONL as historical record but are excluded from injection (the domain pack trap supersedes them)

### 4. Pruning (periodic)

- Corrections with acceptance < 50% over 5+ observations → review for removal (may be false positive patterns)
- If JSONL exceeds ~50 entries / ~2000 tokens → graduate high-frequency items and archive low-frequency ones to `iii_corrections_archive.jsonl` (same directory, full record preserved)

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
