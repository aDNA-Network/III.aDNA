---
type: standard_spec
title: "III.aDNA Airlock Standard"
version: "0.2.0"
status: reference_implementation
created: 2026-05-08
updated: 2026-05-08
last_edited_by: agent_stanley
governs: how/airlock/AIRLOCK.md
predecessor_version: "0.1.0"
inbound_proposal: who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md
worked_example: ~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md
defers_schema_to: what/artifacts/iii_airlock_request_schema.yaml  # authored MC-2 2026-05-08
defers_substrate_to: MC-4 substrate enforcement (pending)
covers: [entry_paths, cross_vault_request_patterns]
authoring_mission: MC-1
campaign: campaign_c_airlock_standard
tags: [standard, airlock, spec, v0_2, cross_vault_request, entry_paths]
---

# III.aDNA Airlock Standard — v0.2.0

> **Status note**: This is the *standard* — the contract that the operational airlock at `how/airlock/AIRLOCK.md` and downstream consumer wrappers implement. It defines the schema; the AIRLOCK.md file is the reference *instance*. When the spec changes, AIRLOCK.md and consumer wrappers update against it; the spec itself is the source of truth for what an airlock-conformant artifact looks like.

## §1 Purpose & Scope

The III.aDNA Airlock Standard governs **vault-to-vault traffic** — the ways agents and artifacts cross context-graph boundaries between aDNA vaults. v0.2.0 covers two complementary surfaces:

- **Entry paths** (carried forward from v0.1.0): inbound, pull-based, single-direction. An external agent enters a vault to run a quality-improvement loop (or other localized work) using the vault's own context. Reference instance: `how/airlock/AIRLOCK.md`.
- **Cross-vault request patterns** (added in v0.2): bidirectional, ephemeral, two-vault handshake. An agent in vault A commissions an agent in vault B to do work for vault A. Reference instance: cross-vault request memos in the receiving vault's `who/coordination/`. Worked example on file: VideoForge → CanvasForge Carly + Herb deck commission.

### What this standard does NOT govern

- **Intra-vault session protocol** — how an agent inside a single vault runs sessions, opens/closes campaigns, manages tasks. That is the vault's own concern.
- **Federation pin negotiation** — when consumer wrappers update their `federation_ref.version`. That lives in [ADR-002 §3](../decisions/adr_002_consumer_federation_contract.md).
- **Lattice-internal contract types** — module I/O, lattice edge schemas, learning store records. Those are governed by the III modules, lattice yaml, and ADR-003.
- **Cross-graph federation negotiation** — when a consumer initially federates against a forge or framework. Covered by the Forge Canonical Pattern Spec (`SiteForge.aDNA/what/artifacts/sf_forge_pattern_spec.md`) and ADR-002.

### What this standard does

For every vault-to-vault interaction, this spec gives:
1. A **shape contract**: required and optional fields, body sections, lifecycle states.
2. A **routing rule**: where the artifact lives on disk, who picks it up, who replies.
3. A **safety contract**: how secrets cross (or don't), how duplicate work is suppressed.
4. A **versioning rule**: how consumers know whether their pinned version still matches what they're loading.

---

## §2 The Two Surfaces

The two surfaces are complementary, not competing. Most vault interactions use exactly one; some (e.g., a consumer that both runs III review locally AND occasionally commissions a forge to build an artifact) use both.

### §2.1 Decision matrix

| If the agent is doing this... | ...use surface |
|-------------------------------|----------------|
| Coming into vault B to read context and do work locally (e.g., run III review on vault B's artifact) | **Entry** (§3) |
| Asking an agent in vault B to do work and ship results back to vault A (e.g., VideoForge requests CanvasForge build a deck for VideoForge's output sink) | **Request** (§4) |
| Federating against vault B at wrapper-creation time (one-time pin) | Out of scope — see ADR-002 |
| Sending a structured proposal for vault B's roadmap | Out of scope in v0.2 — deferred to `proposed/` channel (v0.3+) |

### §2.2 Surface comparison

| Property | Entry (§3) | Request (§4) |
|----------|-----------|--------------|
| Direction | Inbound to the vault being entered | Bidirectional between two vaults |
| Initiator | The arriving agent | The requesting agent |
| Persistence | Ephemeral session (agent enters, runs, exits) | Ephemeral memo (filed, picked up, shipped, closed) |
| Reference instance | `how/airlock/AIRLOCK.md` | Cross-vault request memo (receiving vault's `who/coordination/`) |
| Required for v0.2 conformance | Yes (5 entry paths today; additive across minor bumps) | Yes for cross-vault work; lightweight profile for trivial cases |
| Handshake | None — pull-based | Optional in lightweight profile; required in full profile |
| Audit identifier | Session id at the entered vault | Audit id reconciled across both vaults |

---

## §3 Entry Path Standard

An entry path is a routing recipe that maps an arriving agent's profile and target artifact to the minimum-viable context-loading order. v0.1.0 shipped 5 entry paths (A text, B web/visual, C code, D video, E vault maintenance). v0.2 generalizes the *schema* — what every entry path declares — and additively allows new paths.

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

A cross-vault request is an ephemeral, file-on-disk artifact that lets one agent commission work from another. v0.2 absorbs five gaps surfaced by VideoForge's exercise of v0.1.0 against a real cross-forge commission: initiation ceremony, handshake, payload schema, secret delegation, idempotency.

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
| `rejected` | Receiver declined (capacity, scope mismatch, lattice incompatibility) |
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
tags: [<list>]
---
```

**Required minimum** (Gap 3 core): `type`, `spec_path`, `output_sink`. A request with only these fields is conformant; everything else is optional and shaped by the work being commissioned.

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

**Implementation guidance** (preflight script structure, error-message conventions, audit-log schema): authored at MC-4 (substrate enforcement; pending). The contract above is normative regardless of implementation maturity.

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

**Implementation guidance** (cache structure, 30-day window mechanics, archive-search performance): authored at MC-4. The contract above is normative.

**Scaling rationale**: Low-priority for the current 2-forge workspace; matters when ≥ 5 forges with concurrent activity could double-file requests.

---

## §5 Worked Example Reference

The canonical worked example is `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` — VideoForge's commission of a Carly + Herb sprint onboarding deck from CanvasForge. The memo predates v0.2 and was authored against the v0.1.0 coord-memo fallback; it conforms to the §4.3 shape with two additive deltas v0.2 introduces.

### §5.1 Coverage map (worked example → spec sections)

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

### §5.2 Additive deltas

The worked example was authored against v0.1.0 and conforms to v0.2 with two purely additive amendments (no breaking changes):

1. **`secrets_handled` block** — can be added retroactively as `not_passed: "deck render uses local Anthropic API key only; no cross-vault delegation"`. The original implicit non-passthrough becomes explicit.
2. **`idempotency_key` field** — optional; absent in the original because no duplicate filing was anticipated. Can be added as `videoforge_carly_herb_deck_v1` to opt into v0.2 dedup.

Both deltas are non-blocking: the memo as authored remains valid evidence under v0.2.

---

## §6 Versioning Policy

The Airlock Standard versions independently from III.aDNA core (skill, modules, lattice). However, AIRLOCK.md v0.X is pinned in the III.aDNA repo, so III.aDNA git tags carry the airlock version forward (e.g., III.aDNA `v0.2.0` ships AIRLOCK.md `v0.2.0`).

### §6.1 Bump rules

| Bump | Example | Trigger | Consumer review |
|------|---------|---------|-----------------|
| **Patch** | `v0.2.0 → v0.2.1` | Typo / clarity / cross-reference fix | None — transparent on next session |
| **Minor** | `v0.2 → v0.3` | Additive (new entry path; new optional payload field; new optional lifecycle state) | Required per ADR-002 §3 — consumer agent reads the airlock changelog and decides whether to update the pinned version |
| **Major** | `v0.x → v1.0` | Breaking (entry-path removal; required-field reshape; lifecycle-state semantics change) | Required — explicit human decision at each consumer wrapper |

### §6.2 Consumer pinning

Every III.aDNA consumer wrapper pins the airlock version implicitly through its `federation_ref.version` (which is the III.aDNA version). When III.aDNA bumps, the airlock may or may not bump along with it. Consumers MUST consult the III.aDNA changelog at every minor or major III.aDNA bump to determine whether airlock review is required.

The consumer-wrapper schema in `iii/CLAUDE.md` does NOT carry a separate `airlock_version` field — that would create drift surface against `federation_ref.version`. The III.aDNA version pin transitively governs the airlock version.

### §6.3 Reference instance pinning

`how/airlock/AIRLOCK.md` carries its own `version:` frontmatter field that MUST match the airlock standard version it implements. At AIRLOCK.md ship time the spec version and the implementation version are equal; if a patch bump on the spec doesn't require an AIRLOCK.md text change, the implementation version follows the spec version anyway (so consumers can verify the pair without reading both files).

### §6.4 ADR-002 federation

This versioning policy refines [ADR-002 §3](../decisions/adr_002_consumer_federation_contract.md) for the airlock surface specifically. ADR-002 §3 establishes `version_policy: minor | locked` for the consumer→III federation pin in general; this section §6.1 clarifies what a minor airlock bump entails (additive only) versus what a major bump entails (breaking). The two policies do not conflict — ADR-002 governs *whether* the consumer reviews; this section governs *what they're reviewing for*.

---

## §7 Coverage and Boundaries

### §7.1 What v0.2 covers

| Surface | Covered |
|---------|---------|
| Inbound entry paths (5+) | ✅ §3 |
| Cross-vault requests (single-receiver) | ✅ §4 |
| Request-initiation ceremony | ✅ §4.1 |
| Handshake (lightweight + full profiles) | ✅ §4.2 |
| Payload schema (frontmatter + 10-section body) | ✅ §4.3 |
| Cross-vault secret delegation contract | ✅ §4.4 |
| Idempotency contract | ✅ §4.5 |
| Versioning policy | ✅ §6 |

### §7.2 What v0.2 does NOT cover (deferred)

| Surface | Defer to | Rationale |
|---------|----------|-----------|
| `proposed/` channel for proposal triage | v0.3+ | Channel does not yet exist in III.aDNA (per VideoForge ADR-005 RLHF pattern source); coord memo at `who/coordination/` is the v0.2 fallback |
| Cross-graph federation pin negotiation (consumer ↔ forge initial federation) | ADR-002; sf_forge_pattern_spec.md | Out of airlock scope; airlock governs traffic, federation governs pinning |
| Multi-vault transactional requests (one requester, multiple receivers, atomic ship) | v0.3+ | No present-day worked example; design speculative |
| Async batched requests (one memo, many artifacts shipped over time) | v0.3+ | No present-day worked example; scaling concern only |
| Substrate-enforcement *implementation* (preflight scripts, idempotency cache code) | MC-4 | This spec carries the contract; MC-4 carries the implementation guidance |
| Reply-comment template for handshake | MC-3 | This spec carries the field requirements; MC-3 ships the template at `how/templates/template_cross_vault_request_reply.md` |
| Machine-readable YAML schema | MC-2 ✅ 2026-05-08 | This spec carries the human-readable shape; MC-2 shipped the validator-friendly YAML at `what/artifacts/iii_airlock_request_schema.yaml` (JSON Schema Draft 2020-12) |

### §7.3 Forward-references in this spec

Three sections originally cited deliverables that did not yet exist at v0.2.0 ship; one is now resolved (MC-2), two remain pending (MC-4):

- §4.3 cites `what/artifacts/iii_airlock_request_schema.yaml` — **authored at MC-2 2026-05-08; live link**.
- §4.4 cites "implementation guidance authored at MC-4" — pending.
- §4.5 cites "implementation guidance authored at MC-4" — pending.

A consumer agent encountering these forward-references during a v0.2.0 session SHOULD treat the contracts (the prose in §4.3–§4.5) as normative even when the cited file is absent. The cited files become live links as Campaign C progresses; the contracts do not change between MC-1 close and MC-5 close.

---

## §8 Provenance

### §8.1 v0.1.0 origin

The airlock pattern originated in Campaign A MA-4 (2026-05-08, III.aDNA commit `1628793`). v0.1.0 elevated `how/airlock/AIRLOCK.md` from a stub to a reference implementation with 5 entry paths, a Path Selection matrix, and an explicit "What v0.1.0 does NOT cover" anti-regression section deferring cross-vault request patterns to v0.2 / Campaign C MC-1.

### §8.2 v0.2 origin

v0.2 absorbs `who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md` — VideoForge.aDNA's contribution after exercising v0.1.0 against the Carly + Herb deck commission. The proposal named 5 specific gaps (Gaps 1–5 in this spec map 1:1 to §4.1–§4.5).

The proposal landed as a coord memo because III.aDNA does not yet have a `proposed/` channel for proposal triage. When that channel exists, the proposal will migrate to it; until then, `who/coordination/` is the canonical inbound-proposal surface.

### §8.3 Inbound proposal acknowledgment

This spec accepts the VideoForge proposal in full. All 5 gaps absorbed; no gap deferred. The proposal's "Suggested v0.2 Mission Sequencing" was adopted as Campaign C's mission queue (MC-1 spec → MC-2 schema → MC-3 reference impl → MC-4 substrate → MC-5 validation).

### §8.4 Cross-references

- Campaign C charter: `how/campaigns/campaign_c_airlock_standard/campaign_c_airlock_standard.md`
- Reference instance (entry paths): `how/airlock/AIRLOCK.md` (v0.1.0 ships; v0.2.0 bump at MC-3)
- Reference instance (cross-vault request, worked example): `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md`
- Inbound proposal: `who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md`
- Federation-pin policy: `what/decisions/adr_002_consumer_federation_contract.md` §3
- Learning-store ownership (canonical upstream + per-vault forks): `what/decisions/adr_003_learning_store_ownership.md`
- Forge canonical pattern (entry-time federation, separate from airlock): `~/lattice/SiteForge.aDNA/what/artifacts/sf_forge_pattern_spec.md`
- VideoForge ADR-003 (cross-graph entry contract; pattern source for §4.4): `~/lattice/VideoForge.aDNA/what/decisions/adr_003_cross_graph_entry_contract.md`
- VideoForge ADR-005 (RLHF channel; pattern source for `proposed/` deferred to §7.2): `~/lattice/VideoForge.aDNA/what/decisions/adr_005_context_learning_rlhf.md`

---

*Standard maintained by III.aDNA (Argus Panoptes). Bumps and breaking changes go through Campaign C cycles.*
