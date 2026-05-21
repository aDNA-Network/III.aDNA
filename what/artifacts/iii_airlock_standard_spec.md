---
type: standard_spec
title: "III.aDNA Airlock Standard"
version: "0.3.0"
status: reference_implementation
created: 2026-05-08
updated: 2026-05-21
last_edited_by: agent_argus
governs: how/airlock/AIRLOCK.md
predecessor_version: "0.2.0"
inbound_proposal: who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md
worked_example: ~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md
defers_schema_to: what/artifacts/iii_airlock_request_schema.yaml  # authored MC-2 2026-05-08
defers_substrate_to: what/artifacts/iii_airlock_substrate_implementation.md  # v0.2 authored MC-4 2026-05-13; v0.3 extension authored at MD-A2 (next mission)
defers_federation_substrate_to:
  - ~/lattice/LatticeNetwork.aDNA/who/governance/adr_014_re_id_semantics_node_canonical_id_transition.md   # ledger event semantics + idempotency (accepted 2026-05-20)
  - ~/lattice/LatticeNetwork.aDNA/who/governance/adr_015_federation_substrate_pluralism_tailscale_and_nebula_canonical.md   # substrate pluralism + transport.substrates[] + Ed25519 per-purpose identity (accepted 2026-05-20)
  - what/decisions/adr_002_consumer_federation_contract.md   # consumer federation pin policy (minor-bump review trigger)
load_bearing_dependency: "LatticeNetwork.aDNA pc_01 Phase A federation substrate (Ed25519 keypair LIVE on wga_l1 + bore.pub:42561 tunnel + 4-event admission ceremony; landed 2026-05-20) + Phase B1 dual-substrate ratification (ADR-014 v0.1 + ADR-015 v0.1 accepted 2026-05-20; cross-substrate first-emit validated at HTTP 200 2026-05-21)"
ln_substrate_version_pin: "LN.aDNA pc_01 Phase A close + Phase B1 close — ADR-014 v0.1 + ADR-015 v0.1 (both accepted 2026-05-20)"
covers: [entry_paths, cross_vault_request_patterns, federation_substrate_awareness]
authoring_mission: MD-A1
campaign: campaign_d_federation_adaptive_loop
tags: [standard, airlock, spec, v0_3, cross_vault_request, entry_paths, federation_aware, ed25519, ledger_observation, multi_substrate, ln_substrate_load_bearing]
---

# III.aDNA Airlock Standard — v0.3.0

> **Status note**: This is the *standard* — the contract that the operational airlock at `how/airlock/AIRLOCK.md` and downstream consumer wrappers implement. It defines the schema; the AIRLOCK.md file is the reference *instance*. When the spec changes, AIRLOCK.md and consumer wrappers update against it; the spec itself is the source of truth for what an airlock-conformant artifact looks like.

> **v0.3.0 minor-bump scope** (additive only; v0.2-conformant artifacts remain valid): adds federation-substrate awareness as a third interaction surface (§5); adds Ed25519 signature verification at request acceptance as a co-federated preflight (§4.6); adds the `federation_signature` + `federation_signature_key_version` optional frontmatter fields; adds the `COMPLIANCE_AUDIT` event emission shape (assumes_draft pending LN Carry 3 EP-1 fold-in). LOAD-BEARING on LatticeNetwork.aDNA pc_01 Phase A federation substrate + Phase B1 ADR-014/-015 dual-substrate ratification. Consumer review required per ADR-002 §3 (minor-bump).

## §1 Purpose & Scope

The III.aDNA Airlock Standard governs **vault-to-vault traffic** — the ways agents and artifacts cross context-graph boundaries between aDNA vaults. v0.3.0 covers three complementary surfaces:

- **Entry paths** (carried forward from v0.1.0): inbound, pull-based, single-direction. An external agent enters a vault to run a quality-improvement loop (or other localized work) using the vault's own context. Reference instance: `how/airlock/AIRLOCK.md`.
- **Cross-vault request patterns** (added in v0.2): bidirectional, ephemeral, two-vault handshake. An agent in vault A commissions an agent in vault B to do work for vault A. Reference instance: cross-vault request memos in the receiving vault's `who/coordination/`. Worked example on file: VideoForge → CanvasForge Carly + Herb deck commission.
- **Federation-substrate awareness** (added in v0.3): observation-only, identity-binding. An agent in vault A observes vault B's federation-substrate state (ledger events, substrate enumeration, signing-key validity) without entering vault B or commissioning work. Reference contract: §5 Federation-Substrate Awareness. Load-bearing on LatticeNetwork.aDNA pc_01 Phase A federation substrate + Phase B1 dual-substrate ratification (ADR-014/-015).

### What this standard does NOT govern

- **Intra-vault session protocol** — how an agent inside a single vault runs sessions, opens/closes campaigns, manages tasks. That is the vault's own concern.
- **Federation pin negotiation** — when consumer wrappers update their `federation_ref.version`. That lives in [ADR-002 §3](../decisions/adr_002_consumer_federation_contract.md).
- **Lattice-internal contract types** — module I/O, lattice edge schemas, learning store records. Those are governed by the III modules, lattice yaml, and ADR-003.
- **Cross-graph federation negotiation** — when a consumer initially federates against a forge or framework. Covered by the Forge Canonical Pattern Spec (`SiteForge.aDNA/what/artifacts/sf_forge_pattern_spec.md`) and ADR-002.
- **Federation-substrate internals** — how nodes join Tailscale or Nebula, how the membership ledger emits chained events, how Ed25519 keypairs are provisioned, how substrate health is detected. Those are LatticeNetwork.aDNA's authority — see [LN ADR-014](~/lattice/LatticeNetwork.aDNA/who/governance/adr_014_re_id_semantics_node_canonical_id_transition.md) (ledger event semantics + idempotency) and [LN ADR-015](~/lattice/LatticeNetwork.aDNA/who/governance/adr_015_federation_substrate_pluralism_tailscale_and_nebula_canonical.md) (substrate pluralism + `transport.substrates[]` + Ed25519 per-purpose identity). v0.3 of this spec *consumes* that substrate; it does not redefine it.

### What this standard does

For every vault-to-vault interaction, this spec gives:
1. A **shape contract**: required and optional fields, body sections, lifecycle states.
2. A **routing rule**: where the artifact lives on disk, who picks it up, who replies.
3. A **safety contract**: how secrets cross (or don't), how duplicate work is suppressed, how identity is verified across federation boundaries (v0.3+).
4. A **versioning rule**: how consumers know whether their pinned version still matches what they're loading.

---

## §2 The Three Surfaces

The three surfaces are complementary, not competing. Most vault interactions use exactly one; some use combinations (e.g., a consumer that both runs III review locally AND occasionally commissions a forge to build an artifact AND observes ledger events for compliance audit).

### §2.1 Decision matrix

| If the agent is doing this... | ...use surface |
|-------------------------------|----------------|
| Coming into vault B to read context and do work locally (e.g., run III review on vault B's artifact) | **Entry** (§3) |
| Asking an agent in vault B to do work and ship results back to vault A (e.g., VideoForge requests CanvasForge build a deck for VideoForge's output sink) | **Request** (§4) |
| Observing vault B's federation-substrate state without entering or requesting (e.g., a consumer reads vault B's ledger events to verify continuity for cross-vault audit) | **Federation-substrate awareness** (§5) |
| Federating against vault B at wrapper-creation time (one-time pin) | Out of scope — see ADR-002 |
| Sending a structured proposal for vault B's roadmap | Out of scope in v0.3 — deferred to `proposed/` channel (v0.4+) |

### §2.2 Surface comparison

| Property | Entry (§3) | Request (§4) | Federation-substrate awareness (§5) |
|----------|-----------|--------------|-------------------------------------|
| Direction | Inbound to the vault being entered | Bidirectional between two vaults | Read-only observation of the substrate (no vault entered or commissioned) |
| Initiator | The arriving agent | The requesting agent | The observing agent |
| Persistence | Ephemeral session (agent enters, runs, exits) | Ephemeral memo (filed, picked up, shipped, closed) | Continuous subscription OR point-in-time read; no memo artifact |
| Reference instance | `how/airlock/AIRLOCK.md` | Cross-vault request memo (receiving vault's `who/coordination/`) | LN membership ledger event stream + the §5.3.1 `COMPLIANCE_AUDIT` event emission shape |
| Required for conformance | v0.2: yes (5 entry paths); v0.3: unchanged | v0.2: yes (cross-vault work); v0.3: unchanged | v0.3: opt-in per consumer wrapper (no mandate) |
| Handshake | None — pull-based | Optional (lightweight) or required (full) per §4.2 | None at the substrate boundary — observation is non-interactive; Ed25519 verification at §4.6 fires on requests, not observations |
| Audit identifier | Session id at the entered vault | Audit id reconciled across both vaults | Event_id per ADR-014 §c (idempotent re-emission); `audit_id` linkage on emitted `COMPLIANCE_AUDIT` events |
| Substrate awareness | Not required at v0.3 (Entry is intra-vault from arrival onward) | Optional at v0.3 — Ed25519 sig-verify (§4.6) when both vaults co-federated | Required at v0.3 — observation is the substrate |

---

## §3 Entry Path Standard

An entry path is a routing recipe that maps an arriving agent's profile and target artifact to the minimum-viable context-loading order. v0.1.0 shipped 5 entry paths (A text, B web/visual, C code, D video, E vault maintenance). v0.2 generalized the *schema* — what every entry path declares — and additively allowed new paths. v0.3 does not modify §3.

### §3.1 Entry path schema (required fields)

Every entry path in a vault's AIRLOCK.md MUST declare:

| Field | Purpose | Example |
|-------|---------|---------|
| **Profile** | Who this path is for (one-line agent description) | "Any agent reviewing a text artifact for precision and reasoning quality" |
| **Use cases** | 3–5 concrete artifact types this path covers | Whitepapers, ADR drafts, mission files, AARs, context packs |
| **Out of scope** | What this path explicitly does not cover, with redirects to the correct path | "Web-rendered layout review → Path B; YAML schemas → Path C" |
| **Expected output** | The artifact contract the agent ships at the end of the loop | "`Finding[]` → `Introspection[]` → `ImprovementTable + VerificationResult` → optional `LearningStoreDelta`" |
| **Context budget** | Approximate number of files loaded and target token count | "~4–5 files; under ~30k tokens for a 10k-word artifact" |
| **Load in order** | Numbered list of files to load before running the path | (See AIRLOCK.md Path A for canonical example) |

Optional fields:
- **Skip** — files NOT to load even if a related-pattern recipe might suggest them.
- **Note** — path-specific subtleties (e.g., "Code INSPECT verifies referenced symbols against the live codebase").

### §3.2 Path Selection matrix

A vault's AIRLOCK.md MUST include a one-table matrix that lets an arriving agent self-route in a single read. Columns: `Your artifact` × `Your profile` → `Path`. Rows are exhaustive within the v0.2 path set. Multi-modal artifacts (a whitepaper that ships with code) are addressed by an explicit policy — typically "pick the dominant modality first, then re-enter for secondary passes".

### §3.3 Reference instance

`how/airlock/AIRLOCK.md` v0.2.0 ships 5 entry paths conformant to §3.1 and a Path Selection matrix conformant to §3.2:

- **Path A** — Text Improvement (whitepapers, ADRs, missions, AARs, context packs)
- **Path B** — Web / Visual Improvement (web pages, SiteForge output, CanvasForge HTML decks)
- **Path C** — Code Improvement (source, `*.module.yaml`, `*.lattice.yaml`, scripts, CI configs)
- **Path D** — Video / Multimedia Improvement (VideoForge artifacts, transcripts, operation catalogs)
- **Path E** — Vault Maintenance Improvement (staleness, orphans, frontmatter drift)

New paths are additive (minor version bump). Removal or reshape of an existing path is breaking (major version bump; reserved for v1.0).

### §3.4 Entry-path extensibility

External parties (consumer vaults; partner vaults) MAY extend the entry-path set by:
1. Authoring a new path conformant to §3.1 in their own `how/airlock/AIRLOCK.md` (consumer-side path; lives in the consumer wrapper).
2. Submitting a PR to III.aDNA proposing the path as a core entry path. Acceptance criteria: the path's profile applies to ≥ 2 unrelated consumers; the path's `Load in order` list cites at least one III.aDNA core pack.

---

## §4 Cross-Vault Request Standard

A cross-vault request is an ephemeral, file-on-disk artifact that lets one agent commission work from another. v0.2 absorbed five gaps surfaced by VideoForge's exercise of v0.1.0 against a real cross-forge commission: initiation ceremony, handshake, payload schema, secret delegation, idempotency. v0.3 additively introduces a sixth sub-contract: Ed25519 federation signing-key verification (§4.6) — fires only when the requesting and receiving vaults are co-federated.

### §4.1 Initiation ceremony — Gap 1

**Where the request lives**: in the receiving vault's `who/coordination/` directory, with filename pattern:

```
who/coordination/coord_<YYYY_MM_DD>_<requesting_vault>_requests_<artifact>.md
```

Example: `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md`.

**Why this convention** (not a new `inbox/` directory):
- Receiving vault is the gatekeeper; the request artifact is its intake record. Filing it under the receiver's `who/coordination/` keeps responsibility local.
- Filename pattern is greppable across the workspace (`coord_*_requests_*`) for inventorying outstanding requests.
- One coordination surface per vault — no proliferation of `inbox/`, `requests/`, `commissions/` subdirectories.

**Acceptance ceremony** (3 steps; receiver):
1. **Validate inputs** — confirm every path in `artifact_request.spec_path`, `output_sink`, and (where present) `voice_mapping` / `lattice_path` resolves on disk.
2. **Assign audit id** — generate a UUID v4, populate the `audit_id` field in the memo's frontmatter, mirror it in the receiver's session file.
3. **Reply-comment** — append a § Acceptance section to the memo with status, audit id, ETA (if applicable), and a one-line confirmation that inputs validated.

**Rejection ceremony** (1 step; receiver):
1. **Reply-comment** with reason in a § Rejection section; flip `status: rejected` in frontmatter; commit on the receiver side.

**Lifecycle states** (in frontmatter `status`):

| State | Meaning |
|-------|---------|
| `open` | Filed by requester; awaiting receiver pickup |
| `accepted` | Receiver validated inputs and assigned audit id |
| `rendering` | Receiver pipeline executing |
| `shipped` | Artifacts written to `output_sink`; awaiting requester acknowledgement |
| `closed` | Requester acknowledged; both sides moved memos to history |
| `rejected` | Receiver declined (capacity, scope mismatch, lattice incompatibility, signature verification failure per §4.6) |
| `cancelled` | Requester withdrew before pickup |
| `queued` | Optional — receiver accepted but is over-capacity; position in queue documented |

### §4.2 Handshake protocol — Gap 2

A handshake is the receiver's acknowledgement that the request was seen, accepted, and reserved for execution. v0.2 makes the handshake **OPTIONAL** to keep trivial 2-forge requests trivial.

**Lightweight profile** (default for occasional 2-forge ad-hoc requests):
- States used: `open → shipped → closed`. (`accepted` and `rendering` are skipped; the receiver's first reply-comment is the shipped artifacts.)
- Acceptable when both vaults trust each other's pickup behaviour (no calendar urgency, no capacity concerns).

**Full profile** (required when either vault is in active rendering/serving mode, or when calendar deadlines apply):
- States used: full set from §4.1.
- Reply-comment on acceptance MUST carry: `status`, `audit_id`, `eta` (ISO 8601), `reserved_capacity` (e.g., session-minutes or render-passes), and any preconditions (e.g., "blocked on requesting vault publishing missing voice mapping").
- If receiver is over-capacity: `status: queued` + position number + estimated dequeue time.
- If receiver declines: `status: rejected` + reason.

**Profile selection rule**: Lightweight is the default. A receiver MAY upgrade an incoming lightweight request to full profile by responding with a full handshake — the requester is not entitled to lightweight handling. A receiver MAY NOT downgrade a request marked `priority: high` or `priority: urgent` to lightweight.

### §4.3 Payload schema — Gap 3

A cross-vault request memo conforms to the schema below. The canonical machine-readable schema (YAML) lives at `what/artifacts/iii_airlock_request_schema.yaml` (authored at MC-2 2026-05-08; JSON Schema Draft 2020-12 expressed in YAML form). This section is the human-readable contract.

#### Frontmatter

```yaml
---
type: cross_vault_request
title: "<short human-readable title>"
status: open                # open | accepted | rendering | shipped | closed | rejected | cancelled | queued
direction: "inbound (<receiving_vault> receives)"
requesting_vault: <vault.aDNA>
requesting_persona: <persona>
receiving_vault: <vault.aDNA>
receiving_persona: <persona>
requesting_agent: <agent_id>
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
priority: low                # low | medium | high | urgent
deadline: <iso8601 | "no_calendar_urgency">
audit_id: <pending | uuid_v4>      # populated by receiver on acceptance
artifact_request:
  type: <deck | comic | video | website | review | dataset | ...>
  spec_path: <path/to/spec.md>     # required
  output_sink: <path/to/sink/>     # required
  voice_mapping: <path>            # optional
  lattice_path: <path>             # optional
  quality_threshold: <0.0-1.0>     # optional
  forbidden_registers: [<list>]    # optional
constraints:
  max_revisions: <int>             # optional
  human_gate_required: <bool>      # optional
  budget_ceiling_usd: <float>      # optional
secrets_handled:                   # optional; see §4.4
  needed: [<list of secret types>]
  passthrough: [<env var names>]
  not_passed: <one-line statement>
idempotency_key: <string>          # optional; see §4.5
force_new: false                   # optional; see §4.5
federation_signature: <base64_ed25519_sig>             # optional v0.3+; see §4.6 — required when both vaults co-federated
federation_signature_key_version: <ln_manifest_pin>    # optional v0.3+; see §4.6 — companion to federation_signature
tags: [<list>]
---
```

**Required minimum** (Gap 3 core): `type`, `spec_path`, `output_sink`. A request with only these fields is conformant; everything else is optional and shaped by the work being commissioned. v0.3 adds two optional federation fields (`federation_signature` + `federation_signature_key_version`) which become required at request acceptance under the §4.6 co-federation rule.

#### Body sections (10-section canonical shape)

A conformant memo has these sections in order:

1. **TL;DR** — ≤ 3 sentences. What is being requested, by whom, with what calendar urgency.
2. **Request Context** — why this request exists; upstream campaign / mission / decision driving it.
3. **What Receiving Vault Receives** — slide-by-slide / file-by-file / dataset-by-dataset enumeration of inputs.
4. **What Receiving Vault Does Not Receive** — explicit non-deliveries (forge internals, unrelated context, secrets).
5. **Constraints + Notes** — operational constraints, voice/design rules, quality gates, persona handoffs.
6. **Acceptance Protocol** — concrete steps the receiver follows on pickup (validate, assign id, run, gate, ship).
7. **Rollback / Cancel** — failure handling: rejection path, partial-failure path, requester-side cancel path.
8. **Why This Memo** — rationale for using the request pattern (vs direct invocation or coord-memo fallback).
9. **Cross-References** — paths to related ADRs, charters, sibling memos, worked examples.
10. **Status Log** — table updated through the lifecycle (date | status | note).

A conformant memo MAY additionally include sections beyond these (e.g., `Risk Register`) but MUST include all 10 above.

### §4.4 Cross-vault secret delegation contract — Gap 4

When a request requires the receiver to use a secret (API key, credential, token), v0.2 specifies the delegation surface as the `secrets_handled` block in the request frontmatter:

```yaml
secrets_handled:
  needed: [ANTHROPIC_API_KEY, S3_OUTPUT_WRITE_TOKEN]
  passthrough: [ANTHROPIC_API_KEY]      # crosses the boundary as env-var only
  not_passed: "S3_OUTPUT_WRITE_TOKEN must be present in receiver's environment; not transmitted by requester."
```

**Receiver-side preflight contract** (MUST):
1. **Verify presence**: every name in `secrets_handled.needed` MUST be present in the receiver's environment at acceptance time.
2. **Reject on absence**: if any required secret is absent, the receiver MUST flip `status: rejected` with reason `missing_secret: <name>` rather than `accepted`.
3. **Audit names only**: the receiver MAY log secret *names* and presence-confirmation; the receiver MUST NOT log secret *values*.
4. **Boundary integrity**: the receiver MUST NOT export `passthrough` secrets to logs, attached files, or commits at either vault.

**Requester-side contract** (SHOULD):
1. **Minimize passthrough**: prefer `not_passed` (receiver provisions independently) over `passthrough` (requester transmits).
2. **Document expectations**: the `not_passed` field is a one-line human-readable statement of what does NOT cross — not a suppression list.

**Implementation guidance** is canonicalized at `what/artifacts/iii_airlock_substrate_implementation.md` §2 (authored at MC-4 close 2026-05-13 — preflight script structure, error-message conventions, audit-log schema). The contract above is normative regardless of implementation maturity.

**ADR cross-reference**: VideoForge ADR 003 §D2 names env-var passthrough as the consumer-side substrate for single-vault federation; v0.2 generalizes that pattern to cross-vault.

### §4.5 Idempotency contract — Gap 5

A cross-vault request MAY include an `idempotency_key` field for receiver-side deduplication:

```yaml
idempotency_key: videoforge_carly_herb_deck_v1
force_new: false
```

**Recommended key convention**: `<requesting_vault_short>_<artifact_kind>_<version_or_iteration>`.

**Receiver-side dedup contract** (MUST when `idempotency_key` is present):
1. **Match check**: on acceptance, the receiver MUST search its request archive (active + last 30 days closed/shipped) for a memo with matching `idempotency_key` from the same `requesting_vault`.
2. **Surface existing**: if a match is found and `force_new: false` (default), the receiver MUST reply-comment on the new memo with `status: duplicate_of: <existing_memo_path>` and link the prior `audit_id`. The new memo flips to `status: cancelled`.
3. **Allow override**: if `force_new: true`, the receiver opens a fresh session normally; the duplicate is logged but not blocked.
4. **No-key fallthrough**: if `idempotency_key` is absent, no dedup is performed; behaviour matches v0.1.0 (every memo is independent).

**Implementation guidance** is canonicalized at `what/artifacts/iii_airlock_substrate_implementation.md` §3 (authored at MC-4 close 2026-05-13 — cache structure, 30-day window mechanics, archive-search performance). The contract above is normative.

**Scaling rationale**: Low-priority for the current 2-forge workspace; matters when ≥ 5 forges with concurrent activity could double-file requests.

### §4.6 Federation signing-key verification contract — Gap 6 (v0.3 addition)

When the requesting vault and receiving vault are both participants in the same federation substrate (per [LN ADR-015 §a](~/lattice/LatticeNetwork.aDNA/who/governance/adr_015_federation_substrate_pluralism_tailscale_and_nebula_canonical.md) — Tailscale + Nebula canonical at v0.1), v0.3 introduces an Ed25519 signature verification preflight at request acceptance. §4.6 is the request-acceptance gate at which Ed25519 enforcement fires; §5 defines the broader federation-substrate observation contract that the gate consumes.

**Requester-side contract** (MUST when both vaults are co-federated; SHOULD when only the requesting vault is federated):

1. **Sign**: the requesting vault MUST sign the canonical-JSON serialization of the request frontmatter with its federation signing key (per [LN ADR-013 §a](~/lattice/LatticeNetwork.aDNA/who/governance/adr_013_federation_signing_key_infrastructure.md) operator-attested per-purpose-slot pattern). Place the base64-encoded Ed25519 signature in the `federation_signature:` field at the request frontmatter top level (added at §4.3).
2. **Cite key version**: include `federation_signature_key_version:` referencing the LN membership manifest entry version active at request-creation time. A key rotation per ADR-014 §a `MEMBERSHIP_NODE_RE_IDENTIFIED` event invalidates prior versions; this field lets the receiver resolve which key was authoritative when the request was signed.
3. **Canonical-JSON discipline**: per ADR-014 §c — sorted keys; NFC string normalization; whitespace trimmed; case preserved. The signed payload is the request frontmatter excluding the `federation_signature` field itself (the signature does not sign itself). Body sections are NOT signed at v0.3 (rationale: §4.3 body is human-edited prose; a wire-shape signature catches frontmatter tampering; v0.4 may add body-section signing).

**Receiver-side preflight contract** (MUST when both vaults are co-federated):

1. **Resolve requesting persona's Ed25519 pubkey**: from the LatticeNetwork.aDNA membership manifest (`transport.substrates[*].identity` per ADR-015 §b + ADR-013 §b per-purpose-slot dict). The `requesting_persona` field in the request frontmatter maps to a `canonical_id` per [ADR-010](~/lattice/LatticeNetwork.aDNA/who/governance/adr_010_canonical_node_id_schema_three_universes.md), which maps to a pubkey in the manifest.
2. **Verify signature**: over the canonical-JSON serialization of the request frontmatter (excluding the signature field itself).
3. **Reject on failure**: if the pubkey is absent OR the signature does not verify OR the pubkey is revoked per ADR-015 §c `MEMBERSHIP_CERT_REVOKED` events, the receiver MUST flip `status: rejected` with reason `signature_verification_failed: <subreason>`. Sub-reasons: `pubkey_absent`, `pubkey_revoked`, `signature_mismatch`, `key_version_unknown`.
4. **Boundary integrity**: the receiver MUST NOT log Ed25519 private keys (which it does not possess); MAY log the signature value (public artifact) and verification outcome to the audit log per the same JSONL pattern as the §4.4 secrets preflight.

**Profile applicability** — receiver MAY downgrade verification to advisory (warn-on-failure rather than reject) when:

- The requesting vault is not yet federated (no membership manifest entry exists; treat as v0.2 trust model — substrate fallback)
- The receiver itself is not federated (no canonical_id; ADR-015 §j non-pilot-tier defaults apply)
- A formal v0.2-compatibility mode is declared at the wrapper level (e.g., a consumer that hasn't yet adopted v0.3)

Advisory mode MUST be recorded in the audit log; promotion from advisory to enforce is a per-wrapper minor-bump decision.

**Implementation guidance** is **RESOLVED at MD-A2 close 2026-05-21** — see `what/artifacts/iii_airlock_substrate_implementation.md` §4 (Federation Signing-Key Verification (Ed25519) Preflight; 9 sub-sections covering preflight script structure, canonical-JSON serialization algorithm per ADR-014 §c, pubkey resolution from LN membership manifest per ADR-013 §b + ADR-015 §b/§d, 5-value sub-reason taxonomy, advisory-mode handling, pubkey cache + §5-driven invalidation, audit-log JSONL schema, boundary integrity, performance profile <200ms). §4.6 contract above is normative regardless of implementation maturity.

**Cross-references**: §5.2 Ed25519 verification contract semantics; §5.3 Ledger observation contract (where revoked-key state arrives from); §5.4 multi-substrate identity handling (where the substrate-traversal validation interacts with signature verification).

---

## §5 Federation-Substrate Awareness

v0.3 acknowledges that vault-to-vault traffic now traverses a federation substrate — the Lattice Protocol's identity, membership, and ledger layer. §5 specifies the airlock's contract WITH that substrate: how it observes ledger events, how it handles multi-substrate identity, and how it emits its own audit events back to the substrate.

The federation substrate itself is authored at LatticeNetwork.aDNA — see [LN ADR-014](~/lattice/LatticeNetwork.aDNA/who/governance/adr_014_re_id_semantics_node_canonical_id_transition.md) (ledger event semantics + idempotency) and [LN ADR-015](~/lattice/LatticeNetwork.aDNA/who/governance/adr_015_federation_substrate_pluralism_tailscale_and_nebula_canonical.md) (substrate pluralism + `transport.substrates[]` + Ed25519 identity). §5 binds the airlock to that substrate without redefining it.

### §5.1 Substrate-pluralism acknowledgement

Per ADR-015 §a, both **Tailscale** + **Nebula** are canonical Lattice Protocol federation substrates at v0.1. The airlock spec MUST be substrate-agnostic at the contract layer — every contract in §5 holds regardless of which substrate carries the traffic. Per-substrate identity details vary; per-substrate health and failover semantics are governed by the substrate's own state machine.

The authoritative substrate enumeration per node is the `transport.substrates[]` field in the node's `inventory_memberships.yaml` (under `node.aDNA/what/inventory/` per ADR-015 §b). The airlock SHOULD treat this list as truth and SHOULD NOT cache per-substrate identity locally beyond a short TTL (caching invites drift against substrate state events; if caching is implemented, ADR-015 §c `MEMBERSHIP_*` events MUST drive invalidation).

Canonical-pilot-tier nodes (per ADR-015 §e) MUST have ≥2 substrate entries (Tailscale + Nebula at v0.1). Non-pilot-tier nodes default to Tailscale-only (per ADR-015 §j); the airlock treats single-substrate and dual-substrate participants symmetrically at the contract layer — the difference is only in the size of `transport.substrates[]`, not in the contract semantics that §5.2–§5.4 govern.

### §5.2 Ed25519 signature verification contract

When the requesting and receiving vaults are co-federated, request acceptance includes Ed25519 signature verification per §4.6. The signing key is sourced from the LN membership manifest — the airlock MUST resolve the requesting persona's Ed25519 pubkey from `transport.substrates[*].identity` (per-purpose-slot dict per ADR-013 §b + ADR-015 §d).

Verification semantics (the contract; implementation deferred to MD-A2):

- **Canonical-JSON serialization**: per ADR-014 §c — sorted keys; NFC normalization; whitespace trimmed; case preserved.
- **Signed payload**: the request frontmatter excluding the `federation_signature` field itself. Body sections are NOT signed at v0.3.
- **Verification failure reasons**: `pubkey_absent`, `pubkey_revoked` (per ADR-015 §c `MEMBERSHIP_CERT_REVOKED`), `signature_mismatch`, `key_version_unknown` — each maps to a `rejected` lifecycle state with the matching sub-reason per §4.6.
- **Per-purpose-slot binding**: the signature MUST use the `federation_signing_pubkey_sha256` per-purpose slot per ADR-013 §b (NOT a transport-substrate cert key like `nebula_cert_sha`; transport-substrate keys authenticate substrate-layer connectivity, not airlock-layer requests).

§5.2 contract is identical to §4.6 viewed from the substrate side; §4.6 governs the request-acceptance gate, §5.2 governs the substrate-awareness side. Both must be satisfied together for an accepted co-federated request.

### §5.3 Ledger observation contract

The airlock MAY observe LN ledger events as a read-only consumer. Observation is opt-in per consumer wrapper (no v0.3 mandate); when opted in, the contract is:

**Observation surface** — the airlock subscribes to:

- `MEMBERSHIP_*` events per ADR-015 §c (`MEMBERSHIP_SUBSTRATE_JOINED`, `MEMBERSHIP_SUBSTRATE_LEFT`, `MEMBERSHIP_CERT_ISSUED`, `MEMBERSHIP_CERT_REVOKED`, `MEMBERSHIP_LIGHTHOUSE_SET`) + ADR-014 `MEMBERSHIP_NODE_RE_IDENTIFIED`.
- `CONNECTION_*` events per ADR-015 §c extension (`CONNECTION_ESTABLISHED`, `CONNECTION_TERMINATED`, `CONNECTION_HEALTH_DEGRADED`, `CONNECTION_FAILOVER` — each carrying a `payload.substrate_kind` discriminator at v0.3+; backward-compat default Tailscale per ADR-015 §c).

**Observation semantics** (read-only):

- The airlock MUST NOT write to the ledger directly. It MAY emit its own `COMPLIANCE_AUDIT` events (defined in §5.3.1 below); these are airlock-attested observations of acceptance/rejection events, NOT modifications of the ledger membership state.
- The airlock MAY use observed `MEMBERSHIP_CERT_REVOKED` events to invalidate cached pubkeys (when §5.2 verification needs a hot pubkey lookup).
- The airlock MAY use observed `MEMBERSHIP_NODE_RE_IDENTIFIED` events (per ADR-014) to re-resolve the `canonical_id → pubkey` mapping when a key version changes.
- The airlock MAY use observed `CONNECTION_HEALTH_DEGRADED` events to drive §5.4 multi-substrate failover decisions.

**Subscription model** — the airlock client implementation (MD-A2) chooses between polling, pub/sub, or chain-walk; the contract above is implementation-agnostic. Subscription scope SHOULD be the receiving vault's own canonical_id and its declared peers; broader subscription is permitted but increases ingest cost.

#### §5.3.1 `COMPLIANCE_AUDIT` event emission (v0.3 candidate; assumes_draft)

The airlock MAY emit a `COMPLIANCE_AUDIT` event upon request acceptance or rejection, recording the audit_id, the request memo path, the disposition, and a hash of the request frontmatter at acceptance time. Schema:

```yaml
event_type: "COMPLIANCE_AUDIT"               # assumes_draft per ADR-003 rule 2 + ADR-015 §c Carry 3 EP-1 pattern; native enum membership at LN Phase 5 fold-in
event_id: "evt_<uuid_v4>"                    # generated per ADR-006 §a + ledger_sync._generate_event_id
timestamp: "<UTC ISO-8601 Z>"                # acceptance or rejection close time
source_did: "<receiving_vault_canonical_id>" # the airlock-hosting node
target_did: "<requesting_vault_canonical_id>"
previous_event_hash: "<prev_entry_hash>"
payload_hash: "<sha256_of_canonical_json_payload>"
signature: "<ed25519_sig_or_empty>"           # ADR-013 §a operator-attestation
payload:
  audit_id: "<uuid_v4>"                       # matches the request memo's audit_id field
  request_path: "<vault_relative_path>"       # e.g., who/coordination/coord_2026_05_21_videoforge_requests_demo_deck.md
  disposition: "<accepted | rejected | cancelled | duplicate_of>"
  rejection_reason: "<sub_reason | null>"     # populated only when disposition == rejected (e.g., signature_verification_failed.pubkey_absent)
  request_frontmatter_hash: "<sha256_of_canonical_json>"   # hash of the frontmatter that was acted upon (excluding federation_signature)
  airlock_spec_version: "0.3.0"
  substrate_kind: "<tailscale | nebula>"      # which substrate carried the inbound request
  signature_verified: <true | false | "advisory_mode">
  operator_attestation: "<persona, timestamp, optional free-text rationale>"
```

**Idempotency** (per ADR-014 §c): re-emission with identical inputs MUST produce identical event_id + payload_hash + entry_hash. The deterministic-ordering requirement applies to any list-valued payload fields (none at v0.3.0 but reserved).

**`assumes_draft` binding**: `COMPLIANCE_AUDIT` is a new event-type candidate at v0.3 — same pattern as ADR-014's `MEMBERSHIP_NODE_RE_IDENTIFIED` (per ADR-014 §e) and ADR-015's 5 substrate-tagged candidates (per ADR-015 §c). Native `LedgerEventType` enum membership lands at LN Carry 3 EP-1 ratification (Phase 5 integration seam per both LN ADRs). Until then, direct-JSON-write workaround applies (same canonical hash algorithm as native enum path; see F-S23-02 disposition in LN backlog).

### §5.4 Multi-substrate identity handling

When a node participates in multiple substrates (canonical-pilot-tier per ADR-015 §e), the airlock MUST handle multi-substrate identity coherently:

1. **canonical_id resolution**: the requesting persona's `canonical_id` (per ADR-010) is the substrate-independent identity; the airlock resolves it to a `transport.substrates[]` list and validates that the inbound request arrived via a substrate present in that list.
2. **Per-substrate validation**: the request's transport substrate (which overlay/TCP path it arrived on) MUST match one of the requesting node's enumerated substrates. A request arriving via Tailscale from a canonical_id whose membership manifest lists only Nebula is a substrate-mismatch — receiver MUST flip `status: rejected` with reason `substrate_mismatch` (a §4.6 sub-reason class).
3. **Failover semantics**: when one substrate degrades (per ADR-015 §c `CONNECTION_HEALTH_DEGRADED` event observed), the receiver MAY accept requests on the alternate substrate (if both nodes are dual-substrate per ADR-015 §e). Failover is a substrate-level concern; the airlock observes substrate-failover state via §5.3 ledger events but does not initiate failover itself.
4. **Pilot vs non-pilot symmetry**: the airlock applies the SAME §5.4 contract to pilot-tier (dual-substrate) and non-pilot-tier (typically single-substrate Tailscale per ADR-015 §j) nodes; the difference is only in the size of the `transport.substrates[]` list, not in contract semantics.
5. **`substrate_kind` in `COMPLIANCE_AUDIT`**: every emitted audit event (§5.3.1) MUST carry `payload.substrate_kind` recording the substrate the request arrived on; this provides substrate-traceability per ADR-015 §e clause 5 for downstream audit reconciliation.

### §5.5 Forward-references

Six §4 + §5 contracts have implementation forward-referenced to subsequent missions; the v0.3 implementation rows resolved at MD-A2 close 2026-05-21:

- **§4.6 + §5.2 Ed25519 verification implementation**: **RESOLVED at MD-A2 close 2026-05-21** — see `what/artifacts/iii_airlock_substrate_implementation.md` §4 (9 sub-sections: preflight script + canonical-JSON algorithm + pubkey resolution + sub-reason taxonomy + advisory mode + cache invalidation + audit schema + boundary integrity + performance budget).
- **§5.3 Ledger observation subscription model**: **RESOLVED at MD-A2 close 2026-05-21** — see `what/artifacts/iii_airlock_substrate_implementation.md` §5 (7 sub-sections; recommendation: polling at 60s cadence over LN Tier-1 file-ledger; pub/sub deferred to v0.4+; chain-walk rejected).
- **§5.3.1 `COMPLIANCE_AUDIT` emission implementation**: **RESOLVED at MD-A2 close 2026-05-21** — see `what/artifacts/iii_airlock_substrate_implementation.md` §6 (7 sub-sections: trigger sites + payload assembly + assumes_draft direct-JSON-write workaround + idempotency mechanics + local audit-log mirror + substrate_kind propagation + failure modes).
- **Cross-vault RLHF aggregation over the §5 contracts**: deferred to **MD-B3** — uses the federation-aware airlock for cross-vault learning-store signal transit (per Campaign D charter §Critical Path: MD-B3 cannot ship before MD-A1 v0.3 spec exists; MD-A2 substrate-impl now satisfies). RESOLVED at MD-B3.
- **End-to-end validation across the live federation substrate**: deferred to **MD-A5** — re-exercise of the VideoForge → CanvasForge worked example with Ed25519 signing + ledger observation active. RESOLVED at MD-A5.
- **`COMPLIANCE_AUDIT` native enum membership**: deferred to **LN Carry 3 EP-1 ratification at Phase 5 integration seam** (per ADR-014 §e + ADR-015 §c cluster amendment). Until then, `assumes_draft: true` per ADR-003 rule 2; MD-A2 §6.3 direct-JSON-write workaround in production.

§5 contracts (the prose in §5.1–§5.4) are normative at v0.3.0; implementation maturity follows in MD-A2 per the §4.4 + §4.5 precedent.

---

## §6 Worked Example Reference

The canonical worked example is `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` — VideoForge's commission of a Carly + Herb sprint onboarding deck from CanvasForge. The memo predates v0.2 and was authored against the v0.1.0 coord-memo fallback; it conforms to the §4.3 shape with three additive deltas v0.2 + v0.3 introduce (`secrets_handled`, `idempotency_key`, and the v0.3 `federation_signature` + `federation_signature_key_version` pair).

### §6.1 Coverage map (worked example → spec sections)

| Spec section | Worked-example evidence |
|--------------|--------------------------|
| §4.1 Initiation ceremony — filename + location | Lives at `CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` ✅ |
| §4.1 Lifecycle states | Frontmatter `status: open`; status log carries pending acceptance/rendering/shipped/closed rows ✅ |
| §4.2 Handshake (full profile applies — receiver in active mode) | Status log models the full state set; ETA-equivalent encoded as "no calendar urgency / weeks-of-flexibility" ✅ |
| §4.3 Frontmatter — required minimum | `type`, `artifact_request.type` (deck), `spec_path` (slide outline), `output_sink` (`presentationforge/decks/`) ✅ |
| §4.3 Frontmatter — optional | `priority`, `deadline`, `audit_id` (pending shape), `voice_mapping`, `lattice_path`, `quality_threshold`, `forbidden_registers` ✅ |
| §4.3 Body — TL;DR | § TL;DR ✅ |
| §4.3 Body — Request Context | § Request Context ✅ |
| §4.3 Body — What Receiving Vault Receives | § What CanvasForge Receives ✅ |
| §4.3 Body — What Receiving Vault Does Not Receive | § What CanvasForge Does Not Receive ✅ |
| §4.3 Body — Constraints + Notes | § Constraints + Notes ✅ |
| §4.3 Body — Acceptance Protocol | § Acceptance Protocol ✅ |
| §4.3 Body — Rollback / Cancel | § Rollback / Cancel ✅ |
| §4.3 Body — Why This Memo | § Why This Memo + Wrapper, Instead of Direct Pipeline Invocation? ✅ |
| §4.3 Body — Cross-References | § Cross-References ✅ |
| §4.3 Body — Status Log | § Status Log ✅ |
| §4.4 Secret delegation | "Pipeline secrets (none required for a deck-build; no API keys cross the boundary)" — `secrets_handled` block can be added retroactively as `not_passed: "deck render uses local Anthropic API key only; no cross-vault delegation"` ✅ (additive delta) |
| §4.5 Idempotency | Not present in worked example — additive delta; future commissions of `videoforge_carly_herb_deck_v1` would surface this memo ✅ (additive delta) |
| §4.6 Federation signing-key verification | Not present in worked example — both vaults are co-federated as of LN Phase B1 (2026-05-21) so a v0.3 re-exercise would require `federation_signature` + `federation_signature_key_version` per §4.6 ✅ (additive delta; re-exercised at MD-A5) |

### §6.2 Additive deltas

The worked example was authored against v0.1.0 and conforms to v0.3 with three purely additive amendments (no breaking changes):

1. **`secrets_handled` block** (v0.2 delta) — can be added retroactively as `not_passed: "deck render uses local Anthropic API key only; no cross-vault delegation"`. The original implicit non-passthrough becomes explicit.
2. **`idempotency_key` field** (v0.2 delta) — optional; absent in the original because no duplicate filing was anticipated. Can be added as `videoforge_carly_herb_deck_v1` to opt into v0.2 dedup.
3. **`federation_signature` + `federation_signature_key_version` pair** (v0.3 delta) — required at the v0.3 re-exercise since both VideoForge.aDNA and CanvasForge.aDNA participate in the same federation post-Phase B1. MD-A5 validation will exercise this delta against the live substrate.

All three deltas are non-blocking: the memo as authored remains valid evidence under v0.3, with the v0.3 federation-signature delta deferred to MD-A5 for live-substrate validation.

---

## §7 Versioning Policy

The Airlock Standard versions independently from III.aDNA core (skill, modules, lattice). However, AIRLOCK.md v0.X is pinned in the III.aDNA repo, so III.aDNA git tags carry the airlock version forward (e.g., III.aDNA `v0.2.0` ships AIRLOCK.md `v0.2.0`; v0.3.0 spec ships within Campaign D and lands the matching III.aDNA `v0.3.0` tag at DG-D close).

### §7.1 Bump rules

| Bump | Example | Trigger | Consumer review |
|------|---------|---------|-----------------|
| **Patch** | `v0.3.0 → v0.3.1` | Typo / clarity / cross-reference fix | None — transparent on next session |
| **Minor** | `v0.2 → v0.3` | Additive (new entry path; new optional payload field; new optional lifecycle state; new optional preflight contract; new optional event-type emission). v0.2→v0.3 is the canonical example: federation-substrate awareness (§5) + Ed25519 sig-verify (§4.6) + new optional frontmatter fields + new optional `COMPLIANCE_AUDIT` event-type — all additive; v0.2-conformant requests remain valid; consumers MAY adopt §5 contracts at their wrapper-level minor-bump review per ADR-002 §3. | Required per ADR-002 §3 — consumer agent reads the airlock changelog and decides whether to update the pinned version |
| **Major** | `v0.x → v1.0` | Breaking (entry-path removal; required-field reshape; lifecycle-state semantics change; advisory → mandatory promotion of Ed25519 verification across all federation contexts) | Required — explicit human decision at each consumer wrapper |

### §7.2 Consumer pinning

Every III.aDNA consumer wrapper pins the airlock version implicitly through its `federation_ref.version` (which is the III.aDNA version). When III.aDNA bumps, the airlock may or may not bump along with it. Consumers MUST consult the III.aDNA changelog at every minor or major III.aDNA bump to determine whether airlock review is required.

The consumer-wrapper schema in `iii/CLAUDE.md` does NOT carry a separate `airlock_version` field — that would create drift surface against `federation_ref.version`. The III.aDNA version pin transitively governs the airlock version.

### §7.3 Reference instance pinning

`how/airlock/AIRLOCK.md` carries its own `version:` frontmatter field that MUST match the airlock standard version it implements. At AIRLOCK.md ship time the spec version and the implementation version are equal; if a patch bump on the spec doesn't require an AIRLOCK.md text change, the implementation version follows the spec version anyway (so consumers can verify the pair without reading both files). v0.3.0 spec ships with the AIRLOCK.md v0.3.0 bump landed alongside MD-A3 (activation kit) per Campaign D charter §Critical Path.

### §7.4 ADR-002 federation

This versioning policy refines [ADR-002 §3](../decisions/adr_002_consumer_federation_contract.md) for the airlock surface specifically. ADR-002 §3 establishes `version_policy: minor | locked` for the consumer→III federation pin in general; this section §7.1 clarifies what a minor airlock bump entails (additive only) versus what a major bump entails (breaking). The two policies do not conflict — ADR-002 governs *whether* the consumer reviews; this section governs *what they're reviewing for*.

### §7.5 Federation-substrate version pin

The airlock spec versions independently from the LN federation substrate (ADR-014 + ADR-015). However, v0.3+ depends on specific LN substrate features (`transport.substrates[]` field; `MEMBERSHIP_*` event types; Ed25519 keypair infrastructure per ADR-013). The frontmatter `ln_substrate_version_pin` field declares the minimum LN substrate state this airlock spec version requires:

- **v0.3.0 ln_substrate_version_pin**: LN.aDNA pc_01 Phase A close (Ed25519 keypair LIVE + bore.pub:42561 tunnel + wga_l1 4-event admission; landed 2026-05-20) + Phase B1 ADR-014 v0.1 + ADR-015 v0.1 (both accepted 2026-05-20; cross-substrate first-emit validated at HTTP 200 2026-05-21).

When LN substrate state advances (e.g., ADR-014 v0.2 archive-pointer integration; ADR-015 v0.2 broader substrate vocabulary; Carry 3 EP-1 enum-widening fold-in at Phase 5), an airlock spec patch or minor bump may be required to declare the new pin:

- **Patch bump suffices** when LN advances are purely additive (new optional events; new optional payload fields that the airlock doesn't observe).
- **Minor bump fires** when the airlock contract surface itself extends (e.g., new event-type observation added to §5.3; new substrate-kind value added to ADR-015 §b — requiring airlock-side validation).
- **Major bump fires** when LN advances break airlock contracts (e.g., revocation semantics change in a way that breaks §4.6 verification).

R1 mitigation per Campaign D charter: this version pin lets v0.3 spec ship at the current LN substrate state without forecasting future LN evolution. Re-pin at LN Phase 3 cleanup or Phase 5 spec-rev hub as needed.

---

## §8 Coverage and Boundaries

### §8.1 What v0.3 covers

| Surface | Covered |
|---------|---------|
| Inbound entry paths (5+) | ✅ §3 (v0.1.0; unchanged) |
| Cross-vault requests (single-receiver) | ✅ §4 (v0.2; unchanged) |
| Request-initiation ceremony | ✅ §4.1 (v0.2; unchanged) |
| Handshake (lightweight + full profiles) | ✅ §4.2 (v0.2; unchanged) |
| Payload schema (frontmatter + 10-section body) | ✅ §4.3 (v0.2 + 2 new optional v0.3 fields: `federation_signature` + `federation_signature_key_version`) |
| Cross-vault secret delegation contract | ✅ §4.4 (v0.2; unchanged) |
| Idempotency contract | ✅ §4.5 (v0.2; unchanged) |
| Federation signing-key verification contract | ✅ §4.6 (v0.3) |
| Federation-substrate awareness — substrate-pluralism acknowledgement | ✅ §5.1 (v0.3) |
| Federation-substrate awareness — Ed25519 verification (substrate side) | ✅ §5.2 (v0.3) |
| Federation-substrate awareness — read-only ledger observation contract | ✅ §5.3 (v0.3; opt-in per wrapper) |
| `COMPLIANCE_AUDIT` event emission shape | ✅ §5.3.1 (v0.3; assumes_draft per ADR-015 §c Carry 3 EP-1 pattern) |
| Federation-substrate awareness — multi-substrate identity handling | ✅ §5.4 (v0.3) |
| Versioning policy (patch/minor/major) | ✅ §7 (v0.2 + §7.5 federation-substrate version pin in v0.3) |

### §8.2 What v0.3 does NOT cover (deferred)

| Surface | Defer to | Rationale |
|---------|----------|-----------|
| `proposed/` channel for proposal triage | v0.4+ | Channel does not yet exist in III.aDNA (per VideoForge ADR-005 RLHF pattern source); coord memo at `who/coordination/` is the v0.3 fallback |
| Cross-graph federation pin negotiation (consumer ↔ forge initial federation) | ADR-002; sf_forge_pattern_spec.md | Out of airlock scope; airlock governs traffic, federation governs pinning |
| Multi-vault transactional requests (one requester, multiple receivers, atomic ship) | v0.4+ | No present-day worked example; design speculative |
| Async batched requests (one memo, many artifacts shipped over time) | v0.4+ | No present-day worked example; scaling concern only |
| Body-section signing (extend §4.6 from frontmatter-only to body-inclusive) | v0.4+ | Wire-shape signature catches frontmatter tampering at v0.3; body signing requires canonical-Markdown discipline not yet specified |
| Ed25519 verification implementation (preflight scripts; key resolution from LN manifest; canonical-JSON algorithm; performance budget) | Authored at MD-A2 ✅ 2026-05-21 — impl-doc §4 | Implementation guidance shipped at `what/artifacts/iii_airlock_substrate_implementation.md` §4 v0.3.0 (documentation-grade per R3) |
| `COMPLIANCE_AUDIT` event emission implementation (ledger client integration; idempotency mechanics; assumes_draft direct-JSON-write workaround) | Authored at MD-A2 ✅ 2026-05-21 — impl-doc §6 | Implementation guidance shipped at `what/artifacts/iii_airlock_substrate_implementation.md` §6 v0.3.0; assumes_draft workaround spec'd until LN Carry 3 EP-1 Phase 5 fold-in |
| Ledger observation client subscription model (§5.3 deferred to MD-A2 implementation chooses) | Authored at MD-A2 ✅ 2026-05-21 — impl-doc §5 | Implementation guidance shipped at `what/artifacts/iii_airlock_substrate_implementation.md` §5 v0.3.0; polling at 60s cadence recommended; pub/sub deferred v0.4+ |
| Cross-vault RLHF aggregation contract (per Campaign D Track D2; rides on §5 federation-aware airlock) | **MD-B3** | Track D2 work; uses §5 contracts as substrate for cross-vault learning-store signal transit |
| End-to-end validation across the live federation substrate (re-exercise of VideoForge → CanvasForge worked example with v0.3 federation features active) | **MD-A5** | Federation-integration validation mission per Campaign D Track D1 |
| `COMPLIANCE_AUDIT` native `LedgerEventType` enum membership | LN **Carry 3 EP-1 fold-in at Phase 5** | Cluster amendment with ADR-014 + ADR-015 candidates; until then `assumes_draft: true` per ADR-003 rule 2 |
| Substrate-enforcement *implementation* (preflight scripts, idempotency cache code) for §4.4 + §4.5 | MC-4 ✅ 2026-05-13 | Implementation guidance shipped at `what/artifacts/iii_airlock_substrate_implementation.md` §2 + §3 (documentation-grade per R3; executable runtime deferred to a future Platform.aDNA integration) |
| Reply-comment template for handshake | MC-3 ✅ 2026-05-10 | Authored at `how/templates/template_cross_vault_request_reply.md` (Acceptance full-profile + Rejection variants; profile-selection rules cross-linked to schema `x-handshake-profiles`) |
| Machine-readable YAML schema | MC-2 ✅ 2026-05-08 | This spec carries the human-readable shape; MC-2 shipped the validator-friendly YAML at `what/artifacts/iii_airlock_request_schema.yaml` (JSON Schema Draft 2020-12); v0.3 frontmatter additions (`federation_signature` + `federation_signature_key_version`) are additive optional fields — schema update folded into MD-A2 |

### §8.3 Forward-references in this spec

Five v0.2-originating forward-references all resolved through DG-C; v0.3 introduces five new forward-references (one resolved in this MD-A1 mission; four pending across MD-A2 / MD-A5 / MD-B3 / LN Phase 5).

**v0.2-originating (all resolved)**:

- §4.3 cited `what/artifacts/iii_airlock_request_schema.yaml` — **authored at MC-2 2026-05-08; live link**.
- §4.4 cited "implementation guidance authored at MC-4" — **resolved at MC-4 close 2026-05-13**; live link to `what/artifacts/iii_airlock_substrate_implementation.md` §2.
- §4.5 cited "implementation guidance authored at MC-4" — **resolved at MC-4 close 2026-05-13**; live link to `what/artifacts/iii_airlock_substrate_implementation.md` §3.
- §8.2 (was §7.2) row "Reply-comment template for handshake" cited `how/templates/template_cross_vault_request_reply.md` — **authored at MC-3 2026-05-10; live link**.
- §9.5 (was §8.4) cross-references row "Reference instance (entry paths): `how/airlock/AIRLOCK.md` (v0.1.0 ships; v0.2.0 bump at MC-3)" — **bumped at MC-3 2026-05-10**; AIRLOCK.md carries `version: "0.2.0"`; v0.3.0 bump scheduled for MD-A3 (activation kit).
- MC-5 validation forward-reference — **RESOLVED at MC-5 close 2026-05-20** (commit `1ea1c4d`; `what/artifacts/mc5_validation_videoforge_canvasforge_v0_2.md` authored; zero regression confirmed; inbound proposal flipped `absorbed → closed`).

**v0.3-originating (three resolved at MD-A2, four pending)**:

- §4.6 + §5.2 Ed25519 verification implementation — **RESOLVED at MD-A2 close 2026-05-21** — see `what/artifacts/iii_airlock_substrate_implementation.md` §4 (9 sub-sections; documentation-grade per R3).
- §5.3 Ledger observation subscription model — **RESOLVED at MD-A2 close 2026-05-21** — see `what/artifacts/iii_airlock_substrate_implementation.md` §5 (7 sub-sections; polling at 60s recommended; pub/sub v0.4+; chain-walk rejected).
- §5.3.1 `COMPLIANCE_AUDIT` emission implementation — **RESOLVED at MD-A2 close 2026-05-21** — see `what/artifacts/iii_airlock_substrate_implementation.md` §6 (7 sub-sections; assumes_draft direct-JSON-write workaround until LN Carry 3 EP-1).
- AIRLOCK.md v0.2.0 → v0.3.0 bump + activation kit packaging — deferred to **MD-A3**. RESOLVED at MD-A3.
- 6 consumer wrappers minor-bump review v0.2.0 → v0.3.0 (lattice-labs/iii v0.1.0 → v0.3.0 carry-forward absorbed) — deferred to **MD-A4**. RESOLVED at MD-A4.
- End-to-end validation across the live federation substrate — deferred to **MD-A5**. RESOLVED at MD-A5.
- Cross-vault RLHF aggregation contract using §5 — deferred to **MD-B3** (Campaign D Track D2). RESOLVED at MD-B3.
- `COMPLIANCE_AUDIT` native enum membership — deferred to **LN Carry 3 EP-1 ratification at Phase 5 integration seam** per ADR-014 §e + ADR-015 §c cluster amendment.

A consumer agent reading this spec at v0.3.0 SHOULD treat the contracts (the prose in §4.6 + §5.1–§5.4) as normative. Implementation guidance for §4.6 + §5 landed at MD-A2 ✅ (impl-doc v0.3.0; documentation-grade per R3); live-substrate validation lands at MD-A5; consumer adoption fires at MD-A4 minor-bump sweep.

---

## §9 Provenance

### §9.1 v0.1.0 origin

The airlock pattern originated in Campaign A MA-4 (2026-05-08, III.aDNA commit `1628793`). v0.1.0 elevated `how/airlock/AIRLOCK.md` from a stub to a reference implementation with 5 entry paths, a Path Selection matrix, and an explicit "What v0.1.0 does NOT cover" anti-regression section deferring cross-vault request patterns to v0.2 / Campaign C MC-1.

### §9.2 v0.2 origin

v0.2 absorbs `who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md` — VideoForge.aDNA's contribution after exercising v0.1.0 against the Carly + Herb deck commission. The proposal named 5 specific gaps (Gaps 1–5 in this spec map 1:1 to §4.1–§4.5).

The proposal landed as a coord memo because III.aDNA does not yet have a `proposed/` channel for proposal triage. When that channel exists, the proposal will migrate to it; until then, `who/coordination/` is the canonical inbound-proposal surface.

### §9.3 v0.2 inbound proposal acknowledgment

The v0.2 spec accepted the VideoForge proposal in full. All 5 gaps absorbed; no gap deferred. The proposal's "Suggested v0.2 Mission Sequencing" was adopted as Campaign C's mission queue (MC-1 spec → MC-2 schema → MC-3 reference impl → MC-4 substrate → MC-5 validation). Campaign C closed 2026-05-20 9/9 DG-C; the proposal flipped `absorbed → closed` at MC-5 commit `1ea1c4d`.

### §9.4 v0.3 origin

v0.3 absorbs the federation-substrate intersect surfaced by:

1. **III MC-4.5 alignment recon dossier** (`what/artifacts/mc4_5_alignment_recon_dossier.md` §3.4 + §5.2) — surveyed LatticeNetwork.aDNA pc_01 Phase A federation substrate landing 2026-05-20; identified D1 federation-aware airlock v0.3 as the load-bearing intersect.
2. **Outbound coord memo to LN Venus** (`who/coordination/coord_2026_05_20_iii_to_ln_federation_substrate_intersect.md`) — fired at Campaign D charter ratification 2026-05-21T00:50Z; explicitly stated D1 candidate cadence including substrate-observation contract intersect.
3. **LatticeNetwork.aDNA pc_01 Phase A + Phase B1 landing** — Ed25519 keypair LIVE on wga_l1 (2026-05-20T03:29:08Z provisioning); bore.pub:42561 durable tunnel; first cross-machine `COMPLIANCE_AUDIT` ledger emit; 4-event admission ceremony; Phase B1 dual-substrate ratification (ADR-014 + ADR-015 both accepted 2026-05-20); cross-substrate first-emit validated at HTTP 200 (2026-05-21).
4. **Campaign D charter** (`how/campaigns/campaign_d_federation_adaptive_loop/campaign_d_federation_adaptive_loop.md`) ratified 2026-05-20 with D1 Federation-Aware Airlock v0.3 as one of two parallel tracks; MD-A1 (this spec) declared first sequential step of D1.

v0.3 introduces the third interaction surface (federation-substrate awareness) as additive to v0.2; no v0.2 contracts are reshaped or removed. Per Campaign D charter §Risk Register R1, the spec declares an `ln_substrate_version_pin` (§7.5) so v0.3 ships at current LN substrate state without forecasting future LN evolution.

### §9.5 Cross-references

- Campaign C charter: `how/campaigns/campaign_c_airlock_standard/campaign_c_airlock_standard.md` (CLOSED 2026-05-20 9/9)
- Campaign D charter: `how/campaigns/campaign_d_federation_adaptive_loop/campaign_d_federation_adaptive_loop.md` (OPEN; MD-A1 + MD-B1 + MD-B2 closed at v0.3.0 ship; DG-D 3/11)
- Reference instance (entry + request surfaces): `how/airlock/AIRLOCK.md` (v0.2.0 — bumped at MC-3 2026-05-10; substrate-enforcement forward-reference at line 258 resolved at MC-4 close 2026-05-13; v0.3.0 bump deferred to MD-A3 activation kit)
- Substrate implementation guidance: `what/artifacts/iii_airlock_substrate_implementation.md` (v0.2.0 — authored at MC-4 2026-05-13; resolves §4.4 + §4.5 forward-references; v0.3 extension authored at MD-A2 — resolves §4.6 + §5.2 + §5.3.1 forward-references)
- Reference instance (cross-vault request, worked example): `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md`
- Inbound proposal (v0.2): `who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md` (absorbed at v0.2; closed at MC-5 2026-05-20)
- Federation-pin policy: `what/decisions/adr_002_consumer_federation_contract.md` §3
- Learning-store ownership (canonical upstream + per-vault forks): `what/decisions/adr_003_learning_store_ownership.md`
- RLHF Signal Channel ADR (Campaign D MD-B1 sibling deliverable; cross-vault aggregation rides on §5 at MD-B3): `what/decisions/adr_005_rlhf_signal_channel.md`
- Adaptive-Improvement Loop Architecture ADR (Campaign D MD-B1 sibling deliverable): `what/decisions/adr_007_adaptive_improvement_loop.md`
- Adaptive-improvement loop spec (Campaign D MD-B1 sibling artifact; established v0.3 frontmatter cadence patterns this spec follows): `what/artifacts/iii_adaptive_improvement_loop_spec.md`
- Forge canonical pattern (entry-time federation, separate from airlock): `~/lattice/SiteForge.aDNA/what/artifacts/sf_forge_pattern_spec.md`
- VideoForge ADR-003 (cross-graph entry contract; pattern source for §4.4): `~/lattice/VideoForge.aDNA/what/decisions/adr_003_cross_graph_entry_contract.md`
- VideoForge ADR-005 (RLHF channel; pattern source for `proposed/` deferred to §8.2): `~/lattice/VideoForge.aDNA/what/decisions/adr_005_context_learning_rlhf.md`
- **LatticeNetwork.aDNA ADR-014** (`MEMBERSHIP_NODE_RE_IDENTIFIED` ledger event v0.1 + idempotency contract): `~/lattice/LatticeNetwork.aDNA/who/governance/adr_014_re_id_semantics_node_canonical_id_transition.md`
- **LatticeNetwork.aDNA ADR-015** (federation-substrate pluralism v0.1 — Tailscale + Nebula canonical; `transport.substrates[]` schema; 5 substrate-tagged event types): `~/lattice/LatticeNetwork.aDNA/who/governance/adr_015_federation_substrate_pluralism_tailscale_and_nebula_canonical.md`
- LatticeNetwork.aDNA ADR-013 (federation signing-key infrastructure; per-purpose-slot pattern — pubkey source for §4.6 + §5.2): `~/lattice/LatticeNetwork.aDNA/who/governance/adr_013_federation_signing_key_infrastructure.md`
- LatticeNetwork.aDNA ADR-010 (canonical-node-id schema three universes; `canonical_id` resolution for §5.4): `~/lattice/LatticeNetwork.aDNA/who/governance/adr_010_canonical_node_id_schema_three_universes.md`
- LatticeNetwork.aDNA pc_01 mission (load-bearing dependency closed 2026-05-20/21): `~/lattice/LatticeNetwork.aDNA/how/campaigns/campaign_alphalattice_genesis/missions/mission_pc_01_mccoy_macmini_wga_canonicalization_exemplar.md`
- Outbound coord memo to LN Venus (fired at Campaign D charter; this spec resolves its load-bearing intersect ask): `who/coordination/coord_2026_05_20_iii_to_ln_federation_substrate_intersect.md`
- Outbound coord memo to LL Berthier (fired at Campaign D charter; flags LL.aDNA as plausible D1 Day-1 consumer): `who/coordination/coord_2026_05_20_iii_to_ll_airlock_status_post_mc4_5.md`

---

*Standard maintained by III.aDNA (Argus Panoptes). Bumps and breaking changes go through Campaign C/D cycles; v0.3.0 authored at Campaign D MD-A1 (2026-05-21).*
