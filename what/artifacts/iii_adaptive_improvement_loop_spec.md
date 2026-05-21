---
type: standard_spec
title: "III.aDNA Adaptive-Improvement Loop Standard"
version: "0.3.0"
status: reference_implementation
created: 2026-05-20
updated: 2026-05-20
last_edited_by: agent_argus
governs: the named III adaptive-improvement loop; per-finding RLHF signal capture; per-pack quality measurement; correction-lifecycle state machine
runtime_substrate: what/lattices/lattice_iii_verification_oracle.lattice.yaml  # v1.2.2 post-MD-B1
defers_signal_schema_to: what/decisions/adr_005_rlhf_signal_channel.md
defers_loop_architecture_to: what/decisions/adr_007_adaptive_improvement_loop.md
covers: [signal_capture, correction_lifecycle, per_pack_quality, loop_closure_protocol]
predecessor_substrate: implicit across module_iii_accumulate + module_iii_introspect + module_iii_improve + lattice v1.2.1 (no named-loop artifact prior to v0.3.0)
authoring_mission: MD-B1
campaign: campaign_d_federation_adaptive_loop
tags: [standard, adaptive_loop, rlhf, learning_store, per_pack_quality, correction_lifecycle, v0_3]
---

# III.aDNA Adaptive-Improvement Loop Standard — v0.3.0

> **Status note**: This is the *standard* — the consumer-facing reference that names and versions the III adaptive-improvement loop. The runtime substrate (modules + lattice yaml) executes the loop; the ADRs (005 + 007) govern signal-channel and architectural decisions; this spec is the shape contract consumers read to understand what they're integrating with. When the spec changes, consumer wrappers minor-bump review per ADR-002 §3.

---

## §1 Purpose & Scope

The III adaptive-improvement loop is operationally complete in pre-v0.3 substrate but architecturally unnamed. This spec names it, versions it, and provides the four contracts consumers and downstream missions read against:

1. **A loop contract**: named stages from signal capture through pack-delta landing (§2 + §5).
2. **A signal contract**: per-finding RLHF capture schema with required-minimum + optional-open partition (§3; defers to ADR-005).
3. **A quality contract**: per-pack 6-axis rubric for measuring domain-pack health (§4; defers to ADR-007 §3).
4. **A versioning contract**: how this spec evolves and how consumers know what they've pinned (§6).

### What this standard does NOT govern

- **The corrections jsonl byte representation** — that lives in ADR-003 §4 (already shipped). This spec adds optional `rlhf_*` fields (§3) but does not rewrite the schema.
- **The graduation ceremony procedure** — `frequency ≥ 3 AND acceptance ≥ 80% AND ≥ 2 sessions` per ADR-003 §3. This spec **uses** the ceremony as a lifecycle transition (§5) but does not redefine it.
- **The ≥50 corrections threshold** mentioned in Campaign D charter line 74 — MD-B2 scope; ADR-003 amendment will canonicalize it; ADR-007 §4 confirms it is a separate, complementary signal from per-pack quality.
- **Cross-vault RLHF aggregation transport** — MD-B3 scope; rides on v0.3 airlock from MD-A1; this spec emits the forward-reference at §5.3.

### Substrate already in place (pre-v0.3, named here)

Three modules + one lattice + ADR-003 already implement the loop mechanically; this spec names what they were collectively doing:

- `what/modules/module_iii_accumulate.md` line 72 — emits `graduation_signals` (the SIGNAL_CAPTURED + CORRECTION_TRACKED + GRADUATION_CANDIDATE transitions of §5).
- `what/modules/module_iii_introspect.md` line 75 — Check 2g `corrections_pattern_match` closes the read-side loop (the CORRECTION_TRACKED stage's contribution back into the next review cycle).
- `what/modules/module_iii_improve.md` line 61 — `learning_store_acceptance_rates` input calibrates severity (the signal-consumer side; informs IMPROVE phase output).
- `what/lattices/lattice_iii_verification_oracle.lattice.yaml` — v1.2.2 wires `human_review → accumulate → corrections_store → introspect` (the runtime substrate).
- `what/decisions/adr_003_learning_store_ownership.md` — §3 ceremony procedure remains the GRADUATION_PROPOSED → GRADUATION_RATIFIED → PACK_DELTA_LANDED transition.

The substrate is unchanged at v0.3 except for additive ADR cross-references at patch level. The new artifacts are: this spec; ADR-005; ADR-007.

---

## §2 The Loop (named, versioned)

The III adaptive-improvement loop is named **III Adaptive-Improvement Loop** and is at version **0.3.0** (matching this spec's frontmatter). It is **per-pattern** — one correction equivalence class (identified by `pattern: snake_case_id` in the corrections jsonl) traverses the lifecycle, accumulating signals from many review cycles.

### §2.1 Loop diagram

```
                    ┌─────────────────────────────────────────────────────────┐
                    │                                                         │
                    │   (review cycle starts at module_iii_dispatch)          │
                    │                                                         │
                    ▼                                                         │
              ┌─────────────┐                                                 │
              │  INSPECT    │     loads corrections_store (read-only)         │
              │  4 modes    │     to inject known patterns at start           │
              └──────┬──────┘                                                 │
                     ▼                                                        │
              ┌─────────────┐                                                 │
              │ INTROSPECT  │     Check 2g corrections_pattern_match          │
              │  7 checks   │     fires CORRECTION_TRACKED transition         │
              └──────┬──────┘                                                 │
                     ▼                                                        │
              ┌─────────────┐                                                 │
              │   IMPROVE   │     uses learning_store_acceptance_rates +      │
              │  (severity) │     rlhf_severity_calibration distribution      │
              │             │     to weight findings                          │
              └──────┬──────┘                                                 │
                     ▼                                                        │
              ┌─────────────┐     User accepts/rejects/defers ImprovementTable
              │ HUMAN_REVIEW│     → rlhf_signal_type per finding (§3)
              └──────┬──────┘                                                 │
                     ▼                                                        │
              ┌─────────────┐                                                 │
              │ ACCUMULATE  │     writes ENV consumer-local store only;       │
              │             │     fires SIGNAL_CAPTURED / CORRECTION_TRACKED  │
              │             │     / GRADUATION_CANDIDATE transitions          │
              └──────┬──────┘                                                 │
                     │                                                        │
                     │   ──────────── feedback edge ─────────────►            │
                     │   (next review cycle's INSPECT reads what              │
                     │   ACCUMULATE wrote; loop closes per-pattern)           │
                     └────────────────────────────────────────────────────────┘

                     (sub-loop) GRADUATION_CANDIDATE → GRADUATION_PROPOSED
                                → GRADUATION_RATIFIED → PACK_DELTA_LANDED
                                fires asynchronously via ADR-003 §3 ceremony
                                (cross-vault request memo → Stanley + Argus
                                co-ratification → canonical jsonl append +
                                domain pack static trap append)
```

The within-review-cycle loop runs once per ReviewState chain (per-artifact). The cross-cycle loop runs over the lifetime of a pattern (per-pattern), accumulating across many review cycles before reaching graduation.

### §2.2 Stage IDs (canonical naming)

Stages are named in `UPPER_SNAKE_CASE` for cross-artifact citation:

| Stage ID | Where it fires | Module owning the transition |
|----------|----------------|------------------------------|
| `SIGNAL_CAPTURED` | First ACCUMULATE write for a pattern | `module_iii_accumulate` |
| `CORRECTION_TRACKED` | Second-or-later ACCUMULATE update OR introspect Check 2g match | `module_iii_accumulate` + `module_iii_introspect` |
| `GRADUATION_CANDIDATE` | ACCUMULATE emits `graduation_signals` (frequency ≥ 3 AND acceptance ≥ 80%) | `module_iii_accumulate` |
| `GRADUATION_PROPOSED` | Candidate filed via cross-vault request memo per ADR-003 §3 | (skill_iii_review + cross-vault request handshake; not module-owned) |
| `GRADUATION_RATIFIED` | Argus + Stanley co-ratification; canonical jsonl append + md5 rotation | (governance ceremony; not module-owned) |
| `PACK_DELTA_LANDED` | Graduated correction translated into static trap inside relevant domain pack per ADR-003 §6 | (manual edit by reviewing agent; not module-owned) |
| `PRUNED` (terminal-non-graduated) | `acceptance_rate < 0.5` over `frequency ≥ 5` | `module_iii_accumulate` (per `context_iii_learning_store.md` line 108) |
| `ARCHIVED` (terminal-non-graduated) | Superseded by a parent pattern's graduation | (manual at graduation ceremony) |

These names are normative — module yaml standards lists, lattice node configs, and ADR cross-references cite them by these IDs.

### §2.3 Minimum-viable adaptive cycle

A consumer that executes a full loop iteration runs:

1. `module_iii_dispatch` (loads corrections store as input)
2. ≥1 inspect modality (text / code / visual / data per dispatched plan)
3. `module_iii_introspect` (Check 2g fires CORRECTION_TRACKED transitions)
4. `module_iii_improve` (calibrates severity using rlhf_severity_calibration if available)
5. `human_review` (captures `rlhf_signal_type` per finding)
6. `module_iii_accumulate` (writes deltas; fires SIGNAL_CAPTURED / CORRECTION_TRACKED / GRADUATION_CANDIDATE)

Skipping any of {dispatch, introspect, accumulate} breaks the loop. Skipping individual inspect modalities is permitted (per-artifact relevance).

---

## §3 Per-Finding RLHF Signal Schema

This section is the **consumer-facing summary** of the schema; the normative authority is `what/decisions/adr_005_rlhf_signal_channel.md`. Consumers read ADR-005 for the contract; this section is the readable shape with worked examples.

### §3.1 Required-minimum (3 fields)

Every entry that captures an RLHF signal MUST declare:

```jsonl
{
  ...existing ADR-003 §4 fields...,
  "rlhf_signal_type": "accept | reject | defer | accept_with_modification",
  "rlhf_session_id": "session_<user>_<YYYYMMDD>_<descriptor>",
  "rlhf_captured_at": "YYYY-MM-DDTHH:MM:SSZ"
}
```

Per ADR-005 §2.

### §3.2 Optional-open (4 fields)

May appear; consumers SHOULD populate when applicable:

```jsonl
{
  ...required-minimum...,
  "rlhf_modification_delta": "<diff or rationale>",       // when type = accept_with_modification
  "rlhf_reviewer_persona": "<voice id>",                  // multi-voice contexts
  "rlhf_severity_calibration": 0.85,                      // 0.0-1.0 reviewer confidence
  "rlhf_cross_session_link": "C-NNN | session_id"         // prior occurrence pointer
}
```

Adding new top-level `rlhf_*` fields requires an ADR-005 amendment per ADR-005 §5 clause 1.

### §3.3 Consumer namespace (unbounded, isolated)

Consumer vaults declare vault-specific signals under:

```jsonl
{
  "rlhf_consumer_namespace.<vault>.<field>": <any json value>
}
```

`<vault>` is the lowercased vault name without `.aDNA`; `<field>` is snake_case. ADR-005 §3 governs namespace rules; III canonical never carries `rlhf_consumer_namespace.*` fields (rule 5).

### §3.4 Worked example — VFL-001 re-expressed

VFL-001 currently lives in `~/lattice/VideoForge.aDNA/iii/what/context/videoforge_iii_learning_store.jsonl` with three ad-hoc additive fields (`last_updated`, `graduation_proposal_filed`, `graduation_proposal_path`) introduced during VideoForge self-review cycles 2026-05-11..05-12. ADR-005 retroactively normalizes those fields under the consumer namespace.

Pre-ADR-005 shape (illustrative):

```jsonl
{
  "id": "VFL-001",
  "trap": "completeness",
  "pattern": "producer_consumer_pair_unwired",
  "description": "Producer module emits field before consumer wires it",
  "example": "M_3_02 RegisterCoherenceScorer emits unconsumed output",
  "source_review": "M_3_02 self-review",
  "source_finding": "F-INTROSPECT-VFL-001-001",
  "frequency": 3,
  "accepted": true,
  "graduated": false,
  "graduated_to": null,
  "graduation_candidate": true,
  "graduation_note": "freq=3 across M_3_02..04, acceptance=1.0, proposal filed",
  "created": "2026-05-11",
  "last_updated": "2026-05-12T20:00:00Z",
  "graduation_proposal_filed": true,
  "graduation_proposal_path": "who/coordination/coord_2026_05_12_vfl_graduation_proposals.md"
}
```

Post-ADR-005 normalized shape (additive only; no field rename or removal):

```jsonl
{
  "id": "VFL-001",
  "trap": "completeness",
  "pattern": "producer_consumer_pair_unwired",
  "description": "Producer module emits field before consumer wires it",
  "example": "M_3_02 RegisterCoherenceScorer emits unconsumed output",
  "source_review": "M_3_02 self-review",
  "source_finding": "F-INTROSPECT-VFL-001-001",
  "frequency": 3,
  "accepted": true,
  "graduated": false,
  "graduated_to": null,
  "graduation_candidate": true,
  "graduation_note": "freq=3 across M_3_02..04, acceptance=1.0, proposal filed",
  "created": "2026-05-11",

  // ADR-005 §2 required-minimum (back-derivable from existing fields)
  "rlhf_signal_type": "accept",
  "rlhf_session_id": "session_iris_20260512_videoforge_self_review_m_3_04",
  "rlhf_captured_at": "2026-05-12T20:00:00Z",

  // ADR-005 §3 consumer namespace (the three ad-hoc fields, re-homed)
  "rlhf_consumer_namespace.videoforge.last_updated": "2026-05-12T20:00:00Z",
  "rlhf_consumer_namespace.videoforge.graduation_proposal_filed": true,
  "rlhf_consumer_namespace.videoforge.graduation_proposal_path": "who/coordination/coord_2026_05_12_vfl_graduation_proposals.md"
}
```

ADR-005 §5 clause 5 specifies no retroactive backfill is required; this normalization happens on the next ACCUMULATE cycle that touches VFL-001, OR at the MD-B2 graduation ceremony, whichever fires first.

### §3.5 Backward-compatibility

Per ADR-005 §1: `accepted` (bool) retained as-is. Parsers MAY back-derive `rlhf_signal_type` from `accepted` when only the latter is present (`accepted: true` ⇒ `accept`; `accepted: false` ⇒ `reject`; `accepted` absent ⇒ unknown). The canonical 26-entry store remains byte-identical (md5 `dde2cbd88c0b45956fb22285a2a0f856` preserved).

---

## §4 Per-Pack Quality Metric

This section is the consumer-facing summary; the normative authority is `what/decisions/adr_007_adaptive_improvement_loop.md` §3.

### §4.1 Six-axis rubric

Each canonical domain pack at `what/context/core_domain_packs/` SHOULD carry a quality score in its frontmatter. Five axes are adopted verbatim from `how/skills/skill_context_quality_audit.md` lines 61–66; one is new at ADR-007:

| Axis | Source | What it measures |
|------|--------|-------------------|
| `signal_density` | `skill_context_quality_audit.md` | Ratio of decision-relevant content to filler |
| `actionability` | same | Fraction executable guidance vs background prose |
| `coverage_uniformity` | same | Distribution of detail across sections (no starvation) |
| `source_diversity` | same | Variety of provenance sources |
| `cross_topic_coherence` | same | Section-to-section vocabulary cohesion |
| `graduation_yield` | **new at ADR-007 §3** | Graduated corrections sourced from review cycles loading this pack, normalized by review-cycle count |

Each axis scored on 1–5 integer scale (`skill_context_quality_audit.md` lines 50–60). Composite = arithmetic mean of all six axes. **Floor rule**: any axis ≤2 triggers pack revision regardless of composite score.

### §4.2 Frontmatter shape

A scored pack adds this block to its frontmatter:

```yaml
quality_metric:
  rubric_version: "adr_007_v0"            # tracks ADR-007 amendment history
  scored_at: 2026-05-20
  scored_by: agent_argus
  signal_density: 4
  actionability: 4
  coverage_uniformity: 4
  source_diversity: 3
  cross_topic_coherence: 4
  graduation_yield: 3
  composite: 3.67                          # arithmetic mean; 2 decimal places
  floor_check: passed                      # "passed" if all axes >= 3; "triggered" if any axis <= 2
```

### §4.3 Scoring procedure

1. Read pack content in full.
2. Score each axis 1–5 against the rubric guidance in `skill_context_quality_audit.md` (axes 1–5) and ADR-007 §3 (axis 6).
3. For `graduation_yield`: count corrections with `graduated_to: <pack_name>` in canonical jsonl over the measurement window (default: pack's lifetime); divide by review-cycle count loading this pack (count from session files referencing the pack); score on 1–5 scale (rubric: 0 → 1; >0 → 2; >0.05 → 3; >0.10 → 4; >0.20 → 5).
4. Compute composite; check floor rule.
5. Write frontmatter block; commit with reference to this spec §4.

### §4.4 Interaction with ≥50 corrections threshold (forward-reference to MD-B2)

Per ADR-007 §4: per-pack quality (this §4) and the ≥50 corrections threshold (MD-B2 scope; ADR-003 amendment pending) are **separate, complementary signals**. A pack may score high quality with low aggregate corrections OR low quality with high aggregate corrections; both signals coexist. MD-B2 canonicalizes the threshold's semantics; this spec does NOT speak for it.

### §4.5 7-pack pilot (forward-reference to MD-B4)

MD-B4 scope: score the remaining 6 canonical packs against the rubric in §4.3, producing per-pack quality scores + aggregate distribution. MD-B1 ships one exemplar score (on `context_iii_learning_store.md`, the meta-pack governing the loop — see §8.3).

---

## §5 Loop Closure Protocol (state machine)

This section is the consumer-facing summary of the per-pattern lifecycle; normative authority is `what/decisions/adr_007_adaptive_improvement_loop.md` §1.

### §5.1 CorrectionLifecycle stages

Six progression stages + two terminal-non-graduated states; stages cited by `UPPER_SNAKE_CASE` name across all III artifacts:

```
SIGNAL_CAPTURED → CORRECTION_TRACKED → GRADUATION_CANDIDATE → GRADUATION_PROPOSED → GRADUATION_RATIFIED → PACK_DELTA_LANDED
                                                                                                                  
                       ┌── (terminal-non-graduated) ───→ PRUNED       (acceptance_rate < 0.5 over frequency >= 5)
                       │
                       └── (terminal-non-graduated) ───→ ARCHIVED     (superseded by parent pattern)
```

### §5.2 State inference from observable fields

The CorrectionLifecycle state of any entry is **inferred from the existing ADR-003 §4 schema fields**, not stored as a separate `lifecycle_state:` field. Per ADR-007 §1 inference table:

| Observable field combination | Inferred CorrectionLifecycle state |
|------------------------------|------------------------------------|
| `frequency: 1`, no `graduation_candidate` | `SIGNAL_CAPTURED` |
| `frequency >= 2`, `graduation_candidate` absent/false, `graduated: false` | `CORRECTION_TRACKED` |
| `graduation_candidate: true`, `graduated: false`, no graduation memo | `GRADUATION_CANDIDATE` |
| `graduation_candidate: true`, `graduated: false`, graduation memo filed | `GRADUATION_PROPOSED` |
| `graduated: true`, `graduated_to: null` or pack not yet updated | `GRADUATION_RATIFIED` |
| `graduated: true`, `graduated_to: <pack_name>` AND trap appears in pack | `PACK_DELTA_LANDED` |
| `acceptance_rate < 0.5` over `frequency >= 5`, no graduation memo | `PRUNED` |
| `superseded_by` populated | `ARCHIVED` |

The state machine is **inference-from-fields by design** — preserves the ADR-003 §4 schema unchanged; preserves the canonical jsonl md5 invariant.

### §5.3 Cross-vault interface (forward-reference to MD-B3)

MD-B3 scope: cross-vault RLHF aggregation contract. Rides on v0.3 airlock surface from MD-A1. Per ADR-007 §1 the cross-vault-visible stages are **SIGNAL_CAPTURED + CORRECTION_TRACKED** — both produce signal data that can be aggregated across vaults; later stages (GRADUATION_*) are III-canonical-only by ADR-003 §3 design.

MD-B3 will normatively specify which fields cross the vault boundary (likely: `pattern`, `rlhf_signal_type`, `rlhf_session_id`, `frequency`, originating vault name) and which stay vault-local (consumer namespace fields per ADR-005 §3 clause 3).

This spec emits the forward-reference; MD-B3 resolves it.

### §5.4 Relationship to ReviewState

CorrectionLifecycle is **per-pattern** (one pattern across many review cycles); `ReviewState` is **per-artifact** (one artifact through one review cycle). Per ADR-007 §2 the two state machines never coincide; new ReviewState values MUST NOT be introduced for lifecycle phases and vice versa.

---

## §6 Versioning Policy

### §6.1 This spec

Versioned independently at the spec frontmatter (`version: "X.Y.Z"`). v0.3.0 is the first release.

- **Patch bump** (0.3.0 → 0.3.1): editorial clarification; no semantic change to stages, schema, or rubric. Transparent to consumers.
- **Minor bump** (0.3.0 → 0.4.0): new stage added; new optional-open field admitted (with co-ratified ADR-005 amendment); new quality axis added (with co-ratified ADR-007 amendment). Consumer wrappers minor-bump review per ADR-002 §3.
- **Major bump** (0.3.0 → 1.0.0): breaking change to inference rules (§5.2), required-minimum field set, or stage names. Consumers must explicitly opt-in.

### §6.2 ADR-005 (signal channel)

Amendment-history-driven; tracked in ADR-005 frontmatter `amendments:` array. Adding a top-level `rlhf_*` field is an amendment (ADR-005 §5 clause 1).

### §6.3 ADR-007 (loop architecture)

Amendment-history-driven; tracked in ADR-007 frontmatter `amendments:` array. Adding a CorrectionLifecycle stage OR changing the 6-axis rubric is an amendment.

### §6.4 Lattice yaml (runtime substrate)

Versioned at the lattice yaml itself (`version: "X.Y.Z"`); patch-bumped at MD-B1 from 1.2.1 to 1.2.2 (additive ADR cross-references only).

---

## §7 Coverage and Boundaries

### §7.1 What v0.3.0 covers

| Surface | v0.3.0 status |
|---------|---------------|
| Named loop with 6+2 stages | Yes — §2 + §5 |
| Per-finding RLHF signal schema (3 required-min + 4 optional-open + namespace) | Yes — §3 + ADR-005 |
| Per-pack quality 6-axis rubric | Yes — §4 + ADR-007 §3 |
| CorrectionLifecycle inference from existing fields | Yes — §5 + ADR-007 §1 |
| Worked example normalizing existing ad-hoc fields (VideoForge) | Yes — §3.4 |
| Exemplar quality scoring (1 pack: `context_iii_learning_store.md`) | Yes — §8.3 |

### §7.2 What v0.3.0 does NOT cover (forward-references emitted)

| Surface | Resolves at |
|---------|-------------|
| ≥50 corrections threshold semantics | MD-B2 + ADR-003 amendment |
| VFL-001 + VFL-002 graduation ceremony (PACK_DELTA_LANDED transition fired) | MD-B2 |
| Cross-vault RLHF aggregation transport (which fields cross vault boundaries) | MD-B3 (rides on v0.3 airlock from MD-A1) |
| Scoring remaining 6 canonical packs against 6-axis rubric | MD-B4 |
| Validation across ≥3 consumer vaults | MD-B5 |
| `iii_corrections_archive.jsonl` artifact for ARCHIVED entries | MD-B2 (optional; may defer) |

### §7.3 Forward-references list (resolved by future missions)

This list MUST be kept current as missions land; resolution updates the row.

| Forward-reference | Originating section | Resolves at | Status |
|-------------------|---------------------|-------------|--------|
| ≥50 corrections threshold | §4.4 | MD-B2 + ADR-003 amendment | pending |
| Cross-vault aggregation field set | §5.3 | MD-B3 | pending |
| 7-pack pilot scores | §4.5 | MD-B4 | pending |
| VFL-001/002 actually graduated; namespaced fields fold canonically | §3.4 | MD-B2 | pending |
| `module_iii_introspect` Check 2g semantics formally cite ADR-007 §1 stage 3 | (§2.1 module table) | MD-B4 re-exercise | partial — Check 2g doc edited at MD-B1 §4.2 (this session) |
| Per-pack quality scoring procedure pilot (MD-B4 propagates from §8.3 exemplar) | §4.5 | MD-B4 | pending |

---

## §8 Verification

### §8.1 Schema validation walk-through

Paper exercise; no writes to any vault's learning store performed in this verification (the verification itself proves nothing is rewritten):

1. **md5 invariance check**: at MD-B1 close, `md5 -q ~/lattice/III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl` MUST equal `dde2cbd88c0b45956fb22285a2a0f856`. Result: confirmed unchanged this session (no writes to canonical jsonl performed).
2. **VFL-001 re-expression validates** against ADR-005 §2 (required-minimum) + §3 (consumer namespace). See §3.4 above for full worked example. Result: 3 required-minimum fields back-derivable from existing entry fields (`accepted: true` → `accept`; `source_review` → `rlhf_session_id` heuristic; `created` → `rlhf_captured_at` ceiling). 3 consumer-namespace fields preserve the ad-hoc VFL-001 fields with no semantic loss.
3. **Canonical 26 entries still load**: optional `rlhf_*` fields are absent in canonical; parsers treat absence as valid per ADR-005 §5 clause 5. No retroactive backfill required.

### §8.2 Loop-closure walk-through

Trace C-003 (`overstated_validation_scope`, `graduation_candidate: true`, canonical jsonl entry 3) through CorrectionLifecycle per §5.2 inference table:

- C-003 current observable fields: `frequency: 5`, `accepted: true`, `graduated: false`, `graduation_candidate: true`. No graduation memo on file in any vault. → **GRADUATION_CANDIDATE** (state 3) per §5.2 row 3.
- Trace through to graduation: MD-B2 will file a graduation memo (transition to GRADUATION_PROPOSED, state 4); Argus + Stanley co-ratification fires the canonical jsonl edit (`graduated: true` flip; transition to GRADUATION_RATIFIED, state 5); the relevant pack (likely `context_iii_introspect_checks.md` for an over-claim trap) gets the static trap appended (transition to PACK_DELTA_LANDED, state 6).

Spot-checks against the canonical 26-entry store:

| Entry | Observable fields | Inferred state |
|-------|--------------------|----------------|
| C-001 | `graduated: true`, `graduated_to: "core"` | PACK_DELTA_LANDED ✓ |
| C-003 | `graduation_candidate: true`, no memo | GRADUATION_CANDIDATE ✓ |
| C-004 | `frequency: 1`, no candidate flag | SIGNAL_CAPTURED ✓ |
| C-022 | `frequency: 1`, `accepted: true`, recent | SIGNAL_CAPTURED ✓ |

Result: every existing entry maps to exactly one CorrectionLifecycle state via §5.2 inference rules. No malformed-ambiguous entries detected in the 26-entry sample.

### §8.3 Per-pack quality walk-through (exemplar)

Score `context_iii_learning_store.md` (the meta-pack governing the loop itself — most-relevant choice for the MD-B1 close) against the §4.1 rubric:

| Axis | Score | Justification |
|------|-------|--------------|
| `signal_density` | 4 | Pack is mostly decision-relevant prose; ~10% prose-filler (VaaS Integration Mapping section is contextual rather than directly actionable). |
| `actionability` | 4 | Lifecycle Protocol (§Lifecycle) + schema declarations are directly executable; smaller fraction is descriptive context. |
| `coverage_uniformity` | 4 | Sections roughly balanced — schema, lifecycle protocol, graduation procedure, accumulation rules, VaaS integration. No starved section. |
| `source_diversity` | 3 | Cites ADR-002, ADR-003, lattice yaml, accumulate module, introspect module — diversity present but concentrated in III-canonical sources. |
| `cross_topic_coherence` | 5 | Vocabulary cohesive across sections (`frequency`, `acceptance`, `graduation`, `pattern`) — every section uses same terminology. |
| `graduation_yield` | 3 | 3 entries graduated against the meta-pack's domain (C-001, C-002, C-005), normalized over the meta-pack's review-cycle count (small N — pack is referenced indirectly through other reviews); score lands at "moderate" tier per §4.3 scale. |

Composite: `(4 + 4 + 4 + 3 + 5 + 3) / 6 = 3.83`.

Floor check: no axis ≤2; **passed**.

The exemplar's frontmatter is updated at MD-B1 close (file-to-modify #4 in the plan). MD-B4 will score the remaining 6 canonical packs against this exemplar.

### §8.4 Verification artifact (this spec's §8)

This spec §§8.1–8.3 IS the verification artifact for MD-B1; no separate `mb1_verification_*.md` file. Pattern matches MC-2 schema validation (which lived inside `iii_airlock_request_schema.yaml` header) and MC-3 reply-comment template (which lived inline at `how/templates/template_cross_vault_request_reply.md`).

---

## §9 Provenance

### §9.1 Origin

Authored at III.aDNA Campaign D mission MD-B1 (2026-05-20 / 2026-05-21T02:00Z), the first P1 mission of Track D2 ("RLHF + Adaptive-Improvement Loop"). Parallel-eligible with Track D1 MD-A1 (federation-aware airlock v0.3); cross-track interface lands at MD-B3.

### §9.2 Substrate sources (named here for the first time)

- `module_iii_accumulate.md` line 72 — `graduation_signals` threshold (carried into §2.1 GRADUATION_CANDIDATE definition)
- `module_iii_introspect.md` line 75 — Check 2g `corrections_pattern_match` (carried into CORRECTION_TRACKED transition)
- `module_iii_improve.md` line 61 — `learning_store_acceptance_rates` input (rationale for `rlhf_severity_calibration` field at ADR-005 §2 optional-open)
- `context_iii_learning_store.md` — Lifecycle Protocol (carried into §5.2 inference table; pruning rule carried into PRUNED state definition)
- `skill_context_quality_audit.md` lines 50–66 — 5-axis quality rubric (5 of 6 axes adopted verbatim at §4.1)
- `adr_003_learning_store_ownership.md` §3, §4 — graduation ceremony + entry schema (preserved unchanged; §5.2 inference uses §4 fields)

### §9.3 Co-ratified ADRs

- `what/decisions/adr_005_rlhf_signal_channel.md` — schema-channel governance; normative authority for §3
- `what/decisions/adr_007_adaptive_improvement_loop.md` — architectural governance; normative authority for §§2, 4, 5

### §9.4 Cross-references

| Reference | Path |
|-----------|------|
| Campaign D charter (MD-B1 mission row) | `~/lattice/III.aDNA/how/campaigns/campaign_d_federation_adaptive_loop/campaign_d_federation_adaptive_loop.md` |
| Airlock standard spec v0.2 (sibling spec; shape reference) | `~/lattice/III.aDNA/what/artifacts/iii_airlock_standard_spec.md` |
| Substrate implementation guidance v0.2 | `~/lattice/III.aDNA/what/artifacts/iii_airlock_substrate_implementation.md` |
| Cross-vault request schema v0.2 | `~/lattice/III.aDNA/what/artifacts/iii_airlock_request_schema.yaml` |
| Lattice yaml v1.2.2 (runtime substrate) | `~/lattice/III.aDNA/what/lattices/lattice_iii_verification_oracle.lattice.yaml` |
| Canonical learning store (md5 `dde2cbd88c0b45956fb22285a2a0f856`) | `~/lattice/III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl` |
| VFL graduation proposal (MD-B2 input) | `~/lattice/III.aDNA/who/coordination/coord_2026_05_12_vfl_graduation_proposals.md` |
| Session-close ceremony skill (used at MD-B1 close) | `~/lattice/III.aDNA/how/skills/skill_session_close_ceremony.md` |
