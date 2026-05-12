---
type: decision
adr_id: "002"
title: Consumer Federation Contract — wrapper pattern, version policy, federation_ref schema
status: ratified
created: 2026-05-07
updated: 2026-05-12
last_edited_by: agent_stanley
signed_by: Stanley (Chief Steward)
amendments:
  - date: 2026-05-12
    mission: campaign_b_iii_federation MB-7
    summary: §1 formal `kind:` enum for `local_extensions` (5 values + extension policy)
tags: [adr, federation, consumer, iii]
---

# ADR-002: Consumer Federation Contract

## Status

Ratified — 2026-05-07 (Stanley, Chief Steward)

## Context

Multiple vaults already consume III capabilities informally — lattice-labs via skill path, SiteForge via embedded domain pack, VideoForge via ADR-006 operation catalog. This informal consumption creates:
- No version tracking (consumers can't tell when III changes)
- No isolation (lattice-labs III context is entangled with lattice-labs governance)
- No discoverability (external agents don't know where to find III context)

The SiteForge consumer wrapper pattern (from `sf_consumer_wrapper_spec.md`) is the proven model: lightweight `<forge>/` directory in the consumer vault with `federation_ref:` blocks + consumer-specific configuration. Apply that pattern to III.

## Decision

### 1. Consumer wrapper structure

Consumer vaults create an `iii/` lightweight wrapper directory:

```
consumer_vault.aDNA/
└── iii/
    ├── CLAUDE.md                              # Required: federation_ref + consumer config
    └── what/context/                          # Optional: consumer-specific extensions
        ├── <vault>_iii_domain_pack.md         # Vault-specific trap pack (recommended)
        ├── <vault>_iii_reviewers.yaml         # Adapted multi-voice reviewers (optional)
        └── <vault>_iii_learning_store.jsonl   # Local downstream corrections (optional)
```

### 2. `federation_ref` schema (III-specific)

Required fields in consumer `iii/CLAUDE.md`:

```yaml
federation_ref:
  source_vault: III.aDNA
  source_skill: how/skills/skill_iii_review.md
  version: "X.Y.Z"            # III.aDNA version pinned at wrapper creation
  version_policy: minor        # "minor" (review on bump) | "locked" (manual only)
  packs_used:                  # Which III.aDNA core packs this consumer loads
    - context_iii_inspect_procedures
    - context_iii_introspect_checks
    - context_iii_domain_packs_web_design   # example
  modules_used:                # Optional: which modules if partial consumption
    - module_iii_dispatch
    - module_iii_inspect_text
    - module_iii_introspect
    - module_iii_improve
  local_extensions:            # Consumer-specific extensions declared here
    - kind: domain_pack        # See § "local_extensions kind enum" below
      path: what/context/<vault>_iii_domain_pack.md
      rationale: <one-line consumer-specific justification>
```

### 1a. `local_extensions` `kind` enum

The `kind:` field discriminates the structural role of each `local_extension` entry. Five values are valid; each entry must declare exactly one. Live across the five Campaign B P2 wrappers (lattice-labs MB-1, SiteForge MB-2, VideoForge MB-3, CanvasForge MB-4, wga MB-5):

| `kind:` | Role | Introduced at | Example |
|---------|------|--------------|---------|
| `domain_pack` | Consumer-specific trap pack extending (not replacing) canonical packs. Subject to graduation ceremony per ADR-003 §3 if domain-general. | MB-1 (lattice-labs KINN brand-voice pack) | `path: ~/lattice/lattice-labs/iii/what/context/context_iii_kinn_branding.md` |
| `reviewer_registry` | YAML registry of named voices for multi-voice review composition (e.g., Voice Critic / Design / UX / SEO / Brand). Consumed by `module_iii_semantic_reviewer` (composite_reference) or an equivalent consumer-side orchestrator. | MB-2 (SiteForge 5-voice registry) | `path: ~/lattice/SiteForge.aDNA/what/context/siteforge/siteforge_reviewers.yaml` |
| `bridge_pack` | Pointer-only domain pack that bridges III canonical procedures to a consumer-specific operation catalog or ADR-defined operation set. **Carries `not_graduating_to_canonical: true`** per ADR-002 §6 modality-agnostic-core boundary. Consumer-side only; never absorbed into core. | MB-3 (VideoForge ADR-006 catalog bridge), reused at MB-4 (CanvasForge canvas pack) | `path: ~/lattice/VideoForge.aDNA/iii/what/context/videoforge_iii_domain_pack.md` |
| `local_skill` | Consumer-specific review skill that composes multi-voice voice definitions + modality-specific dispatch logic. **Does not replace** the canonical III skill at `federation_ref.source_skill`; supplies consumer-side orchestration the canonical skill cannot know. | MB-4 (CanvasForge 5-voice canvas review skill) | `path: ~/lattice/CanvasForge.aDNA/how/skills/skill_canvas_iii_review.md` |
| `learning_store_local` | Per-vault JSONL fork of the canonical learning store. ACCUMULATE writes target this file (never canonical). Schema per ADR-003 §4. Required for any consumer that runs ACCUMULATE cycles. | MB-1 (lattice-labs local store); now in all 5 wrappers | `path: ~/lattice/wga.aDNA/iii/what/context/wga_iii_learning_store.jsonl` |

**Extension policy**:

1. **New `kind:` values require an ADR amendment.** A consumer that needs a structurally new extension shape (not just a new instance of an existing kind) must (a) coordinate via the v0.2 cross-vault request surface to authorize the new kind, then (b) propose an amendment to this ADR co-registering it in the table above. The consumer wrapper SHOULD NOT ship the new kind until the amendment lands.
2. **New instances of existing kinds are additive.** A consumer freely declares additional `domain_pack` / `bridge_pack` / `local_skill` / `learning_store_local` / `reviewer_registry` entries; no ADR amendment needed.
3. **Co-registration at minor bump.** When a new kind is introduced informally during a wrapper authoring session (as `bridge_pack` was at MB-3 and `local_skill` was at MB-4), the III.aDNA-side `kind` registry amendment SHOULD land at or before the next minor version bump — not later. This ADR's amendment history is the audit log.
4. **Each entry carries a `rationale:` field.** Consumer-side justification (one line). Reviewers use this when minor-bumping their wrapper version pin per ADR-002 §3.
5. **`not_graduating_to_canonical: true` is permitted on any kind.** Default is unstated (graduation-eligible per ADR-003 §3 if the kind is `domain_pack`). Setting it `true` is the standard mechanism for consumer-specific content that intentionally stays consumer-side (bridges, modality-specific dispatch, brand-voice packs).


### 3. Version policy

- **minor**: Consumer reviews when III.aDNA bumps minor version. Consumer agent reads the III.aDNA CHANGELOG diff and decides whether to update the pinned version. Default for most consumers.
- **locked**: Consumer only updates via explicit human decision. For consumers in highly stable production configurations (e.g., ongoing long-running campaigns).
- **Patch bumps** (X.Y.Z → X.Y.Z+1): transparent to consumers; no review required; applies automatically on next session.

### 4. Core pack loading protocol

When a consumer session invokes III review:
1. Load consumer `iii/CLAUDE.md` to determine `packs_used`
2. For each declared pack, load from `III.aDNA/what/context/core_domain_packs/<pack_name>.md`
3. Load `iii_corrections_canonical.jsonl` (always; caps at 50 entries per token budget)
4. Load any `local_extensions` declared in consumer's federation_ref
5. Proceed to DISPATCH using merged pack set

### 5. Consumer-specific domain packs

Consumer packs extend the core packs; they do not replace them. Format: follow the domain pack schema from `context_iii_learning_store.md` (trap format: `name`, `description`, `signal`, `fix`). Consumer packs live in the consumer vault's `iii/what/context/` directory and are NOT part of III.aDNA canonical packs.

Graduation path: when a consumer-pack trap accumulates frequency ≥ 3 with acceptance ≥ 80% across multiple consumer vaults, the trap is a candidate for promotion to III.aDNA core. This requires ADR-003 graduation ceremony PR.

### 6. What consumers do NOT do

- Never copy or embed III skill/module files into the consumer vault
- Never modify the canonical learning store (`iii_corrections_canonical.jsonl`) directly — use ADR-003 PR process
- Never hard-code III core paths in campaign/session files — always route through `iii/CLAUDE.md` federation_ref

### 7. Existing consumer migration path

Pre-existing informal consumers migrate to this pattern in Campaign B:
1. Create `iii/CLAUDE.md` with federation_ref pinned at III.aDNA v0.1.0
2. Move vault-local III context into `iii/what/context/` as consumer extensions
3. Add forward stub in the vault's existing III context path pointing to `iii/CLAUDE.md`
4. Verify active campaigns can resolve III context via the new path

## Consequences

- lattice-labs, SiteForge, VideoForge, CanvasForge, wga each get `iii/` wrappers (Campaign B MB-1..MB-5)
- `sf_consumer_wrapper_spec.md` updated to add `iii/` as an optional standard sub-wrapper alongside `siteforge/` (Campaign B MB-2)
- aDNA base template includes `skill_iii_setup.md` to automate wrapper creation (Campaign B MB-6)
- The workspace `CLAUDE.md` "Forge Ecosystem" section gains a note: III.aDNA consumers use `iii/` wrappers

## Amendment History

| Date | Mission | Amendment summary |
|------|---------|-------------------|
| 2026-05-12 | campaign_b_iii_federation MB-7 | §1 formalized the `local_extensions` `kind:` enum (5 values: `domain_pack`, `reviewer_registry`, `bridge_pack`, `local_skill`, `learning_store_local`); added §1a "kind enum" table + extension policy. Pre-amendment §1 named two kinds informally (`domain_pack`, `learning_store_local`); MB-2/3/4 introduced `reviewer_registry`, `bridge_pack`, `local_skill` during wrapper authoring with inline rationale + downstream-safety notes; MB-5 confirmed minimal-wrapper baseline introduces no new kinds. Amendment is a documentation reconciliation — all 5 kinds were already live in production wrappers when this amendment landed; no consumer wrapper needs to change. Co-registration backstop for future kind introductions: extension policy clause 3 ("at or before the next minor version bump").
