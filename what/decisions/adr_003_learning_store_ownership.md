---
type: decision
adr_id: "003"
title: Learning Store Ownership — canonical upstream, per-vault forks, graduation ceremony
status: ratified
created: 2026-05-07
updated: 2026-05-12
last_edited_by: agent_stanley
signed_by: Stanley (Chief Steward)
amendments:
  - date: 2026-05-12
    mission: campaign_b_iii_federation MB-7
    summary: §4 schema aligned with live canonical jsonl (field names + boolean acceptance)
tags: [adr, learning-store, corrections, graduation, iii]
---

# ADR-003: Learning Store Ownership

## Status

Ratified — 2026-05-07 (Stanley, Chief Steward)

## Context

The III learning store (`iii_corrections.jsonl`) accumulates recurring failure patterns from review cycles. Since migration to III.aDNA, the canonical store lives at `III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl`. However, multiple consumers accumulate local corrections in their own vault's `iii/what/context/<vault>_iii_learning_store.jsonl`.

This creates an ownership question: which store is authoritative? When should local corrections graduate to the canonical store? Who approves?

Pre-migration state: lattice-labs holds the only `iii_corrections.jsonl` (26 entries, 5 graduated). Post-migration, III.aDNA is the canonical upstream.

## Decision

### 1. Canonical upstream

`III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl` is the **canonical upstream learning store**. It is the single source of truth for cross-vault pattern knowledge.

Every consumer's local `iii_learning_store.jsonl` is a **downstream fork**. Local forks are updated by consumer-vault review cycles; they do not automatically propagate upstream.

### 2. Per-vault local stores

Consumers maintain a local store at `iii/what/context/<vault>_iii_learning_store.jsonl`. Format is identical to the canonical store schema. Local store accumulation happens during normal ACCUMULATE phase per skill_iii_review.md Step 3b.

On load: a consumer session loads both the canonical store (≤ 50 entries) AND the local store (≤ 20 entries). Local entries take precedence if pattern names conflict (consumer knows its domain better).

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

The 26 entries migrated from lattice-labs are the founding canonical set. Provenance to `lattice-labs` is captured in the per-entry `migration_provenance` block in each pack's `[MIGRATED]` stub plus the canonical store's `iii_corrections_canonical.jsonl` md5 invariant (`dde2cbd88c0b45956fb22285a2a0f856`). The 5 already-graduated entries (C-001, C-002, C-005 + 2 more — see entries with `graduated: true` + `graduated_date` populated) retain their `graduated_to: "core"` value. Graduation candidates C-003 and C-009 remain flagged.

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
| 2026-05-12 | campaign_b_iii_federation MB-7 | §4 correction-entry schema aligned with the live canonical jsonl (md5 `dde2cbd88c0b45956fb22285a2a0f856` invariant preserved). Pre-amendment §4 specified `trap_pack` / `acceptance_rate` (numeric) / `graduated_from` (consumer-provenance direction); live entries since the founding C-001..C-026 import used `trap` / `accepted` (boolean) / `graduated_to` (target-pack direction). The drift was a documentation-vs-implementation gap from the Campaign A MA-1 head-start migration; the live shape was always the operational schema. Amendment aligns ADR §4 to live without rewriting any jsonl. §5 prose updated to drop the `graduated_from: lattice-labs` reference (the field never existed in canonical) and point to the canonical md5 invariant instead.
