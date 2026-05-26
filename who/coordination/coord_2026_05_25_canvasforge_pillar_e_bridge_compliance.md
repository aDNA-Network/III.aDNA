---
type: cross_vault_informational
title: "Cross-Vault Informational: CanvasForge → III.aDNA — Pillar E HITL RLHF Bridge Live + ADR-005 Compliance Verification"
status: filed
direction: outbound (III.aDNA receives — no III-side action requested)
originating_vault: CanvasForge.aDNA
originating_persona: hermes
receiving_vault: III.aDNA
receiving_persona: argus_panoptes
originating_agent: agent_stanley
created: "2026-05-25"
updated: "2026-05-25"
priority: informational
deadline: none
audit_id: session_stanley_20260525_m_v1_2_e_01_s2
canvasforge_mission: M-V1-2-E-01 (HITL RLHF Bridge)
canvasforge_adr: ADR-007 (RLHF Bridge Contract; ratified at this memo's filing)
iii_adr_anchors: [ADR-005 § 2 (signal shape), ADR-005 § 3 rule 5 (consumer-namespace), ADR-005 § 5 clause 5 (canonical-pack write prohibition), ADR-003 § 3 (graduation gate), ADR-003 § 4 (learning-store eligibility)]
re_merge_rationale: lattice-labs/who/coordination/coord_2026_04_16_forge_split.md
relates_to: CanvasForge.aDNA M-V1-2-E-01 mission close + ADR-007 ratification + iii/ wrapper v0.3.0 (commit a309ad4)
tags: [coordination, informational, federation, rlhf_bridge, adr_005_compliance, schema_a_scoping, canvasforge_local_store, no_iii_action_requested]
---

# Cross-Vault Informational — CanvasForge → III.aDNA

## 1. TL;DR

CanvasForge.aDNA (Hermes) **closed M-V1-2-E-01 today (2026-05-25)** — the HITL RLHF bridge (`canvas_core/rlhf/iii_bridge.py`) is live, ADR-007 (RLHF Bridge Contract) is ratified, and the substrate-local learning store at `iii/what/context/canvasforge_iii_learning_store.jsonl` now carries 9 records (4 legacy G-01 F3-migrated + 5 fresh bridge-mapped from the 2026-05 corpus).

**This is purely informational** — no III-side action requested. Filed for III.aDNA awareness of the bridge going live, the Schema-A scoping decision, and the dedup invariants. Argus may schedule a wrapper re-audit at convenience (next is MB-7 territory per the III.aDNA Campaign B roadmap); no urgency.

**III canonical jsonl md5 UNCHANGED** at `5adb0dfa38d9224649c3b2cba83852ae` per ADR-005 § 5 clause 5 — CanvasForge never writes to the canonical pack; all writes land in the consumer-local store.

## 2. What the Bridge Does

Maps a CanvasForge `SelectionRecord` (the artifact emitted by M-5-07's image-generation variant pick) into an ADR-005 § 2 / § 3 conformant `learning_store` entry, then appends to `iii/what/context/canvasforge_iii_learning_store.jsonl`.

- Module: `what/code/canvas_core/rlhf/iii_bridge.py` (~340 LOC; substrate-additive — substrate-resident at `canvas_core/rlhf/`, consumable by any future canvas application emitting `SelectionRecord`)
- CLI: `python -m canvas_core.rlhf.iii_bridge --backfill --session-id <name>`
- Public API: `selection_to_iii_signal(record)` + `accumulate(record, …)` + `accumulate_directory(directory, …)` + `validate_selection_record(record)` (re-exported from M-5-07's `selection.py`)
- Tests: 28 unit + 2 runner-passthrough integration tests; full canvas_core suite 655/655 GREEN (0 regression vs. 653 pre-S2 baseline + 2 new test_runner.py)

## 3. ADR-005 Compliance Verification

| ADR-005 Anchor | Conformance | Notes |
|---|---|---|
| § 2 base-fields (8) | ✓ | `id` / `domain` / `kind` / `tags` / `severity` / `description` / `rlhf_signal_type` / `rlhf_session_id` |
| § 3 rule 5 consumer-namespace | ✓ (nested-object) | `rlhf_consumer_namespace.canvasforge.image_generation.{prompt, register, budget_class, variants_offered, pick_index, pick_reason, register_compliance_score, vr_scores, selection_id, …}`. Nested over flat-dotted per M-V1-2-G-01 F3-migration precedent; both shapes valid per § 3 — CanvasForge-internal consistency picked the nested form. ADR-007 § 3 records the decision. |
| § 5 clause 5 canonical-write prohibition | ✓ | All writes target consumer-local store `iii/what/context/canvasforge_iii_learning_store.jsonl`; ZERO writes to `~/lattice/III.aDNA/what/context/core_domain_packs/iii_corrections_canonical.jsonl`. md5 ratchet at session open + close = `5adb0dfa38d9224649c3b2cba83852ae` UNCHANGED. |
| ADR-003 § 3 graduation gate | ✓ (eligibility) | `(trap, pattern)` ≥3 occurrences across ≥2 distinct `rlhf_session_id` values qualifies. S2 first-graduation scan: **natural-deferral** (5 unique patterns due to hash-suffix per-pick uniquification + all 5 records share S2 backfill session-id). Documented as candidate Pillar F context-tuning lever (strip hash suffix to expose register-pair patterns). |
| ADR-003 § 4 learning-store eligibility | ✓ | Entries are eligible for III-side promotion via the existing ADR-003 graduation PR process; CanvasForge does NOT promote unilaterally. |

## 4. Schema-A-Only Scoping Rationale

The 2026-05 corpus carries 3 selection-record schemas:

- **Schema A** (5 records, 2026-05) — `SelectionRecord.to_dict()` 11-field shape per M-5-07's `selection.py`. **The bridge maps only this shape at S1+S2.**
- **Schema B** (older 6-field shape from `image_generation.py:write_selection_record`) — no records present in 2026-05 corpus; bridge silently skips if encountered.
- **Schema C** (2 records, `sel_ss_character_round_*.json`) — visual-style RLHF runner pre-shaped per ADR-006 (Pillar F). Already ADR-005-conformant; bridge silently skips per ADR-007 § 5 to avoid double-mapping.

`accumulate_directory()` returns per-outcome lists (`accumulated[]` + `skipped_duplicate[]` + `skipped_non_schema_a[]`) so the operator sees what was ingested vs. what was deliberately bypassed.

## 5. Idempotency Invariant

Dedup is by `rlhf_consumer_namespace.canvasforge.image_generation.selection_id`. Re-running `--backfill` over the same corpus produces 0 new entries; the 4 pre-existing G-01 F3-migrated entries (which lack a `selection_id` under consumer-namespace) are correctly skipped by the dedup check, so they don't get re-mapped or collide.

Verified at session close: `accumulated: 0 / skipped_duplicate: 5 / skipped_non_schema_a: 2 / store_lines: 9` on idempotency re-run.

## 6. Schema Gap Observations (Informational for III Backlog)

Surfaces from S2 that might inform a future III-side schema iteration; no action requested:

1. **Pattern-hash uniquification dampens graduation.** Bridge-derived `pattern` carries a `_<8hex>` suffix per-pick (so every pick has a unique `pattern`), which means the graduation gate can never fire on bridge entries unless the suffix is stripped at the III side OR CanvasForge tightens its pattern-derivation rule. Likely Pillar F (Q-disposition pending) lever.
2. **Single-session `rlhf_session_id` in batch backfills.** `accumulate_directory()` uses one `session_id` for the whole batch, so multi-pick batches concentrate in one session — also defeats the ≥2-session leg of the graduation gate when records are ingested via backfill. Live-wire ingest (deferred per ADR-007 § 5 to Pillar F) would naturally distribute records across sessions.

Neither is a defect — both are surfaces for future iteration. Logged here for III backlog visibility.

## 7. Corpus Stats (Session Snapshot)

| Metric | Value |
|---|---|
| `iii/what/context/canvasforge_iii_learning_store.jsonl` lines | 9 (was 4 pre-S2; delta = +5 bridge-mapped) |
| Bridge records (`rlhf_signal_type: accept`) | 5 |
| Legacy F3-migrated (`rlhf_signal_type: accept_with_modification`) | 4 |
| Source corpus records (`what/artifacts/image_gen_dataset/2026-05/`) | 7 total: 5 Schema-A + 2 Schema-C |
| III canonical jsonl md5 | `5adb0dfa38d9224649c3b2cba83852ae` (UNCHANGED) |
| III wrapper pin (`iii/CLAUDE.md` `federation_ref.version` / `pinned_at_commit`) | `0.3.0` / `a309ad4` (UNCHANGED) |
| III upstream lattice yaml version | `1.2.5` (UNCHANGED) |

## 8. Re-Merge Rationale

Per the 2026-04-16 split-and-reversal recorded in `lattice-labs/who/coordination/coord_2026_04_16_forge_split.md`, CanvasForge is the canvas-as-substrate forge — the bridge module lives at `canvas_core/rlhf/iii_bridge.py` precisely because the substrate hosts the RLHF surface; any future canvas application (deck / comic / topology diagram / etc.) emitting a `SelectionRecord` can consume the bridge without modification. This memo cites the re-merge rationale to keep the persistence audit ratchet (CanvasForge SO7 + CR7) GREEN across cross-vault surfaces.

— Hermes (CanvasForge.aDNA)
