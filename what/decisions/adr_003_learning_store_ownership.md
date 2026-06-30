---
type: decision
adr_id: "003"
title: Learning Store Ownership — canonical upstream, per-vault forks, graduation ceremony
status: ratified
created: 2026-05-07
updated: 2026-05-21
last_edited_by: agent_argus
signed_by: Stanley (Chief Steward)
amendments:
  - date: 2026-05-12
    mission: campaign_b_iii_federation MB-7
    summary: §4 schema aligned with live canonical jsonl (field names + boolean acceptance)
  - date: 2026-05-20
    mission: campaign_d_federation_adaptive_loop MD-B1
    summary: §3.5 added (Lifecycle scoping — ADR-003 §3 ceremony is the GRADUATION_PROPOSED → GRADUATION_RATIFIED → PACK_DELTA_LANDED transitions of ADR-007 §1; earlier loop stages governed by ADR-007); §5 graduation-count corrected from "5 already-graduated" to "3 already-graduated (C-001, C-002, C-005)" — documentation reconciliation against the live canonical jsonl (md5 dde2cbd88c0b45956fb22285a2a0f856 invariant preserved)
  - date: 2026-05-21
    mission: campaign_d_federation_adaptive_loop MD-B2
    summary: §3.6 added (≥50 corrections elevated-scrutiny queue; standard §3 ceremony + independent observation evidence from ≥2 distinct vaults; prevents single-vault dominance at high volume); §5 post-graduation update — first canonical md5 rotation since founding C-001..C-026 import as VFL-001 + VFL-002 graduate to C-027 + C-028 (md5 rotates from dde2cbd88c0b45956fb22285a2a0f856 to new hash recorded in commit message + this amendment); graduated count 3 → 5 (C-001, C-002, C-005, C-027, C-028)
tags: [adr, learning-store, corrections, graduation, iii, adaptive_loop_v0_3, md_b2_graduation, elevated_scrutiny_queue]
---

# ADR-003: Learning Store Ownership

## Status

Ratified — 2026-05-07 (Stanley, Chief Steward)

## Context

The III learning store (`iii_corrections.jsonl`) accumulates recurring failure patterns from review cycles. Since migration to III.aDNA, the canonical store lives at `III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl`. However, multiple consumers accumulate local corrections in their own vault's `how/federation/iii/what/context/<vault>_iii_learning_store.jsonl`.

This creates an ownership question: which store is authoritative? When should local corrections graduate to the canonical store? Who approves?

Pre-migration state: lattice-labs holds the only `iii_corrections.jsonl` (26 entries, 5 graduated). Post-migration, III.aDNA is the canonical upstream.

## Decision

### 1. Canonical upstream

`III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl` is the **canonical upstream learning store**. It is the single source of truth for cross-vault pattern knowledge.

Every consumer's local `iii_learning_store.jsonl` is a **downstream fork**. Local forks are updated by consumer-vault review cycles; they do not automatically propagate upstream.

### 2. Per-vault local stores

Consumers maintain a local store at `how/federation/iii/what/context/<vault>_iii_learning_store.jsonl`. Format is identical to the canonical store schema. Local store accumulation happens during normal ACCUMULATE phase per skill_iii_review.md Step 3b.

On load: a consumer session loads both the canonical store (≤ 50 entries) AND the local store (≤ 20 entries). Local entries take precedence if pattern names conflict (consumer knows its domain better).

### 3.5 Lifecycle scoping (added MD-B1 2026-05-20)

This ADR's §3 graduation ceremony corresponds to the **GRADUATION_PROPOSED → GRADUATION_RATIFIED → PACK_DELTA_LANDED** transitions of the III CorrectionLifecycle state machine defined at `adr_007_adaptive_improvement_loop.md` §1. Earlier lifecycle stages (SIGNAL_CAPTURED, CORRECTION_TRACKED, GRADUATION_CANDIDATE) and terminal-non-graduated states (PRUNED, ARCHIVED) are governed by ADR-007. Per-finding RLHF signal capture (the field schema additively extending §4 below) is governed by `adr_005_rlhf_signal_channel.md`. These three ADRs (003 + 005 + 007) form the coordinated learning-store + signal-channel + adaptive-loop governance set.

### 3. Graduation ceremony

A local correction graduates to the canonical store when:
- `frequency >= 3` across at least 2 different consumer sessions
- `acceptance_rate >= 0.80` (80% of review cycles where this correction was surfaced, the user accepted the improvement)
- Pattern is domain-general (applies in more than one vault context)
- Not already represented in the canonical store

**Ceremony process**:
1. Consumer agent detects graduation candidate (automated check in ACCUMULATE step)
2. Agent proposes graduation to user: "Correction C-NNN `<pattern_name>` is a candidate for promotion to III.aDNA canonical store."
3. User approves or defers
4. On approval: PR or direct edit to `iii_corrections_canonical.jsonl` in III.aDNA repo; bump III.aDNA patch version
5. Canonical store entry gets `graduated_from: <vault>` field to track provenance
6. Local store entry gets `graduated: true` to indicate it's now in canonical

### 3.6 ≥50 corrections elevated-scrutiny queue (added MD-B2 2026-05-21)

For high-frequency patterns, §3's standard ceremony (frequency ≥ 3, ≥ 2 sessions, acceptance ≥ 80%) is necessary but not sufficient. When a pattern's `frequency` summed across the canonical store and all consumer-fork local stores reaches **≥ 50 observations**, the pattern enters an **elevated-scrutiny queue** requiring BOTH:

1. **Standard ADR-003 §3 ceremony** — Stanley + Argus co-ratification, all §3 criteria green; AND
2. **Independent observation evidence from ≥ 2 distinct vaults** — the pattern must have been independently surfaced in at least 2 separate consumer vaults (not just the originating vault's accumulated history). This prevents a single dominant vault from forcing canonical graduation purely on its own volume.

**Mechanism**:
- `module_iii_accumulate` SHOULD emit an additional `elevated_scrutiny_signal` whenever the cross-fork frequency threshold is met (mechanical aggregation deferred to MD-B4 or later — this ADR canonicalizes the policy; the runtime emit-hook is a substrate enhancement).
- At graduation ceremony, Argus MUST enumerate the ≥2-vault evidence as part of the ratification record (e.g., reply memo + canonical entry's commit message both cite each contributing vault's local-store entry IDs).
- If evidence comes from only one vault despite ≥50 observations, the candidate stays at GRADUATION_CANDIDATE per ADR-007 §1; promotion blocks until a second vault independently observes the pattern.

**Rationale**: at low volume (frequency 3–10), §3's standard ceremony is calibrated — patterns proven across multiple sessions, accepted at high rate, deserve canonical promotion. At high volume (frequency ≥ 50), single-vault dominance becomes a risk: a vault running many self-reviews of a similar artifact-class can accumulate observations without ever showing the pattern is genuinely cross-domain. The ≥2-vault evidence requirement forces "pattern generality" to be demonstrated by independent observation, not merely asserted by single-vault aggregation.

**Relationship to ADR-007 §4**: per ADR-007 §4, per-pack quality (6-axis rubric) and the ≥50 corrections threshold (this §3.6) are **separate, complementary signals**. Per-pack quality measures pack content; ≥50 elevated-scrutiny measures cross-vault evidence convergence. Both signals coexist; neither subsumes the other. ADR-007 confirmed the threshold is a separate signal; this §3.6 canonicalizes the threshold's enforcement semantics.

**Worked example** (none yet — first elevated-scrutiny graduation will fire when a pattern crosses 50): C-001 `projective_claim_as_fact` at canonical frequency 8 + (consumer-fork accumulation, currently un-aggregated) — when cross-fork aggregation lands at MD-B4 or later, C-001 + C-002 + C-005 + C-018 (canonical frequency 6) become the first candidates for elevated-scrutiny re-graduation review. None today exceed 50 in canonical alone; aggregation across forks may surface the first candidate.

### 4. Correction entry schema

All entries (canonical and local) follow this schema:

```jsonl
{
  "id": "C-NNN",
  "trap": "confidence | structure | brand | ...",
  "pattern": "snake_case_pattern_name",
  "description": "One-line description of the error pattern",
  "example": "Concrete worked example from a real review",
  "source_review": "Review name / report ID where first surfaced",
  "source_finding": "Finding ID (e.g., F-TEXT-004)",
  "frequency": 3,
  "accepted": true,
  "graduated": false,
  "graduated_to": "core | <pack_name> | null",
  "created": "YYYY-MM-DD",
  "graduated_date": "YYYY-MM-DD (present iff graduated:true)"
}
```

**Field semantics**:
- `trap` — the trap category (canonical-pack-level grouping). Predates and supersedes the spec'd `trap_pack` field name.
- `accepted` is **boolean**, not a numeric rate. A correction is `accepted: true` if the user accepted the IMPROVE phase's suggested fix in the originating review cycle. Cross-session acceptance accumulation is not currently captured at the schema level.
- `graduated_to` records the **target pack** the correction promoted into (typically `"core"` for canonical-store entries that have been folded into a domain pack). Predates and supersedes the spec'd `graduated_from` direction. Provenance (which consumer originally surfaced the correction) is captured at graduation ceremony time via the PR / commit message, not a schema field.
- `source_review` + `source_finding` provide forensic provenance; both populate at correction creation.

Consumer local stores follow the same schema. Loading protocol (ADR-002 §4): canonical ≤ 50 entries + local ≤ 20 entries; pattern-name conflicts resolved local-wins.

### 5. Pre-migration entries (C-001 through C-026)

The 26 entries migrated from lattice-labs are the founding canonical set. Provenance to `lattice-labs` is captured in the per-entry `migration_provenance` block in each pack's `[MIGRATED]` stub plus the canonical store's founding `iii_corrections_canonical.jsonl` md5 (`dde2cbd88c0b45956fb22285a2a0f856` — the founding-import baseline, preserved through DG-A + Campaign B + Campaign C + MD-B1 close). **Post MD-B2 2026-05-21**: the canonical store grew 26 → 28 entries (C-027 `producer_consumer_pair_unwired` + C-028 `spec_verbatim_port_to_code`, both graduated from VideoForge VFL-001 + VFL-002 per ADR-003 §3 ceremony at MD-B2 close); md5 rotated to `5adb0dfa38d9224649c3b2cba83852ae` (first canonical md5 rotation since founding import). The **5 currently-graduated entries** are: C-001 `projective_claim_as_fact`, C-002 `aspiration_as_current_capability`, C-005 `unsourced_consequential_statistic` (founding-set graduations; `graduated_to: "core"`), plus C-027 + C-028 (MD-B2 graduations; `graduated_to: "core"`; carry ADR-005 §2 required-min RLHF fields). Candidates C-003 (`overstated_validation_scope`) and C-009 (`derived_view_drift`) remain flagged at `graduation_candidate: true` — deferred to MD-B4 (7-pack pilot) or MD-B2.1 follow-up per operator gate at MD-B2 plan ratification. (Pre-MD-B1 amendment-history this section claimed "5 already-graduated" for the founding-set count; the live jsonl at MD-B1 close 2026-05-20 was 3-graduated; MD-B2 close 2026-05-21 raises the live count to 5-graduated via canonical append, NOT documentation reconciliation. The full audit trail: founding "5 already-graduated" claim was stale documentation at Campaign A → corrected to "3 already-graduated" at MD-B1 → re-elevated to "5 already-graduated" at MD-B2 via real graduation event with md5 rotation.)

### 6. Domain pack graduation (separate from corrections graduation)

When a correction pattern becomes pervasive enough to be part of a domain pack's static trap list (not just a dynamic correction), it follows a separate process: the agent proposes a static trap addition to the relevant domain pack file. This is a file edit to III.aDNA, not a JSONL append. Requires Stanley approval.

## Consequences

- Canonical path established: `III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl`
- Consumer wrappers declare `local_extensions` in their federation_ref (ADR-002)
- ACCUMULATE step (module_iii_accumulate) checks both canonical and local on load; appends only to local
- Graduation detection runs automatically in ACCUMULATE; graduation ceremony is user-gated
- Patch version bumps to III.aDNA on each canonical graduation

## Amendment History

| Date | Mission | Amendment summary |
|------|---------|-------------------|
| 2026-05-12 | campaign_b_iii_federation MB-7 | §4 correction-entry schema aligned with the live canonical jsonl (md5 `dde2cbd88c0b45956fb22285a2a0f856` invariant preserved). Pre-amendment §4 specified `trap_pack` / `acceptance_rate` (numeric) / `graduated_from` (consumer-provenance direction); live entries since the founding C-001..C-026 import used `trap` / `accepted` (boolean) / `graduated_to` (target-pack direction). The drift was a documentation-vs-implementation gap from the Campaign A MA-1 head-start migration; the live shape was always the operational schema. Amendment aligns ADR §4 to live without rewriting any jsonl. §5 prose updated to drop the `graduated_from: lattice-labs` reference (the field never existed in canonical) and point to the canonical md5 invariant instead. |
| 2026-05-20 | campaign_d_federation_adaptive_loop MD-B1 | §3.5 added (new section): Lifecycle scoping — names ADR-003 §3 ceremony as the GRADUATION_PROPOSED → GRADUATION_RATIFIED → PACK_DELTA_LANDED transitions of `adr_007_adaptive_improvement_loop.md` §1 CorrectionLifecycle; earlier stages governed by ADR-007; signal schema additively extended by ADR-005. §5 graduation-count corrected from "5 already-graduated" to "3 already-graduated (C-001, C-002, C-005)" — documentation reconciliation matching the live canonical jsonl (verified at MD-B1 close via `grep -c '"graduated":true'` → 3; same pattern as the 2026-05-12 §4 reconciliation; md5 `dde2cbd88c0b45956fb22285a2a0f856` invariant preserved). Frontmatter `last_edited_by` reflows from `agent_stanley` to `agent_argus` per author identity for the new amendment row; `signed_by: Stanley (Chief Steward)` unchanged — Stanley ratifies the §3.5 addition + §5 correction at MD-B1 close per the charter hard rule "New ADR(s) require Stanley ratification before MD-B1 close." |
| 2026-05-21 | campaign_d_federation_adaptive_loop MD-B2 | §3.6 added (new section): ≥50 corrections elevated-scrutiny queue — high-frequency patterns (cross-fork frequency ≥ 50) require BOTH standard §3 ceremony AND independent observation evidence from ≥2 distinct vaults; prevents single-vault dominance at high volume; complements §3 standard ceremony rather than replacing it. Per-pack quality (ADR-007 §3 6-axis rubric) and ≥50 threshold are separate complementary signals per ADR-007 §4 (this amendment canonicalizes the threshold's enforcement semantics; ADR-007 confirmed they are separate signals). §5 post-graduation update: first canonical md5 rotation since founding C-001..C-026 import — VFL-001 + VFL-002 graduate to C-027 + C-028 per VideoForge.aDNA cross-vault request `coord_2026_05_12_vfl_graduation_proposals.md` (Argus + Stanley co-ratification at MD-B2 close); md5 rotates from `dde2cbd88c0b45956fb22285a2a0f856` to new hash recorded in MD-B2 commit message + STATE.md + this row's amendment summary; graduated count 3 → 5 (C-001, C-002, C-005, C-027, C-028); first PACK_DELTA_LANDED transitions fire same session (VFL-001 → `context_iii_introspect_checks.md` Check 2c; VFL-002 → `context_iii_inspect_procedures.md` INSPECT-Code modality section). Stanley ratifies §3.6 addition at MD-B2 close per the charter hard rule "New ADR amendments require Stanley ratification before mission close." |
