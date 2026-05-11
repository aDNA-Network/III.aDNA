---
type: manifest
role: project
created: 2026-05-07
updated: 2026-05-10
last_edited_by: agent_stanley
tags: [manifest, governance, iii, framework]
---

# III.aDNA — Project Manifest

## Project Identity

**III.aDNA** — The standalone modular quality improvement framework for the Lattice ecosystem. III implements a three-phase loop — **Inspect** (adversarial precision check), **Introspect** (structural pattern calibration), **Improve** (ranked change orders + learning accumulation) — consumable by any vault or agent via lightweight federation wrappers.

**Category**: Framework.aDNA — vaults that define protocols and methodologies consumed by other context graphs. III.aDNA is the first Framework.aDNA project in the Lattice ecosystem.

**Persona**: Argus Panoptes — the all-seeing giant.

**Version**: `v0.2.0` (cross-vault-request-capable, 2026-05-10) — see § Release History below for full lineage.

**GitHub**: https://github.com/LatticeProtocol/III.aDNA (published at Campaign A wind-down, commit `75aea6f`).

## Architecture

```
III.aDNA/
├── how/
│   ├── airlock/          # External-agent entry point (AIRLOCK.md v0.2.0 reference instance — Entry + Request surfaces)
│   ├── skills/           # Core skill + consumer onboarding skill
│   ├── templates/        # Standard templates + template_cross_vault_request_reply.md (MC-3)
│   └── campaigns/        # Genesis (A), federation (B), airlock standard (C)
├── what/
│   ├── artifacts/        # iii_airlock_standard_spec.md v0.2.0 + iii_airlock_request_schema.yaml (JSON Schema Draft 2020-12)
│   ├── context/
│   │   └── core_domain_packs/   # 7 canonical packs + learning store
│   ├── decisions/         # ADRs 000–004 (founding set)
│   ├── modules/           # 8 composable III modules (.md + .module.yaml each)
│   └── lattices/          # lattice_iii_verification_oracle.lattice.yaml v1.2.0
├── who/
│   ├── coordination/      # Cross-vault coord memos (absorbed: VideoForge airlock v0.2 proposal)
│   └── governance/
```

## Entry Points

| If you want to… | Start here |
|-----------------|-----------|
| Run III on a document (any domain) | `how/skills/skill_iii_review.md` |
| Choose between two airlock surfaces (read context locally vs commission work) | `how/airlock/AIRLOCK.md` (Entry — read context + run loop locally; Request — commission work from a vault) |
| Understand the airlock standard contract (both surfaces) | `what/artifacts/iii_airlock_standard_spec.md` (v0.2.0) |
| Validate a cross-vault request memo (mechanical schema) | `what/artifacts/iii_airlock_request_schema.yaml` (JSON Schema Draft 2020-12 in YAML form) |
| Reply to a cross-vault request as the receiving vault | `how/templates/template_cross_vault_request_reply.md` (Acceptance full-profile + Rejection variants) |
| Add III to a new vault | `how/skills/skill_iii_setup.md` |
| Federate a vault against III.aDNA (consumer wrapper contract) | `what/decisions/adr_002_consumer_federation_contract.md` |
| Use III as a composable VaaS oracle | `what/lattices/lattice_iii_verification_oracle.lattice.yaml` |
| Understand the module boundary model | `what/decisions/adr_001_module_architecture.md` |
| Publish a new domain pack | `what/decisions/adr_003_learning_store_ownership.md` (graduation protocol) |

## Active Consumers

| Vault | Wrapper | Packs used | Status |
|-------|---------|-----------|--------|
| `lattice-labs` | `lattice-labs/iii/` | whitepaper, kinn_branding, vault_maintenance, canvas_visual | **MB-1 ✅ 2026-05-08** — pinned at `v0.1.0` (commit `1628793`); consumer-side review pending v0.2.0 minor bump per ADR-002 §3 |
| `SiteForge.aDNA` | `SiteForge.aDNA/iii/` | web_design | MB-2 (pending; will pin against `v0.2.0`) |
| `VideoForge.aDNA` | `VideoForge.aDNA/iii/` | web_design, + ADR-006 extension | MB-3 (pending; will pin against `v0.2.0`) |
| `CanvasForge.aDNA` | `CanvasForge.aDNA/iii/` | canvas_visual | MB-4 (pending; will pin against `v0.2.0`) |
| `wga.aDNA` | `wga.aDNA/iii/` | whitepaper, educational_content (stub) | MB-5 (pending; will pin against `v0.2.0`) |
| `LPWhitepaper.aDNA` | `LPWhitepaper.aDNA/iii/` | whitepaper | MB-8 (pending; gap surfaced during MB-1 wikilink sweep) |

## Release History

| Tag | Date | Closure commit | Summary |
|-----|------|----------------|---------|
| `v0.1.0` | 2026-05-08 | `1628793` (MA-4) | Campaign A genesis baseline — AIRLOCK.md reference implementation (5 entry paths: A text / B web+visual / C code / D video / E vault maintenance); Path Selection matrix; anti-regression block deferring cross-vault request patterns to v0.2. Initial publication to `LatticeProtocol/III.aDNA`. |
| `v0.2.0` | 2026-05-10 | `246124d` (MC-3) | Airlock Standard v0.2.0 reference instance — adds cross-vault request surface (spec §4 + JSON Schema Draft 2020-12 request schema + Acceptance/Rejection reply-comment template); 5 v0.1.0 entry paths preserved byte-identical against `v0.1.0` tag; VideoForge proposal (5 gaps) absorbed in full; first cross-vault-request-capable release. |

Future releases follow ADR-002 §3 and airlock spec §6: patch transparent (no consumer review); minor consumer-reviewed (additive only); major reserved for v1.0 breaking changes (explicit human decision per consumer).

## Known Carry-Forwards

Captured at MC-3 wind-down (2026-05-10) for future sessions:

- **AIRLOCK.md Path E "v0.1.0" wording** — v0.2.x patch candidate. Path E `Out of scope` line reads `not yet covered by v0.1.0; track in Campaign C v0.2 follow-ups`. Verbatim preservation contract (spec §3.3) carried this line forward through the v0.2.0 bump; Campaign C v0.2 did not add federation_ref pin drift coverage (deferred to v0.3+). Fix wording AND the corresponding spec §3 reference simultaneously to keep the verbatim contract enforceable.
- **`lattice-labs/iii/CLAUDE.md` federation_ref pin at `v0.1.0`** — consumer-side ADR-002 §3 minor-bump review. Per `lattice-labs/iii/CLAUDE.md:99`, the wrapper agent reviews the upstream CHANGELOG diff before updating `version:`. Not gating any current work; not III.aDNA's responsibility.
- **`CHANGELOG.md` formalization at vault root** — v0.3 candidate. ADR-002 §3 names "the airlock changelog" as the consumer review surface. Today the `v0.1.0` and `v0.2.0` annotated tag messages carry release-note prose; a dedicated `CHANGELOG.md` would consolidate that surface and make consumer-side diffing easier.

## Originating System

Migrated from `lattice-labs/how/skills/skill_iii_review.md` (created 2026-04-01, campaign_iii_evolution COMPLETED 2026-04-03). Learning store: 26 corrections accumulated, 5 graduated to domain packs. Cross-vault III campaigns active at migration: whitepaper_iii_deep_review (M02/25 cycles), campaign_kinn_branding_iii (50/100 cycles).

v0.1.0 (Campaign A) and v0.2.0 (Campaign C) shipped within 3 days of vault inception; both releases governed by ADR-002 federation contract.
