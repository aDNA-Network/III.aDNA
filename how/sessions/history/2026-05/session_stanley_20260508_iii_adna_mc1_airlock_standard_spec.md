---
type: session
session_id: session_stanley_20260508_iii_adna_mc1_airlock_standard_spec
mission: MC-1 Airlock Standard Spec v0.2.0
campaign: campaign_c_airlock_standard
created: 2026-05-08
updated: 2026-05-08
status: completed
operator: Stanley
agent: Argus (Claude Opus 4.7)
last_edited_by: agent_stanley
parent_plan: /Users/stanley/.claude/plans/please-read-the-claude-md-encapsulated-wirth.md
tags: [session, airlock, standard, v0_2, mc-1, dg_c_p1, cross_vault_request]
---

# Session: III.aDNA MC-1 — Airlock Standard Spec v0.2.0

## Objectives

Open Campaign C P1 by authoring `what/artifacts/iii_airlock_standard_spec.md` v0.2.0 — the canonical standard that codifies both inbound entry paths (carrying v0.1.0 forward) and the new cross-vault request patterns (absorbing the 5 VideoForge gaps).

This is a documentation deliverable. No AIRLOCK.md edits (MC-3), no schema YAML (MC-2), no substrate-enforcement code (MC-4), no consumer-wrapper changes, no git tag.

## Decisions Carried In (from plan)

1. **Spec describes pattern; AIRLOCK.md is the reference instance.** Spec defines entry-path *schema* (required fields per path) and cites AIRLOCK.md as the canonical v0.2 instance. No verbatim duplication of Path A–E bodies — that creates drift across two surfaces.

2. **Request memo filename convention**: `<receiving_vault>/who/coordination/coord_<YYYY_MM_DD>_<requesting_vault>_requests_<artifact>.md`. Adopt the worked-example pattern (CanvasForge memo); do not introduce a new `inbox/` directory.

3. **Schema illustrative in spec; canonical at MC-2.** The v0.2 spec body shows the YAML frontmatter shape and 10-section markdown body. Machine-readable schema lands at `what/artifacts/iii_airlock_request_schema.yaml` (MC-2).

4. **Substrate contracts in §4.4 + §4.5 (this spec); implementation in MC-4.** Receiver MUST verify `secrets_handled.needed` are present in env before flipping `accepted`. Receiver MUST surface in-flight or last-30-day match against `idempotency_key` unless `force_new: true`. The spec carries the contract; MC-4 carries preflight script structure.

5. **Handshake (Gap 2) is OPTIONAL in v0.2.** Lightweight 2-forge requests use `open → shipped → closed`. Full handshake (`accepted`, `rendering`, `queued`, etc.) required only when both vaults are in active rendering/serving mode.

6. **No git tag.** v0.2.0 tag waits for MC-3 (or MC-5).

## Source Material (read-only inputs)

- `how/campaigns/campaign_c_airlock_standard/campaign_c_airlock_standard.md` — charter, DG-C criteria, mission table
- `who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md` — VF inbound proposal, 5 gaps, schema source
- `how/airlock/AIRLOCK.md` v0.1.0 — entry-path content the spec generalizes from
- `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` — worked example
- `what/decisions/adr_002_consumer_federation_contract.md` — federation-pin + version-policy reference (§3 minor/locked/patch)
- `how/sessions/history/2026-05/session_stanley_20260508_iii_adna_mb1_lattice_labs_wrapper.md` — session-file format precedent

## Activity Log

1. `git pull` — already up-to-date at `0ef4488` (MB-1 closure).
2. Read ADR-002 (federation pin / version policy) — confirmed §3 minor/locked/patch convention; spec §6.4 federates with this.
3. Created `what/artifacts/` directory (mkdir -p).
4. Authored `what/artifacts/iii_airlock_standard_spec.md` (29 KB; 8 body sections per plan structure).
5. Flipped `who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md` status `open → accepted`; status-log row added with explicit reservation entries for `absorbed` (MC-3) and `closed` (MC-5).
6. Updated Campaign C charter — frontmatter `status: in_flight`; MC-1 row + DG-C MC-1 box checked; P1 phase IN-FLIGHT; Promises section updated (#1 + #3 DELIVERED 2026-05-08; #2 holds until MC-3).
7. Updated STATE.md — Current Phase, Campaigns table, Mission Queue, Blockers, Latest Direction (full MC-1 closure block), Fresh-session boot pointer.
8. Verification: 6/6 passes — no AIRLOCK.md edits, all wikilinks resolve, spec at canonical path, 5/5 gap coverage in §4.1–§4.5, forward-references tagged pending with mission owners, file size 29 339 B.
9. Session moved to `how/sessions/history/2026-05/`.

## AAR (lightweight, 5-line)

**What worked**: Spec design as schema-not-instance kept the artifact tight (no AIRLOCK.md duplication; AIRLOCK.md remains the operational surface, spec carries the contract). VF proposal absorbed verbatim — Gap 1–5 → §4.1–§4.5 1:1; zero design synthesis required to bridge gaps to spec sections.
**What surprised**: §6.2 explicit-no-second-version-field decision (no `airlock_version` in consumer wrappers) emerged during writing — drift surface concern not anticipated in plan; spec self-corrected.
**What's next**: MC-2 (request schema YAML at `what/artifacts/iii_airlock_request_schema.yaml`) is the natural sequential follow-up; small (0.5 sess); unblocks MC-3 reply-comment template; together MC-2 + MC-3 cut the `v0.2.0` git tag.
**Carry-forward**: AIRLOCK.md line 226 ("to be authored") is technically stale; deliberately left for MC-3 to rewrite alongside the v0.2.0 bump (avoids touching AIRLOCK.md at MC-1 per plan discipline).
**No blockers**.
