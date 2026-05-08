---
type: airlock
version: "0.1.0"
status: reference_implementation
created: 2026-05-07
updated: 2026-05-08
last_edited_by: agent_stanley
closes_dg_a_criterion: airlock_5_entry_paths
covers: entry_paths
defers_to_v0_2: cross_vault_request_patterns
inbound_findings: who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md
---

# III.aDNA Airlock

> If you are an external agent entering this vault from another context graph, start here. This file maps your profile and purpose to the minimum-viable context loading recipe. Do not load more than your entry path specifies — keep context budgets tight.

## What is III.aDNA?

The Inspect / Introspect / Improve quality improvement framework. A modular loop for finding, calibrating, and fixing quality issues in any artifact type (text, code, visual, data). Consumable by any vault via a lightweight `iii/` consumer wrapper.

## Path Selection

Use this matrix to self-route in one read. Pick the row that matches your artifact, then jump to the named path below.

| Your artifact | Your profile | Path |
|---------------|--------------|------|
| Whitepaper, ADR, mission/session file, AAR, context pack, prose doc | Reviewer of any text artifact | **A** |
| Web page, SiteForge build output, CanvasForge HTML deck, rendered visual landing | SiteForge / VideoForge / CanvasForge frontend reviewer | **B** |
| Source code, `*.module.yaml`, `*.lattice.yaml`, scripts, CI / build configs | Code reviewer | **C** |
| Video file, transcript, VideoForge operation-catalog entry | VideoForge agent | **D** |
| Whole vault — staleness, orphans, frontmatter drift, broken wikilinks | Vault custodian / org-vault hygiene sweep | **E** |

If no row matches, drop to "Can't find what you need?" at the bottom of this file. Multi-modal artifacts (e.g., a whitepaper that ships with code snippets) usually run multiple passes — pick the dominant modality first, then a secondary pass on the others. Cross-vault *request* patterns (you want another vault to do work for you, not to enter this one to do work) are out of scope for v0.1.0 — see "What v0.1.0 does NOT cover" below.

## Entry Paths

---

### Path A — Text Improvement (whitepaper, ADR, context file, mission doc)

**Agent profile**: Any agent reviewing a text artifact for precision, reasoning quality, and completeness.

**Use cases**:
- Lattice Protocol whitepaper section under revision (`whitepaper/`)
- ADR drafts during ratification (e.g., `*.aDNA/what/decisions/adr_*.md`)
- Mission file or session AAR pre-archive
- A context pack (a meta-review: review the file that teaches a reviewer)

**Out of scope**:
- Web-rendered layout review → Path B (the page is the artifact, not the prose)
- `*.yaml` module / lattice definitions → Path C (structure + schema, not prose quality)
- Video transcripts where the catalog matters → Path D (operation-catalog dispatch differs)
- Whole-vault hygiene → Path E

**Expected output**: `Finding[]` (text-modality findings ranked by severity) → `Introspection[]` (7 calibration checks applied) → `ImprovementTable + VerificationResult` (ranked change orders, each with a verification outcome) → optional `LearningStoreDelta` to the consumer's local corrections store.

**Context budget**: ~4–5 files (skill + 2 core packs + corrections.jsonl + optional local extension); typical loaded context for a 10k-word artifact stays under ~30k tokens.

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

**Use cases**:
- SiteForge-built marketing or partner-facing landing pages
- VideoForge episode landing page or web preview surface
- CanvasForge HTML deck export or print-CMYK proof
- Visual content where layout, color, and typography matter alongside copy

**Out of scope**:
- Source code generating the page → Path C (review the generator, not the output)
- Pure prose with no rendered surface → Path A
- Video frames inside an episode → Path D (operation-catalog dispatch)
- Vault-level link rot or orphan files → Path E

**Expected output**: same triple as Path A (`Finding[]` → `Introspection[]` → `ImprovementTable + VerificationResult`); when `<vault>_iii_reviewers.yaml` is loaded, the Reviewer Orchestra adds cross-voice notes (Critic / Design / UX / SEO / Brand) attached to each finding.

**Context budget**: ~6 files (skill + 3 packs + corrections + optional reviewers.yaml). Larger than Path A because web review requires both prose and visual trap packs.

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

**Use cases**:
- aDNA `*.module.yaml` and `*.lattice.yaml` definitions (typed-IO contracts, edge wiring)
- Build / deploy scripts and CI configurations
- Source files in sibling code repos (`lattice-protocol/`, `latlab/`, `rareharness/`)
- API surfaces where function names and signatures get cited in prose elsewhere

**Out of scope**:
- Prose explanation of the code → Path A (review the doc, not the implementation)
- Rendered code-snippet web pages → Path B
- A YAML frontmatter block inside a Markdown file → Path A (the prose is the artifact; the frontmatter is metadata)
- Vault-wide YAML schema drift → Path E

**Expected output**: `Finding[]` (correctness + naming + CLI + schema findings) → `Introspection[]` → `ImprovementTable + VerificationResult`. Verification step verifies referenced symbols against the live codebase rather than against the document.

**Context budget**: ~4 files plus codebase-read access. The codebase itself is loaded on-demand during INSPECT verification, not eagerly.

**Load in order**:
1. `how/skills/skill_iii_review.md`
2. `what/context/core_domain_packs/context_iii_inspect_procedures.md` — code modality section
3. `what/context/core_domain_packs/context_iii_introspect_checks.md`
4. `what/context/core_domain_packs/iii_corrections_canonical.jsonl`

**Note**: Code INSPECT verifies referenced function names, file paths, and CLI commands against the live codebase. Ensure the codebase is accessible in your session.

---

### Path D — Video / Multimedia Improvement (VideoForge artifacts)

**Agent profile**: VideoForge agent running III loop on video artifacts, scripts, or operation catalogs.

**Use cases**:
- Episode video file post-render (visual + audio + caption pass)
- Auto-generated transcript review against source script
- Operation-catalog entry pre-execution (the catalog is the dispatch contract)
- Multi-platform video variant (YouTube long-form / Shorts / TikTok) consistency check

**Out of scope**:
- Episode landing page or web preview surface → Path B
- Narrative script as pure prose, pre-render → Path A (script becomes Path D once it has timing + operations attached)
- VideoForge-internal code → Path C (`lattice-video-forge/` source files)
- VideoForge vault hygiene → Path E

**Expected output**: same triple, routed through VideoForge ADR-006 operation catalog. The dispatch mechanics inside ADR-006 select which III modules fire and in what order; III core (this vault) provides the underlying loop.

**Context budget**: ~5 files (ADR-006 + skill + 2 core packs + corrections). VideoForge artifacts often reference cross-vault assets — load only the artifact and its declared dependencies, not the full asset library.

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

**Use cases**:
- Quarterly vault hygiene sweep (orphans, broken wikilinks, frontmatter drift)
- Pre-Decision-Gate readiness check ("does the vault state match the campaign claims?")
- STATE.md staleness audit (does the operational snapshot still match the missions on disk?)
- ADR / mission / session metadata-consistency pass

**Out of scope**:
- Single-artifact content review → Path A (or B/C/D for the modality)
- Logic correctness of code or schemas inside the vault → Path C
- Cross-vault relationships (federation_ref pin drift across sibling vaults) → not yet covered by v0.1.0; track in Campaign C v0.2 follow-ups

**Expected output**: same triple, weighted toward structural findings (broken wikilinks, missing frontmatter fields, orphaned files, stale "last_edited_by" entries). Findings tend to be high-volume and low-individual-severity; the Improvement Table groups by trap rather than by file.

**Context budget**: ~4 files plus on-demand directory traversal. Vault audits read directory listings, not full file bodies, except where a frontmatter spot-check is warranted.

**Load in order**:
1. `how/skills/skill_iii_review.md`
2. `what/context/core_domain_packs/context_iii_vault_maintenance.md` — 6 vault-specific traps
3. `what/context/core_domain_packs/context_iii_introspect_checks.md`
4. `what/context/core_domain_packs/iii_corrections_canonical.jsonl`

**Skip**: Visual and code packs unless the vault contains multi-modal content.

---

## Version Contract

This airlock is valid for III.aDNA **v0.1.0** (pre-federation baseline). Your consumer wrapper's `federation_ref` version pin determines which version of these files you load.

**On minor version bump** (v0.1 → v0.2 → ...): re-read this AIRLOCK.md to check for updated entry-path file lists before proceeding. The v0.2 addition of cross-vault *request* patterns (see "What v0.1.0 does NOT cover" below) is additive and does not invalidate the v0.1.0 entry paths — your existing recipes still work.

**On patch bump** (v0.1.0 → v0.1.1): no review required; patch bumps cover typo and clarity fixes that don't change file paths or recipes.

**Canonical path changes since v0.1.0**: none (initial version).

---

## What v0.1.0 does NOT cover

The five entry paths above describe **inbound agent entry** — an external agent comes into III.aDNA's context graph to run a quality-improvement loop. v0.1.0 deliberately does not formalize **cross-vault request patterns** — the case where Vault A's agent commissions Vault B's agent to do work for Vault A (e.g., "VideoForge requests CanvasForge build a partner-onboarding deck"). The two are different shapes:

- **Entry** (covered here): inbound, pull-based, agent reads context and runs the loop locally.
- **Request** (deferred to v0.2): bidirectional, ephemeral, two agents handshake across vault boundaries with status, schema, and idempotency.

Until v0.2 ships, cross-vault requests use the **coord-memo fallback**: file a memo at the receiving vault's `who/coordination/coord_<date>_<requesting_vault>_requests_<artifact>.md`. The canonical worked example is `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` (8-section structure: TL;DR / Context / Receives / Doesn't Receive / Constraints / Acceptance / Rollback / Why memo).

**v0.2 spec authorship is queued for Campaign C MC-1**: `what/artifacts/iii_airlock_standard_spec.md` will formalize both entry paths *and* request patterns. The inbound proposal — VideoForge.aDNA's exercise of v0.1.0 producing 5 specific gaps (request-initiation ceremony, handshake, payload schema, secret delegation, idempotency) — is on file at `who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md`.

If you arrive in v0.1.0 expecting a request pattern: file a coord memo at the receiving vault following the worked example above. v0.2 will define the schema retroactively; existing memos remain valid evidence.

---

## Can't find what you need?

If your use case doesn't match any entry path above, open `how/skills/skill_iii_review.md` directly — it is the authoritative procedure. Entry paths here are convenience recipes, not gatekeepers.

To add a new entry path (for a new consumer profile), submit a PR to III.aDNA or open a mission in the `how/campaigns/campaign_a_iii_genesis/` tracker.

---

*Standard spec*: `what/artifacts/iii_airlock_standard_spec.md` (MC-1, Campaign C — to be authored)
