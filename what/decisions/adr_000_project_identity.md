---
type: decision
adr_id: "000"
title: Project Identity — Framework.aDNA category, Argus persona, mission scope
status: ratified
created: 2026-05-07
updated: 2026-05-07
last_edited_by: agent_stanley
signed_by: Stanley (Chief Steward)
tags: [adr, identity, framework, iii]
---

# ADR-000: Project Identity

## Status

Ratified — 2026-05-07 (Stanley, Chief Steward)

## Context

The III (Inspect / Introspect / Improve) quality review loop has been operational inside `lattice-labs` since 2026-04-01. It has grown into the primary quality layer across the Lattice ecosystem — referenced in SiteForge, VideoForge, CanvasForge, wga, WilhelmAI, the VaaS layer, and multiple active campaigns. The system has:

- 4 INSPECT modalities (text, code, visual, data)
- 7 INTROSPECT calibration checks
- A graduated learning store (26 corrections, 5 promoted to domain packs)
- 5 domain packs covering web design, whitepaper, canvas visual, vault maintenance, KINN branding
- A lattice definition (`lattice_iii_verification_oracle.lattice.yaml` v1.1.0)
- Active campaigns in lattice-labs, SiteForge, wga, and VideoForge

Living inside `lattice-labs` limits III's accessibility — external vaults can only consume it through informal path dependencies. Externalizing III as a standalone vault makes it a first-class, versioned, federable service.

## Decision

### 1. Category: Framework.aDNA (new aDNA category)

III.aDNA is classified as a **Framework.aDNA** project — a new aDNA category for vaults that define protocols, methodologies, or operational standards that other vaults federate against.

Rationale for new category (not Forge, Platform, or Org-Vault):
- **Not a Forge**: Forges produce artifacts (websites, decks, videos). III produces quality improvement loops — a service, not a deliverable artifact.
- **Not a Platform**: Platforms are deployable runtimes at partner institutions. III has no dedicated compute surface; it runs as a capability inside any agent session.
- **Not an Org-Vault**: III has no organizational governance body. It is a methodology standard, not an organization.
- **Framework**: III defines how quality improvement works, provides consumable modules, and establishes a learning store graduation protocol that other vaults follow.

VaaS (VAASLattice.aDNA) is a candidate second Framework.aDNA instance. The category promotes once a second instance exists.

### 2. Persona: Argus Panoptes

Argus Panoptes — the hundred-eyed giant of Greek mythology assigned to never-sleeping vigilance. No escape from Argus's sight; everything is seen. The INSPECT phase makes Argus's attribute concrete. The IMPROVE phase is the consequence of having seen truly.

Argus is distinct from all existing Lattice personas (Berthier, Hermes, Iris, Asclepius, Mnemosyne, Hygieia, Daedalus) and non-overlapping in scope.

### 3. Mission scope

III.aDNA's mission: provide a modular, federable, extensible quality improvement protocol for any object type (text, code, visual, data) with:
- Composable modules for each loop phase
- Domain-agnostic core + domain-specific pack extensions
- A canonical learning store that accumulates cross-vault pattern knowledge
- A consumer federation contract enabling any vault to wire in improvement cycles
- An airlock for external-agent entry

In scope: text/code/visual/data improvement cycles, domain pack standard, learning store graduation, consumer wrapper pattern, airlock standard.

Out of scope: domain-specific content production (stays in respective Forges), deployment orchestration (stays in Platforms), organizational governance (stays in Org-Vaults).

### 4. Version policy

Semantic versioning. III.aDNA starts at v0.1.0 (pre-federation baseline, 2026-05-07). Consumer wrappers pin a version. **Minor bump** triggers consumer review (not automatic update). Patch bumps are transparent.

## Consequences

- Workspace `CLAUDE.md` gets a new "Framework Ecosystem" table entry for III.aDNA.
- aDNA base template references III.aDNA as the standard improvement-cycles framework (MB-5).
- Consumer vaults create `iii/` lightweight wrappers per ADR-002 pattern.
- Campaign A proceeds to bootstrap the vault; Campaigns B and C handle federation and airlock propagation.
