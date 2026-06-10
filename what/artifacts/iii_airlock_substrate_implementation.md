---
type: standard_implementation_guidance
version: "0.3.0"
status: reference_implementation
created: 2026-05-13
updated: 2026-05-21
last_edited_by: agent_argus
authoring_mission: campaign_d_federation_adaptive_loop MD-A2
governed_by: what/artifacts/iii_airlock_standard_spec.md
resolves_forward_references:
  - "spec §4.4 line 261 (secrets_handled implementation guidance — MC-4)"
  - "spec §4.5 line 282 (idempotency_key implementation guidance — MC-4)"
  - "how/airlock/AIRLOCK.md line 258 (Executable substrate enforcement deferral — MC-4)"
  - "spec §4.6 line 325 (Ed25519 verification implementation guidance — MD-A2)"
  - "spec §5.2 (Ed25519 verification contract substrate-side semantics — MD-A2)"
  - "spec §5.3 line 374 (ledger observation subscription model — MD-A2)"
  - "spec §5.3.1 line 403 (COMPLIANCE_AUDIT emission + assumes_draft workaround — MD-A2)"
  - "spec §5.5 lines 419-420 (MD-A2 forward-reference rows)"
ships_with_spec_version: "0.3.0"
documentation_grade: true
non_runnable: true
ln_substrate_version_pin: "LN.aDNA pc_01 Phase A close + Phase B1 close — ADR-014 v0.1 + ADR-015 v0.1 (both accepted 2026-05-20)"
tags: [airlock, substrate, secrets, idempotency, audit_log, implementation_guidance,
       v0_2, mc4,
       v0_3, md_a2,
       ed25519, federation_signing, sigverify_preflight,
       ledger_observation, polling_recommendation,
       compliance_audit_emit, assumes_draft_workaround, carry_3_ep_1,
       ln_substrate_load_bearing, federation_aware]
---

# Airlock Substrate Implementation Guidance (v0.3.0)

> **Companion document** to `what/artifacts/iii_airlock_standard_spec.md`. The spec carries the **normative contracts**; this artifact carries the **implementation guidance** that resolves the spec's forward-references at §4.4, §4.5, §4.6, §5.2, §5.3, and §5.3.1, plus the AIRLOCK.md reference instance's parallel forward-reference at line 258.

## §1 Purpose & Scope

This artifact is the canonical answer to "how does a receiver implement spec §4.4 + §4.5 + §4.6 + §5.2 + §5.3 + §5.3.1?" It is **documentation-grade per Campaign C Risk R3**: there is no executable runtime in v0.3. The first executable enforcement lands when a Platform.aDNA integration (e.g., RareHarness, VAASLattice) consumes the airlock standard at runtime.

**Authoring rule** (per MC-4 Plan-agent C carve-out, carried forward at MD-A2):
- **Pseudocode the scripts.** Preflight + dedup + sig-verify + ledger-client + event-emit flows are sketched in language-agnostic pseudocode plus illustrative bash + python fragments. The fragments are not runnable; an implementer hand-translates them when wiring an executable substrate.
- **Schema the data shapes.** Audit-log records, optional sidecar-index records, ledger event envelopes + payloads, and the receiver's reply-comment fill-in formats are **fully specified** — every field, every type, every enum value. This matches MC-2 precedent (shipped a Draft 2020-12 JSON Schema even though no validator runs in III.aDNA).

**Boundary discipline**: this artifact MUST NOT amend the normative contract at spec §4.4 / §4.5 / §4.6 / §5.1–§5.4. If a guidance recommendation conflicts with the spec prose, the **spec wins**. Conflicts are bugs in this artifact and resolve via patch-level edits here, not via spec rewrite. The sub-reason enums, payload schemas, and substrate-traceability fields are reproduced from the spec — not re-defined here.

**v0.2 forward-references resolved** (carry):
1. Spec §4.4 line 261: "Implementation guidance (preflight script structure, error-message conventions, audit-log schema): authored at MC-4" → §2 below.
2. Spec §4.5 line 282: "Implementation guidance (cache structure, 30-day window mechanics, archive-search performance): authored at MC-4" → §3 below.
3. AIRLOCK.md line 258: "MC-4 will author implementation guidance" → §2 + §3 below (cited from AIRLOCK.md at MC-4 close via patch-level edit).

**v0.3 forward-references resolved** (MD-A2, this cycle):
4. Spec §4.6 line 325: "Implementation guidance is deferred to MD-A2 (substrate implementation v0.3) per the §4.4 / §4.5 precedent" → §4 below.
5. Spec §5.2: "Verification semantics (the contract; implementation deferred to MD-A2)" → §4 below (substrate-side view of the same contract; impl-doc §4 satisfies both spec §4.6 and §5.2).
6. Spec §5.3 line 374: "the airlock client implementation (MD-A2) chooses between polling, pub/sub, or chain-walk; the contract above is implementation-agnostic" → §5 below (polling at 60s cadence recommended; tradeoffs documented).
7. Spec §5.3.1 line 403: "Native `LedgerEventType` enum membership lands at LN Carry 3 EP-1 ratification ... Until then, direct-JSON-write workaround applies" → §6 below.

---

## §2 `secrets_handled` Preflight Implementation Guidance

Resolves spec §4.4. The normative contract is at spec §4.4 lines 251-260 (receiver-side preflight MUST steps 1-4 + requester-side SHOULD steps 1-2). This section specifies the **shape** an implementer translates.

### §2.1 Preflight script structure

The preflight runs at receiver-side **on request acceptance**, before the receiver's pipeline allocates any compute / writes any artifact / pulls any context. Its inputs are the parsed request frontmatter's `secrets_handled` block (spec §4.4 lines 244-249); its outputs are a pass/fail signal that drives the reply-comment template (MC-3 deliverable) and an audit-log record (§2.3 below).

**Pseudocode flow**:

```
INPUT: parsed_frontmatter.secrets_handled = { needed: [...], passthrough: [...], not_passed: "..." }
INPUT: receiver_env = environment snapshot at acceptance time
INPUT: request_path, requesting_vault, audit_id  (from request frontmatter / receiver-allocated)

STEP 1: confirmed ← []; missing ← []
STEP 2: for name in parsed_frontmatter.secrets_handled.needed:
            if receiver_env.has(name) and receiver_env.get(name) is non-empty:
                confirmed.append(name)
            else:
                missing.append(name)
                BREAK    # first-missing-fails-fast; see §2.2

STEP 3: emit_audit_record(...)   # §2.3 schema; ALWAYS emit, on both branches

STEP 4a: if missing is empty:
            RETURN { outcome: "accepted", names_confirmed: confirmed }
STEP 4b: else:
            RETURN { outcome: "rejected", reason: "missing_secret: " + missing[0] }
```

**Bash sketch** (illustrative; not runnable):

```bash
#!/usr/bin/env bash
# Receiver-side preflight: secrets_handled
# Inputs: $REQUEST_PATH, $AUDIT_ID, $REQUESTING_VAULT
# Expects: parse_yaml shim that emits SECRETS_NEEDED as space-sep names
set -euo pipefail

confirmed=(); missing=()
for name in $SECRETS_NEEDED; do
  if [[ -n "${!name-}" ]]; then
    confirmed+=("$name")
  else
    missing+=("$name")
    break
  fi
done

emit_audit_record  # see §2.3

if (( ${#missing[@]} == 0 )); then
  echo "accepted: ${confirmed[*]}"
  exit 0
else
  echo "rejected: missing_secret: ${missing[0]}"
  exit 64
fi
```

**Python sketch** (illustrative; not runnable):

```python
import os
from typing import Sequence

def preflight_secrets(needed: Sequence[str]) -> dict:
    confirmed, missing = [], []
    for name in needed:
        if os.environ.get(name):
            confirmed.append(name)
        else:
            missing.append(name)
            break  # first-missing-fails-fast (§2.2)
    emit_audit_record(...)  # §2.3
    if not missing:
        return {"outcome": "accepted", "names_confirmed": confirmed}
    return {"outcome": "rejected", "reason": f"missing_secret: {missing[0]}"}
```

**Implementer notes**:
- Audit emission is unconditional (both branches). The audit record IS the contract evidence.
- `os.environ.get(name)` returns the value but the implementation MUST discard it after the presence check (do not bind it to a logging variable; do not pass it to `repr`/`str`).
- For testability, the env probe MAY be parameterized (dependency-injected env mapping). The presence semantics MUST remain identical to `os.environ.get(name) is non-empty`.

### §2.2 Error-message conventions

**Single-value-per-rejection (first-missing-fails-fast)**. When more than one secret is absent, the preflight reports only the **first** missing name. Rationale: (a) the receiver pipeline cannot make progress without ALL secrets, so enumerating subsequent failures is wasted information; (b) the rejection reason field in the reply-comment template (MC-3 deliverable) takes a single value; (c) the requester learns about secret #2 only after fixing secret #1 — they would have noticed the second one anyway when re-filing.

**Rejection reason format** — the receiver replies via the reply-comment template's Rejection block (shipped at MC-3 in `how/templates/template_cross_vault_request_reply.md`). The Rejection `reason:` enum (extended at MD-A2 with `signature_verification_failed:<subreason>` per §4) already includes `missing_secret`; this guidance fills in the canonical body shape:

```
reason: missing_secret: <SECRET_NAME>
```

Examples:
- `reason: missing_secret: ANTHROPIC_API_KEY`
- `reason: missing_secret: S3_OUTPUT_WRITE_TOKEN`

**Acceptance reason format** — the Acceptance fenced block in the reply-comment template carries the **Secret presence** slot (line 38 in the template). Canonical fill-in:

```
- **Secret presence**: <comma-sep list of confirmed-present names — never values>
```

Examples:
- `**Secret presence**: ANTHROPIC_API_KEY, S3_OUTPUT_WRITE_TOKEN`
- `**Secret presence**: (none required)` — when `secrets_handled.needed` is empty / absent.

### §2.3 Audit-log schema

**Storage location**: `<receiver_vault_root>/who/coordination/.audit/audit_<YYYY-MM>.jsonl`. Per-month rotation. The `.audit/` dotfile directory signals "infrastructure, not content" and is colocated with the coord-memo directory the request lives in.

**`.gitignore` recommendation** (each receiver vault adds to its own gitignore):

```gitignore
# Airlock substrate audit log — per-vault, never committed
who/coordination/.audit/audit_*.jsonl
```

The audit log is **receiver-local evidence**. Cross-vault federation of audit logs is OUT OF SCOPE for v0.3 (deferred indefinitely; see §8).

**Single audit file is multi-purpose** at v0.3 — it carries five `event_type` values: `secrets_preflight` (§2), `idempotency_check` (§3.6), `federation_sigverify` (§4.7), `ledger_observation` (§5.6), `compliance_audit_emit` (§6.5). The `event_type` discriminator distinguishes them; v0.2 shipped two types, v0.3 adds three more (additive, backward-compatible).

**Record shape — `event_type: secrets_preflight`** (fully specified; one JSON object per line; UTF-8; no trailing comma):

```jsonl
{
  "ts": "<RFC 3339 / ISO 8601 timestamp; UTC; second precision; e.g. 2026-05-13T14:30:00Z>",
  "event_type": "secrets_preflight",
  "audit_id": "<receiver-allocated identifier; UUIDv4 or vault_session_id form; matches the request memo's frontmatter.audit_id once populated>",
  "vault": "<receiver vault short name; e.g. canvasforge>",
  "requesting_vault": "<requester vault short name; e.g. videoforge>",
  "request_path": "<absolute or vault-relative path to the request memo; e.g. who/coordination/coord_2026_05_13_videoforge_requests_X.md>",
  "secrets_required": ["NAME1", "NAME2"],
  "secrets_present": ["NAME1"],
  "secrets_missing": ["NAME2"],
  "outcome": "<one of: accepted | rejected>",
  "outcome_reason": "<one of: ok | missing_secret: <NAME>>"
}
```

**Field rules**:
- `ts`: RFC 3339 / ISO 8601; UTC; second precision. Implementations MAY use millisecond precision but the canonical form is seconds.
- `event_type`: exactly `secrets_preflight` (string literal).
- `audit_id`: receiver-allocated at acceptance time; the same value populates the request memo's `frontmatter.audit_id` field (which begins as `pending` per `iii_airlock_request_schema.yaml` line 151). UUIDv4 is recommended; vault-local session IDs are acceptable when uniqueness within the receiver vault is guaranteed.
- `vault`: receiver short name; lowercase; no spaces; matches the `requesting_vault_short` convention from spec §4.3.
- `requesting_vault`: requester short name; same conventions.
- `request_path`: prefer vault-relative path (portable); absolute path acceptable for receiver-local diagnostics.
- `secrets_required`: ALL names from `frontmatter.secrets_handled.needed` (preserves declared order).
- `secrets_present`: subset of `secrets_required` that passed the env-presence check.
- `secrets_missing`: subset of `secrets_required` that failed the env-presence check. Length 0 when `outcome: accepted`; length 1 when `outcome: rejected` (first-missing-fails-fast per §2.2).
- `outcome`: closed enum `accepted | rejected`.
- `outcome_reason`: `ok` when accepted; `missing_secret: <NAME>` when rejected (matches §2.2).

**Sample records**:

Acceptance (all secrets present):

```jsonl
{"ts":"2026-05-13T14:30:00Z","event_type":"secrets_preflight","audit_id":"7f8c2b1a-3e9d-4f1c-a8b2-5d6e7f8a9b0c","vault":"canvasforge","requesting_vault":"videoforge","request_path":"who/coordination/coord_2026_05_13_videoforge_requests_carly_herb_deck_v2.md","secrets_required":["ANTHROPIC_API_KEY"],"secrets_present":["ANTHROPIC_API_KEY"],"secrets_missing":[],"outcome":"accepted","outcome_reason":"ok"}
```

Rejection (one secret missing):

```jsonl
{"ts":"2026-05-13T14:31:42Z","event_type":"secrets_preflight","audit_id":"9c1e4d3b-7a2f-4e8d-b0c9-1a2b3c4d5e6f","vault":"canvasforge","requesting_vault":"videoforge","request_path":"who/coordination/coord_2026_05_13_videoforge_requests_X.md","secrets_required":["ANTHROPIC_API_KEY","S3_OUTPUT_WRITE_TOKEN"],"secrets_present":["ANTHROPIC_API_KEY"],"secrets_missing":["S3_OUTPUT_WRITE_TOKEN"],"outcome":"rejected","outcome_reason":"missing_secret: S3_OUTPUT_WRITE_TOKEN"}
```

### §2.4 Boundary integrity

Spec §4.4 step 3 requires "audit names only" and step 4 requires "boundary integrity (the receiver MUST NOT export `passthrough` secrets to logs, attached files, or commits at either vault)". This guidance enumerates what NEVER lands in any artifact under any condition:

**Never audited / logged / committed**:
1. **Secret values** (the contents of the env var). `os.environ.get(name)` returns the value; the value MUST be discarded after the presence check.
2. **Request bodies that contain secrets**. If a requester inadvertently inlines a value in `request_path` or a body section, the receiver MUST NOT mirror that body into the audit log; the audit record references the request by path only.
3. **Error stack traces that capture env**. Some runtimes (Python `Exception.__repr__`, JS Error objects with debug flags) dump environment in trace output. Audit emission MUST sanitize trace text — names-only — before persisting.
4. **Process listing snapshots**. `ps eaxf` shows process env on some systems; preflight scripts MUST NOT `exec` or `spawn` subprocesses with secret values passed as argv.

**Tools that violate boundary integrity** (do not use in preflight or reply-comment authoring):
- `env > some_file` (dumps the entire process env including values).
- `printenv | tee` (same).
- Any logging library configured with `log.debug(os.environ)` or equivalent.

**Boundary-integrity check in code review** — when reviewing a future executable preflight implementation, grep for `os.environ` / `process.env` / `getenv` and verify each occurrence either (a) uses the value transiently for the presence check and discards, or (b) never reads the value at all (uses `os.environ.__contains__` / `name in process.env`-style membership checks).

### §2.5 Sample-record coordination with MC-5

The sample records in §2.3 use values intentionally aligned with the MC-5 worked-example dimensions:

- `requesting_vault: videoforge` + `vault: canvasforge` — matches the MC-5 re-exercise target (VideoForge → CanvasForge per spec §6).
- `request_path` references a hypothetical `coord_2026_05_13_videoforge_requests_carly_herb_deck_v2.md` — when MC-5 authors the v0.2-conformant request memo, the audit emission can use these exact dimensions without retrofit.
- `secrets_required: ["ANTHROPIC_API_KEY"]` — matches the worked-example pipeline (deck render uses local Anthropic API key only; no cross-vault secret delegation per spec §6.2 additive delta 1).

MC-5 authors MAY adopt these dimensions verbatim or override. If overridden, the canonical sample values here remain as illustrative — they are not test fixtures.

---

## §3 `idempotency_key` Dedup Implementation Guidance

Resolves spec §4.5. The normative contract is at spec §4.5 lines 276-281 (receiver-side dedup MUST steps 1-4). This section specifies the **shape** an implementer translates.

### §3.1 Canonical archive-search filter

When a request frontmatter contains `idempotency_key` (and `force_new` is absent or false), the receiver's acceptance flow MUST search its request archive for a prior matching memo. The filter is:

```
match := for each memo m in receiver_vault.who/coordination/coord_*.md:
            m.frontmatter.idempotency_key == K
        AND m.frontmatter.requesting_vault == V
        AND (
              m.frontmatter.status in {open, accepted, rendering, shipped}
              OR
              (m.frontmatter.status == closed AND m.frontmatter.updated >= now - 30d)
              OR
              (m.frontmatter.status == cancelled AND m.frontmatter.updated >= now - 30d)
            )
        AND m.path != current_request.path     # exclude the current memo itself
```

Where:
- `K` = `current_request.frontmatter.idempotency_key`
- `V` = `current_request.frontmatter.requesting_vault`
- `now` = receiver-clock at acceptance time

**Returns**:
- `match == [single memo]` → `outcome: duplicate_of` per spec §4.5 step 3.
- `match == []` → `outcome: no_match` per spec §4.5 step 2.
- `match == [multiple memos]` → SHOULD NOT happen by construction (matching key on the same requesting vault within 30 days indicates a prior dedup failure or upstream key reuse); receiver MUST select the most-recent match by `updated` field and emit a `clock_source_fallback` audit field (§3.6) flagging the violation for human review.

**Force-new override**: if `current_request.frontmatter.force_new == true`, the filter is still computed (audit-log emission documents what would have matched), but the new memo proceeds as if `match == []` (acceptance pipeline allocates compute). Outcome enum value: `bypassed_force_new` per §3.6.

### §3.2 30-day window semantics

The 30-day window applies ONLY to closed-state and cancelled-state matches. Open-state matches (open / accepted / rendering / shipped) match regardless of age — these are in-flight or recently-shipped pipelines whose results are reusable.

**Clock source precedence**:
1. `m.frontmatter.updated` (the canonical signal; populated at memo creation and refreshed at every status transition).
2. `m.frontmatter.created` (fallback when `updated` is missing).
3. Memo file `mtime` (fallback when neither frontmatter field exists; clock_source_fallback level 3).
4. Memo file `ctime` (last-resort fallback; clock_source_fallback level 4).

An implementer SHOULD warn (audit log entry with `event_type: idempotency_check` + a `clock_source_fallback: <2|3|4>` field) when falling back below precedence 1. The audit-log schema in §3.6 accommodates an optional `clock_source_fallback` field.

### §3.3 Cache structure

**The existing `who/coordination/` directory IS the cache.** No separate state file is required for vaults with O(100) memos. The acceptance-time linear scan is O(n) in coord-memo count and completes in sub-second time on local disk (see §3.4 performance profile).

**Optional sidecar index** for vaults with O(1K)+ memos: `<receiver_vault_root>/who/coordination/.idempotency_index.jsonl`. Same per-month rotation pattern as `.audit/audit_<YYYY-MM>.jsonl`. One JSON object per line:

```jsonl
{
  "idempotency_key": "<K>",
  "requesting_vault": "<V>",
  "memo_path": "<vault-relative path>",
  "status": "<open | accepted | rendering | shipped | closed | cancelled>",
  "updated": "<RFC 3339 timestamp; UTC>"
}
```

The index is **append-only on memo creation**; updates land as new lines (last-write-wins semantics by line order). Status transitions can re-write the line OR append a new record (implementation choice); the scan logic in §3.1 must converge on the latest record per `(idempotency_key, requesting_vault, memo_path)` triple.

**Reading the index**:
- Sequential scan from end of file (latest line wins per triple).
- Skip lines older than 30 days for closed/cancelled status (the §3.2 window).
- Match against current request's `K + V` pair.

**Index hygiene**: append-only. Receivers MAY occasionally compact (filter out obsolete lines) as a maintenance task; compaction is not required for correctness.

### §3.4 Performance profile

| Memo count | Scan strategy | Expected latency |
|---|---|---|
| < 100 | Linear scan of coord_*.md frontmatters | < 50 ms (cold disk; <10 ms warm) |
| 100–1000 | Linear scan with frontmatter-only parse | < 200 ms (cold; < 50 ms warm) |
| 1000–10000 | Add `.idempotency_index.jsonl` per §3.3 | < 50ms with index; 1–10s without |
| > 10000 | Index + monthly rotation | < 50 ms |

The 200 ms target matches the acceptance-gate budget that the rest of the airlock honors (request acceptance is human-paced; sub-second is responsive enough; sub-200ms is the receiver's commitment).

**Implementer notes**:
- The linear scan reads frontmatter ONLY (the body is irrelevant for dedup). A YAML parser that stops at the closing `---` separator is appropriate.
- The index in §3.3 trades disk-write cost (one line per memo write) for lookup speed. The trade-off pays off at O(1K)+ memos.
- The optional `.idempotency_index.jsonl` is per-receiver-vault, never committed (same gitignore pattern as `.audit/audit_*.jsonl`).

### §3.5 Force-new override semantics

When `force_new == true`, the dedup pipeline behavior is:

| Pre-condition | Filter result | Outcome | Reply-comment template fill-in |
|---|---|---|---|
| `idempotency_key` declared, `force_new: true`, match exists | duplicate found | `bypassed_force_new` | `bypassed (force_new: true) — would have matched duplicate_of: <path>` |
| `idempotency_key` declared, `force_new: true`, no match | no match | `no_match_force_new_set` | `bypassed (force_new: true) — no match would have applied anyway` |
| `idempotency_key` declared, `force_new: false`, match exists | duplicate found | `duplicate_of` | `duplicate_of: <existing_memo_path>; prior audit_id <id>` |
| `idempotency_key` declared, `force_new: false`, no match | no match | `no_match` | `no match` |
| `idempotency_key` absent | (not run) | `no_key` | `no key declared` |

The receiver MUST emit an audit record for ALL five branches (force-new bypass is audit-bookkeeping evidence).

### §3.6 Audit-log emit

Audit records for idempotency events land in the **same** `<receiver_vault>/who/coordination/.audit/audit_<YYYY-MM>.jsonl` file as secrets-preflight events (§2.3). The `event_type` discriminator distinguishes them.

**Record shape — `event_type: idempotency_check`** (fully specified; one JSON object per line):

```jsonl
{
  "ts": "<RFC 3339 / ISO 8601 timestamp; UTC; second precision>",
  "event_type": "idempotency_check",
  "audit_id": "<receiver-allocated identifier>",
  "vault": "<receiver vault short name>",
  "requesting_vault": "<requester vault short name>",
  "request_path": "<vault-relative path to the request memo>",
  "idempotency_key": "<key string>",
  "force_new": <bool>,
  "outcome": "<one of: no_match | duplicate_of | bypassed_force_new | no_match_force_new_set | no_key>",
  "duplicate_of": "<path of matched prior memo | null>",
  "prior_audit_id": "<audit_id of matched prior memo | null>",
  "clock_source_fallback": <optional int 2|3|4 if §3.2 fallback triggered>
}
```

**Field rules**:
- `event_type`: exactly `idempotency_check`.
- `idempotency_key`: copied from `current_request.frontmatter.idempotency_key`. If absent on the request, this field is `null` and `outcome: no_key`.
- `force_new`: boolean; default false; copied from request frontmatter.
- `outcome`: closed enum. `no_match` (key present, no prior match, force_new false). `duplicate_of` (key present, match found, force_new false; new memo flips to cancelled). `bypassed_force_new` (key present, match would have hit, force_new true; new memo proceeds normally). `no_match_force_new_set` (key present, no prior match, force_new true; rare — opt-in bypass with nothing to bypass). `no_key` (key absent; record is informational; included for audit-log completeness).
- `duplicate_of` / `prior_audit_id`: non-null only when `outcome in {duplicate_of, bypassed_force_new}`.
- `clock_source_fallback`: optional; integer 2, 3, or 4 per §3.2 precedence list. Omitted when `clock_source` is `m.frontmatter.updated` (precedence 1, the canonical case).

**Sample records**:

No prior match (clean acceptance):

```jsonl
{"ts":"2026-05-13T14:30:01Z","event_type":"idempotency_check","audit_id":"7f8c2b1a-3e9d-4f1c-a8b2-5d6e7f8a9b0c","vault":"canvasforge","requesting_vault":"videoforge","request_path":"who/coordination/coord_2026_05_13_videoforge_requests_carly_herb_deck_v2.md","idempotency_key":"videoforge_carly_herb_deck_v2","force_new":false,"outcome":"no_match","duplicate_of":null,"prior_audit_id":null}
```

Duplicate detected (new memo flips to cancelled):

```jsonl
{"ts":"2026-05-13T14:35:22Z","event_type":"idempotency_check","audit_id":"3d4e5f6a-7b8c-9d0e-1f2a-3b4c5d6e7f80","vault":"canvasforge","requesting_vault":"videoforge","request_path":"who/coordination/coord_2026_05_13_videoforge_requests_carly_herb_deck_v2_retry.md","idempotency_key":"videoforge_carly_herb_deck_v2","force_new":false,"outcome":"duplicate_of","duplicate_of":"who/coordination/coord_2026_05_13_videoforge_requests_carly_herb_deck_v2.md","prior_audit_id":"7f8c2b1a-3e9d-4f1c-a8b2-5d6e7f8a9b0c"}
```

Force-new bypass observed:

```jsonl
{"ts":"2026-05-13T14:40:08Z","event_type":"idempotency_check","audit_id":"5a6b7c8d-9e0f-1a2b-3c4d-5e6f7a8b9c0d","vault":"canvasforge","requesting_vault":"videoforge","request_path":"who/coordination/coord_2026_05_13_videoforge_requests_carly_herb_deck_v2_force.md","idempotency_key":"videoforge_carly_herb_deck_v2","force_new":true,"outcome":"bypassed_force_new","duplicate_of":"who/coordination/coord_2026_05_13_videoforge_requests_carly_herb_deck_v2.md","prior_audit_id":"7f8c2b1a-3e9d-4f1c-a8b2-5d6e7f8a9b0c"}
```

---

## §4 Federation Signing-Key Verification (Ed25519) Preflight

Resolves spec §4.6 + §5.2 (the spec sections present the same contract from two angles — §4.6 is the request-acceptance gate; §5.2 is the substrate-side view; both bind to a single implementation, which §4 below specifies). The normative contract is at spec §4.6 lines 304-315 (requester-side MUST steps 1-3 + receiver-side MUST steps 1-4) + spec §5.2 (canonical-JSON discipline; per-purpose-slot binding; verification failure reasons). This section specifies the **shape** an implementer translates.

### §4.1 Preflight script structure

The Ed25519 sig-verify preflight runs at receiver-side **on request acceptance**, after `secrets_handled` (§2) + `idempotency_key` (§3) and before pipeline allocation. Its inputs are the parsed request frontmatter (the canonical-JSON of which was signed by the requester per spec §4.6 step 1), the receiver's resolution of the LN membership manifest, and the receiver's `profile_mode` configuration (`enforce` vs `advisory` per spec §4.6 lines 317-323).

**Pseudocode flow**:

```
INPUT: parsed_frontmatter                      # full request memo frontmatter
INPUT: requesting_persona                      # parsed_frontmatter.requesting_persona
INPUT: requesting_vault                        # parsed_frontmatter.requesting_vault
INPUT: receiver_vault_canonical_id             # receiver's own canonical_id (ADR-010)
INPUT: ln_membership_manifest                  # latest read of the federation manifest
INPUT: federation_pubkey_cache                 # §4.6 cache (24h TTL; observation-invalidated)
INPUT: profile_mode                            # one of: enforce | advisory  (wrapper-level config)
INPUT: inbound_substrate                       # transport substrate that carried the request (tailscale | nebula)
INPUT: audit_id, request_path                  # receiver-allocated / request frontmatter

STEP 1: co_fed ← co_federated(requesting_vault, receiver_vault_canonical_id, ln_membership_manifest)
        if NOT co_fed:
            emit_audit(outcome="skipped_not_co_federated")
            RETURN { outcome: "accepted_no_verify", reason: "not_co_federated" }

STEP 2: sig ← parsed_frontmatter.get("federation_signature")
        key_version ← parsed_frontmatter.get("federation_signature_key_version")
        if sig is null OR key_version is null:
            if profile_mode == "advisory":
                emit_audit(outcome="accepted_advisory", reason="unsigned")
                RETURN { outcome: "accepted_advisory" }
            else:
                emit_audit(outcome="rejected", subreason="pubkey_absent")
                RETURN { outcome: "rejected", reason: "signature_verification_failed:pubkey_absent" }

STEP 3: canonical_id ← resolve_canonical_id(requesting_persona, ln_membership_manifest)
                                                        # per ADR-010 §d roster lookup
        if canonical_id is null:
            emit_audit(outcome="rejected", subreason="pubkey_absent")
            RETURN { outcome: "rejected", reason: "signature_verification_failed:pubkey_absent" }

STEP 4: pubkey, pubkey_version ← resolve_pubkey(canonical_id, key_version,
                                                 ln_membership_manifest,
                                                 federation_pubkey_cache)
                                                        # see §4.3
        if pubkey is null:
            emit_audit(outcome="rejected", subreason="pubkey_absent")
            RETURN { outcome: "rejected", reason: "signature_verification_failed:pubkey_absent" }
        if pubkey in cached_revoked_set:                # §5 observation drove invalidation
            emit_audit(outcome="rejected", subreason="pubkey_revoked")
            RETURN { outcome: "rejected", reason: "signature_verification_failed:pubkey_revoked" }
        if pubkey_version != key_version:
            emit_audit(outcome="rejected", subreason="key_version_unknown")
            RETURN { outcome: "rejected", reason: "signature_verification_failed:key_version_unknown" }

STEP 5: signed_payload ← canonical_json(parsed_frontmatter \ {"federation_signature"})
                                                        # see §4.2 canonical-JSON algorithm
                                                        # MUST exclude the signature field itself

STEP 6: ok ← ed25519_verify(pubkey, signed_payload_bytes, base64_decode(sig))
        if NOT ok:
            emit_audit(outcome="rejected", subreason="signature_mismatch")
            RETURN { outcome: "rejected", reason: "signature_verification_failed:signature_mismatch" }

STEP 7: allowed_substrates ← ln_membership_manifest.transport.substrates[*].kind
                                                        # per ADR-015 §b + §5.4 spec
        if inbound_substrate NOT IN allowed_substrates:
            emit_audit(outcome="rejected", subreason="substrate_mismatch")
            RETURN { outcome: "rejected", reason: "signature_verification_failed:substrate_mismatch" }

STEP 8: emit_audit(outcome="accepted_verified",
                   pubkey_sha256=hash(pubkey), key_version=pubkey_version,
                   substrate_kind=inbound_substrate)
        RETURN { outcome: "accepted_verified" }
```

**Python sketch** (illustrative; not runnable):

```python
import base64, json, hashlib, unicodedata
from nacl.signing import VerifyKey      # PyNaCl preferred; cryptography.hazmat is fallback
from nacl.exceptions import BadSignatureError

def sigverify_preflight(req_fm: dict, *, requesting_persona: str, requesting_vault: str,
                        receiver_canonical_id: str, ln_manifest: dict,
                        pubkey_cache, profile_mode: str, inbound_substrate: str,
                        audit_id: str, request_path: str) -> dict:
    if not _co_federated(requesting_vault, receiver_canonical_id, ln_manifest):
        _emit_audit(event_type="federation_sigverify", outcome="skipped_not_co_federated",
                    audit_id=audit_id, request_path=request_path,
                    profile_mode=profile_mode, substrate_kind_inbound=inbound_substrate)
        return {"outcome": "accepted_no_verify", "reason": "not_co_federated"}

    sig = req_fm.get("federation_signature")
    key_version = req_fm.get("federation_signature_key_version")
    if not sig or not key_version:
        if profile_mode == "advisory":
            _emit_audit(outcome="accepted_advisory", subreason=None, ...)
            return {"outcome": "accepted_advisory"}
        _emit_audit(outcome="rejected", subreason="pubkey_absent", ...)
        return {"outcome": "rejected", "reason": "signature_verification_failed:pubkey_absent"}

    canonical_id = _resolve_canonical_id(requesting_persona, ln_manifest)
    pubkey, pubkey_version = _resolve_pubkey(canonical_id, key_version, ln_manifest, pubkey_cache)
    # … steps 4-7 mirror the pseudocode …

    signed_payload = _canonical_json({k: v for k, v in req_fm.items() if k != "federation_signature"})
    try:
        VerifyKey(pubkey).verify(signed_payload.encode("utf-8"), base64.b64decode(sig))
    except BadSignatureError:
        _emit_audit(outcome="rejected", subreason="signature_mismatch", ...)
        return {"outcome": "rejected", "reason": "signature_verification_failed:signature_mismatch"}

    allowed = {s["kind"] for s in ln_manifest["transport"]["substrates"]}
    if inbound_substrate not in allowed:
        _emit_audit(outcome="rejected", subreason="substrate_mismatch", ...)
        return {"outcome": "rejected", "reason": "signature_verification_failed:substrate_mismatch"}

    _emit_audit(outcome="accepted_verified", pubkey_sha256=hashlib.sha256(pubkey).hexdigest(),
                key_version=pubkey_version, substrate_kind=inbound_substrate, ...)
    return {"outcome": "accepted_verified"}
```

**Implementer notes**:
- Audit emission MUST be unconditional on every branch — the audit record IS the evidence the receiver verified (or chose not to verify) the signature.
- Library choice: PyNaCl preferred (smaller surface; well-audited Ed25519 primitives). `cryptography.hazmat.primitives.asymmetric.ed25519` is an acceptable fallback. NodeJS implementations use `node:crypto.verify("ed25519", ...)`; Go uses `crypto/ed25519`.
- The `_co_federated` helper checks that BOTH requesting_vault and receiver share at least one federation membership in the manifest. Single-substrate or dual-substrate participation is symmetric per spec §5.4 cl. 4.
- The `_canonical_json` helper is detailed in §4.2.

### §4.2 Canonical-JSON serialization

Per spec §4.6 step 3 + spec §5.2 + LN ADR-014 §c, the signed payload is the request frontmatter (excluding the `federation_signature` field) serialized to canonical JSON with the following rules:

1. **Sorted keys**. Every object's keys sorted in lexicographic ascending byte order (Unicode code-point order; matches Python `sort_keys=True` and Go `encoding/json` post-sort).
2. **Unicode NFC normalization**. Every string leaf (keys + values) normalized to Unicode Normalization Form Canonical Composition (NFC) per `unicodedata.normalize("NFC", s)` semantics.
3. **Whitespace trimmed**. Leading + trailing whitespace stripped from every string leaf. Internal whitespace preserved.
4. **Case preserved**. No case-folding. The receiver and requester MUST agree on case at frontmatter authoring time.
5. **Minimal separators**. `","` between items; `":"` between key-value (no spaces around either). Matches Python `separators=(",", ":")`.
6. **ASCII-only escaping disabled**. `ensure_ascii=False` (UTF-8 bytes flow through; no `\uXXXX` escaping of non-ASCII). Matches LN ADR-014 §c spec.
7. **No trailing newline.** The serialized bytes end at the closing `}`.

**Python recipe** (illustrative; not runnable):

```python
import json, unicodedata

def _canonical_json(obj):
    def normalize(x):
        if isinstance(x, str):
            return unicodedata.normalize("NFC", x.strip())
        if isinstance(x, dict):
            return {normalize(k): normalize(v) for k, v in x.items()}
        if isinstance(x, list):
            return [normalize(v) for v in x]
        return x
    return json.dumps(normalize(obj), sort_keys=True, separators=(",", ":"), ensure_ascii=False)
```

**Validation requirement**: the requester's signing path and the receiver's verification path MUST produce byte-identical canonical-JSON bytes for the same input. Drift = canonical-JSON algorithm divergence (bug); implementers MUST round-trip-test with at least one shared fixture (`spec_canonical_json_fixture.json` is reserved at v0.4 for cross-implementation interop testing; advisory at v0.3).

**Why this matters**: Ed25519 signs bytes, not JSON values. Two implementations that serialize the same dict differently will produce signatures the other cannot verify. The strict rules above eliminate the divergence surface.

### §4.3 Pubkey resolution from LN membership manifest

Per spec §4.6 step 1 + spec §5.2 + LN ADR-013 §b + LN ADR-015 §b/§d, the receiver resolves the requesting persona's Ed25519 pubkey by walking the LN membership manifest.

**Manifest path** (the source-of-truth file lives at the requesting vault's home `node.aDNA/what/inventory/inventory_memberships.yaml` per LN ADR-015 §b; the receiver MAY mirror it locally for read-only consumption — see §5):

```yaml
memberships:
  alpha_lattice:                                # the lattice / federation network ID
    lattice_protocol:
      identity:
        canonical_id: stanley_l1                # per ADR-010 §d roster
        federation_signing_pubkey_sha256: 7c2e8f...d4b1   # 64-char lowercase hex
                                                          # SHA-256 over raw 32-byte Ed25519 pubkey
                                                          # per ADR-013 §b
        federation_signing_pubkey_versions:               # NEW at v0.3 receiver-side convention
          - version: v1
            sha256: 7c2e8f...d4b1
            issued_at: 2026-05-15T00:00:00Z
            revoked_at: null
        transport:
          substrates:
            - kind: tailscale
              # ... per ADR-015 §b ...
            - kind: nebula
              # ... per ADR-015 §b ...
```

**Resolution steps**:

1. **Map `requesting_persona` → `canonical_id`** per ADR-010 §d 11-row roster. The persona (e.g., `argus` for III; `iris` for VideoForge) is the persona name; the canonical_id (e.g., `iris_videoforge` or `stanley_l1`) is the substrate-independent node identity.
2. **Walk manifest** to `memberships.<federation_id>.lattice_protocol.identity` for that canonical_id (the federation_id discriminator is typically `alpha_lattice` at v0.1; v0.2+ may add additional lattices).
3. **Read `federation_signing_pubkey_sha256`** — this is the 64-char lowercase hex SHA-256 fingerprint per ADR-013 §b. The actual raw 32-byte Ed25519 pubkey MUST be available at a sibling registry (the LN membership manifest carries the fingerprint; the raw pubkey bytes are distributed via the per-substrate identity provisioning per ADR-013 §c keygen tooling).
4. **Verify fingerprint** — compute SHA-256 over the raw pubkey bytes; assert match against the manifest fingerprint. Mismatch is a `signature_mismatch` rejection (the binding contract failed even before signature verify).
5. **Match key version** — the request's `federation_signature_key_version` field MUST match a known `federation_signing_pubkey_versions[].version` for that canonical_id. Unknown version → `key_version_unknown` rejection per §4.4.
6. **Check revocation** — if the matched version has `revoked_at` populated (or if §5 observation has cached a `MEMBERSHIP_CERT_REVOKED` event covering the fingerprint), the pubkey is revoked → `pubkey_revoked` rejection per §4.4.

**Caching**: the resolved `(canonical_id, key_version) → (pubkey_bytes, revocation_status)` tuple is cached per §4.6. The cache is the §5 ledger-observation client's sole `MEMBERSHIP_CERT_REVOKED` / `MEMBERSHIP_NODE_RE_IDENTIFIED` consumer at v0.3 (no other receiver-side state depends on observed events).

**Implementer notes**:
- The manifest pubkey distribution mechanism (how the receiver gets the raw 32-byte pubkey from the SHA-256 fingerprint) is **out of MD-A2 scope**. It rides on LN ADR-013 §c source-side keygen tooling + the federation-attestation transmission ceremony (`latlab federation transmit` per ADR-004 §a). The receiver consumes the result; it does not own the distribution.
- For airlock-first integrations that pre-date a full federation deployment, the receiver MAY fall back to a local trust-store (`<receiver_vault>/who/coordination/.airlock/federation_pubkeys.yaml`) seeded out-of-band by the operator. This fallback is documented at the wrapper level (not normative here).

### §4.4 Rejection sub-reason taxonomy

The receiver MUST flip `status: rejected` with `reason: signature_verification_failed:<subreason>` where `<subreason>` is exactly one of the **5 closed-enum values** below. This taxonomy reproduces spec §4.6 line 314 (4 sub-reasons) + spec §5.4 line 410 (substrate_mismatch added as "a §4.6 sub-reason class"); MD-A2 enumerates them in one table.

| Sub-reason | Trigger | Spec source |
|---|---|---|
| `pubkey_absent` | (a) `federation_signature` or `federation_signature_key_version` field absent from request frontmatter, OR (b) requesting persona's `canonical_id` does not resolve to a manifest entry, OR (c) no `federation_signing_pubkey_sha256` populated for the canonical_id | spec §4.6 line 314 |
| `pubkey_revoked` | Cached `MEMBERSHIP_CERT_REVOKED` event covers the resolved pubkey, OR the manifest entry has `revoked_at` populated for the matched version | spec §4.6 line 314 + ADR-015 §c |
| `signature_mismatch` | Ed25519 verification returned `false` (the signed bytes do not verify under the resolved pubkey + provided signature) | spec §4.6 line 314 |
| `key_version_unknown` | `federation_signature_key_version` references a version not present in the manifest's `federation_signing_pubkey_versions[]` list for the canonical_id | spec §4.6 line 314 |
| `substrate_mismatch` | The inbound substrate (the transport that carried the request — Tailscale or Nebula at v0.1) is not in the requesting node's `transport.substrates[*].kind` list per ADR-015 §b | spec §5.4 line 410 ("a §4.6 sub-reason class") |

**Closed enum.** Adding a new sub-reason requires a spec amendment per spec §4.6 versioning policy; the impl-doc MUST NOT introduce new sub-reasons. If an implementer encounters a verification failure that doesn't fit one of the 5, it is one of:
- Bug in the implementation (covered cases mishandled),
- Spec gap (file a v0.4 proposal),
- Substrate-side fault outside airlock scope (substrate-layer connectivity issue; not a sig-verify rejection).

In all three cases, the right disposition is NOT to ship a new sub-reason from MD-A2.

### §4.5 Advisory-mode handling

Per spec §4.6 lines 317-323, the receiver MAY downgrade verification from `enforce` to `advisory` (warn-on-failure rather than reject) when:
- The requesting vault is not yet federated (no membership manifest entry exists).
- The receiver itself is not federated (no canonical_id assigned).
- A formal v0.2-compatibility mode is declared at the wrapper level (e.g., a consumer that hasn't adopted v0.3).

**Implementation pattern** — advisory mode is a **wrapper-level configuration knob**, NOT a per-request override. The receiver vault's `iii/CLAUDE.md` (or equivalent) declares:

```yaml
airlock:
  sigverify_profile_mode: advisory   # or "enforce"
  # ... other airlock config ...
```

In advisory mode, the §4.1 STEP 2 branch (signature absent + advisory mode) returns `accepted_advisory` with an audit-log record carrying `outcome: accepted_advisory` and `signature_verified: "advisory_mode"`. The §4.1 STEPs 4-7 branches (resolution / verification failures) similarly emit advisory-mode records but DO NOT flip `status: rejected`.

**Promotion from advisory to enforce** is a per-wrapper minor-bump decision (per spec §4.6 line 323). The MD-A4 minor-bump sweep is the canonical opportunity to flip wrappers from advisory to enforce; impl-doc does NOT specify when a wrapper SHOULD make that flip. That decision belongs to the consumer.

**Boundary**: MD-A2 documents the advisory-mode mechanism. MD-A2 does NOT prescribe adoption criteria (e.g., "advisory until X% of consumers are federated"). Spec §4.6 line 323 governs.

### §4.6 Pubkey cache + invalidation

The resolved-pubkey cache is keyed by `(canonical_id, key_version)` and stores the raw 32-byte pubkey + revocation flag.

**TTL and invalidation rules**:

| Trigger | Action |
|---|---|
| Cache hit, age < 24h | Return cached pubkey; emit audit with `pubkey_source: "local_mirror"` |
| Cache hit, age ≥ 24h | Re-resolve from manifest (cold lookup); refresh cache |
| §5 observation: `MEMBERSHIP_CERT_REVOKED` covering pubkey fingerprint | Mark pubkey as revoked in cache; ALL subsequent §4 verifications fail with `pubkey_revoked` |
| §5 observation: `MEMBERSHIP_NODE_RE_IDENTIFIED` for the canonical_id | Invalidate ALL cached entries for that canonical_id (forces re-resolve from manifest on next request) |
| Manifest entry absent at resolution time | Cache `(canonical_id, key_version) → null` for 60s (negative cache; short TTL prevents thrash); audit with `pubkey_source: "absent"` |

**Cache structure** (advisory shape; implementer chooses storage):

```python
pubkey_cache: dict[tuple[str, str], dict] = {
    ("iris_videoforge", "v1"): {
        "pubkey_bytes": b"...",                          # raw 32-byte Ed25519 pubkey
        "fingerprint_sha256": "7c2e8f...d4b1",          # lowercase hex
        "fetched_at": "2026-05-21T15:00:00Z",
        "revoked": False,
        "revocation_observed_at": None,                  # populated when §5 fires MEMBERSHIP_CERT_REVOKED
    },
    # ...
}
```

**Performance**: cache hit < 5 ms; cache miss < 50 ms (cold manifest read). The §5 observation invalidation is push-driven (the ledger client writes to the cache); the §4 verification reads only.

### §4.7 Audit-log schema

**Storage location**: same as §2.3 / §3.6 — `<receiver_vault>/who/coordination/.audit/audit_<YYYY-MM>.jsonl`. The `event_type` discriminator distinguishes Ed25519 records.

**Record shape — `event_type: federation_sigverify`** (fully specified; one JSON object per line):

```jsonl
{
  "ts": "<RFC 3339 / ISO 8601 timestamp; UTC; second precision>",
  "event_type": "federation_sigverify",
  "audit_id": "<receiver-allocated identifier>",
  "vault": "<receiver vault short name>",
  "requesting_vault": "<requester vault short name>",
  "request_path": "<vault-relative path to the request memo>",
  "requesting_persona_canonical_id": "<per ADR-010 §d; e.g. iris_videoforge>",
  "federation_signature_present": <bool>,
  "federation_signature_key_version": "<string or null; e.g. v1>",
  "pubkey_sha256_resolved": "<64-char lowercase hex or null>",
  "pubkey_source": "<one of: ln_membership_manifest | local_mirror | absent>",
  "canonical_json_hash": "<sha256 hex of the signed payload bytes; for cross-impl interop diagnostics>",
  "substrate_kind_inbound": "<tailscale | nebula | null>",
  "substrate_kinds_allowed": ["tailscale","nebula"],
  "outcome": "<one of: accepted_verified | accepted_advisory | accepted_no_verify | rejected | skipped_not_co_federated>",
  "rejection_subreason": "<one of 5 enum values | null>",
  "profile_mode": "<enforce | advisory>",
  "verify_duration_ms": <int>
}
```

**Field rules**:
- `event_type`: exactly `federation_sigverify`.
- `requesting_persona_canonical_id`: resolved per ADR-010 §d; `null` if resolution failed (and `outcome: rejected` with `rejection_subreason: pubkey_absent`).
- `federation_signature_present` / `federation_signature_key_version`: copied from request frontmatter; both `null` if request was unsigned.
- `pubkey_sha256_resolved`: 64-char lowercase hex of the SHA-256 over the raw 32-byte pubkey; `null` if not resolved.
- `pubkey_source`: `ln_membership_manifest` (resolved live), `local_mirror` (cache hit), `absent` (no manifest entry).
- `canonical_json_hash`: SHA-256 of the canonical-JSON bytes the receiver verified against. For implementer cross-validation: if requester and receiver produce different `canonical_json_hash` values for the same logical input, the canonical-JSON algorithm has drifted (per §4.2).
- `substrate_kind_inbound`: the substrate that carried the inbound request (`tailscale` | `nebula`); `null` for non-co-federated requests where substrate is irrelevant.
- `substrate_kinds_allowed`: array copied from the requesting node's manifest `transport.substrates[*].kind` list.
- `outcome`: closed 5-value enum. `accepted_verified` (full verification passed). `accepted_advisory` (advisory mode; would have rejected). `accepted_no_verify` (non-co-federated; verification skipped). `rejected` (verification failed in enforce mode). `skipped_not_co_federated` (non-co-federated; no audit-evidence value beyond marker).
- `rejection_subreason`: one of `pubkey_absent | pubkey_revoked | signature_mismatch | key_version_unknown | substrate_mismatch | null`. Non-null only when `outcome: rejected`.
- `profile_mode`: wrapper-level configuration value at time of preflight.
- `verify_duration_ms`: total preflight wall time in milliseconds; for performance-budget validation (§4.9).

**Sample records**:

Acceptance with verification (the happy path; pilot-tier wga_l1 → stanley_l1 federation request):

```jsonl
{"ts":"2026-05-21T15:30:00Z","event_type":"federation_sigverify","audit_id":"e1f2a3b4-c5d6-7e8f-9a0b-c1d2e3f4a5b6","vault":"stanley_l1","requesting_vault":"wga_l1","request_path":"who/coordination/coord_2026_05_21_wga_l1_requests_iii_review.md","requesting_persona_canonical_id":"wga_l1","federation_signature_present":true,"federation_signature_key_version":"v1","pubkey_sha256_resolved":"7c2e8f4a1b9d6e3c5a8b0d2e4f6a8c1b3d5e7f9a0c2b4d6e8f1a3c5b7d9e2f4a","pubkey_source":"ln_membership_manifest","canonical_json_hash":"d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5","substrate_kind_inbound":"nebula","substrate_kinds_allowed":["tailscale","nebula"],"outcome":"accepted_verified","rejection_subreason":null,"profile_mode":"enforce","verify_duration_ms":42}
```

Rejection (key version unknown — requester re-keyed but receiver hasn't observed the rotation event):

```jsonl
{"ts":"2026-05-21T15:35:18Z","event_type":"federation_sigverify","audit_id":"a8b7c6d5-e4f3-2a1b-0c9d-8e7f6a5b4c3d","vault":"stanley_l1","requesting_vault":"wga_l1","request_path":"who/coordination/coord_2026_05_21_wga_l1_requests_iii_review_rekey.md","requesting_persona_canonical_id":"wga_l1","federation_signature_present":true,"federation_signature_key_version":"v2","pubkey_sha256_resolved":null,"pubkey_source":"ln_membership_manifest","canonical_json_hash":"f6e5d4c3b2a1098765432109876543210fedcba9876543210fedcba987654321","substrate_kind_inbound":"nebula","substrate_kinds_allowed":["tailscale","nebula"],"outcome":"rejected","rejection_subreason":"key_version_unknown","profile_mode":"enforce","verify_duration_ms":18}
```

Advisory mode (consumer not yet on v0.3; verification would have failed but receiver downgrades):

```jsonl
{"ts":"2026-05-21T15:42:09Z","event_type":"federation_sigverify","audit_id":"b9c8d7e6-f5a4-3b2c-1d0e-9f8a7b6c5d4e","vault":"canvasforge","requesting_vault":"videoforge","request_path":"who/coordination/coord_2026_05_21_videoforge_requests_carly_herb_deck.md","requesting_persona_canonical_id":"iris_videoforge","federation_signature_present":false,"federation_signature_key_version":null,"pubkey_sha256_resolved":null,"pubkey_source":"absent","canonical_json_hash":"0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef","substrate_kind_inbound":"tailscale","substrate_kinds_allowed":["tailscale"],"outcome":"accepted_advisory","rejection_subreason":null,"profile_mode":"advisory","verify_duration_ms":3}
```

### §4.8 Boundary integrity

Building on §2.4 (secrets boundary integrity), the Ed25519 layer carries its own discipline:

**Never audited / logged / committed**:
1. **Ed25519 private keys.** The receiver does not possess them and has no reason to ever encounter them; if a receiver implementation somehow accesses a private key (e.g., via a misconfigured signing path), the audit emission MUST sanitize it. Pre-commit hooks SHOULD scan audit logs for entropy patterns matching private-key shapes.
2. **Full request frontmatter bodies** beyond the audit fields enumerated in §4.7. The canonical-JSON hash IS the integrity signal; the full payload is not duplicated into the audit log.

**Safe to log / commit** (PUBLIC artifacts; recorded for verification):
1. **Signature values** (`federation_signature` field; base64 of the 64-byte Ed25519 signature). Public-by-construction; no secret content.
2. **Pubkey fingerprints** (`pubkey_sha256_resolved`; 64-char lowercase hex). Public per ADR-013 §b semantics.
3. **Pubkey versions** (`federation_signature_key_version`; e.g. `v1`). Public version label.
4. **Canonical-JSON hashes** (`canonical_json_hash`). Identifies WHAT was signed without revealing the bytes themselves.

**Audit-log invariant**: the audit JSONL records produced by §4 MUST be safely committable to a public git repository (provided the request memo itself is committable — which is the §2/§3 status quo). The audit records are signing-attestation evidence, not secret material.

### §4.9 Performance profile

| Component | Budget | Measured-at |
|---|---|---|
| Ed25519 verify (sig 64 B; payload ~4 KB) | < 1 ms | PyNaCl benchmark; modern x86 / ARM |
| Pubkey cache lookup (hot) | < 5 ms | In-process dict lookup |
| Pubkey cache lookup (cold; manifest read) | < 50 ms | Local file I/O |
| Canonical-JSON serialization | < 5 ms | 4 KB frontmatter, ~50 fields |
| Total acceptance-gate cost (sig-verify only) | < 200 ms | Matches §3.4 SLA |
| Total acceptance-gate cost (sig-verify + secrets + idempotency) | < 500 ms | Sum of §2 + §3 + §4 budgets |

**Implementer notes**:
- The 1 ms Ed25519 verify is the dominant cost only in trivial cases; pubkey resolution + canonical-JSON serialization can dominate on cold-cache paths.
- For very-high-throughput receivers (>100 acceptances/second), the cache invalidation via §5 observation becomes the bottleneck — design the §5 client to push invalidations asynchronously so the §4 path stays read-only and lock-free.
- Audit emission is fire-and-forget at acceptance time; receivers MAY batch audit-log writes if disk I/O is the bottleneck (the audit JSONL is append-only by construction; batching does not reorder).

---

## §5 Ledger Observation Client

Resolves spec §5.3. The normative contract is at spec §5.3 lines 358-374 + spec §5.3 line 374 explicitly defers the subscription model choice to MD-A2 ("the airlock client implementation (MD-A2) chooses between polling, pub/sub, or chain-walk; the contract above is implementation-agnostic"). This section recommends **polling at 60s cadence** as the v0.3 implementation, with stated tradeoffs.

### §5.1 Subscription model — polling at 60s recommended

**Recommendation**: 60-second polling over the LN Tier-1 file-ledger path (`~/.lattice/ledger/<network>/<YYYY-MM>/<event_id>.json` per LN Tier-1 conventions; bore.pub:42561 tunnel-mirrored to all federation members per pc_01 Phase A close 2026-05-20).

**Justification** — grounded in LN ADR-015 §c event cadence:

| Substrate event | Expected cadence | Receiver sensitivity |
|---|---|---|
| `MEMBERSHIP_CERT_ISSUED` / `MEMBERSHIP_CERT_REVOKED` | Quarterly drills + ad-hoc (per ADR-015 §e cl. 3) | Hot — drives §4 cache |
| `MEMBERSHIP_NODE_RE_IDENTIFIED` | Once per node lifecycle (per ADR-014 §b) | Hot |
| `MEMBERSHIP_SUBSTRATE_JOINED` / `MEMBERSHIP_SUBSTRATE_LEFT` | Onboarding-time (rare) | Cold |
| `MEMBERSHIP_LIGHTHOUSE_SET` | One-shot per overlay | Cold |
| `CONNECTION_*` (4 event types) | Per-connection (could be O(10/h) under load) | Warm |

60-second polling gives:
- **60s worst-case revocation-detection latency**: tolerable for human-paced acceptance (most cross-vault requests are minutes apart; a freshly-revoked cert that signs a request will be caught within one polling window).
- **< 10 ms per-poll cost**: the Tier-1 file-ledger is a flat directory with predictable file naming; the scan walks new files since the last cursor only.
- **Zero new infrastructure**: the Tier-1 file ledger is already shared via the bore.pub:42561 durable tunnel; the receiver does not need to subscribe to anything new.
- **Trivially restartable**: the cursor is the last `event_id` processed; receiver crashes resume cleanly from the JSONL audit log.

**Tradeoffs**:

| Model | Pros | Cons | v0.3 disposition |
|---|---|---|---|
| **Polling at 60s** | Zero new infra; restart-safe; predictable cost | 60s revocation latency; wasted I/O when quiet | **RECOMMENDED at v0.3** |
| Pub/sub | Sub-second latency; event-driven | Requires LN-side broker not yet specified (out of ADR-015 §k spec-impact); daemon lifecycle ownership | **Deferred to v0.4+** (when LN ships broker) |
| Chain-walk on-demand | Zero background cost (no daemon) | In-band cost into §4 path; breaks the < 200 ms budget under degraded substrate | **Rejected** (in-band cost) |

**Boundary** — this is a v0.3 **implementation recommendation**, not a normative restriction. Spec §5.3 line 374 explicitly leaves the choice to MD-A2; the impl-doc preserves that framing. Future consumers MAY adopt pub/sub or chain-walk; the spec contract is unchanged.

### §5.2 Subscription scope

The receiver subscribes to ledger events scoped by relevance:

- **Default scope**: the receiver vault's own `canonical_id` + the canonical_ids of its **declared peers** (the federation members it has had at least one prior cross-vault interaction with; the set is local-receiver-state, persisted at `<receiver_vault>/who/coordination/.airlock/federation_peers.yaml` or equivalent).
- **Broader scope** is permitted but increases ingest cost. A receiver MAY subscribe to the entire federation (all canonical_ids in the manifest) at the cost of polling all `MEMBERSHIP_*` and `CONNECTION_*` events from any sender.
- **Narrower scope** is NOT permitted — the receiver MUST observe at least its own canonical_id's events (otherwise its own re-identification or substrate changes are invisible).

### §5.3 Observed event-type taxonomy

The §5 client observes the following event types (per LN ADR-015 §c + ADR-014 §a):

**MEMBERSHIP_\*** (5 event types per ADR-015 §c + 1 per ADR-014):

| Event type | Receiver action |
|---|---|
| `MEMBERSHIP_SUBSTRATE_JOINED` | Record advisory; update local view of node's substrate list |
| `MEMBERSHIP_SUBSTRATE_LEFT` | Record advisory; node's substrate list shrinks; §4.4 `substrate_mismatch` checks tighten |
| `MEMBERSHIP_CERT_ISSUED` | Warm pubkey cache for the subject `canonical_id` (proactive resolution) |
| `MEMBERSHIP_CERT_REVOKED` | **Invalidate cached pubkey** for the subject `canonical_id` + fingerprint; subsequent §4 verifications fail `pubkey_revoked` |
| `MEMBERSHIP_LIGHTHOUSE_SET` | Record advisory; no airlock-side state change at v0.3 |
| `MEMBERSHIP_NODE_RE_IDENTIFIED` (ADR-014 §a) | **Invalidate all cached entries** for the `prior_canonical_id` + `new_canonical_id`; force re-resolve on next request |

**CONNECTION_\*** (4 event types per ADR-015 §c extension; all carry `payload.substrate_kind`):

| Event type | Receiver action |
|---|---|
| `CONNECTION_ESTABLISHED` | Record advisory |
| `CONNECTION_TERMINATED` | Record advisory |
| `CONNECTION_HEALTH_DEGRADED` | Mark the affected substrate as degraded in local health cache; informs spec §5.4 failover decisions |
| `CONNECTION_FAILOVER` | Update local substrate-health cache to reflect the new active substrate per the failover event |

### §5.4 Receiver-side handlers

Pseudocode for the polling loop:

```
STATE: cursor = last processed event_id (persisted at <receiver_vault>/who/coordination/.airlock/ledger_cursor)
STATE: pubkey_cache (§4.6)
STATE: substrate_health_cache (per-canonical_id, per-substrate; health states: ok | degraded | failover_to_<peer>)

LOOP every 60 seconds:
    events ← fetch_since(cursor, scope=subscription_scope)
    for evt in events sorted-by-event_id-lexicographic:  # ADR-014 §c determinism
        match evt.event_type:
            "MEMBERSHIP_CERT_REVOKED":
                pubkey_cache.invalidate(evt.payload.cert_subject_canonical_id,
                                         fingerprint=evt.payload.cert_sha256)
                receiver_action ← "pubkey_invalidated"

            "MEMBERSHIP_NODE_RE_IDENTIFIED":
                pubkey_cache.invalidate_all(evt.payload.prior_canonical_id)
                pubkey_cache.invalidate_all(evt.payload.new_canonical_id)
                receiver_action ← "pubkey_invalidated"

            "MEMBERSHIP_CERT_ISSUED":
                pubkey_cache.warm(evt.payload.cert_subject_canonical_id,
                                   fingerprint=evt.payload.cert_sha256)
                receiver_action ← "pubkey_warmed"

            "MEMBERSHIP_SUBSTRATE_JOINED" | "MEMBERSHIP_SUBSTRATE_LEFT":
                substrate_membership_view.update(evt)
                receiver_action ← "substrate_membership_updated"

            "MEMBERSHIP_LIGHTHOUSE_SET":
                receiver_action ← "recorded_advisory"

            "CONNECTION_HEALTH_DEGRADED":
                substrate_health_cache.mark_degraded(evt.payload.subject_canonical_id,
                                                     substrate_kind=evt.payload.substrate_kind)
                receiver_action ← "substrate_marked_degraded"

            "CONNECTION_FAILOVER":
                substrate_health_cache.flip(evt.payload.subject_canonical_id,
                                             from_substrate=evt.payload.from_substrate_kind,
                                             to_substrate=evt.payload.to_substrate_kind)
                receiver_action ← "substrate_failover_recorded"

            "CONNECTION_ESTABLISHED" | "CONNECTION_TERMINATED":
                receiver_action ← "recorded_advisory"

            default:
                # Unknown event type — log and continue; do NOT halt the loop
                receiver_action ← "ignored_unknown_event_type"

        emit_audit(event_type="ledger_observation", observed=evt, receiver_action=receiver_action)
        cursor ← evt.event_id
        persist_cursor(cursor)
```

**Implementer notes**:
- The 60s loop is the v0.3 recommendation; sub-60s polling is permitted (operator choice; cost-driven).
- Sorting by `event_id` lexicographic ascending matches the ADR-014 §c determinism contract. The receiver does NOT need to sort by timestamp (which can have clock-skew variance); event_id ordering is canonical.
- On startup with empty cursor (first run), the receiver MAY scan from the beginning of the current month's ledger or skip to the latest event — operator choice. Skipping forward is recommended for production receivers that don't need historical context.

### §5.5 Read-only enforcement

The §5 client MUST NOT write to the LN ledger. The only ledger-write surface in the airlock implementation is §6 (`COMPLIANCE_AUDIT` emission), which is a separate code path with its own audit discipline.

**Static-analysis recipe** — when reviewing a future executable §5 client implementation:

```bash
# Assert no ledger-write API calls in the §5 client module
grep -E "ledger\.(write|emit|append|publish)|POST /events|/ledger/.+/(write|emit)" \
     <receiver_repo>/airlock/ledger_client.py
# Expected: zero hits.
```

A §5 client that imports a ledger-write API (even if unused) is a code-review red flag — the dependency itself is a potential boundary violation surface. Keep the §5 client API surface to a strict read-only subset.

### §5.6 Audit-log schema

**Storage location**: same as §2.3 / §3.6 / §4.7 — `<receiver_vault>/who/coordination/.audit/audit_<YYYY-MM>.jsonl`.

**Record shape — `event_type: ledger_observation`** (fully specified; one JSON object per line):

```jsonl
{
  "ts": "<RFC 3339 / ISO 8601 timestamp; UTC; second precision>",
  "event_type": "ledger_observation",
  "audit_id": "<per-subscription-session id; NOT per-request — distinct from §2/§3/§4 audit_ids>",
  "vault": "<receiver vault short name>",
  "observed_event_id": "<evt_<uuid> of the LN ledger event>",
  "observed_event_type": "<one of: MEMBERSHIP_CERT_REVOKED | MEMBERSHIP_NODE_RE_IDENTIFIED | MEMBERSHIP_CERT_ISSUED | MEMBERSHIP_SUBSTRATE_JOINED | MEMBERSHIP_SUBSTRATE_LEFT | MEMBERSHIP_LIGHTHOUSE_SET | CONNECTION_ESTABLISHED | CONNECTION_TERMINATED | CONNECTION_HEALTH_DEGRADED | CONNECTION_FAILOVER>",
  "observed_event_ts": "<RFC 3339 timestamp from the observed event; UTC>",
  "subject_canonical_id": "<canonical_id the event relates to | null>",
  "substrate_kind": "<tailscale | nebula | null>",
  "receiver_action": "<one of: pubkey_invalidated | pubkey_warmed | substrate_membership_updated | substrate_marked_degraded | substrate_failover_recorded | recorded_advisory | ignored_unknown_event_type>",
  "cursor_advanced_to": "<evt_<uuid>>"
}
```

**Field rules**:
- `audit_id`: per-subscription-session UUID (the receiver allocates one at §5 client startup and reuses it for the lifetime of the polling loop). Distinct from §2/§3/§4 per-request audit_ids.
- `observed_event_id`: the `event_id` from the LN ledger entry (canonical-JSON event_id per ADR-014 §c).
- `observed_event_type`: closed enum of the 10 known types (5 MEMBERSHIP_* per ADR-015 + 1 per ADR-014 + 4 CONNECTION_*); unknown event types observed via the `default` branch in §5.4 carry `observed_event_type` as the literal string seen and `receiver_action: ignored_unknown_event_type`.
- `subject_canonical_id`: payload-derived; `null` for events that have no single subject (rare).
- `substrate_kind`: payload-derived from `payload.substrate_kind` for CONNECTION_* events; `null` for MEMBERSHIP_* events that don't carry the field (e.g., `MEMBERSHIP_NODE_RE_IDENTIFIED`).
- `receiver_action`: closed enum of 7 values matching the §5.4 handler set.
- `cursor_advanced_to`: the event_id AFTER processing this event; persisted to disk for restart safety.

**Sample records**:

Cert revocation observed (the §4 cache invalidates):

```jsonl
{"ts":"2026-05-21T15:45:00Z","event_type":"ledger_observation","audit_id":"f1e2d3c4-b5a6-9788-7766-5544332211ff","vault":"stanley_l1","observed_event_id":"evt_a1b2c3d4-e5f6-7890-1234-567890abcdef","observed_event_type":"MEMBERSHIP_CERT_REVOKED","observed_event_ts":"2026-05-21T15:43:22Z","subject_canonical_id":"iris_videoforge","substrate_kind":"nebula","receiver_action":"pubkey_invalidated","cursor_advanced_to":"evt_a1b2c3d4-e5f6-7890-1234-567890abcdef"}
```

Substrate failover observed:

```jsonl
{"ts":"2026-05-21T15:50:00Z","event_type":"ledger_observation","audit_id":"f1e2d3c4-b5a6-9788-7766-5544332211ff","vault":"stanley_l1","observed_event_id":"evt_b2c3d4e5-f6a7-8901-2345-67890abcdef0","observed_event_type":"CONNECTION_FAILOVER","observed_event_ts":"2026-05-21T15:49:18Z","subject_canonical_id":"wga_l1","substrate_kind":"nebula","receiver_action":"substrate_failover_recorded","cursor_advanced_to":"evt_b2c3d4e5-f6a7-8901-2345-67890abcdef0"}
```

### §5.7 Performance profile

| Component | Budget |
|---|---|
| 60s poll cycle (default scope: receiver canonical_id + declared peers) | < 10 ms per poll (no events) |
| Per-event handler dispatch | < 5 ms (cache invalidation is in-process dict mutation) |
| Audit emission per event | < 1 ms (append to JSONL) |
| Cursor persistence | < 5 ms (overwrite a single file) |
| Restart from persisted cursor | < 100 ms (read cursor; resume polling) |
| Worst-case revocation-detection latency | 60s (one polling interval) |

**Implementer notes**:
- For high-event-rate substrates (CONNECTION_* under heavy load), the receiver MAY batch process events within a single poll cycle; audit emission can also batch as long as ordering by event_id is preserved.
- The cursor file is small (single line; ~50 bytes); atomic-rename-on-write pattern (`write to .tmp; rename to canonical`) prevents partial cursor reads on crash.
- A receiver that misses a polling interval (e.g., due to system suspend) catches up automatically on next poll — events accumulate in the file-ledger until the cursor advances.

---

## §6 `COMPLIANCE_AUDIT` Event Emission

Resolves spec §5.3.1. The normative contract is at spec §5.3.1 lines 376-403 (event schema; idempotency requirement; `assumes_draft` binding). This section specifies the **implementation** of the emission, including the **direct-JSON-write workaround** that holds until LN Carry 3 EP-1 ratifies native `LedgerEventType` enum membership at Phase 5.

### §6.1 Emission trigger sites

The receiver emits a `COMPLIANCE_AUDIT` event upon **request acceptance or rejection close** — i.e., at the moment the receiver's reply-comment is appended to the request memo and the memo frontmatter `status` flips to one of `accepted | rejected | cancelled` (or `duplicate_of` via the §3 dedup pathway).

**Trigger map**:

| Acceptance / rejection path | Triggers `COMPLIANCE_AUDIT`? | Disposition value |
|---|---|---|
| `secrets_preflight` accepted (§2) → pipeline allocates | Yes (one per accepted request) | `accepted` |
| `secrets_preflight` rejected (§2) `missing_secret` | Yes | `rejected` |
| `idempotency_check` produces `duplicate_of` (§3) | Yes | `duplicate_of` |
| `idempotency_check` produces `bypassed_force_new` (§3) | Yes (rare; flagged for review) | `accepted` (with audit-payload note `force_new_observed: true`) |
| `federation_sigverify` `accepted_verified` (§4) | Yes (signature_verified: true) | `accepted` |
| `federation_sigverify` `accepted_advisory` (§4) | Yes (signature_verified: "advisory_mode") | `accepted` |
| `federation_sigverify` `rejected` (§4) | Yes (rejection_reason populated) | `rejected` |
| Memo manually cancelled by receiver (after acceptance but before pipeline complete) | Yes | `cancelled` |

**Idempotency**: §6.4 specifies the deterministic event_id mechanism. Re-emission with identical inputs is a no-op (the second emission writes the same bytes to the same path; receivers detect duplicate emission via event_id collision).

### §6.2 Payload assembly + canonical-JSON hash

The receiver assembles the **exact 9-field payload** from spec §5.3.1 lines 390-399. Field-by-field provenance:

| Field | Source |
|---|---|
| `audit_id` | The request memo's `frontmatter.audit_id` (receiver-allocated at acceptance per §2.3) |
| `request_path` | Vault-relative path to the request memo (matches §2.3 / §3.6 / §4.7 `request_path` field) |
| `disposition` | Closed enum `accepted | rejected | cancelled | duplicate_of` — derived from the §6.1 trigger map |
| `rejection_reason` | Populated only when `disposition == rejected`; value is the same string the reply-comment template's Rejection block carries (e.g., `signature_verification_failed:pubkey_absent` or `missing_secret:S3_OUTPUT_WRITE_TOKEN`) |
| `request_frontmatter_hash` | SHA-256 hex of the canonical-JSON serialization of the request frontmatter EXCLUDING `federation_signature` (matches the §4.2 signed-payload computation) |
| `airlock_spec_version` | The spec version at acceptance time (e.g., `"0.3.0"`) |
| `substrate_kind` | The substrate that carried the inbound request (`"tailscale"` | `"nebula"`); matches §4.7 `substrate_kind_inbound` |
| `signature_verified` | One of `true` (sig-verify passed) | `false` (sig-verify failed; emitted alongside rejection) | `"advisory_mode"` (advisory profile; verification was advisory not enforced) |
| `operator_attestation` | A single-string blob: `"<persona>, <RFC 3339 timestamp>[, <optional free-text rationale>]"`. Personas are the receiver's persona (e.g., `argus` for III, `iris` for VideoForge). |

**Payload hash** (per spec §5.3.1 line 401):
```
payload_hash = sha256(canonical_json(payload))    # per §4.2 algorithm
```

The hash MUST be deterministic per the §4.2 canonical-JSON algorithm — re-emission with identical payload produces identical hash.

**Event envelope** assembled around the payload (per spec §5.3.1 lines 381-389):
```
event_type:           "COMPLIANCE_AUDIT"          # assumes_draft per §6.3
event_id:             evt_<deterministic_uuid>   # §6.4 derivation
timestamp:            <RFC 3339 / UTC at close>
source_did:           <receiving_vault_canonical_id>
target_did:           <requesting_vault_canonical_id>
previous_event_hash:  <prev_entry_hash from ledger tip>
payload_hash:         <above>
signature:            <ed25519_sign(receiver_signing_key, canonical_json(envelope))>
                                                  # signature signs envelope EXCLUDING signature field
payload:              { 9-field payload above }
```

### §6.3 `assumes_draft` workaround — direct-JSON-write

Per spec §5.3.1 line 403 + LN ADR-014 §e + ADR-015 §c (Carry 3 EP-1): the `COMPLIANCE_AUDIT` event-type literal is not yet a native member of the latlab `LedgerEventType` enum. Native enum membership lands at Phase 5 integration seam (per the cluster amendment including `MEMBERSHIP_NODE_RE_IDENTIFIED` per ADR-014 §e and the 5 substrate-tagged candidates per ADR-015 §c).

Until Phase 5, the receiver uses a **direct-JSON-write** path that:

1. **Computes the same hashes** that the native enum path would compute (per §4.2 canonical-JSON algorithm).
2. **Writes bytes-on-disk byte-identical** to the native enum emission path.
3. **Bypasses the `LedgerEventType` enum validation** at the source-side ledger client by writing the JSON envelope directly to the Tier-1 file-ledger path (`~/.lattice/ledger/<network>/<YYYY-MM>/<event_id>.json`).
4. **Annotates** the call site with `# assumes_draft: true per ADR-003 rule 2; flips to native at LN Carry 3 EP-1 ratification`.

**Workaround code skeleton** (illustrative; not runnable):

```python
def emit_compliance_audit(payload: dict, *, source_did: str, target_did: str,
                          receiver_signing_key, ledger_tip_entry_hash: str) -> str:
    # 1. canonical-JSON of payload (§4.2 algorithm)
    payload_canonical = _canonical_json(payload)
    payload_hash = hashlib.sha256(payload_canonical.encode("utf-8")).hexdigest()

    # 2. deterministic event_id (§6.4)
    event_id_seed = f"{payload_hash}:{ledger_tip_entry_hash}"
    event_id = f"evt_{uuid.uuid5(uuid.NAMESPACE_OID, event_id_seed)}"

    # 3. envelope
    envelope = {
        "event_type": "COMPLIANCE_AUDIT",       # assumes_draft: true per ADR-003 rule 2;
                                                 # flips to native at LN Carry 3 EP-1 ratification
        "event_id": event_id,
        "timestamp": datetime.now(timezone.utc).strftime("%Y-%m-%dT%H:%M:%SZ"),
        "source_did": source_did,
        "target_did": target_did,
        "previous_event_hash": ledger_tip_entry_hash,
        "payload_hash": payload_hash,
        "signature": "",                         # placeholder; filled after canonical-JSON of envelope\{signature}
        "payload": payload,
    }
    envelope_to_sign = {k: v for k, v in envelope.items() if k != "signature"}
    envelope_canonical = _canonical_json(envelope_to_sign)
    envelope["signature"] = base64.b64encode(receiver_signing_key.sign(envelope_canonical.encode("utf-8")).signature).decode("ascii")

    # 4. direct-JSON-write to Tier-1 file-ledger path
    month = envelope["timestamp"][:7]            # YYYY-MM
    path = pathlib.Path(f"~/.lattice/ledger/alpha_lattice/{month}/{event_id}.json").expanduser()
    path.parent.mkdir(parents=True, exist_ok=True)
    path.write_text(_canonical_json(envelope), encoding="utf-8")

    # 5. local audit-log mirror (§6.5) — independent of the ledger fire
    _emit_audit(event_type="compliance_audit_emit",
                compliance_audit_event_id=event_id,
                compliance_audit_event_path=str(path),
                ledger_emit_mode="direct_json_write",
                assumes_draft=True,
                ...)
    return event_id
```

**Migration to native enum** (when LN Carry 3 EP-1 ratifies at Phase 5):

```python
# Single branch flip:
- path.write_text(_canonical_json(envelope), encoding="utf-8")
+ ledger_client.emit(envelope)          # native LedgerEventType.COMPLIANCE_AUDIT
```

The migration is **bytes-identical** — the ledger client's native `emit()` writes the same canonical-JSON bytes to the same path. Prior direct-JSON-written events are recognized natively at migration without re-emit or re-hash. The audit-log mirror records the migration via `ledger_emit_mode` flipping from `direct_json_write` to `native_enum`.

**Correctness assertion for implementers**: for any direct-JSON-written event, re-hash the on-disk payload + envelope; the resulting SHA-256 hash MUST match the originally-stored hash. Drift indicates either (a) canonical-JSON algorithm divergence (bug in §4.2) or (b) post-write mutation of the on-disk bytes (different bug). Either way, fix before Phase 5 migration.

**Cross-reference**: F-S23-02 disposition in the LN backlog tracks the enum-coerce gap. The §6.3 workaround is the airlock-side mitigation; the LN-side enum widening is the substrate-side fix.

### §6.4 Idempotency mechanics

Per spec §5.3.1 line 401 + ADR-014 §c: re-emission with identical inputs MUST produce **identical event_id + payload_hash + entry_hash**.

**Deterministic event_id derivation**:
```
event_id_seed = f"{payload_hash}:{previous_event_hash}"
event_id = "evt_" + uuid5(NAMESPACE_OID, event_id_seed)
```

This produces a UUID v5 deterministically from the payload + ledger tip. Two emissions with the same payload AND same ledger tip produce the same event_id; receivers detect duplicates by event_id collision when writing to the Tier-1 file-ledger (file already exists at `<event_id>.json` path → it's the same bytes; safe to no-op).

**Why uuid5 over uuid4**: spec §5.3.1 line 382 specifies `event_id: "evt_<uuid_v4>"` — but §5.3.1 line 401 ALSO mandates identical event_id under re-emission with identical inputs. `uuid_v4` is random; `uuid_v5` is deterministic. The text on line 382 is a shape-example (the prefix and structure), not a generation-method requirement. The deterministic-emission requirement at line 401 governs — implementers MUST use uuid5 (or any deterministic seeded UUID generator producing the same shape).

**Idempotency-index sidecar** (optional; recommended for high-volume receivers):

```
<receiver_vault>/who/coordination/.audit/compliance_audit_index.jsonl
```

One line per emitted COMPLIANCE_AUDIT event:
```jsonl
{
  "audit_id": "<receiver request audit_id>",
  "disposition": "<accepted | rejected | cancelled | duplicate_of>",
  "compliance_audit_event_id": "evt_<uuid5>",
  "compliance_audit_event_path": "<abs path to Tier-1 file-ledger entry>",
  "first_emitted_at": "<RFC 3339; UTC>"
}
```

The map `(audit_id, disposition) → event_id` lets the receiver detect duplicate-emission attempts in O(1) before computing payload hashes. The sidecar is **receiver-local evidence**, not the ledger — the canonical event lives at the Tier-1 file-ledger path.

### §6.5 Local audit-log mirror

Building on §2.3 / §3.6 / §4.7 / §5.6, the receiver emits a LOCAL audit-log record (in `who/coordination/.audit/audit_<YYYY-MM>.jsonl`) for every `COMPLIANCE_AUDIT` emission. The local record is **receiver-only state** and MUST NOT leak into the emitted ledger payload (spec §5.3.1 payload is the 9-field shape; nothing else).

**Record shape — `event_type: compliance_audit_emit`** (fully specified; one JSON object per line):

```jsonl
{
  "ts": "<RFC 3339 / ISO 8601 timestamp; UTC; second precision>",
  "event_type": "compliance_audit_emit",
  "audit_id": "<matches request memo's audit_id; same value as §2/§3/§4 records>",
  "vault": "<receiver vault short name>",
  "requesting_vault": "<requester vault short name>",
  "request_path": "<vault-relative path to the request memo>",
  "compliance_audit_event_id": "<evt_<uuid5> — the ledger event_id>",
  "compliance_audit_event_path": "<abs path to Tier-1 file-ledger entry>",
  "disposition": "<one of: accepted | rejected | cancelled | duplicate_of>",
  "rejection_subreason": "<spec §4.4 sub-reason value | missing_secret:<NAME> | duplicate_of | null>",
  "substrate_kind": "<tailscale | nebula>",
  "signature_verified": "<true | false | advisory_mode>",
  "request_frontmatter_hash": "<sha256 hex of canonical-JSON of frontmatter\{federation_signature}>",
  "ledger_emit_mode": "<one of: native_enum | direct_json_write>",
  "assumes_draft": <true | false>
}
```

**Field rules**:
- `event_type`: exactly `compliance_audit_emit`.
- `audit_id`: matches the request memo's audit_id (same value as the §2/§3/§4 records for this request — single audit_id ties the full chain together).
- `compliance_audit_event_id`: the deterministic uuid5-derived event_id from §6.4.
- `compliance_audit_event_path`: absolute path on the receiver's filesystem; useful for forensic re-hash + audit reconciliation.
- `disposition`: matches the payload `disposition` field (one of 4 closed enum values).
- `rejection_subreason`: extends the §4.4 5-value enum with §2 `missing_secret:<NAME>` and §3 `duplicate_of`; `null` for accepted/cancelled.
- `signature_verified`: matches the payload `signature_verified` field (true | false | "advisory_mode").
- `request_frontmatter_hash`: matches the payload `request_frontmatter_hash` field (re-hash on demand for verification).
- `ledger_emit_mode`: `direct_json_write` until LN Carry 3 EP-1 ratifies; `native_enum` after migration.
- `assumes_draft`: `true` while ledger_emit_mode == direct_json_write; `false` after Phase 5 migration.

**Sample record** (acceptance with full sig-verify; emitted to Tier-1 file-ledger via direct-JSON-write):

```jsonl
{"ts":"2026-05-21T15:30:05Z","event_type":"compliance_audit_emit","audit_id":"e1f2a3b4-c5d6-7e8f-9a0b-c1d2e3f4a5b6","vault":"stanley_l1","requesting_vault":"wga_l1","request_path":"who/coordination/coord_2026_05_21_wga_l1_requests_iii_review.md","compliance_audit_event_id":"evt_7a8b9c0d-1e2f-5345-9876-fedcba9876ab","compliance_audit_event_path":"/Users/stanley/.lattice/ledger/alpha_lattice/2026-05/evt_7a8b9c0d-1e2f-5345-9876-fedcba9876ab.json","disposition":"accepted","rejection_subreason":null,"substrate_kind":"nebula","signature_verified":"true","request_frontmatter_hash":"d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5","ledger_emit_mode":"direct_json_write","assumes_draft":true}
```

### §6.6 `substrate_kind` propagation

Per spec §5.4 cl. 5: every emitted `COMPLIANCE_AUDIT` event MUST carry `payload.substrate_kind` recording the substrate the request arrived on. This provides substrate-traceability per ADR-015 §e cl. 5.

The `substrate_kind` field flows through three records for a single request:

1. **§4.7 `federation_sigverify` audit record** — `substrate_kind_inbound` = the actual transport substrate observed at acceptance.
2. **§6.5 `compliance_audit_emit` audit record** — `substrate_kind` = copied from §4.7.
3. **Emitted COMPLIANCE_AUDIT payload** — `substrate_kind` = same value; lives in the ledger event for downstream substrate-traceability.

**Determination**: the receiver discovers `substrate_kind` at request-acceptance time by inspecting the inbound connection's transport. The mechanism is integration-dependent (Tailscale: the request arrived from a `100.x.x.x` IP in the tailnet; Nebula: the request arrived from a `10.42.x.x` IP in the overlay). For airlock-substrate integrations that don't yet have transport-aware acceptance (current state for most consumer wrappers at v0.2/v0.3), the field MAY be set to `null` or to the operator-declared default per the wrapper config.

### §6.7 Failure modes

| Failure | Disposition |
|---|---|
| Tier-1 file-ledger path not writable (filesystem permission, disk full) | Audit `compliance_audit_emit` with `ledger_emit_mode: "direct_json_write_failed"`; receiver SHOULD escalate (operator notification); request lifecycle continues — sig-verify decision was already made, COMPLIANCE_AUDIT is an observation event |
| Tier-1 file-ledger path exists at `<event_id>.json` already | Duplicate emission detected; receiver re-hashes existing file + payload; if match → no-op (idempotent success); if mismatch → critical error (canonical-JSON drift); audit the mismatch |
| Receiver signing key absent | Cannot sign envelope; degrade to `"signature": ""` (unsigned event); audit with `ledger_emit_mode: "direct_json_write_unsigned"`; same failure-mode class as ADR-013 §a operator-attestation pattern |
| §5 client crashed mid-loop (stale ledger tip) | Use last-known `previous_event_hash` from local cache; tip will reconcile on next §5 poll cycle |
| Spec version mismatch (`airlock_spec_version` differs from receiver's running version) | Emit anyway with receiver's current version; downstream consumers see version drift via the payload field |

**Operator escalation channel**: when `ledger_emit_mode != "native_enum"` AND `assumes_draft: true`, the receiver's wrapper-level config MAY include an alert hook (`<receiver_vault>/who/coordination/.airlock/escalation_hook.sh` or equivalent). Hook firing is OUT OF MD-A2 SCOPE (operational concern; wrapper-level decision); MD-A2 documents the hook surface but does not prescribe contents.

---

## §7 Reply-Comment Template Integration

The reply-comment template at `how/templates/template_cross_vault_request_reply.md` (shipped at MC-3 2026-05-10; patched at MD-A2 for v0.3 federation-signature rejection) has slots wired to consume the outputs of §2 + §3 + §4:

**Acceptance fenced block (template line 38)** — Secret presence:
```
- **Secret presence** (if `secrets_handled.needed` declared): <list of confirmed-present names — never values>
```

Filled per §2.2 (Acceptance reason format). Receiver writes the comma-separated list of names from `§2.3 audit record.secrets_present` (which matches `secrets_required` exactly on the accepted branch).

**Acceptance fenced block (template line 39)** — Idempotency check:
```
- **Idempotency check** (if `idempotency_key` declared): <"no match" | "duplicate_of: <existing_memo_path>; prior audit_id <id>">
```

Filled per §3.5 (reply-comment template fill-in). Receiver writes one of the four canonical strings: `no match`, `duplicate_of: ...`, `bypassed (force_new: true) — would have matched duplicate_of: ...`, or `no key declared`.

**Acceptance — NEW at v0.3 — Federation signature** (recommended addition to the Acceptance fenced block on minor-bump wrapper sweep at MD-A4):
```
- **Federation signature** (if co-federated): <"verified (key_version: v1; substrate: nebula)" | "advisory_mode (unsigned; profile downgrade)" | "skipped (not co-federated)">
```

Filled per §4.7 audit record. Receivers MAY add this slot to their Acceptance block at MD-A4 sweep; absence is acceptable at v0.3 (Acceptance audit log carries the same information at §4.7).

**Rejection fenced block (template line ~60)** — `reason:` enum:
- The MC-3 enum included: `validation_failed | missing_secret: <NAME> | scope_mismatch | lattice_incompatible | budget_ceiling_exceeded | over_capacity_no_queue | other`.
- **MD-A2 adds** (template patch at MD-A2): `signature_verification_failed: <pubkey_absent | pubkey_revoked | signature_mismatch | key_version_unknown | substrate_mismatch>` — exact 5-value sub-reason enum from §4.4.

**Ed25519 rejection body example**:
```
## Rejection (2026-05-21)

- **Status**: rejected
- **Reason**: signature_verification_failed: pubkey_revoked
- **Details**: The federation signing key version v1 for canonical_id `iris_videoforge` was revoked at 2026-05-20T18:00Z per MEMBERSHIP_CERT_REVOKED event evt_a1b2c3d4. Receiver's ledger-observation cache invalidated the pubkey; current sig-verify failed.
- **Remediation**: Re-key per ADR-013 §c rotation policy; emit MEMBERSHIP_CERT_ISSUED for the new key; re-file request with `federation_signature_key_version: v2` and a v2-key signature.
- **Next steps**: requester to refile after re-key.
```

**Status Log row** (no change vs MC-3):
```
| 2026-05-21 | rejected | signature_verification_failed: pubkey_revoked; audit_id e1f2…a5b6 |
```

---

## §8 Cross-References

**Spec**:
- `what/artifacts/iii_airlock_standard_spec.md` §4.4 lines 240-263 — normative contract for `secrets_handled`. This artifact's §2 resolves the line-261 forward-reference.
- `what/artifacts/iii_airlock_standard_spec.md` §4.5 lines 265-285 — normative contract for `idempotency_key`. This artifact's §3 resolves the line-282 forward-reference.
- `what/artifacts/iii_airlock_standard_spec.md` §4.6 lines 300-327 — NEW at v0.3 — normative contract for Ed25519 federation signing-key verification. This artifact's §4 resolves the line-325 forward-reference.
- `what/artifacts/iii_airlock_standard_spec.md` §5.1-§5.4 lines 331-413 — NEW at v0.3 — Federation-Substrate Awareness contract. This artifact's §4 + §5 resolve §5.2 + §5.3 forward-references.
- `what/artifacts/iii_airlock_standard_spec.md` §5.3.1 lines 376-403 — NEW at v0.3 — `COMPLIANCE_AUDIT` event-emission schema. This artifact's §6 resolves the line-403 forward-reference (including the `assumes_draft` workaround).
- `what/artifacts/iii_airlock_standard_spec.md` §5.5 lines 415-425 — forward-references list. This artifact's §4 + §5 + §6 resolve the MD-A2 rows (Ed25519, ledger observation, COMPLIANCE_AUDIT).
- `what/artifacts/iii_airlock_standard_spec.md` §7.5 — Federation-substrate version pin. This artifact inherits the pin via its frontmatter `ln_substrate_version_pin` field.
- `what/artifacts/iii_airlock_standard_spec.md` §8.1-§8.3 — coverage map, deferral table, forward-references list. Flipped at MD-A2 close.

**LN ADRs consumed by reference** (NEW at v0.3):
- `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_010_canonical_node_id_schema_three_universes.md` §d 11-row roster + §b canonical-pin policy. Cited from §4.3 (canonical_id resolution).
- `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_013_federation_signing_key_infrastructure.md` §a operator-attestation trust model; §b per-purpose-slot pubkey dict + Ed25519 algorithm + SHA-256 fingerprint format; §c source-side keygen tooling (out-of-MD-A2-scope); §e verify-step binding to ADR-004 §a transmission ceremony. Cited from §4.1 / §4.3 / §4.6.
- `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_014_re_id_semantics_node_canonical_id_transition.md` §a `MEMBERSHIP_NODE_RE_IDENTIFIED` event payload schema; §c canonical-JSON idempotency contract (sorted keys, NFC, whitespace trimmed, case preserved, SHA-256 over canonical JSON); §e Carry 3 EP-1 deferred-binding at Phase 5 integration seam. Cited from §4.2 / §5.4 / §6.2 / §6.3 / §6.4.
- `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_015_federation_substrate_pluralism_tailscale_and_nebula_canonical.md` §a Tailscale + Nebula pluralism; §b `transport.substrates[]` manifest field; §c 5 MEMBERSHIP_* + 4 CONNECTION_* event-type candidates + `substrate_kind` discriminator; §d per-substrate identity schema extending ADR-013 §b; §e canonical-pilot-tier doctrine; §j non-pilot-tier defaults. Cited from §4.3 / §4.4 / §5.3 / §5.4 / §6.6.
- `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_003_assumes_draft_true_pattern.md` rule 2 inheritance-not-duplication. Cited from §6.3 (assumes_draft annotation).

**III ADRs**:
- `what/decisions/adr_002_consumer_federation_contract.md` §3 minor-bump consumer-review trigger. Cited from §4.5 (advisory-mode promotion is a per-wrapper minor-bump decision).
- `what/decisions/adr_003_learning_store_canonicalization_protocol.md` rule 2 (`assumes_draft: true` inheritance pattern; ADR-014 §e + ADR-015 §c carry forward). Cited from §6.3.

**Schema**:
- `what/artifacts/iii_airlock_request_schema.yaml` lines 237-271 — `secrets_handled` block definition (cited from §2).
- `what/artifacts/iii_airlock_request_schema.yaml` lines 273-292 — `idempotency_key` + `force_new` block definitions (cited from §3).
- v0.3 schema additions for `federation_signature` + `federation_signature_key_version` are documented in spec §4.3 (payload schema) and added to the request schema YAML at the MD-A2 minor-bump sweep (MD-A4 carry-forward; schema YAML untouched at MD-A2 per artifact-only mission disposition).

**Reply-comment template**:
- `how/templates/template_cross_vault_request_reply.md` line 38 — Secret presence slot (§7 above).
- `how/templates/template_cross_vault_request_reply.md` line 39 — Idempotency check slot (§7 above).
- `how/templates/template_cross_vault_request_reply.md` Rejection block line 60 — `reason:` enum **extended at MD-A2** with `signature_verification_failed: <subreason>` (§7 above).
- `how/templates/template_cross_vault_request_reply.md` Rejection example block — NEW at MD-A2 — demonstrates Ed25519 rejection body shape (§7 above).

**Reference instance**:
- `how/airlock/AIRLOCK.md` — Cross-Vault Requests § "Executable substrate enforcement" deferral. Patch-level edit at MC-4 close points readers at §2 + §3; an MD-A4 follow-up edit will point at §4 + §5 + §6 once the wrapper minor-bump sweep flips downstream activation.

**Audit-log scheme** (v0.3 — single `audit_<YYYY-MM>.jsonl` file carries 5 `event_type` values):
- `secrets_preflight` — §2
- `idempotency_check` — §3.6
- `federation_sigverify` — §4.7
- `ledger_observation` — §5.6
- `compliance_audit_emit` — §6.5

**Sidecar files** (v0.3 — receiver-local; never committed):
- `<receiver_vault>/who/coordination/.audit/audit_<YYYY-MM>.jsonl` — multi-event-type audit log (5 types).
- `<receiver_vault>/who/coordination/.idempotency_index.jsonl` (optional; O(1K)+ memos) — §3.3.
- `<receiver_vault>/who/coordination/.audit/compliance_audit_index.jsonl` (optional; high-volume receivers) — §6.4.
- `<receiver_vault>/who/coordination/.airlock/ledger_cursor` — §5.4 polling cursor.
- `<receiver_vault>/who/coordination/.airlock/federation_peers.yaml` (optional) — §5.2 declared peers list.
- `<receiver_vault>/who/coordination/.airlock/federation_pubkeys.yaml` (optional; pre-federation-deployment fallback) — §4.3.

**Campaign**:
- `how/campaigns/campaign_c_airlock_standard/campaign_c_airlock_standard.md` — MC-4 mission row + DG-C Criteria checklist (v0.2 origin).
- `how/campaigns/campaign_d_federation_adaptive_loop/campaign_d_federation_adaptive_loop.md` — MD-A2 mission row + DG-D Criteria checklist (v0.3 origin).

**Out-of-scope / deferred**:
- Executable preflight runner — Platform.aDNA scope; first integration in a future RareHarness / VAASLattice consumer.
- Cross-vault audit-log federation — multi-vault audit aggregation deferred beyond v0.3.
- Persistent idempotency cache backed by SQLite / DBM — deferred; the optional `.idempotency_index.jsonl` per §3.3 covers O(10K) without a database.
- **Pub/sub subscription model for §5** — deferred to v0.4+ when LN ships a broker (per ADR-015 §k spec-impact + amendment path).
- **Native `LedgerEventType` enum membership for `COMPLIANCE_AUDIT`** — deferred to LN Carry 3 EP-1 ratification at Phase 5 integration seam (per ADR-014 §e + ADR-015 §c cluster amendment).
- **Body-section Ed25519 signing** — airlock v0.4 per spec §8.2 (v0.3 signs frontmatter only).
- **Multi-operator attestation for `COMPLIANCE_AUDIT`** — ADR-013 v0.2 per ADR-013 §a rotation policy (v0.3 supports single-operator attestation per ADR-013 §a).
- **Schema YAML additions for `federation_signature` / `federation_signature_key_version`** — deferred to MD-A4 wrapper minor-bump sweep (schema lives at `what/artifacts/iii_airlock_request_schema.yaml`; untouched at MD-A2 per artifact-only mission).
- **End-to-end federation-integration validation** — deferred to MD-A5 (re-exercise VideoForge → CanvasForge or wga_l1 → stanley_l1 with Ed25519 + ledger observation active over live LN substrate).
- **Cross-vault RLHF aggregation over §5 ledger observation** — deferred to MD-B3 per Campaign D charter §Critical Path ("MD-B3 cannot ship before MD-A1 v0.3 spec exists" + extended at MD-A2 close to "before MD-A2 v0.3 implementation guidance exists").

**Authoring provenance**:
- **v0.2 origin**: Mission `MC-4` of Campaign C: Airlock Standard v0.2 (`campaign_c_airlock_standard`). Closed: 2026-05-13 disk / 2026-05-20 git retroactive `c8ce621`.
- **v0.3 origin**: Mission `MD-A2` of Campaign D: Federation-Aware Airlock v0.3 + RLHF/Adaptive Loop (`campaign_d_federation_adaptive_loop`). Closed: 2026-05-21.
- No git tag bump at MD-A2 close (additive disposition; mirrors MB-6/MB-7/MB-8/MC-4/DG-B/MC-5/DG-C/MD-B1/MD-B2/MD-A1). `v0.2.0` (commit `246124d`, annotated tag object `5cd210e`) remains the production pin. `v0.3.0` (or `v1.0.0`) tag deferred to DG-D close per Campaign B+C precedent.
- Canonical learning store md5 invariance: `iii_corrections_canonical.jsonl` md5 = `5adb0dfa38d9224649c3b2cba83852ae` (preserved across MD-A2 close; first canonical md5 rotation since founding C-001..C-026 import occurred at MD-B2 close 2026-05-21 from `dde2cbd88c0b45956fb22285a2a0f856` to current).
- LN substrate version pin: LN.aDNA pc_01 Phase A close + Phase B1 close — ADR-014 v0.1 + ADR-015 v0.1 (both accepted 2026-05-20). Dual-substrate Tailscale + Nebula federation operational.
