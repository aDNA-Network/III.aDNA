---
type: standard_implementation_guidance
version: "0.2.0"
status: reference_implementation
created: 2026-05-13
updated: 2026-05-13
last_edited_by: agent_argus
authoring_mission: campaign_c_airlock_standard MC-4
governed_by: what/artifacts/iii_airlock_standard_spec.md
resolves_forward_references:
  - "spec §4.4 line 261 (secrets_handled implementation guidance)"
  - "spec §4.5 line 282 (idempotency_key implementation guidance)"
  - "how/airlock/AIRLOCK.md line 258 (Executable substrate enforcement deferral)"
ships_with_spec_version: "0.2.0"
documentation_grade: true
non_runnable: true
tags: [airlock, substrate, secrets, idempotency, audit_log, implementation_guidance, v0_2, mc4]
---

# Airlock Substrate Implementation Guidance (v0.2.0)

> **Companion document** to `what/artifacts/iii_airlock_standard_spec.md`. The spec carries the **normative contracts**; this artifact carries the **implementation guidance** that resolves the spec's forward-references at §4.4 and §4.5, plus the AIRLOCK.md reference instance's parallel forward-reference at line 258.

## §1 Purpose & Scope

This artifact is the canonical answer to "how does a receiver implement spec §4.4 + §4.5?" It is **documentation-grade per Campaign C Risk R3**: there is no executable runtime in v0.2. The first executable enforcement lands when a Platform.aDNA integration (e.g., RareHarness, VAASLattice) consumes the airlock standard at runtime.

**Authoring rule** (per MC-4 Plan-agent C carve-out):
- **Pseudocode the scripts.** Preflight + dedup flows are sketched in language-agnostic pseudocode plus illustrative bash + python fragments. The fragments are not runnable; an implementer hand-translates them when wiring an executable substrate.
- **Schema the data shapes.** Audit-log records, optional idempotency-index records, and the receiver's reply-comment fill-in formats are **fully specified** — every field, every type, every enum value. This matches MC-2 precedent (shipped a Draft 2020-12 JSON Schema even though no validator runs in III.aDNA).

**Boundary discipline**: this artifact MUST NOT amend the normative contract at spec §4.4 + §4.5. If a guidance recommendation conflicts with the spec prose, the **spec wins**. Conflicts are bugs in this artifact and resolve via patch-level edits here, not via spec rewrite.

**Three forward-references resolved**:
1. Spec §4.4 line 261: "Implementation guidance (preflight script structure, error-message conventions, audit-log schema): authored at MC-4" → §2 below.
2. Spec §4.5 line 282: "Implementation guidance (cache structure, 30-day window mechanics, archive-search performance): authored at MC-4" → §3 below.
3. AIRLOCK.md line 258: "MC-4 will author implementation guidance" → §2 + §3 below (cited from AIRLOCK.md at MC-4 close via patch-level edit).

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

**Rejection reason format** — the receiver replies via the reply-comment template's Rejection block (shipped at MC-3 in `how/templates/template_cross_vault_request_reply.md`). The 7-value Rejection `reason:` enum already defined by MC-3 includes `missing_secret`; this guidance fills in the canonical body shape:

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

The audit log is **receiver-local evidence**. Cross-vault federation of audit logs is OUT OF SCOPE for v0.2 (deferred indefinitely; see §5).

**Single audit file is dual-purpose** — it carries both `secrets_preflight` events (§2) and `idempotency_check` events (§3.6). The `event_type` discriminator field distinguishes them.

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

- `requesting_vault: videoforge` + `vault: canvasforge` — matches the MC-5 re-exercise target (VideoForge → CanvasForge per spec §5).
- `request_path` references a hypothetical `coord_2026_05_13_videoforge_requests_carly_herb_deck_v2.md` — when MC-5 authors the v0.2-conformant request memo, the audit emission can use these exact dimensions without retrofit.
- `secrets_required: ["ANTHROPIC_API_KEY"]` — matches the worked-example pipeline (deck render uses local Anthropic API key only; no cross-vault secret delegation per spec §5.2 additive delta 1).

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

Where `K` = `current_request.frontmatter.idempotency_key` and `V` = `current_request.frontmatter.requesting_vault`.

**Pseudocode walk**:

```
INPUT: current_request_frontmatter, receiver_vault_coordination_dir
STEP 1: K ← current_request_frontmatter.idempotency_key
        V ← current_request_frontmatter.requesting_vault
        force_new ← current_request_frontmatter.get("force_new", false)
        IF K is absent: RETURN { outcome: "no_key", proceed: true }
STEP 2: candidates ← glob(receiver_vault_coordination_dir + "/coord_*.md")
STEP 3: for each path in candidates:
            fm ← parse_frontmatter(path)
            if fm.idempotency_key == K and fm.requesting_vault == V:
                if not_self(path, current_request) and in_window(fm.status, fm.updated):
                    emit_audit_record(outcome="duplicate_of", duplicate_of=path, ...)
                    RETURN { outcome: "duplicate_of", existing_memo: path, prior_audit_id: fm.audit_id }
STEP 4: emit_audit_record(outcome="no_match", ...)
        RETURN { outcome: "no_match" }
```

The `in_window` predicate implements §3.2; `not_self` excludes the just-filed memo from the match set (the dedup search happens after the memo is on disk but before its `status` flips out of `open`).

### §3.2 30-day window semantics

Spec §4.5 step 1 specifies "active + last 30 days closed/shipped". This guidance pins the semantics:

**Window scope**:
- **Active statuses** (always in dedup window): `open`, `accepted`, `rendering`, `shipped`.
- **Terminal statuses** (in dedup window only if `updated >= now - 30d`): `closed`, `cancelled`.
- **Not in window at any time**: `duplicate_of` — a memo with this status was itself a duplicate-rejection of a prior request; including it in dedup search would chain rejections artificially.

**Calendar-day, not business-day**. `now - 30d` is a wall-clock subtraction. UTC reference. A receiver in a non-UTC timezone normalizes timestamps to UTC for comparison.

**Clock source** (in order of precedence):
1. `m.frontmatter.updated` if present and parseable as ISO 8601 / RFC 3339.
2. `m.frontmatter.created` if `updated` is absent.
3. Filesystem mtime if both frontmatter fields are absent (graceful fallback for malformed memos; rare).
4. Git commit date of the memo's last touching commit (final fallback; useful for vaults that auto-update frontmatter via hooks).

An implementer SHOULD warn (audit log entry with `event_type: idempotency_check` + a `clock_source_fallback: <2|3|4>` field) when falling back below precedence 1. The audit-log schema in §3.6 accommodates an optional `clock_source_fallback` field.

### §3.3 Cache structure

**The existing `who/coordination/` directory IS the cache.** No separate state file is required for vaults with O(100) memos. The acceptance-time linear scan is O(n) in coord-memo count and completes in sub-second time on local disk (see §3.4 performance profile).

**Optional index** — for vaults with O(1K)+ memos, an `<receiver_vault>/who/coordination/.idempotency_index.jsonl` MAY be maintained. The index is:
- **Append-only** (one record per memo write; receiver pipeline writes a single line at memo creation + memo-status-flip events).
- **Rebuildable** (a `rebuild_idempotency_index.sh`-style script walks all `coord_*.md` files, parses frontmatter, and writes a fresh index). Loss of the index never costs evidence — only acceptance-time latency.
- **Gitignored by default** (per-vault local infrastructure; matches the `.audit/` convention from §2.3).

**Index record schema** (one JSON object per line):

```jsonl
{
  "idempotency_key": "<key string>",
  "requesting_vault": "<short name>",
  "audit_id": "<UUID or vault session id>",
  "request_path": "<vault-relative path>",
  "status": "<open | accepted | rendering | shipped | closed | cancelled | duplicate_of>",
  "updated": "<RFC 3339 timestamp>",
  "indexed_at": "<RFC 3339 timestamp; when the index line was written>"
}
```

**Field rules**:
- `idempotency_key`: copy from `m.frontmatter.idempotency_key`. Absent memos do not appear in the index.
- `requesting_vault`: from `m.frontmatter.requesting_vault`.
- `audit_id`: from `m.frontmatter.audit_id`.
- `request_path`: vault-relative.
- `status`: enum matching spec lifecycle. Status-change events emit a fresh index line; the index search returns the most-recent line per `(idempotency_key, requesting_vault, request_path)` triple.
- `updated`: from `m.frontmatter.updated`.
- `indexed_at`: receiver-allocated at index-write time.

`.gitignore` recommendation:

```gitignore
# Airlock idempotency index — per-vault, rebuildable, never committed
who/coordination/.idempotency_index.jsonl
```

### §3.4 Performance profile

| Coord-memo count | Recommended approach | Expected lookup latency |
|------------------|----------------------|-------------------------|
| 1–100 | Linear scan over `who/coordination/coord_*.md` | < 50ms on local SSD |
| 100–1000 | Linear scan acceptable | 50–200ms on local SSD |
| 1000–10000 | Add `.idempotency_index.jsonl` per §3.3 | < 50ms with index; 1–10s without |
| 10000+ | Index mandatory; consider SQLite if grep performance degrades | < 50ms with index |

**SLA target for the acceptance-time dedup lookup**: ≤ 200ms at any reasonable scale. The lookup runs once per acceptance; throughput is not a concern (acceptance is a human-paced ceremony). The SLA exists to keep receiver acceptance responsive — a multi-second pause would degrade the reply-comment authoring flow.

**Implementation notes**:
- Frontmatter parsing dominates linear-scan cost. Use a fast YAML parser (`ruamel.yaml`, `js-yaml`, or `grep`-based pre-filter on `idempotency_key:` followed by yaml parse) rather than loading and parsing each entire memo body.
- Pre-filter with `grep -l "idempotency_key:" who/coordination/coord_*.md` before parsing — typical hit ratio is < 10% (most coord memos are non-airlock, no idempotency key).
- The index in §3.3 trades disk-write cost (one line per memo write) for lookup speed. The trade-off pays off at O(1K)+ memos.

### §3.5 Force-new override semantics

Spec §4.5 step 3 allows `force_new: true` to bypass dedup. This guidance pins the receiver-side handling:

**On `force_new: true` AND a match would have been found**:
- The receiver MUST emit an `event_type: idempotency_check` audit record with `outcome: bypassed_force_new` and `duplicate_of: <path of the would-have-matched memo>`. This makes the bypass *observable* — a future audit-log review can detect intentional duplicates.
- The receiver MUST proceed with normal acceptance (open a fresh session, allocate pipeline resources, etc.). The duplicate is logged but not blocked.

**On `force_new: true` AND no match found**:
- The receiver MUST emit an audit record with `outcome: no_match_force_new_set`. The `force_new` declaration is still observable even when it was unnecessary.

**Reply-comment template fill-in** — the Acceptance fenced block's **Idempotency check** slot (line 39 in the template) takes one of:
- `**Idempotency check**: no match` — clean acceptance.
- `**Idempotency check**: duplicate_of: <existing_memo_path>; prior audit_id <id>` — match found; new memo flips to `status: cancelled`.
- `**Idempotency check**: bypassed (force_new: true) — would have matched duplicate_of: <path>; prior audit_id <id>` — bypass observed.
- `**Idempotency check**: no key declared` — request omitted `idempotency_key`; behaviour matches v0.1.0 (every memo independent).

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

## §4 Reply-Comment Template Integration

The reply-comment template at `how/templates/template_cross_vault_request_reply.md` (shipped at MC-3 2026-05-10) has two slots already wired to consume the outputs of §2 + §3:

**Acceptance fenced block (template line 38)**:
```
- **Secret presence** (if `secrets_handled.needed` declared): <list of confirmed-present names — never values>
```

Filled per §2.2 (Acceptance reason format). Receiver writes the comma-separated list of names from `§2.3 audit record.secrets_present` (which matches `secrets_required` exactly on the accepted branch).

**Acceptance fenced block (template line 39)**:
```
- **Idempotency check** (if `idempotency_key` declared): <"no match" | "duplicate_of: <existing_memo_path>; prior audit_id <id>">
```

Filled per §3.5 (reply-comment template fill-in). Receiver writes one of the four canonical strings: `no match`, `duplicate_of: ...`, `bypassed (force_new: true) — would have matched duplicate_of: ...`, or `no key declared`.

**Rejection fenced block (template line ~50)**:
The Rejection block's `reason:` enum already includes `missing_secret` per MC-3. Body shape per §2.2:

```
reason: missing_secret: <SECRET_NAME>
```

No template change is required at MC-4 — the MC-3 template already carries the slots and enum values this guidance fills in. This section is **documentation cross-reference**, not a contract change.

---

## §5 Cross-References

**Spec**:
- `what/artifacts/iii_airlock_standard_spec.md` §4.4 lines 240-263 — normative contract for `secrets_handled`. This artifact's §2 resolves the line-261 forward-reference.
- `what/artifacts/iii_airlock_standard_spec.md` §4.5 lines 265-285 — normative contract for `idempotency_key`. This artifact's §3 resolves the line-282 forward-reference.
- `what/artifacts/iii_airlock_standard_spec.md` §7.2 row 4 — flips ✅ at MC-4 close.
- `what/artifacts/iii_airlock_standard_spec.md` §7.3 forward-references list — MC-4 row resolves.

**Schema**:
- `what/artifacts/iii_airlock_request_schema.yaml` lines 237-271 — `secrets_handled` block definition (cited from §2).
- `what/artifacts/iii_airlock_request_schema.yaml` lines 273-292 — `idempotency_key` + `force_new` block definitions (cited from §3).

**Reply-comment template**:
- `how/templates/template_cross_vault_request_reply.md` line 38 — Secret presence slot (§4 above).
- `how/templates/template_cross_vault_request_reply.md` line 39 — Idempotency check slot (§4 above).
- `how/templates/template_cross_vault_request_reply.md` Rejection block — `reason:` enum including `missing_secret` (§4 above).

**Reference instance**:
- `how/airlock/AIRLOCK.md` line 258 — Cross-Vault Requests § "Executable substrate enforcement" deferral. Patch-level edit at MC-4 close points readers at this artifact's §2 + §3.

**Campaign**:
- `how/campaigns/campaign_c_airlock_standard/campaign_c_airlock_standard.md` — MC-4 mission row + DG-C Criteria checklist.

**Out-of-scope / deferred (see Campaign C R3)**:
- Executable preflight runner — Platform.aDNA scope; first integration in a future RareHarness / VAASLattice consumer.
- Cross-vault audit-log federation — multi-vault audit aggregation deferred beyond v0.2.
- Persistent idempotency cache backed by SQLite / DBM — deferred; the optional `.idempotency_index.jsonl` per §3.3 covers O(10K) without a database.

**Authoring provenance**:
- Mission `MC-4` of Campaign C: Airlock Standard v0.2 (`campaign_c_airlock_standard`).
- Closed: 2026-05-13.
- No git tag bump at MC-4 close (additive disposition; mirrors MB-6/MB-7/MB-8). `v0.2.0` (commit `246124d`, annotated tag object `5cd210e`) remains the production pin. `v0.2.1` reserved for DG-C close after MC-5.
- Canonical learning store md5 invariance: `iii_corrections_canonical.jsonl` md5 = `dde2cbd88c0b45956fb22285a2a0f856` (preserved across MC-4 close).
