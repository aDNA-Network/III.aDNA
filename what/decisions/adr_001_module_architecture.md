---
type: decision
adr_id: "001"
title: Module Architecture — 8 composable modules, boundary model, I/O types
status: ratified
created: 2026-05-07
updated: 2026-05-07
last_edited_by: agent_stanley
signed_by: Stanley (Chief Steward)
tags: [adr, modules, architecture, iii]
---

# ADR-001: Module Architecture

## Status

Ratified — 2026-05-07 (Stanley, Chief Steward)

## Context

The existing `skill_iii_review.md` is a monolithic skill file combining dispatch logic, 4 INSPECT modalities, 7 INTROSPECT checks, IMPROVE ordering, and ACCUMULATE into a single document. This works for single-vault consumption but prevents:

- Selective module adoption (e.g., a consumer that only needs text INSPECT + INTROSPECT)
- Independent module versioning (DISPATCH logic can evolve separately from INTROSPECT checks)
- Third-party module extensions (domain experts cannot publish a new modality without forking the whole skill)
- Composability with VaaS-style verification pipelines

The existing lattice definition (`lattice_iii_verification_oracle.lattice.yaml` v1.1.0) already models III as a 9-node composable graph, confirming the architecture is inherently modular.

## Decision

### 1. Eight composable modules

Decompose the III pipeline into 8 formal aDNA modules (each with `.md` + `.module.yaml`):

| Module | Input type | Output type | Notes |
|--------|-----------|-------------|-------|
| `module_iii_dispatch` | ArtifactPath + Frontmatter | InspectionConfig | Pack selection, depth assignment, modality activation |
| `module_iii_inspect_text` | TextArtifact + DomainPack | InspectionReport | Five Traps + pack traps + corrections; VCRIT-compatible |
| `module_iii_inspect_code` | CodeArtifact + DomainPack | InspectionReport | Function names, file paths, CLI commands vs codebase |
| `module_iii_inspect_visual` | VisualArtifact + DomainPack | InspectionReport | Label accuracy, layout hierarchy, voice consistency |
| `module_iii_inspect_data` | DataArtifact + DomainPack | InspectionReport | Schema, statistics vs prose, data quality |
| `module_iii_introspect` | InspectionReport + Corrections | CalibratedReport | 7-check calibration; graduation candidate detection |
| `module_iii_improve` | CalibratedReport + DomainContext | ImprovementPlan | Ranked change orders; user approval gate |
| `module_iii_accumulate` | CompletedCycle | JsonlDelta | Learning store append; graduation candidate flagging |

### 2. Module boundary rules

- **Modules are pure.** A module does not load domain packs — the orchestrating skill loads packs and passes them as inputs. This keeps modules domain-agnostic and testable independently.
- **Modules do not call each other.** The skill file or consuming lattice orchestrates the pipeline. No module imports another.
- **INSPECT modules are parallel.** Dispatch activates the required inspect modalities; they run independently and their outputs merge before INTROSPECT.
- **ACCUMULATE is post-cycle.** It runs after the user responds to IMPROVE. It is never a precondition for subsequent pipeline stages.

### 3. Minimum viable cycle

Consumers may use any subset. The minimum viable improvement cycle is:
```
dispatch → (one or more inspect modules) → introspect → improve
```
ACCUMULATE is recommended but optional for read-only review contexts.

### 4. I/O type vocabulary

Module inputs/outputs use the aDNA canonical type vocabulary (19 types, 4 tiers). Key mappings:
- `TextArtifact` → `structured_text` (Tier 2)
- `CodeArtifact` → `code` (Tier 2)
- `VisualArtifact` → `image` (Tier 4)
- `DataArtifact` → `table` or `json` (Tier 2)
- `InspectionReport`, `CalibratedReport`, `ImprovementPlan`, `InspectionConfig` → `structured_text` (Tier 2, with schema defined in module .yaml)
- `JsonlDelta` → `json` (Tier 2, JSONL format per learning store schema)

### 5. Versioning

Each module versions independently within the III.aDNA version space. The skill file (`skill_iii_review.md`) declares which module versions it orchestrates. This allows patch updates to a single inspect modality without bumping the top-level III.aDNA version.

## Consequences

- Campaign A MA-3 creates 8 module files in `what/modules/`
- Module `.module.yaml` files follow the FAIR metadata standard (`fair:` block with keywords + license)
- The existing lattice definition continues to reference the modular architecture
- Consumers can declare partial module consumption in their `federation_ref` block (field: `modules_used`)
- Third-party modules conforming to this I/O contract may be published and referenced by consumers without requiring III.aDNA core changes
