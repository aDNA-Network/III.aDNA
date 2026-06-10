---
type: session
session_id: session_stanley_20260508_iii_adna_mc2_request_schema
mission: MC-2 Airlock Request Payload Schema (YAML)
campaign: campaign_c_airlock_standard
created: 2026-05-08
updated: 2026-05-08
status: completed
operator: Stanley
agent: Argus (Claude Opus 4.7)
last_edited_by: agent_stanley
parent_plan: /Users/stanley/.claude/plans/please-read-the-claude-md-compiled-badger.md
tags: [session, airlock, schema, v0_2, mc-2, dg_c_p1, cross_vault_request, json_schema]
---

# Session: III.aDNA MC-2 — Airlock Request Payload Schema (YAML)

## Objectives

Author `what/artifacts/iii_airlock_request_schema.yaml` — the machine-readable, validator-friendly schema that codifies the cross-vault request frontmatter contract from `iii_airlock_standard_spec.md` §4.3.

Resolve the three forward-references in the spec that cite this file (frontmatter `defers_schema_to`, §4.3 prose, §7.2 row 7, §7.3 bullet 1).

This is a schema-authoring deliverable. No AIRLOCK.md edits (MC-3), no reply-comment template (MC-3), no preflight scripts (MC-4), no validation re-exercise (MC-5), no git tag.

## Decisions Carried In (from plan)

1. **JSON Schema Draft 2020-12 expressed in YAML form.** Spec §4.3 + §7.2 say "validator-friendly YAML" verbatim. `jsonschema` libraries accept YAML-loaded dicts; comments survive; reads cleanly alongside the prose spec. Precedent: `what/lattices/lattice_yaml_schema.json` (JSON form); MC-2 transliterates to YAML.

2. **Required-minimum exactly per spec §4.3.** Top-level `required: [type, artifact_request]`; nested `artifact_request.required: [spec_path, output_sink]`. Everything else optional.

3. **Open at root + `artifact_request`; closed on `constraints` and `secrets_handled`.** Consumer vaults may add domain-specific fields without schema bumps; closed-shape sub-objects prevent typo drift on the substrate-critical contracts.

4. **Body sections documented, not validated.** Captured as `x-body-sections` extension list. Schema is the *frontmatter* contract; body shape is markdown.

5. **No spec version bump.** Edits to spec resolving forward-references are patch-level per §6.1 (cross-reference fix; transparent on next session). Spec stays v0.2.0.

6. **No git tag.** Annotated v0.2.0 tag still waits for MC-3 close.

## Source Material (read-only inputs)

- `what/artifacts/iii_airlock_standard_spec.md` v0.2.0 — schema source of truth (§4.3 frontmatter contract, §4.1 lifecycle, §4.4 secrets, §4.5 idempotency)
- `~/aDNA/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md` — validation target (worked example)
- `what/lattices/lattice_yaml_schema.json` — JSON Schema style precedent
- `how/campaigns/campaign_c_airlock_standard/campaign_c_airlock_standard.md` — charter, DG-C criteria
- `how/sessions/history/2026-05/session_stanley_20260508_iii_adna_mc1_airlock_standard_spec.md` — session-file format precedent

## Activity Log

1. `git pull --ff-only` — already up-to-date at `df00413` (MC-1 closure).
2. Read MC-1 spec, Campaign C charter, VideoForge inbound proposal, CanvasForge worked example, lattice-yaml-schema.json precedent, and MC-1 session file.
3. Created session file at `how/sessions/active/session_stanley_20260508_iii_adna_mc2_request_schema.md`.
4. Authored `what/artifacts/iii_airlock_request_schema.yaml` — JSON Schema Draft 2020-12 in YAML form (~13 KB; 23 properties; 5 `x-*` extensions).
5. Validation v1: ran 9-test suite via `uv run --with jsonschema --with pyyaml`. Caught a real bug — `oneOf` with overlapping branches (`enum` + open `string`) on `artifact_request.type` failed when value matched both. Fixed by switching to plain `type: string` with `examples` annotation (idiomatic JSON Schema for "open enum with documented known values").
6. Validation v2: full 9/9 pass — schema validity (Draft 2020-12); required-minimum positive; 4 negative-path tests; full conformant memo positive; novel `artifact_request.type` accepted; closed-shape constraint typo rejected; UPPER_SNAKE secret-name pattern enforced. Worked-example walk: correctly fails with `'artifact_request' is a required property` (top-level `type: coordination` + body-promoted paths) — both gaps expected per spec §5.2.
7. Spec edits (patch-level; no version bump) — `defers_schema_to` frontmatter comment + §4.3 prose + §7.2 row 7 + §7.3 forward-reference list. All four references resolved.
8. Charter edits — DG-C MC-2 box ✅; mission table MC-2 row ✅ COMPLETE; Phase Plan P1 row ✅ COMPLETE; frontmatter `phase: P1 → P2`.
9. STATE.md edits — last_session pointer; Current Phase + MC-2 closure note; Campaigns table; Mission Queue MC-2 row; Blockers; new Latest Direction (post-MC-2) block; superseded the post-MC-1 boot block; preserved chronological history.
10. Verification: 6 files modified (1 new + 5 edits) + 1 session file in active/. Schema parses + validates. Session moves to `how/sessions/history/2026-05/` at close.

## AAR (lightweight, 5-line)

- **What worked**: spec §4.3 prose was a clean 1:1 source for the schema; mechanical validation caught the `oneOf` overlap bug before commit; closed-shape `additionalProperties: false` on `constraints` + `secrets_handled` adds typo protection without blocking consumer extension elsewhere.
- **What surprised**: the worked example (CanvasForge memo) is structurally further from v0.2 than spec §5.1 implied — `spec_path` and `output_sink` are body-promoted, not just additive-delta absent. Spec §5.2 still covers this under "additive-deltas; non-blocking", but a hypothetical MC-5 re-exercise would need to author a new memo or substantially restructure the original.
- **What carried forward**: nothing left for MC-2; carry-forwards belong to MC-3 (AIRLOCK.md text bump + reply-comment template + `v0.2.0` tag) and MC-4 (preflight scripts referenced in spec §4.4 + §4.5).
- **Cycle hygiene**: `git pull` clean; single mission scope held; no scope creep into MC-3 territory; verification step caught a real bug.
- **Next signal**: Stanley chooses MC-3 (closes the v0.2.0 release loop) or MB-2 (highest-value Campaign B wrapper).
