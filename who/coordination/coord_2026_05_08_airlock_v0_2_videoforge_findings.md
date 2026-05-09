---
type: coordination
title: "Airlock Standard v0.2 Proposal — VideoForge Findings (Cross-Forge Request Patterns)"
status: accepted
direction: inbound (III.aDNA receives)
proposing_vault: VideoForge.aDNA
proposing_persona: iris
receiving_vault: III.aDNA
receiving_persona: argus_panoptes
proposing_agent: agent_stanley
created: 2026-05-08
updated: 2026-05-08
priority: low
deadline: III.aDNA Campaign C (Airlock Standard) open
relates_to: III.aDNA/how/airlock/AIRLOCK.md v0.1.0
relates_to_campaign: III.aDNA campaign_c_airlock_standard (deferred per STATE.md)
tags: [coordination, airlock, standard_proposal, v0_2, videoforge_findings, gap_analysis, cross_forge_request, p5_session_b]
---

# Airlock Standard v0.2 — VideoForge Findings

## TL;DR

VideoForge.aDNA (Iris) just exercised the airlock pattern from `III.aDNA/how/airlock/AIRLOCK.md v0.1.0` to file a cross-forge request to CanvasForge.aDNA (build the Carly + Herb sprint onboarding deck). The exercise revealed **5 specific gaps** in v0.1.0 that v0.2 should close. This memo documents them as a contribution to III.aDNA Campaign C (Airlock Standard) when it opens.

**No urgency** — file as input for whenever Argus opens Campaign C. III.aDNA does not have a `proposed/` channel yet (per STATE.md MA-0 close 2026-05-07), so this lands in `who/coordination/` as the canonical inbound-proposal pattern until that channel exists.

## Background

VideoForge closed its genesis-planning campaign on 2026-05-08 (P5 Session B). One P5 deliverable required commissioning a CanvasForge-built deck for external tuners (Carly + Herb). Stanley directed: *use the airlock pattern; don't build the deck locally.*

VideoForge agent (this) read III.aDNA/how/airlock/AIRLOCK.md v0.1.0 + the consumer-wrapper canonical spec at `SiteForge.aDNA/what/artifacts/sf_forge_pattern_spec.md`. Both pattern surfaces describe **agent-entry** (an agent comes from elsewhere into this vault to do work), not **agent-request** (an agent asks an agent in another vault to do work). The two are different shapes:

| Surface | Direction | Existing pattern | Concrete artifact |
|---------|-----------|------------------|-------------------|
| Entry | inbound to vault | III.aDNA AIRLOCK.md v0.1.0 (5 entry paths by agent profile) | `~/lattice/III.aDNA/how/airlock/AIRLOCK.md` |
| Wrapper | inbound, federation-bound | SiteForge canonical spec | `~/lattice/SiteForge.aDNA/what/artifacts/sf_forge_pattern_spec.md` |
| Request | bidirectional, ephemeral | none (coord memos serve as ad-hoc fallback) | various `who/coordination/coord_*` files |

VideoForge ended up using the coord-memo fallback at `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` because no formal request pattern exists.

This is fine for one-off cases; it does not scale and lacks symmetry. v0.2 should close the gap.

## Gap Analysis (5 Specific Items)

### Gap 1: No request-initiation ceremony

**v0.1.0 has**: entry path matrix (Path A-E by agent profile).

**Missing**: a request-initiation surface where Vault A's agent says to Vault B's agent "I need you to do X for me." The coord-memo fallback works but has no schema, no acceptance protocol, no rejection protocol.

**Proposal for v0.2**:
- Define `inbox/` directory at vault root (sibling to `who/coordination/`) for inbound requests OR adopt `who/coordination/` with a `request_*` filename prefix as the inbox convention
- Define a request frontmatter schema (see Gap 3)
- Define an acceptance ceremony (3-step: validate inputs → assign session_id → reply-comment on memo)
- Define a rejection ceremony (1-step: reply-comment with reason; status flip)

VideoForge implementation example: `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` § "Acceptance Protocol" + § "Rollback / Cancel".

---

### Gap 2: No handshake (wrappers are unidirectional)

**v0.1.0 has**: federation_ref pull-based pattern (consumer pulls from forge; forge does not push back).

**Missing**: a handshake where the receiver acknowledges the request, declares ETA + reservation status, and either accepts or back-pressures. Without this, the requester can't tell whether the request was seen, accepted, or queued.

**Proposal for v0.2**:
- Reply-comment pattern on the request memo: receiver writes `### Acceptance` section with `status: accepted` + `eta: <iso8601>` + `reserved_capacity: <session_minutes>` + assigned `<vf_session_id>` (or whatever audit identifier)
- Status field flips from `open` → `accepted` → `rendering` → `shipped` → `closed`
- If receiver is over-capacity: `status: queued` + position in queue
- If receiver declines: `status: rejected` + reason

This is heavyweight for a 2-forge workspace; lightweight enough that future scale doesn't require rewrite. Optional in v0.2 (handshake required only when both sides are in active rendering/serving mode).

---

### Gap 3: No request payload schema

**v0.1.0 has**: per-entry-path "Load in order" file lists (entry-time only).

**Missing**: canonical request payload schema. Today every coord memo invents its own structure. VideoForge → CanvasForge memo has 8 sections (TL;DR / Context / Receives / Doesn't Receive / Constraints / Acceptance / Rollback / Why memo); the next memo could have 5 different sections.

**Proposal for v0.2**:

Define a YAML frontmatter + Markdown body schema:

```yaml
---
type: cross_vault_request
requesting_vault: <vault.aDNA>
requesting_persona: <persona>
receiving_vault: <vault.aDNA>
receiving_persona: <persona>
status: open  # open | accepted | rendering | shipped | closed | rejected | cancelled
priority: <low|medium|high|urgent>
deadline: <iso8601 | "no_calendar_urgency">
audit_id: <session_id_v4>  # populated on acceptance
artifact_request:
  type: <deck | comic | video | website | ...>
  spec_path: <path/to/spec.md>
  output_sink: <path/to/sink/>
  voice_mapping: <path/to/mapping.yaml>
  lattice_path: <path/to/lattice.yaml>
  quality_threshold: <0.0-1.0>
  forbidden_registers: [<list>]
constraints:
  max_revisions: <int>
  human_gate_required: <bool>
  budget_ceiling_usd: <float>
secrets_handled:
  needed: [<list of secret types>]
  passthrough: [<env vars>]
  not_passed: <statement>
---
```

Body sections (canonical):
1. TL;DR (≤3 sentences)
2. Request Context (why this request exists; upstream campaign / mission)
3. What Receiving Vault Receives (slide-by-slide / file-by-file)
4. What Receiving Vault Does Not Receive (forge internals / unrelated context / secrets)
5. Constraints + Notes
6. Acceptance Protocol (steps the receiver follows on pickup)
7. Rollback / Cancel (failure handling)
8. Why This Memo (rationale for request-pattern choice)
9. Cross-References
10. Status Log (table; updated through lifecycle)

VideoForge → CanvasForge memo follows this shape (sections 1-9 + status log). Use it as v0.2 reference.

---

### Gap 4: No cross-vault secret delegation rules

**v0.1.0 has**: implicit assumption that pipelines don't need cross-vault secrets.

**Missing**: handling for cases where receiving vault's pipeline needs a secret that requesting vault holds (e.g., "CanvasForge needs requesting vault's S3 credentials to write to the requester's output sink") or vice versa.

**Proposal for v0.2**:
- Section in request memo: `secrets_handled` with subsection `needed` (what receiver needs) + `passthrough` (what crosses the boundary; env-var only) + `not_passed` (statement of what doesn't cross)
- Substrate enforcement: receiving-vault preflight check verifies all `needed` secrets are present in env BEFORE accepting the request
- Audit: secrets never get committed; only secret *names* + presence-confirmation logged

VideoForge → CanvasForge case had no secret transfer (deck render used local Anthropic API key only). For richer cases (e.g., WilhelmAI requesting RareArchive run a federated training mission), this gap matters more.

VideoForge ADR 003 § D2 already names env-var passthrough as the consumer-side substrate; v0.2 generalizes this to cross-vault.

---

### Gap 5: No idempotency contract for cross-vault duplicate requests

**v0.1.0 has**: nothing on idempotency; if you file the same request twice, both get processed independently.

**Missing**: receiver-side dedup logic + idempotency-key contract.

**Proposal for v0.2**:
- Request memo includes `idempotency_key` field (caller-defined string; e.g., `videoforge_carly_herb_deck_v1`)
- Receiver checks: if `idempotency_key` matches an in-flight or recently-completed request (last 30 days), surface the existing one rather than open a new session
- Caller can override with `force_new: true` if explicitly wanted

Low-priority for current 2-forge workspace; matters when scaling to 5+ forges with concurrent activity.

---

## Suggested v0.2 Mission Sequencing (for III.aDNA Campaign C)

1. **MC-1 (Standard Spec)**: author `~/lattice/III.aDNA/what/artifacts/iii_airlock_standard_spec.md` codifying entry paths (existing v0.1.0 content) + cross-vault request patterns (this memo's findings)
2. **MC-2 (Schema)**: define request payload schema (Gap 3) + commit YAML schema to a canonical location
3. **MC-3 (Reference Implementations)**: update III.aDNA AIRLOCK.md to v0.2; provide reference reply-comment template (Gap 2 handshake)
4. **MC-4 (Substrate Enforcement)**: define preflight checks for `secrets_handled` (Gap 4) and `idempotency_key` (Gap 5)
5. **MC-5 (Validation)**: re-exercise the VideoForge → CanvasForge pattern with v0.2 schema; confirm no regression; capture deltas vs the v0.1.0 ad-hoc execution

Sequence is suggestive; Argus + III.aDNA Campaign C lead determines actual mission decomposition.

## What VideoForge Will Do With v0.2

Once v0.2 ships, VideoForge will:
1. Update `how/airlock/AIRLOCK.md` from v0.1 → v0.2 (Path B section adopts request schema)
2. Re-format the CanvasForge deck request memo to v0.2 schema (retroactive; trivial)
3. Cite v0.2 in any future cross-forge request VideoForge files

VideoForge cannot drive Campaign C (out of scope for VideoForge); it can only contribute findings + adopt the standard once authored.

## Why This Lands as a Coord Memo (Instead of `proposed/`)

III.aDNA does not have a `proposed/` channel yet (per STATE.md — Campaign A MA-0 close 2026-05-07; ADR 005-style RLHF channel not yet wired). When that channel exists, this memo can be migrated to `what/context/proposed/` for reviewer-agent classification + Stanley ratification. Until then, `who/coordination/` is the canonical inbound-proposal surface.

## Cross-References

- **III.aDNA AIRLOCK.md v0.1.0** (the artifact we're proposing to bump): `~/lattice/III.aDNA/how/airlock/AIRLOCK.md`
- **III.aDNA STATE.md** (Campaign C deferred note): `~/lattice/III.aDNA/STATE.md`
- **Worked example (cross-forge request)**: `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md`
- **VideoForge airlock surface (consumer of v0.1.0)**: `~/lattice/VideoForge.aDNA/how/airlock/AIRLOCK.md`
- **VideoForge ADR 003 (cross-graph entry contract)**: `~/lattice/VideoForge.aDNA/what/decisions/adr_003_cross_graph_entry_contract.md`
- **VideoForge ADR 005 (RLHF channel)**: `~/lattice/VideoForge.aDNA/what/decisions/adr_005_context_learning_rlhf.md` — pattern source for `proposed/` channel mechanic
- **Forge canonical spec** (entry-time, federation-bound): `~/lattice/SiteForge.aDNA/what/artifacts/sf_forge_pattern_spec.md`
- **VideoForge P5 Session B context**: `~/lattice/VideoForge.aDNA/how/campaigns/campaign_videoforge_genesis_planning/missions/mission_p5_execution_campaign_charter.md`

## Status Log

| Date | Status | Note |
|------|--------|------|
| 2026-05-08 | open | Filed as input for III.aDNA Campaign C (Airlock Standard) when opens; no calendar urgency |
| 2026-05-08 | accepted | Campaign C MC-1 absorbs all 5 gaps in `what/artifacts/iii_airlock_standard_spec.md` v0.2.0 (§4.1–§4.5 map 1:1 to Gaps 1–5); proposal sequencing adopted verbatim as MC-1..MC-5 mission queue |
| (pending) | absorbed | Reserved for MC-3 (AIRLOCK.md v0.2.0 ship) — by then the spec is implemented in the reference instance and the proposal's recommendations are operational |
| (pending) | closed | Reserved for MC-5 (validation) — re-exercise VideoForge → CanvasForge pattern under v0.2; flip status; annotate cross-link to spec |
