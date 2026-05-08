---
type: airlock
version: "0.1"
status: stub
updated: 2026-05-07
last_edited_by: agent_stanley
note: "Reference implementation of the Airlock standard. Full implementation in MA-4."
---

# III.aDNA Airlock

> If you are an external agent entering this vault from another context graph, start here. This file maps your profile and purpose to the minimum-viable context loading recipe. Do not load more than your entry path specifies — keep context budgets tight.

## What is III.aDNA?

The Inspect / Introspect / Improve quality improvement framework. A modular loop for finding, calibrating, and fixing quality issues in any artifact type (text, code, visual, data). Consumable by any vault via a lightweight `iii/` consumer wrapper.

## Entry Paths

---

### Path A — Text Improvement (whitepaper, ADR, context file, mission doc)

**Agent profile**: Any agent reviewing a text artifact for precision, reasoning quality, and completeness.

**Load in order**:
1. `how/skills/skill_iii_review.md` — core loop procedure
2. `what/context/core_domain_packs/context_iii_inspect_procedures.md` — text modality procedures
3. `what/context/core_domain_packs/context_iii_introspect_checks.md` — 7 calibration checks
4. `what/context/core_domain_packs/iii_corrections_canonical.jsonl` — learning store (load all non-graduated)
5. Your vault's consumer domain pack (if declared in your `iii/CLAUDE.md` `local_extensions`) — vault-specific traps

**Skip**: `context_iii_domain_packs_web_design.md`, `context_iii_canvas_visual.md` (visual-only packs)

---

### Path B — Web / Visual Improvement (web page, SiteForge output, visual content)

**Agent profile**: SiteForge or VideoForge consumer running III on web pages or visual artifacts.

**Load in order**:
1. `how/skills/skill_iii_review.md`
2. `what/context/core_domain_packs/context_iii_inspect_procedures.md`
3. `what/context/core_domain_packs/context_iii_domain_packs_web_design.md` — web-specific traps
4. `what/context/core_domain_packs/context_iii_introspect_checks.md`
5. `what/context/core_domain_packs/iii_corrections_canonical.jsonl`
6. Your vault's `<vault>_iii_reviewers.yaml` — multi-voice reviewer registry (if using Reviewer Orchestra)

---

### Path C — Code Improvement (source code, scripts, YAML definitions)

**Agent profile**: Agent reviewing code artifacts for correctness, naming, CLI commands, or schema accuracy.

**Load in order**:
1. `how/skills/skill_iii_review.md`
2. `what/context/core_domain_packs/context_iii_inspect_procedures.md` — code modality section
3. `what/context/core_domain_packs/context_iii_introspect_checks.md`
4. `what/context/core_domain_packs/iii_corrections_canonical.jsonl`

**Note**: Code INSPECT verifies referenced function names, file paths, and CLI commands against the live codebase. Ensure the codebase is accessible in your session.

---

### Path D — Video / Multimedia Improvement (VideoForge artifacts)

**Agent profile**: VideoForge agent running III loop on video artifacts, scripts, or operation catalogs.

**Load in order**:
1. `VideoForge.aDNA/what/decisions/adr_006_operation_catalog_iii_loop.md` — VideoForge-specific III extension (operation catalog, dispatch mechanics)
2. `how/skills/skill_iii_review.md` — III core (orchestration)
3. `what/context/core_domain_packs/context_iii_inspect_procedures.md`
4. `what/context/core_domain_packs/context_iii_introspect_checks.md`
5. `what/context/core_domain_packs/iii_corrections_canonical.jsonl`

**Note**: VideoForge ADR-006 wraps III core with video-specific operation catalog. ADR-006 is the entry point for video; III core provides the underlying loop.

---

### Path E — Vault Maintenance Improvement (staleness, orphans, frontmatter drift)

**Agent profile**: Any org-vault agent running routine III audit on vault health.

**Load in order**:
1. `how/skills/skill_iii_review.md`
2. `what/context/core_domain_packs/context_iii_vault_maintenance.md` — 6 vault-specific traps
3. `what/context/core_domain_packs/context_iii_introspect_checks.md`
4. `what/context/core_domain_packs/iii_corrections_canonical.jsonl`

**Skip**: Visual and code packs unless the vault contains multi-modal content.

---

## Version Contract

This airlock is valid for III.aDNA **v0.1.0** (pre-federation baseline). Your consumer wrapper's `federation_ref` version pin determines which version of these files you load.

On minor version bump: re-read this AIRLOCK.md to check for updated entry paths before proceeding.

**Canonical path changes since v0.1.0**: none (initial version).

---

## Can't find what you need?

If your use case doesn't match any entry path above, open `how/skills/skill_iii_review.md` directly — it is the authoritative procedure. Entry paths here are convenience recipes, not gatekeepers.

To add a new entry path (for a new consumer profile), submit a PR to III.aDNA or open a mission in the `how/campaigns/campaign_a_iii_genesis/` tracker.

---

*Standard spec*: `what/artifacts/iii_airlock_standard_spec.md` (MC-1, Campaign C — to be authored)
