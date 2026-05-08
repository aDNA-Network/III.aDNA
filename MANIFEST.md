---
type: manifest
role: project
created: 2026-05-07
updated: 2026-05-07
last_edited_by: agent_stanley
tags: [manifest, governance, iii, framework]
---

# III.aDNA — Project Manifest

## Project Identity

**III.aDNA** — The standalone modular quality improvement framework for the Lattice ecosystem. III implements a three-phase loop — **Inspect** (adversarial precision check), **Introspect** (structural pattern calibration), **Improve** (ranked change orders + learning accumulation) — consumable by any vault or agent via lightweight federation wrappers.

**Category**: Framework.aDNA — vaults that define protocols and methodologies consumed by other context graphs. III.aDNA is the first Framework.aDNA project in the Lattice ecosystem.

**Persona**: Argus Panoptes — the all-seeing giant.

**Version**: v0.1.0 (pre-federation baseline, 2026-05-07)

**GitHub**: TBD (LatticeProtocol/III.aDNA planned)

## Architecture

```
III.aDNA/
├── how/
│   ├── airlock/          # External-agent entry point
│   ├── skills/           # Core skill + consumer onboarding skill
│   └── campaigns/        # Genesis, federation integration, airlock propagation
├── what/
│   ├── context/
│   │   └── core_domain_packs/   # 7 canonical packs + learning store
│   ├── decisions/         # ADRs 000–004 (founding set)
│   ├── modules/           # 8 composable III modules
│   └── lattices/          # lattice_iii_verification_oracle.lattice.yaml
└── who/
    └── governance/
```

## Entry Points

| If you want to… | Start here |
|-----------------|-----------|
| Run III on a document (any domain) | `how/skills/skill_iii_review.md` |
| Understand which packs to load | `how/airlock/AIRLOCK.md` |
| Add III to a new vault | `how/skills/skill_iii_setup.md` |
| Use III as a composable VaaS oracle | `what/lattices/lattice_iii_verification_oracle.lattice.yaml` |
| Understand the module boundary model | `what/decisions/adr_001_module_architecture.md` |
| Publish a new domain pack | `what/decisions/adr_003_learning_store_ownership.md` (graduation protocol) |

## Active Consumers

| Vault | Wrapper | Packs used | Status |
|-------|---------|-----------|--------|
| `lattice-labs` | `lattice-labs/iii/` | whitepaper, kinn_branding, vault_maintenance, canvas_visual | MB-1 (pending) |
| `SiteForge.aDNA` | `SiteForge.aDNA/iii/` | web_design | MB-2 (pending) |
| `VideoForge.aDNA` | `VideoForge.aDNA/iii/` | web_design, + ADR-006 extension | MB-3 (pending) |
| `CanvasForge.aDNA` | `CanvasForge.aDNA/iii/` | canvas_visual | MB-4 (pending) |
| `wga.aDNA` | `wga.aDNA/iii/` | whitepaper, educational_content (stub) | MB-4 (pending) |

## Originating System

Migrated from `lattice-labs/how/skills/skill_iii_review.md` (created 2026-04-01, campaign_iii_evolution COMPLETED 2026-04-03). Learning store: 26 corrections accumulated, 5 graduated to domain packs. Cross-vault III campaigns active at migration: whitepaper_iii_deep_review (M02/25 cycles), campaign_kinn_branding_iii (50/100 cycles).
