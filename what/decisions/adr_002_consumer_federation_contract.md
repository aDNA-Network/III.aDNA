---
type: decision
adr_id: "002"
title: Consumer Federation Contract — wrapper pattern, version policy, federation_ref schema
status: ratified
created: 2026-05-07
updated: 2026-05-07
last_edited_by: agent_stanley
signed_by: Stanley (Chief Steward)
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
  local_extensions:            # Consumer-specific packs declared here
    - what/context/<vault>_iii_domain_pack.md
```

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

- lattice-labs, SiteForge, VideoForge, CanvasForge, wga each get `iii/` wrappers (Campaign B MB-1..MB-4)
- `sf_consumer_wrapper_spec.md` updated to add `iii/` as an optional standard sub-wrapper alongside `siteforge/` (Campaign B MB-2)
- aDNA base template includes `skill_iii_setup.md` to automate wrapper creation (Campaign B MB-5)
- The workspace `CLAUDE.md` "Forge Ecosystem" section gains a note: III.aDNA consumers use `iii/` wrappers
