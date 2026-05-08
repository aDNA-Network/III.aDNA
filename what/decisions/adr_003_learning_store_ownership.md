---
type: decision
adr_id: "003"
title: Learning Store Ownership — canonical upstream, per-vault forks, graduation ceremony
status: ratified
created: 2026-05-07
updated: 2026-05-07
last_edited_by: agent_stanley
signed_by: Stanley (Chief Steward)
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
  "pattern": "snake_case_pattern_name",
  "description": "One-line description of the error pattern",
  "signal": "How to detect this pattern during INSPECT",
  "fix": "Standard correction approach",
  "frequency": 3,
  "total_reviews": 4,
  "acceptance_rate": 0.75,
  "graduated": false,
  "graduated_from": null,
  "trap_pack": "core | web_design | whitepaper | ...",
  "created": "YYYY-MM-DD",
  "last_seen": "YYYY-MM-DD"
}
```

### 5. Pre-migration entries (C-001 through C-026)

The 26 entries migrated from lattice-labs are the founding canonical set. Their `graduated_from` field is set to `lattice-labs` for provenance. The 5 already-graduated entries (C-001, C-002, C-005 + 2) retain `graduated: true`. Graduation candidates C-003 and C-009 are flagged for review at MA-2.

### 6. Domain pack graduation (separate from corrections graduation)

When a correction pattern becomes pervasive enough to be part of a domain pack's static trap list (not just a dynamic correction), it follows a separate process: the agent proposes a static trap addition to the relevant domain pack file. This is a file edit to III.aDNA, not a JSONL append. Requires Stanley approval.

## Consequences

- Canonical path established: `III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl`
- Consumer wrappers declare `local_extensions` in their federation_ref (ADR-002)
- ACCUMULATE step (module_iii_accumulate) checks both canonical and local on load; appends only to local
- Graduation detection runs automatically in ACCUMULATE; graduation ceremony is user-gated
- Patch version bumps to III.aDNA on each canonical graduation
