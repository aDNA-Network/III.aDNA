---
type: skill
lattice_type: skill
skill_type: agent
created: 2026-05-22
updated: 2026-05-22
mission_origin: campaign_d_federation_adaptive_loop MD-A3
status: active
category: setup
trigger: "A consumer vault is moving its `how/airlock/AIRLOCK.md` from inactive stub to active reference instance, OR an existing v0.2-active AIRLOCK.md is bumping to v0.3 federation-substrate awareness. Operator/agent invokes when they say 'activate the airlock', 'bump the airlock to v0.3', 'wire federation-aware airlock', 'move airlock stub to active', or when MD-A4 wrapper-sweep applies the kit across the 6 live consumer wrappers."
last_edited_by: agent_argus
tags: [skill, airlock, activation, federation_aware, v0_3_bump, ed25519, ledger_observation, compliance_audit, consumer_wrapper, verification_harness]
federation:
  discoverable: true
  source_instance: III.aDNA
  version_policy: minor
fair:
  keywords: [airlock, activation, federation, ed25519, ledger_observation, consumer_wrapper, verification]
  license: Apache-2.0
---

# Skill: AIRLOCK Activation (v0.0 stub → v0.3.0 active OR v0.2.0 → v0.3.0 upgrade)

## Overview

Packages the recipe + verification checklist consumer vaults need to move a `how/airlock/AIRLOCK.md` from inactive stub to active reference instance, OR to bump an existing v0.2 active instance to v0.3 federation-substrate awareness. This skill is the operational complement to:

- `skill_iii_setup.md` — bootstraps the `iii/` consumer wrapper (federation_ref + local extensions + Standing Order). Runs once per vault.
- `skill_iii_review.md` — runs the actual review loop. Loaded by `iii/CLAUDE.md`.
- **This skill** — wires the consumer-side AIRLOCK.md so external agents entering the consumer vault can self-route through the airlock pattern.

The activation kit is **documentation-grade per Campaign C R3 + MD-A2 precedent**: no executable runtime, no scripts to run. The skill walks an operator/agent through the manual edits + verification checklist that constitute activation. An executable activation harness is deferred to a future III.aDNA v0.4+ or Platform.aDNA integration.

**Canonical contracts referenced**:
- Airlock standard spec v0.3.0: `~/aDNA/III.aDNA/what/artifacts/iii_airlock_standard_spec.md`
- Substrate implementation guidance v0.3.0: `~/aDNA/III.aDNA/what/artifacts/iii_airlock_substrate_implementation.md`
- Reference instance (the model the kit teaches consumers to mirror): `~/aDNA/III.aDNA/how/airlock/AIRLOCK.md` (v0.3.0 post-MD-A3)
- Reply-comment template (extended at MD-A2 with Ed25519 rejection sub-reasons): `~/aDNA/III.aDNA/how/templates/template_cross_vault_request_reply.md`
- Federation contracts: ADR-002 §3 (consumer minor-bump review); ADR-003 rule 2 (assumes_draft inheritance); LN ADR-013 §b (per-purpose-slot dict); LN ADR-014 §c (canonical-JSON algorithm); LN ADR-015 §b/§d/§e (substrate pluralism + `transport.substrates[]`)

## When to Use

1. **A consumer vault is activating its airlock stub for the first time** (v0.0 → v0.3.0). Examples from the 9-stub ecosystem inventory at MC-4.5 §3.6: LatticeLabs.aDNA, LatticeNetwork.aDNA, LatticeAgent.aDNA, LatticeTerminal.aDNA, SpeechForge.aDNA, MoleculeForge.aDNA, VideoForge.aDNA, `.adna` template, node.aDNA. All currently inactive per ADR-008 design (activation operator-discretionary).
2. **An existing v0.2 active AIRLOCK.md is bumping to v0.3** federation-substrate awareness (the MD-A4 wrapper-sweep case). The only currently-active reference instance is III.aDNA's own `how/airlock/AIRLOCK.md` (was v0.2.0 pre-MD-A3; v0.3.0 post-MD-A3); consumer wrappers' AIRLOCK.md files activate via this skill.
3. **A federation pilot needs Ed25519 + ledger-observation hooks wired** at the airlock surface (paired with a federation_ref pin at v0.3+ in the consumer's `iii/CLAUDE.md`). Day-1 federation pilot candidates per MD-A1 charter: wga_l1 (in LN.aDNA federation; iii/ wrapper v0.2.0); LL.aDNA + LN.aDNA via 2 outbound coord memos fired at Campaign D charter.

Do NOT use this skill to:
- Author the `iii/` consumer wrapper itself (use `skill_iii_setup.md` instead — that runs first; this skill assumes the wrapper exists).
- Activate an airlock in a vault that is not a III consumer (no `iii/` wrapper present). The airlock pattern is III.aDNA-anchored; non-consumers should adopt `skill_iii_setup.md` first.
- Promote advisory-mode Ed25519 verification to enforce-mode across all federation contexts — that is a major bump per spec §7.1 (advisory → mandatory promotion of Ed25519 verification is a v0.x → v1.0 breaking change requiring explicit human decision at every consumer).
- Bump an AIRLOCK.md to a hypothetical v0.4+ (no v0.4 spec exists; this kit is bound to v0.3).

## Prerequisites

- The consumer vault has an `iii/` wrapper at `<vault>/iii/CLAUDE.md` (created via `skill_iii_setup.md`).
- The consumer vault has a `how/airlock/AIRLOCK.md` stub (either fresh-fork from `.adna` template OR a v0.2 active instance ready for bump).
- The III.aDNA upstream is reachable at `~/aDNA/III.aDNA/` (or via the GitHub remote `https://github.com/LatticeProtocol/III.aDNA`) at a commit that ships the v0.3.0 airlock spec + impl-doc (post-MD-A1 + MD-A2; reference instance bumps at MD-A3).
- The operator can decide whether the consumer will participate in federation observation (opt-in per spec §5.3) OR remain substrate-naïve (v0.3 still applies; §5 hooks just stay inert).
- For federation-active consumers: LN.aDNA `pc_01` Phase A close + Phase B1 close are live (Ed25519 keypair + bore.pub:42561 tunnel + ADR-014/-015 accepted 2026-05-20); the consumer's federation signing pubkey is registered in the LN membership manifest at `transport.substrates[*].identity.federation_signing_pubkey_sha256` per ADR-013 §b + ADR-015 §b/§d.

## Inputs / Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `consumer_vault_path` | yes | Absolute path to consumer vault root (e.g., `~/aDNA/wga.aDNA/`) |
| `airlock_path` | yes | Path to the AIRLOCK.md being activated/bumped (e.g., `<vault>/how/airlock/AIRLOCK.md`) |
| `mode` | yes | `activate` (v0.0 stub → v0.3.0) OR `upgrade` (v0.2.0 → v0.3.0). Picks Step 3 (activation) vs Step 4 (upgrade) below. |
| `federation_mode` | yes | `enforce` (require Ed25519 verification at request acceptance) OR `advisory` (warn-on-failure; v0.2-compatibility profile per spec §4.6) OR `inactive` (consumer is not federated; §5 hooks stay dormant) |
| `iii_version` | yes | III.aDNA version the consumer's `iii/` wrapper pins (must be v0.3.0+ for federation-aware activation; v0.2.x pins activate at v0.2 contract only). |
| `ln_substrate_pin` | required when `federation_mode != inactive` | LN substrate version pin for §5 hooks (e.g., "LN.aDNA pc_01 Phase A close + Phase B1 close — ADR-014 v0.1 + ADR-015 v0.1") |
| `ledger_observation` | recommended | `enabled` (consumer participates in LN ledger observation per spec §5.3) OR `disabled` (consumer accepts requests but does not observe ledger events; valid v0.3 profile per spec §5.3 "opt-in per wrapper") |
| `mission_origin` | optional | If invoked from a campaign mission (e.g., MD-A4 wrapper-sweep), record the mission id for provenance. |

## Procedure

### Step 1 — Verify prerequisites and choose mode

1. Confirm the `iii/` wrapper exists at `<vault>/iii/CLAUDE.md`. If absent, abort and route to `skill_iii_setup.md` first.
2. Confirm the `<vault>/how/airlock/AIRLOCK.md` file exists. If absent, the consumer has not yet forked from the `.adna` template; create a stub by copying `.adna/how/airlock/AIRLOCK.md` (the inactive default) to the consumer path.
3. Read the AIRLOCK.md frontmatter `version:` field. Decide the mode:
   - `version:` empty or absent OR `status: inactive` OR `status: stub` → **`activate`** (Step 3 below).
   - `version: "0.2.0"` AND `status: reference_implementation` (or equivalent active state) → **`upgrade`** (Step 4 below).
   - `version: "0.3.0"` already → activation already happened; this skill does not re-activate. Run the verification checklist (Step 6) to confirm; if drift is found, file a coord memo at III.aDNA per spec §4.
4. Decide `federation_mode`:
   - Is the consumer's home node registered in the LN membership manifest (`transport.substrates[*].identity.federation_signing_pubkey_sha256` present)? If YES, default to `advisory` (safe to begin); operator may flip to `enforce` after Day-N validation. If NO, set `inactive` (§5 hooks dormant; v0.3 still applies for the §1-§4 contract surface).
   - When in doubt, default to `advisory`. Promotion `advisory → enforce` is a per-wrapper minor-bump decision per spec §4.6 + ADR-002 §3.

### Step 2 — Pre-flight gating checklist (5 items)

Before any edits, verify all of these pass:

1. **III.aDNA version reachable** — `~/aDNA/III.aDNA/` exists (or remote reachable); `git -C ~/aDNA/III.aDNA tag | grep -q v0.2.0` returns 0 (production pin live); spec at `what/artifacts/iii_airlock_standard_spec.md` has `version: "0.3.0"` (MD-A1 close).
2. **Impl-doc available** — `what/artifacts/iii_airlock_substrate_implementation.md` has `version: "0.3.0"` (MD-A2 close); §4 + §5 + §6 present.
3. **LN substrate prerequisites (federation_mode != inactive)** — LN.aDNA `pc_01` Phase A + Phase B1 closed; ADR-014 + ADR-015 ratified; consumer's `canonical_id` resolves to a `transport.substrates[*].identity.federation_signing_pubkey_sha256` entry in the LN membership manifest.
4. **`iii/` wrapper version pin compatible** — consumer's `<vault>/iii/CLAUDE.md` has `federation_ref.version` ≥ `0.3.0` (or `>=0.2.0` with operator-acknowledged forward-pin to v0.3 at MD-A4 sweep close). If `<0.2.0`, abort and run `skill_iii_setup.md` upgrade flow first.
5. **Reply-comment template current** — `~/aDNA/III.aDNA/how/templates/template_cross_vault_request_reply.md` line 60 (rejection-reason enum) includes `signature_verification_failed:<pubkey_absent|pubkey_revoked|signature_mismatch|key_version_unknown|substrate_mismatch>` (added at MD-A2 close).

If any check fails, abort and resolve the failed prerequisite first. Do not partial-activate.

### Step 3 — Activation procedure (v0.0 stub → v0.3.0)

For consumers activating a fresh inactive stub. Seven steps; estimated 10-15 min total.

#### Step 3.1 — Frontmatter bump

Edit `<vault>/how/airlock/AIRLOCK.md` frontmatter:

```yaml
---
type: airlock
version: "0.3.0"                    # was empty or "0.0"
status: reference_implementation     # was "inactive" or "stub"
created: <YYYY-MM-DD>                # preserve if already set
updated: <YYYY-MM-DD>                # today
last_edited_by: agent_<id>
governed_by: ~/aDNA/III.aDNA/what/artifacts/iii_airlock_standard_spec.md
covers: [entry_paths, cross_vault_request_patterns, federation_substrate_awareness]   # v0.3 third surface
ln_substrate_version_pin: "<from input param ln_substrate_pin>"
federation_mode: <enforce | advisory | inactive>     # from input param
ledger_observation: <enabled | disabled>             # from input param (defaults disabled)
---
```

#### Step 3.2 — Author entry paths

Adopt the 5-path matrix from the III.aDNA reference instance (`~/aDNA/III.aDNA/how/airlock/AIRLOCK.md` §Entry Paths) verbatim where applicable; tailor when the consumer's modality footprint differs. The matrix is:

| Path | Profile | Use cases |
|------|---------|-----------|
| **A** — Text Improvement | Any reviewer of text artifacts | Whitepaper, ADR, mission/session file, AAR, prose doc |
| **B** — Web / Visual Improvement | SiteForge / VideoForge / CanvasForge frontend reviewer | Web page, build output, HTML deck, rendered visual |
| **C** — Code Improvement | Code reviewer | Source, `*.module.yaml`, `*.lattice.yaml`, scripts |
| **D** — Video / Multimedia Improvement | VideoForge agent | Video file, transcript, operation-catalog entry |
| **E** — Vault Maintenance | Vault custodian | Staleness, orphans, frontmatter drift, broken wikilinks |

For each path, include: agent profile, use cases, out-of-scope row, expected output triple (`Finding[]` → `Introspection[]` → `ImprovementTable + VerificationResult`), context budget estimate, **Load in order** list. Reuse III.aDNA's path text where the consumer's profile matches; trim or extend per consumer modality.

**Consumer-tailoring rule**: a consumer with no web modality (e.g., a pure-prose org-vault) drops Path B; a consumer with no code (e.g., a documentation-only vault) drops Path C. The minimum surviving path set is A + E (text + vault hygiene) — every consumer reviews text and every aDNA vault has hygiene needs.

#### Step 3.3 — Author cross-vault request section

Adopt the §Cross-Vault Requests section from the III.aDNA reference instance verbatim — the v0.2-introduced surface (lifecycle states, handshake profiles, secrets delegation, idempotency) is invariant across consumers. Only the §Worked Example reference path changes per consumer (point at a real coord memo in the consumer's own `who/coordination/` if one exists; otherwise point at III.aDNA's canonical worked example at `~/aDNA/CanvasForge.aDNA/who/coordination/coord_2026_05_08_videoforge_requests_carly_herb_deck.md`).

#### Step 3.4 — Author §Federation-Substrate Awareness section (v0.3 third surface)

Adopt a section pointing at the spec §5 contracts. Template:

```markdown
## Federation-Substrate Awareness

v0.3 acknowledges that cross-vault traffic now traverses a federation substrate (Tailscale + Nebula per LN ADR-015 §a). This airlock observes the substrate read-only and verifies Ed25519 signatures at request acceptance.

**Substrate-pluralism (§5.1)**. The contract layer is substrate-agnostic. Per-substrate identity lives in the node's `inventory_memberships.yaml` `transport.substrates[]` per LN ADR-015 §b; this airlock SHOULD treat that list as truth and SHOULD NOT cache per-substrate identity beyond a short TTL.

**Ed25519 signature verification (§4.6 + §5.2)**. Co-federated requests MUST carry a `federation_signature` + `federation_signature_key_version` pair (added at spec §4.3). This receiver:
- resolves the requesting persona's Ed25519 pubkey from the LN membership manifest (`transport.substrates[*].identity.federation_signing_pubkey_sha256` per ADR-013 §b)
- verifies the signature over the canonical-JSON serialization of the request frontmatter (sorted keys; NFC normalization; per ADR-014 §c)
- rejects with `signature_verification_failed:<subreason>` per the 5-value taxonomy (pubkey_absent, pubkey_revoked, signature_mismatch, key_version_unknown, substrate_mismatch)

Implementation guidance: `~/aDNA/III.aDNA/what/artifacts/iii_airlock_substrate_implementation.md` §4.

**Mode** — this airlock is in `<enforce | advisory | inactive>` mode (frontmatter `federation_mode`). Advisory mode warns on failure but does not reject; enforce mode rejects per §4.6. Promotion advisory → enforce is a per-wrapper minor-bump decision.

**Ledger observation (§5.3; opt-in)**. <If `ledger_observation: enabled`: This airlock observes LN ledger events read-only (MEMBERSHIP_*, NODE_RE_IDENTIFIED, CONNECTION_*) per spec §5.3 and impl-doc §5 (polling at 60s cadence recommended). Observed `MEMBERSHIP_CERT_REVOKED` events invalidate cached pubkeys.> <If `ledger_observation: disabled`: This airlock does not observe LN ledger events at present. Activation is opt-in per spec §5.3; flip frontmatter `ledger_observation: enabled` and follow impl-doc §5 to enable.>

**COMPLIANCE_AUDIT emission (§5.3.1; assumes_draft)**. <If `federation_mode != inactive`: This airlock emits `COMPLIANCE_AUDIT` events to the LN ledger at request acceptance/rejection per spec §5.3.1; emission uses the `assumes_draft` direct-JSON-write workaround per impl-doc §6.3 until LN Carry 3 EP-1 ratification at Phase 5 fold-in.> <If `inactive`: This airlock does not emit `COMPLIANCE_AUDIT` events. Emission activates when `federation_mode` flips to advisory or enforce.>

**Multi-substrate identity (§5.4)**. Co-federated requests' transport substrate (Tailscale or Nebula) MUST match one of the requesting node's enumerated `transport.substrates[]`. Mismatch rejects with `signature_verification_failed:substrate_mismatch` per the §4.6 sub-reason taxonomy.
```

Fill the angle-bracket placeholders from the input parameters. The block above is canonical — do not paraphrase; consumers benefit from byte-similar §Federation-Substrate Awareness sections at adjacent vaults for cross-vault audit consistency.

#### Step 3.5 — Author "What v0.3.0 does NOT cover" deferral list

Adopt the deferral list from spec §8.2 verbatim, customized to point at consumer-side resolutions where applicable. Six deferrals at v0.3:

```markdown
## What v0.3.0 does NOT cover

v0.3.0 covers three surfaces — inbound entry (§Entry Paths) + cross-vault requests (§Cross-Vault Requests) + federation-substrate awareness (§Federation-Substrate Awareness). The following are deferred per spec §8.2:

- **`proposed/` channel for proposal triage** — deferred to v0.4+. Until then, structured proposals land in `who/coordination/` as the fallback.
- **Multi-vault transactional requests** — deferred to v0.4+. No present-day worked example.
- **Async batched requests** — deferred to v0.4+. No present-day worked example.
- **Body-section signing** — deferred to v0.4+. v0.3's Ed25519 signs frontmatter only; body sections are not signed.
- **`COMPLIANCE_AUDIT` native enum membership** — deferred to LN Carry 3 EP-1 ratification at Phase 5 fold-in. v0.3 uses the `assumes_draft` direct-JSON-write workaround per impl-doc §6.3.
- **Executable substrate enforcement** — deferred to a future Platform.aDNA integration (e.g., RareHarness, VAASLattice). v0.3 carries documentation-grade implementation guidance only.

If you arrive at a surface not covered here: file a coord memo at the receiving vault per spec §4 request pattern; v0.4+ will define missing schema retroactively.
```

#### Step 3.6 — Wire the `iii/CLAUDE.md` cross-reference

Add a cross-reference from `<vault>/iii/CLAUDE.md` to the activated airlock. Append (or update existing) to the wrapper's § Cross-References section:

```markdown
- Consumer airlock (reference instance v0.3.0): `~/aDNA/<vault>.aDNA/how/airlock/AIRLOCK.md` (activated <YYYY-MM-DD> via III.aDNA `skill_airlock_activation.md`)
```

#### Step 3.7 — Verify activation

Run the Step 6 verification checklist. If all 20 items pass, the airlock is active. If ≥1 item fails, do NOT declare activation complete — fix the failures or roll back via Step 7.

### Step 4 — Upgrade procedure (v0.2.0 → v0.3.0)

For consumers whose AIRLOCK.md is already active at v0.2.0 and is bumping to v0.3.0 federation-substrate awareness. Six steps; estimated 5-10 min (faster than Step 3 because entry paths + cross-vault request section don't change).

#### Step 4.1 — Frontmatter bump

```yaml
version: "0.3.0"                                  # was "0.2.0"
updated: <YYYY-MM-DD>                              # today
covers: [entry_paths, cross_vault_request_patterns, federation_substrate_awareness]   # add federation_substrate_awareness
ln_substrate_version_pin: "<from input param>"     # new at v0.3
federation_mode: <enforce | advisory | inactive>   # new at v0.3
ledger_observation: <enabled | disabled>           # new at v0.3
```

Preserve all other frontmatter (`type`, `created`, `last_edited_by`, `governed_by`, `absorbs_proposal`). The v0.2 `absorbs_proposal` field stays — that history does not change at upgrade.

#### Step 4.2 — Add §Federation-Substrate Awareness section

Insert the §Federation-Substrate Awareness block from Step 3.4 between the existing §Cross-Vault Requests section and §Version Contract section. Do NOT modify the §Entry Paths or §Cross-Vault Requests sections — they are invariant from v0.2 to v0.3 (additive-only minor bump per spec §7.1).

#### Step 4.3 — Update §Version Contract section

Where the v0.2.0 text mentioned v0.2 → v0.3 evolution as "future", flip the language to past-tense:

- "On minor version bump (v0.2 → v0.3 → ...): re-read this AIRLOCK.md..." → preserve; the rule still applies for future bumps.
- "Canonical path changes since v0.1.0: none" → extend with "; v0.3.0 added federation_substrate_awareness as a third surface and 3 new optional frontmatter fields (ln_substrate_version_pin, federation_mode, ledger_observation)."

#### Step 4.4 — Update "What v0.2.0 does NOT cover" → "What v0.3.0 does NOT cover"

Replace the v0.2.0 deferral block with the v0.3.0 deferral block from Step 3.5. The v0.2.0 list mentioned: `proposed/` channel + multi-vault transactional + async batched + executable substrate enforcement. The v0.3.0 list updates this — `executable substrate enforcement` is still deferred (impl-doc is documentation-grade) but the wording shifts ("v0.2 implementation guidance shipped at MC-4; v0.3 federation-substrate implementation guidance shipped at MD-A2"); new deferrals added at v0.3 (body-section signing, COMPLIANCE_AUDIT native enum).

#### Step 4.5 — Update closing references

The v0.2.0 closing block reads:

```
*Standard spec*: `what/artifacts/iii_airlock_standard_spec.md` (v0.2.0; governs this file)
*Request schema*: `what/artifacts/iii_airlock_request_schema.yaml` (v0.2.0; JSON Schema Draft 2020-12 in YAML form)
*Reply-comment template*: `how/templates/template_cross_vault_request_reply.md`
*Absorbs proposal*: `who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md`
```

Bump the version strings to v0.3.0 (spec + request schema both shipped within the v0.3.0 minor bump per spec §7.1) and add an `*Implementation guidance*` line pointing at the impl-doc:

```
*Standard spec*: `what/artifacts/iii_airlock_standard_spec.md` (v0.3.0; governs this file)
*Request schema*: `what/artifacts/iii_airlock_request_schema.yaml` (v0.3.0; JSON Schema Draft 2020-12 in YAML form; v0.3 adds 2 optional frontmatter fields)
*Implementation guidance*: `what/artifacts/iii_airlock_substrate_implementation.md` (v0.3.0; §4 Ed25519 preflight + §5 ledger observation + §6 COMPLIANCE_AUDIT emission)
*Reply-comment template*: `how/templates/template_cross_vault_request_reply.md` (extended at MD-A2 with Ed25519 rejection sub-reasons)
*Absorbs proposals*: `who/coordination/coord_2026_05_08_airlock_v0_2_videoforge_findings.md` (v0.2 origin); MD-A1 substrate-intersect coord memo to LN Venus (v0.3 origin)
```

The `absorbs_proposals` line is plural at v0.3 (was singular at v0.2); the v0.3 origin reference is informational since III.aDNA's own v0.3 spec absorbed LN ADR-014/-015 by reference rather than a proposal channel.

#### Step 4.6 — Verify upgrade

Run the Step 6 verification checklist. The upgrade case skips a few items (entry paths and cross-vault request sections are not re-verified — they're invariant from v0.2); the checklist annotates which items apply to which mode.

### Step 5 — Worked Example: activating `wga.aDNA/iii/` v0.2.0 → v0.3.0 (illustrative)

> **Note**: this worked example is read-only documentation in this skill. It does NOT modify the wga.aDNA vault. Actual wga.aDNA activation lands at MD-A4 (wrapper minor-bump sweep) per Campaign D charter. This walk-through illustrates the mechanics; the real activation will be one of MD-A4's six wrapper events.

**Scenario**: wga.aDNA is a Day-1 federation pilot in LN.aDNA (wga_l1 admitted at pc_01 Phase A close 2026-05-20). Its `iii/` wrapper is currently at v0.2.0 (pinned to III.aDNA commit `246124d`). MD-A4 sweep includes a wga wrapper bump v0.2.0 → v0.3.0; the `iii/AIRLOCK.md` activation rides alongside the wrapper bump.

**Before (v0.2.0 state)**:
```yaml
# ~/aDNA/wga.aDNA/how/airlock/AIRLOCK.md (hypothetical — wga's stub at v0.2.0)
---
type: airlock
version: "0.2.0"
status: reference_implementation
governed_by: ~/aDNA/III.aDNA/what/artifacts/iii_airlock_standard_spec.md
covers: [entry_paths, cross_vault_request_patterns]
---
```

**After (v0.3.0 active, federation-aware)**:
```yaml
# ~/aDNA/wga.aDNA/how/airlock/AIRLOCK.md (post-Step 4)
---
type: airlock
version: "0.3.0"
status: reference_implementation
updated: 2026-05-29
governed_by: ~/aDNA/III.aDNA/what/artifacts/iii_airlock_standard_spec.md
covers: [entry_paths, cross_vault_request_patterns, federation_substrate_awareness]
ln_substrate_version_pin: "LN.aDNA pc_01 Phase A close + Phase B1 close — ADR-014 v0.1 + ADR-015 v0.1 (both accepted 2026-05-20)"
federation_mode: advisory                # safe default for Day-1 federation pilot; promote to enforce after MD-A5 validation
ledger_observation: enabled              # wga is in active federation; observe MEMBERSHIP_* + CONNECTION_* events
---
```

**Body additions**:
- New §Federation-Substrate Awareness section (Step 3.4 template; federation_mode=advisory, ledger_observation=enabled placeholders filled).
- Updated §Version Contract noting v0.3.0 additions.
- Replaced "What v0.2.0 does NOT cover" → "What v0.3.0 does NOT cover" (Step 4.4).
- Updated closing references (Step 4.5).

**Cross-reference in `~/aDNA/wga.aDNA/iii/CLAUDE.md`** appended to § Cross-References:
```markdown
- Consumer airlock (reference instance v0.3.0): `~/aDNA/wga.aDNA/how/airlock/AIRLOCK.md` (activated 2026-05-29 via III.aDNA `skill_airlock_activation.md` at MD-A4 wrapper-sweep)
```

**Federation-pilot-specific notes** (because wga_l1 is in active federation):
- `federation_mode: advisory` is the cautious Day-1 default. Operator may promote to `enforce` after MD-A5 federation-integration validation closes (per Campaign D charter).
- `ledger_observation: enabled` opts wga into the §5.3 contract — polling at 60s cadence over LN Tier-1 file-ledger per impl-doc §5.1 recommendation.
- `COMPLIANCE_AUDIT` emission fires per impl-doc §6 — `assumes_draft: true` until LN Carry 3 EP-1 ratification.

### Step 6 — Verification checklist (~20 items, prose)

Walk the consumer's activated/upgraded `AIRLOCK.md` against each check. ✅ = pass; ❌ = fail; n/a = not applicable to mode. Estimated 3-5 min.

**Schema conformance (frontmatter)**:
1. `type: airlock` present. ✅/❌
2. `version: "0.3.0"` (string, quoted). ✅/❌
3. `status: reference_implementation` (or equivalent active state). ✅/❌
4. `governed_by:` points at `~/aDNA/III.aDNA/what/artifacts/iii_airlock_standard_spec.md` (or relative-path equivalent). ✅/❌
5. `covers:` includes `entry_paths`, `cross_vault_request_patterns`, AND `federation_substrate_awareness` (third surface added at v0.3). ✅/❌
6. `ln_substrate_version_pin:` populated with a recognizable LN version string (e.g., "LN.aDNA pc_01 Phase A close + Phase B1 close — ADR-014 v0.1 + ADR-015 v0.1"). ✅/❌
7. `federation_mode:` set to one of `enforce | advisory | inactive`. ✅/❌
8. `ledger_observation:` set to `enabled` or `disabled`. ✅/❌

**Body structure**:
9. §Entry Paths section present with ≥2 paths (minimum A + E). ✅/❌
10. §Cross-Vault Requests section present and points at spec §4 for the canonical contract. ✅/❌
11. §Federation-Substrate Awareness section present and contains references to spec §4.6 + §5.1–§5.4 + impl-doc §4 + §5 + §6. ✅/❌
12. §Version Contract section present and updated to mention v0.3.0 additions. ✅/❌
13. "What v0.3.0 does NOT cover" deferral list present and covers all 6 v0.3 deferrals (proposed/, multi-vault transactional, async batched, body-section signing, COMPLIANCE_AUDIT native enum, executable substrate enforcement). ✅/❌

**Federation-specific (skip if `federation_mode: inactive`)**:
14. Ed25519 sub-reason taxonomy (5 values: pubkey_absent, pubkey_revoked, signature_mismatch, key_version_unknown, substrate_mismatch) is cited verbatim in the §Federation-Substrate Awareness section. ✅/❌/n/a
15. `transport.substrates[*].identity.federation_signing_pubkey_sha256` pubkey resolution path is documented (or cited via spec §4.6 + ADR-013 §b reference). ✅/❌/n/a
16. Reply-comment template at `~/aDNA/III.aDNA/how/templates/template_cross_vault_request_reply.md` is current (line 60 includes `signature_verification_failed:<subreason>`; check by grep). ✅/❌/n/a
17. If `ledger_observation: enabled`: §5.3 cited; polling cadence (60s recommended) referenced; `MEMBERSHIP_CERT_REVOKED` → pubkey-invalidation linkage documented. ✅/❌/n/a
18. If `federation_mode != inactive`: COMPLIANCE_AUDIT emission cited; `assumes_draft` workaround per impl-doc §6.3 noted. ✅/❌/n/a

**Hygiene**:
19. No dangling wikilinks — grep the file for `[[...]]` patterns; every wikilink resolves to a real file or section. ✅/❌
20. No copy-paste artifacts from another vault — grep for residual vault-name strings (e.g., "VideoForge" or "CanvasForge") that don't belong in the current consumer vault. Tailor consumer-specific paths; do NOT inherit verbatim from III.aDNA. ✅/❌
21. Cross-reference in `<vault>/iii/CLAUDE.md` § Cross-References points at the activated airlock (Step 3.6 / Step 4.5 confirmation). ✅/❌

**Pass criterion**: all applicable items ✅. Any ❌ blocks activation declaration; either fix the issue or roll back via Step 7.

### Step 7 — Rollback procedure (v0.3.0 → v0.2.0)

Use only when activation fails verification AND the consumer needs to operate at v0.2 until the failure is resolved. Rollback is a controlled retreat, not a sign of permanent v0.2 stasis.

**When to roll back**:
- LN substrate prerequisite failure (e.g., consumer's canonical_id is not in the LN membership manifest after federation_mode=enforce was set; sigverify will reject every request).
- Federation incident (e.g., LN ledger emit failures, COMPLIANCE_AUDIT direct-JSON-write path broken at the consumer side).
- ADR-002 §3 minor-bump consumer-review veto (operator reviews the v0.3 diff and chooses to defer activation).

**Steps**:
1. Revert frontmatter — flip `version: "0.3.0"` → `"0.2.0"`; remove `ln_substrate_version_pin`, `federation_mode`, `ledger_observation`; restore `covers:` to `[entry_paths, cross_vault_request_patterns]` (drop `federation_substrate_awareness`).
2. Remove the §Federation-Substrate Awareness section body.
3. Restore the §Version Contract to the v0.2.0 wording (or accept the v0.3-noted-but-deferred wording if minor — both are valid v0.2 states; consumer choice).
4. Restore "What v0.2.0 does NOT cover" to the v0.2 deferral list (drop body-section-signing + COMPLIANCE_AUDIT-native-enum rows; restore executable-substrate-enforcement wording).
5. Restore closing references to v0.2.0 versions.
6. Revert the `iii/CLAUDE.md` § Cross-References row added at Step 3.6.
7. Commit the rollback with a verbose message documenting the trigger (`activation_failed: <which check>` per the audit-log convention from impl-doc §4.7).

Rollback is reversible — the consumer can re-attempt activation at any time after fixing the trigger condition.

### Step 8 — Cross-References

**III.aDNA canonical**:
- Airlock standard spec v0.3.0: `~/aDNA/III.aDNA/what/artifacts/iii_airlock_standard_spec.md` (§4.6 Federation signing-key verification contract; §5 Federation-Substrate Awareness — §5.1 pluralism + §5.2 Ed25519 substrate-side + §5.3 ledger observation + §5.3.1 COMPLIANCE_AUDIT emission shape + §5.4 multi-substrate identity; §7.3 reference-instance pinning; §7.5 federation-substrate version pin)
- Substrate implementation guidance v0.3.0: `~/aDNA/III.aDNA/what/artifacts/iii_airlock_substrate_implementation.md` (§4 Federation Signing-Key Verification (Ed25519) Preflight, 9 sub-sections; §5 Ledger Observation Client, 7 sub-sections; §6 COMPLIANCE_AUDIT Event Emission, 7 sub-sections)
- Reference instance (model the consumer mirrors): `~/aDNA/III.aDNA/how/airlock/AIRLOCK.md` (v0.3.0 post-MD-A3)
- Reply-comment template (Ed25519 rejection sub-reasons added at MD-A2): `~/aDNA/III.aDNA/how/templates/template_cross_vault_request_reply.md`
- Companion skill — consumer wrapper bootstrap: `~/aDNA/III.aDNA/how/skills/skill_iii_setup.md`
- Companion skill — session close discipline: `~/aDNA/III.aDNA/how/skills/skill_session_close_ceremony.md`

**III ADRs**:
- ADR-002 §3 (consumer federation contract — minor-bump consumer-review trigger): `~/aDNA/III.aDNA/what/decisions/adr_002_consumer_federation_contract.md`
- ADR-003 rule 2 (learning store ownership — assumes_draft inheritance pattern, used at COMPLIANCE_AUDIT emission): `~/aDNA/III.aDNA/what/decisions/adr_003_learning_store_ownership.md`
- ADR-008 (airlock pattern design — activation operator-discretionary): `~/aDNA/III.aDNA/what/decisions/adr_008_airlock_pattern.md` (or equivalent path)

**LN ADRs (absorbed by reference at v0.3 spec)**:
- LN ADR-010 (canonical node ID schema): `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_010_canonical_node_id_schema_three_universes.md`
- LN ADR-013 §b (federation signing key infrastructure — per-purpose-slot dict): `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_013_federation_signing_key_infrastructure.md`
- LN ADR-014 §c (canonical-JSON algorithm — sorted keys, NFC, whitespace, case): `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_014_re_id_semantics_node_canonical_id_transition.md`
- LN ADR-015 §b + §d + §e (substrate pluralism + `transport.substrates[]` + canonical-pilot-tier nodes): `~/aDNA/LatticeNetwork.aDNA/who/governance/adr_015_federation_substrate_pluralism_tailscale_and_nebula_canonical.md`

**Provenance**:
- Authored at III.aDNA Campaign D MD-A3 (2026-05-22) after MD-A1 (spec v0.3) + MD-A2 (impl-doc v0.3) closed the v0.3 contract surface. D3 (originally a standalone activation kit mission in the MC-4.5 alignment recon dossier §5.2) was absorbed into MD-A3 per the Campaign D charter — this skill IS the D3 deliverable repackaged as an MD-A3 outcome.
- 9-stub ecosystem discovery: `~/aDNA/III.aDNA/what/artifacts/mc4_5_alignment_recon_dossier.md` §3.6 (10 vaults with `how/airlock/AIRLOCK.md` files; 1 active canonical at III.aDNA, 9 inactive stubs).
- Activation operator-discretionary per ADR-008 design — this skill packages the activation procedure but does NOT auto-activate. Each consumer's activation is a human-gated decision at the consumer's wrapper-minor-bump review cadence.

## Outputs

| Output | Type | Description |
|--------|------|-------------|
| `<vault>/how/airlock/AIRLOCK.md` | edited (activate) or bumped (upgrade) | The activated/upgraded reference instance at v0.3.0 |
| `<vault>/iii/CLAUDE.md` | edited | § Cross-References row added pointing at activated airlock |
| Audit-log JSONL row (per impl-doc §4.7) | optional | `<vault>/who/coordination/.audit/audit_<YYYY-MM>.jsonl` — record of the activation event (event_type: `airlock_activation` is consumer-side-private; not in the §4 5-value enum since it's an activation event, not a sigverify outcome) |

## Anti-Patterns

| Skip | Failure mode |
|------|--------------|
| **Activate without checking LN substrate prerequisites** | `federation_mode: enforce` will reject every request because pubkey resolution fails; the activation appears live but the airlock is unreachable from co-federated peers |
| **Promote advisory → enforce same-day as activation** | First-day federation traffic may include legitimate requests from peers whose substrate state is in-flight; advisory mode lets these through with a warning while you debug; enforce mode rejects them silently |
| **Activate without verifying the iii/ wrapper is at v0.3+** | The wrapper's `federation_ref.version` governs which spec the consumer loads; activating AIRLOCK.md at v0.3 with a wrapper pinned to v0.2 creates an internal version mismatch (the wrapper-cited spec and the activated airlock will disagree on §5 contracts) |
| **Skip Step 6 verification checklist** | Schema drift compounds across the 9-stub ecosystem; one consumer's misconfigured frontmatter becomes copy-paste source-of-truth for the next |
| **Author a §Federation-Substrate Awareness section that paraphrases the spec** | Cross-vault audit reconciliation gets harder when adjacent consumer airlocks word the same contract differently; mirror the Step 3.4 template byte-similar across consumers |
| **Forget to populate `federation_mode` or `ledger_observation` frontmatter** | Defaults are not assumed; an absent field is a verification-checklist ❌ on items 7-8; downstream agents reading the frontmatter cannot infer intent |
| **Activate during an LN substrate incident** | If LN ledger emit is broken at activation time, `COMPLIANCE_AUDIT` emission will fail silently per impl-doc §6.7 failure modes; defer activation until LN reports green |
| **Copy III.aDNA AIRLOCK.md verbatim without tailoring** | Vault-name strings, federation_pilot status, and consumer-specific worked-example paths all need consumer-side adjustments; copy-paste is the first-pass starting point, not the finish line |

## Related

- **Skills Protocol**: `~/aDNA/III.aDNA/how/skills/AGENTS.md`
- **Companion**: `skill_iii_setup.md` (run first — bootstraps the `iii/` wrapper); `skill_iii_review.md` (the review loop the wrapper invokes); `skill_session_close_ceremony.md` (close discipline for the activation session)
- **MD-A4 dependency**: this skill is the load-bearing input to MD-A4 wrapper-sweep — that mission applies this skill across 6 active consumer wrappers (lattice-labs/iii v0.1.0 → v0.3.0 carry-forward absorbed; SiteForge + VideoForge + CanvasForge + wga + LPWhitepaper v0.2.0 → v0.3.0).
- **MD-A5 dependency**: this skill is the substrate-precondition for MD-A5 federation-integration validation (re-exercises the VideoForge → CanvasForge worked example with v0.3 federation features active).
