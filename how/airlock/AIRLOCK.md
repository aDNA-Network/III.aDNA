---
type: airlock
version: "0.3.0"
status: reference_implementation
created: 2026-05-07
updated: 2026-05-22
last_edited_by: agent_argus
governed_by: what/artifacts/iii_airlock_standard_spec.md
covers: [entry_paths, cross_vault_request_patterns, federation_substrate_awareness]
absorbs_proposals:
  - who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md   # v0.2 origin (closed at MC-5)
  - who/coordination/coord_2026_05_20_iii_to_ln_federation_substrate_intersect.md   # v0.3 origin (LN substrate intersect; fired at Campaign D charter)
ln_substrate_version_pin: "LN.aDNA pc_01 Phase A close + Phase B1 close — ADR-014 v0.1 + ADR-015 v0.1 (both accepted 2026-05-20)"
federation_mode: enforce
ledger_observation: enabled
---

# III.aDNA Airlock

> If you are an external agent entering this vault from another context graph, start here. This file maps your profile and purpose to the minimum-viable context loading recipe. v0.3.0 routes three surfaces: **entry** (you come into this vault to read context and do work locally), **cross-vault request** (you ask an agent in this vault to do work and ship results back to you), and **federation-substrate awareness** (your traffic crosses a federation substrate — Tailscale or Nebula per LN ADR-015 — and v0.3 adds Ed25519 signature verification + read-only ledger observation contracts). Pick a surface first via § Surface Selection, then route within that surface. Do not load more than your path or surface specifies — keep context budgets tight.

## What is III.aDNA?

The Inspect / Introspect / Improve quality improvement framework. A modular loop for finding, calibrating, and fixing quality issues in any artifact type (text, code, visual, data). Consumable by any vault via a lightweight `iii/` consumer wrapper.

## Surface Selection

Pick the surface that matches what you are doing. Multi-modal sessions (you both run III locally AND commission an artifact) pick a surface per interaction, not per session.

| What you are doing | Surface |
|--------------------|---------|
| Coming into III.aDNA to read context and do work locally (e.g., run III review on an artifact, audit a vault, study the modules) | **Entry** — see § Entry Paths |
| Asking an agent in III.aDNA to do work and ship results back to your vault | **Request** — see § Cross-Vault Requests |
| Your inbound request crosses a federation substrate (Tailscale or Nebula) and carries an Ed25519 signature; OR you want to know how this airlock observes the LN ledger | **Federation-substrate awareness** — see § Federation-Substrate Awareness |
| Federating against III.aDNA at wrapper-creation time (one-time pin) | Out of scope here — see `what/decisions/adr_002_consumer_federation_contract.md` |

Spec reference: `what/artifacts/iii_airlock_standard_spec.md` §2 (Three Surfaces — decision matrix + surface comparison; renamed from "Two Surfaces" at v0.3).

---

## Entry Paths

Use this matrix to self-route in one read. Pick the row that matches your artifact, then jump to the named path below.

| Your artifact | Your profile | Path |
|---------------|--------------|------|
| Whitepaper, ADR, mission/session file, AAR, context pack, prose doc | Reviewer of any text artifact | **A** |
| Web page, SiteForge build output, CanvasForge HTML deck, rendered visual landing | SiteForge / VideoForge / CanvasForge frontend reviewer | **B** |
| Source code, `*.module.yaml`, `*.lattice.yaml`, scripts, CI / build configs | Code reviewer | **C** |
| Video file, transcript, VideoForge operation-catalog entry | VideoForge agent | **D** |
| Whole vault — staleness, orphans, frontmatter drift, broken wikilinks | Vault custodian / org-vault hygiene sweep | **E** |

If no row matches, drop to "Can't find what you need?" at the bottom of this file. Multi-modal artifacts (e.g., a whitepaper that ships with code snippets) usually run multiple passes — pick the dominant modality first, then a secondary pass on the others. If your task is to commission another vault to do work for you, you are on the wrong surface — go back to § Surface Selection and pick **Request**.

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

## Cross-Vault Requests

A cross-vault request is the inverse direction of an entry path: you stay in your vault and ask an agent in this vault to do work and ship results back to you. The full contract is in the standard spec; this section routes you to the canonical artifacts.

**What this is**. Bidirectional, ephemeral, two-vault handshake. One memo on disk, in the receiving vault's `who/coordination/`, that both sides update through the lifecycle. Full contract: `what/artifacts/iii_airlock_standard_spec.md` §4.

**Where the request lives**. Receiving vault's `who/coordination/` directory, filename pattern:

```
who/coordination/coord_<YYYY_MM_DD>_<requesting_vault>_requests_<artifact>.md
```

The receiving vault is the gatekeeper; the memo is its intake record. (Spec §4.1.)

**Required-minimum payload**. Three fields conform: `type: cross_vault_request`, `artifact_request.spec_path`, `artifact_request.output_sink`. Everything else is optional and shaped by the work being commissioned. Full machine-readable contract: `what/artifacts/iii_airlock_request_schema.yaml` (JSON Schema Draft 2020-12 in YAML form). (Spec §4.3.)

**Lifecycle states** (frontmatter `status`):

`open → accepted → rendering → shipped → closed` (happy path)
`open → rejected` (receiver declines)
`open → cancelled` (requester withdraws before pickup)
`accepted → queued → rendering` (receiver over-capacity but accepting)

Valid transitions enumerated in `iii_airlock_request_schema.yaml` `x-lifecycle-transitions`. (Spec §4.1.)

**Handshake profiles**. Lightweight (default) — states `open → shipped → closed`; the receiver's first reply is the shipped notification. Use for occasional ad-hoc 2-forge requests with no calendar urgency. Full — full state set; required when the receiver is in active rendering/serving mode, calendar deadlines apply, or `priority` is `high`/`urgent`. Profile rules: schema `x-handshake-profiles`. Field requirements: spec §4.2. The receiver MAY upgrade lightweight to full; MAY NOT downgrade a high/urgent request to lightweight.

**Reply-comment template**. Receiver appends an Acceptance or Rejection section to the memo at pickup: `how/templates/template_cross_vault_request_reply.md`.

**Secrets**. The `secrets_handled` frontmatter block specifies cross-vault secret delegation (`needed`, `passthrough`, `not_passed`). Receiver MUST verify presence of every `needed` name at acceptance and reject with `missing_secret: <NAME>` if absent. Names-only audit; never log values. Full contract: spec §4.4.

**Idempotency**. Optional `idempotency_key` enables 30-day dedup on the receiver side. On match (and `force_new: false`), the receiver replies with `duplicate_of: <existing_memo_path>` and links the prior `audit_id`; the new memo flips `cancelled`. Recommended key convention: `<requesting_vault_short>_<artifact_kind>_<version_or_iteration>`. Full contract: spec §4.5.

**Worked example**. VideoForge's commission of the Carly + Herb sprint onboarding deck from CanvasForge: `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md`. Authored against the v0.1.0 coord-memo fallback; conforms to v0.3 with three purely additive deltas documented in spec §6.2 (`secrets_handled` block addable retroactively; `idempotency_key` field optional; `federation_signature` + `federation_signature_key_version` pair required at v0.3 re-exercise since both vaults are co-federated post-LN Phase B1 — MD-A5 validation will exercise this delta against the live substrate).

---

## Federation-Substrate Awareness

v0.3 acknowledges that cross-vault traffic now traverses a federation substrate — the Lattice Protocol's identity, membership, and ledger layer. This section is the airlock's contract WITH that substrate: how it verifies signatures at request acceptance, how it observes ledger events read-only, and how it emits its own audit events back to the substrate. The federation substrate itself is authored at LatticeNetwork.aDNA — see [LN ADR-014](~/lattice/LatticeNetwork.aDNA/who/governance/adr_014_re_id_semantics_node_canonical_id_transition.md) (ledger event semantics + idempotency) and [LN ADR-015](~/lattice/LatticeNetwork.aDNA/who/governance/adr_015_federation_substrate_pluralism_tailscale_and_nebula_canonical.md) (substrate pluralism + `transport.substrates[]` + Ed25519 identity).

**Substrate-pluralism (§5.1)**. Per LN ADR-015 §a, both **Tailscale** and **Nebula** are canonical federation substrates at v0.1. This airlock is substrate-agnostic at the contract layer — the §4.6 + §5.2–§5.4 contracts hold regardless of which substrate carries the traffic. Per-substrate identity is sourced from the node's `inventory_memberships.yaml` `transport.substrates[]` per LN ADR-015 §b; the airlock SHOULD treat that list as truth and SHOULD NOT cache per-substrate identity beyond a short TTL.

**Ed25519 signature verification (§4.6 + §5.2)**. Co-federated requests MUST carry `federation_signature` + `federation_signature_key_version` (added at spec §4.3 optional payload fields). This airlock resolves the requesting persona's Ed25519 pubkey from the LN membership manifest (`transport.substrates[*].identity.federation_signing_pubkey_sha256` per LN ADR-013 §b + ADR-015 §b/§d), verifies the signature over the canonical-JSON serialization of the request frontmatter (sorted keys; NFC normalization; whitespace trimmed; case preserved per LN ADR-014 §c), and rejects with `signature_verification_failed:<subreason>` per the 5-value taxonomy (`pubkey_absent`, `pubkey_revoked`, `signature_mismatch`, `key_version_unknown`, `substrate_mismatch`). Implementation guidance: `what/artifacts/iii_airlock_substrate_implementation.md` §4 (9 sub-sections).

**Mode** — this airlock is in `enforce` mode (frontmatter `federation_mode`). Co-federated requests MUST verify; non-federated requests fall back to the v0.2 trust model (substrate fallback per spec §4.6 advisory-downgrade clause).

**Ledger observation (§5.3; opt-in per wrapper)**. This airlock observes the LN ledger read-only (`enabled` per frontmatter `ledger_observation`). It subscribes to `MEMBERSHIP_*` events (per LN ADR-015 §c), `MEMBERSHIP_NODE_RE_IDENTIFIED` (per LN ADR-014), and `CONNECTION_*` events (per LN ADR-015 §c extension), polling at 60s cadence over LN Tier-1 file-ledger per impl-doc §5.1 recommendation (pub/sub deferred to v0.4+; chain-walk rejected). Observed `MEMBERSHIP_CERT_REVOKED` events invalidate cached pubkeys per §4.6. The airlock MUST NOT write to the ledger directly — it MAY emit `COMPLIANCE_AUDIT` events per §5.3.1 but these are airlock-attested observations, not ledger membership mutations.

**`COMPLIANCE_AUDIT` emission (§5.3.1; assumes_draft)**. This airlock emits a `COMPLIANCE_AUDIT` event per spec §5.3.1 upon request acceptance or rejection — recording `audit_id`, `request_path`, `disposition`, `rejection_reason`, `request_frontmatter_hash` (SHA-256 of canonical-JSON excluding the signature field), `airlock_spec_version`, `substrate_kind` (Tailscale or Nebula), `signature_verified`, and `operator_attestation`. Emission uses the `assumes_draft` direct-JSON-write workaround per impl-doc §6.3 (byte-identical-to-native-enum-path computation) until LN Carry 3 EP-1 ratification at Phase 5 integration seam, when the event-type joins the native `LedgerEventType` enum.

**Multi-substrate identity (§5.4)**. When the node participates in multiple substrates (canonical-pilot-tier per LN ADR-015 §e), this airlock validates that the inbound request's transport substrate matches one of the requesting node's enumerated `transport.substrates[]`. Mismatch rejects with `signature_verification_failed:substrate_mismatch`. Pilot-tier (dual-substrate) and non-pilot-tier (single-substrate Tailscale per LN ADR-015 §j) nodes are treated symmetrically at the contract layer.

**Reply-comment template extension**. The reply-comment template at `how/templates/template_cross_vault_request_reply.md` was extended at MD-A2 with the v0.3 rejection-reason enum value `signature_verification_failed:<subreason>` + a full canonical Ed25519 rejection body example block. Use that template's Rejection variant when sig-verify fails.

---

## Version Contract

This airlock is v0.3.0. Your consumer wrapper's `federation_ref` version pin determines which version of these files you load.

**On minor version bump** (v0.3 → v0.4 → ...): re-read this AIRLOCK.md to check for updated entry-path file lists, new surfaces, or new lifecycle states before proceeding. Minor bumps are additive — your existing recipes and request memos remain valid. Consumer wrappers review the upstream changelog at every minor bump per ADR-002 §3.

**On patch bump** (v0.3.0 → v0.3.1): no review required; patch bumps cover typo and clarity fixes that don't change file paths, recipes, or schemas.

**On major bump** (v0.x → v1.0): reserved for breaking changes (entry-path removal, required-field reshape, lifecycle-state semantics change, advisory → mandatory promotion of Ed25519 verification across all federation contexts). Each consumer wrapper makes an explicit human decision before updating its pin.

**Canonical path changes since v0.1.0**: none — entry paths preserved verbatim; cross-vault request section (v0.2) and federation-substrate awareness section (v0.3) are purely additive. v0.3.0 added the third surface (Federation-Substrate Awareness) and 3 new optional frontmatter fields (`ln_substrate_version_pin`, `federation_mode`, `ledger_observation`); 2 new optional request-payload fields (`federation_signature`, `federation_signature_key_version`); a new optional `COMPLIANCE_AUDIT` ledger event-type emission (assumes_draft until LN Carry 3 EP-1 ratification). The standard policy is detailed in spec §7 (versioning policy) and `what/decisions/adr_002_consumer_federation_contract.md` §3 (federation-pin review rules).

**LN substrate version pin** (frontmatter `ln_substrate_version_pin`). v0.3+ depends on specific LN substrate features (`transport.substrates[]` field; `MEMBERSHIP_*` event types; Ed25519 keypair infrastructure per LN ADR-013). The frontmatter pin declares the minimum LN substrate state this airlock requires; spec §7.5 governs patch/minor/major bumps in response to LN substrate evolution.

---

## What v0.3.0 does NOT cover

v0.3.0 covers three surfaces — inbound entry (§ Entry Paths) + cross-vault requests (§ Cross-Vault Requests) + federation-substrate awareness (§ Federation-Substrate Awareness). The following surfaces are deliberately deferred per spec §8.2:

- **`proposed/` channel for proposal triage** (deferred to v0.4+). When III.aDNA wires a `proposed/` channel (pattern source: VideoForge ADR-005 RLHF), incoming standard proposals migrate there. Until then, structured proposals land in `who/coordination/` as the fallback.
- **Multi-vault transactional requests** (deferred to v0.4+). One requester, multiple receivers, atomic ship. No present-day worked example; design speculative.
- **Async batched requests** (deferred to v0.4+). One memo, many artifacts shipped over time. No present-day worked example; scaling concern only.
- **Body-section signing** (deferred to v0.4+). v0.3 Ed25519 signs the request frontmatter only; body sections are not signed. Wire-shape signature catches frontmatter tampering; body signing requires canonical-Markdown discipline not yet specified.
- **`COMPLIANCE_AUDIT` native `LedgerEventType` enum membership** (deferred to LN Carry 3 EP-1 ratification at Phase 5 integration seam — cluster amendment with LN ADR-014 + ADR-015 candidates). Until then, `assumes_draft: true` per ADR-003 rule 2; MD-A2 §6.3 direct-JSON-write workaround in production.
- **Executable substrate enforcement** (deferred to a future Platform.aDNA for runtime). v0.3 carries the normative contracts in spec §4.4 (secrets preflight) + §4.5 (idempotency dedup) + §4.6 (Ed25519 sig-verify) + §5.3 (ledger observation) + §5.3.1 (COMPLIANCE_AUDIT emission); implementation guidance shipped at MC-4 (`what/artifacts/iii_airlock_substrate_implementation.md` §2 + §3, 2026-05-13) and MD-A2 (impl-doc §4 + §5 + §6, 2026-05-21). The first executable enforcement lands when a Platform.aDNA integration (e.g., RareHarness, VAASLattice) consumes the standard at runtime.

If you arrive at a surface not covered here: file a coord memo at the receiving vault following the spec §4 request pattern; v0.4+ will define the missing schema retroactively, and your memo remains valid evidence.

---

## Can't find what you need?

If your use case doesn't match an entry path AND doesn't fit the cross-vault request pattern, open `how/skills/skill_iii_review.md` directly — it is the authoritative procedure for the loop. Entry paths here are convenience recipes, not gatekeepers.

To add a new entry path (for a new consumer profile), submit a PR to III.aDNA conformant to spec §3.1 (entry-path schema). Acceptance criteria for promoting a consumer-side path to a core entry path: the profile applies to ≥ 2 unrelated consumers; the `Load in order` list cites at least one III.aDNA core pack. (Spec §3.4.)

---

*Standard spec*: `what/artifacts/iii_airlock_standard_spec.md` (v0.3.0; governs this file)
*Request schema*: `what/artifacts/iii_airlock_request_schema.yaml` (v0.3.0; JSON Schema Draft 2020-12 in YAML form; v0.3 adds 2 optional frontmatter fields — `federation_signature` + `federation_signature_key_version`)
*Implementation guidance*: `what/artifacts/iii_airlock_substrate_implementation.md` (v0.3.0; §2 secrets_handled + §3 idempotency_key from MC-4; §4 Ed25519 preflight + §5 ledger observation + §6 COMPLIANCE_AUDIT emission from MD-A2)
*Reply-comment template*: `how/templates/template_cross_vault_request_reply.md` (extended at MD-A2 with the `signature_verification_failed:<subreason>` rejection-reason enum value + canonical Ed25519 rejection body example block)
*Activation kit*: `how/skills/skill_airlock_activation.md` (MD-A3; consumer-side activation/upgrade recipe + verification checklist)
*Absorbs proposals*: `who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md` (v0.2 origin; closed at MC-5); `who/coordination/coord_2026_05_20_iii_to_ln_federation_substrate_intersect.md` (v0.3 origin; fired at Campaign D charter)
