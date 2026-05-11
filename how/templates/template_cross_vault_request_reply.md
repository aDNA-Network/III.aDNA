---
type: template
template_name: cross_vault_request_reply
governs: cross_vault_request memo § Acceptance / § Rejection sections
spec_section: what/artifacts/iii_airlock_standard_spec.md §4.1, §4.2
schema_section: what/artifacts/iii_airlock_request_schema.yaml x-lifecycle-transitions, x-handshake-profiles
ship_mission: MC-3
campaign: campaign_c_airlock_standard
version: "0.2.0"
profile_supports: [lightweight, full]
created: 2026-05-10
updated: 2026-05-10
last_edited_by: agent_stanley
tags: [template, airlock, cross_vault_request, reply_comment, handshake, v0_2]
---

# Cross-Vault Request Reply-Comment Template

> Reference text for the receiving vault's reply-comment appended to a cross-vault request memo at acceptance or rejection. Both Acceptance and Rejection sections live in the same memo file as the original request — they are appended after the body, not separate files. After appending, flip the memo's frontmatter `status` and add a row to its existing § Status Log table.

## When to use which section

- **Acceptance** — receiver picked up the request, validated inputs per spec §4.1 step 1, assigned `audit_id` per step 2. Append the Acceptance section, flip frontmatter `status: open → accepted` (full profile) or skip directly to `shipped` (lightweight profile — see below).
- **Rejection** — inputs failed validation, a required secret is absent, over-capacity with no queue, scope mismatch, lattice incompatibility, or budget ceiling exceeded. Append the Rejection section and flip frontmatter `status: open → rejected`.

## Acceptance reply (full profile)

Required when the receiver is in active rendering / serving mode, when calendar deadlines apply, or when the memo's frontmatter `priority` is `high` or `urgent` (spec §4.2). Reply-comment MUST carry status, audit_id, eta, reserved_capacity, and any preconditions.

```markdown
## Acceptance (<YYYY-MM-DD>)

- **Status**: accepted
- **Audit ID**: <uuid_v4_or_vault_local_session_id>
- **ETA**: <ISO 8601 datetime | "no_calendar_urgency">
- **Reserved capacity**: <e.g. "1 render pass, 3 session-hours"; "tier 1 deck pipeline">
- **Input validation**: all paths in `artifact_request.{spec_path, output_sink, voice_mapping, lattice_path}` resolve on disk
- **Secret presence** (if `secrets_handled.needed` declared): <list of confirmed-present names — never values>
- **Idempotency check** (if `idempotency_key` declared): <"no match" | "duplicate_of: <existing_memo_path>; prior audit_id <id>">
- **Preconditions**: <list, or "none">
- **Next state transition**: → `rendering` on pipeline start
```

## Acceptance under lightweight profile

The lightweight profile (`open → shipped → closed`; spec §4.2) is the default for occasional ad-hoc 2-forge requests with no calendar urgency. Under this profile, the receiver SKIPS the Acceptance section entirely — the first reply on the memo is the `shipped` notification when artifacts land.

Use the Acceptance section above ONLY when:
- The receiver upgrades a lightweight request to full profile (the requester is not entitled to lightweight handling — spec §4.2 profile-selection rule), OR
- The request carries `priority: high` or `priority: urgent` (downgrading to lightweight is forbidden by spec §4.2).

## Rejection reply

Append in any of: validation failed, required secret absent, over-capacity with no queue available, scope outside receiver's pipeline, lattice incompatibility, or budget ceiling exceeded.

```markdown
## Rejection (<YYYY-MM-DD>)

- **Status**: rejected
- **Reason**: <one of: validation_failed | missing_secret: <NAME> | scope_mismatch | lattice_incompatible | budget_ceiling_exceeded | over_capacity_no_queue | other>
- **Details**: <2-4 sentences explaining the rejection cause>
- **Remediation** (if applicable): <e.g. "file a corrected memo with `voice_mapping` path resolved against requester vault root">
- **Next steps**: <e.g. "no further action by receiver"; "requester to refile after remediation">
```

## Status Log row

After appending the Acceptance or Rejection section, add a row to the memo's existing § Status Log table:

```markdown
| <YYYY-MM-DD> | <accepted | rejected> | <one-line note, e.g. "accepted; audit_id <id>; eta <iso>; reserved capacity <…>"> |
```

The Status Log row is the audit trail; the appended section is the structured reply-comment. Both update in the same commit on the receiver side.

## Cross-references

- **Spec — initiation ceremony** (acceptance 3 steps; rejection 1 step): `what/artifacts/iii_airlock_standard_spec.md` §4.1
- **Spec — handshake protocol** (lightweight vs full profile field requirements): `what/artifacts/iii_airlock_standard_spec.md` §4.2
- **Schema — lifecycle transitions**: `what/artifacts/iii_airlock_request_schema.yaml` `x-lifecycle-transitions`
- **Schema — handshake profiles**: `what/artifacts/iii_airlock_request_schema.yaml` `x-handshake-profiles`
- **Worked example** (cross-forge request memo, body sections + status log): `~/lattice/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md`
- **Campaign**: `how/campaigns/campaign_c_airlock_standard/campaign_c_airlock_standard.md`
