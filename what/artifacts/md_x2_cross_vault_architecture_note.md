---
artifact_class: cross_vault_architecture_note
type: artifact
title: "MD-X2 deliverable — Cross-vault architecture note: LiteratureForge.aDNA ⇄ generalized writing-III ⇄ LPWhitepaper.aDNA (pilot)"
mission: MD-X2
campaign: campaign_d_federation_adaptive_loop
produces_for_vault: LiteratureForge.aDNA
destined_path: "LiteratureForge.aDNA/what/context/md_x2_cross_vault_architecture_note.md"
created: 2026-05-25
updated: 2026-05-25
last_edited_by: agent_argus
status: proposal
disposition: "PROPOSAL — not a ratified ADR. ADRs land in the LiteratureForge genesis campaign (P4) + III.aDNA Campaign E."
tags: [artifact, architecture_note, proposal, md_x2, literatureforge, writing_iii, lpwhitepaper, federation, campaign_e_seed, md_b5_reframe]
---

# Cross-Vault Architecture Note (PROPOSAL)

> Satisfies MD-X2 deliverables 3 (cross-vault architecture note), 5 (MD-B5 reframe), and 6 (Campaign E seed). This is a **proposal**, not a ratified ADR — it scopes the topology and the migration so the LiteratureForge genesis campaign (P4) and III.aDNA Campaign E can ratify the specifics.

## §1 Three-vault topology

```
            ┌─────────────────────────────┐
            │   III.aDNA (Argus)          │  Framework.aDNA — quality substrate
            │   generalized "writing-III" │  (review/improvement loop + learning store + graduation)
            └──────────────▲──────────────┘
                           │ federation_ref (consumes review machinery)
                           │ bridge_pack pattern (cf. VideoForge/CanvasForge)
            ┌──────────────┴──────────────┐
            │  LiteratureForge.aDNA       │  Forge.aDNA — composition engine
            │  (Thoth)                    │  (genre submodules + generation pipeline + format specs)
            └──────────────▲──────────────┘
                           │ <forge> wrapper + federation_ref (consumes generation)
            ┌──────────────┴──────────────┐
            │  LPWhitepaper.aDNA          │  pilot / first consumer
            │  (whitepaper artifact)      │  (+ seed corpus for the whitepaper submodule)
            └─────────────────────────────┘
```

**Two distinct federation edges, two directions of value:**
- **LiteratureForge → III** (LiteratureForge is the consumer): the review/improvement loop, learning store, and graduation ceremony are III machinery. LiteratureForge pins a `federation_ref` to a **generalized writing-III** exactly as VideoForge/CanvasForge pin III today.
- **Consumer → LiteratureForge** (consumer vaults are downstream): LPWhitepaper (pilot), ScienceStanley, and future vaults consume *generation* via a `literatureforge/` wrapper + `federation_ref` pull — the standard Forge.aDNA consumer pattern.

## §2 The writing-III boundary (resolves mission §9 Q5)

The clean split (dossier directive D6):

| Concern | Owner | Rationale |
|---|---|---|
| Genre trap-packs (quality knowledge) | **LiteratureForge** (writing pack) | Genre-specific; mirrors a consumer's local domain-pack extension |
| Per-genre reviewer-voice-sets | **LiteratureForge** | Genre-specific reader personas (F-TECH/F-INV/… for whitepaper) |
| Per-genre format-specs | **LiteratureForge** | Genre-specific output contract |
| Generation pipeline (brief→plan→draft→revision) | **LiteratureForge** | Composition is the forge's primary function |
| INSPECT→INTROSPECT→IMPROVE→HUMAN_REVIEW→ACCUMULATE loop | **III (writing-III)** | General review machinery; not genre-specific |
| Learning store + CorrectionLifecycle + graduation | **III** | Canonical memory; pull-up-never-push discipline (Standing Rule 1) |
| RLHF signal channel (ADR-005) | **III** | Cross-vault signal contract |
| Six-axis pack rubric (ADR-007) | **III** | General pack-quality measure; LiteratureForge writing packs are scored by it |

**Boundary test**: if a mechanism is *genre-specific writing knowledge*, it is a LiteratureForge writing pack; if it is *general review/improvement/memory machinery*, it stays canonical III. The generalized writing-III is simply III with its `whitepaper_communication`-specific assumptions lifted out into LiteratureForge writing packs.

## §3 What "replace the III layer on LPWhitepaper" concretely migrates

Today LPWhitepaper consumes III directly (`LPWhitepaper.aDNA/iii/CLAUDE.md`, 6/7 packs incl. `whitepaper_communication`, pinned v0.3.0 / `a309ad4`). After LiteratureForge is built, LPWhitepaper instead consumes **LiteratureForge** (whitepaper submodule), which in turn consumes the writing-III. Concretely:

1. **Generalize `whitepaper_communication` → a LiteratureForge-owned writing pack** (the whitepaper submodule's trap-pack). The 9 traps + 5 escalations move from III canonical into the LiteratureForge whitepaper submodule. *Open question for Campaign E:* does the canonical III pack get deprecated, or retained as a thin shim during transition? (Proposal: retain as shim until LPWhitepaper re-pin validated, then deprecate.)
2. **Re-point LPWhitepaper's `iii/CLAUDE.md` federation_ref** → a `literatureforge/` wrapper `federation_ref` (source_vault: LiteratureForge.aDNA, whitepaper submodule). The vault-local learning store survives the move.
3. **Preserve the in-flight `campaign_iii_deep_review`** (25-cycle, awaiting DG2) + `campaign_sharing_prep` during transition — these must not break. Migration happens *after* DG2, or runs the deep-review to completion on the existing III pin and migrates the *next* review cycle.

## §4 Transition consumption model (resolves mission §9 Q4 — PROPOSAL)

During the migration window, **LPWhitepaper consumes BOTH** LiteratureForge (generation + whitepaper submodule trap-pack/format-spec) **and** III (the general review-loop machinery + learning store) — federation is composable per the airlock spec §6. This avoids a hard cutover: the whitepaper submodule's genre knowledge comes from LiteratureForge while the review loop is still the canonical III loop, until the writing-III generalization (Campaign E) is ratified and the submodule consumes the writing-III transitively. Steady state: LPWhitepaper → LiteratureForge → writing-III (single transitive chain).

## §5 MD-B5 reframe (mission deliverable 5)

**Original MD-B5**: ≥3-vault validation of the v0.3 airlock + adaptive loop, with LPWhitepaper named as the natural Day-1 target (first 6/7-pack consumer).

**Reframe**: LPWhitepaper's III relationship is now slated to migrate (it becomes a LiteratureForge consumer). Validating LPWhitepaper's *current direct-III* configuration in MD-B5 would validate a configuration we intend to migrate. Options for MD-B5's ≥3-vault set:
- **(Recommended)** Keep MD-B5 vault-agnostic: validate against any 3 already-live v0.3 consumers (SiteForge + VideoForge + CanvasForge are all pinned at `a309ad4`) — these are stable and not slated for migration. LPWhitepaper is **deferred from the MD-B5 set** until its LiteratureForge migration lands, at which point it validates the *new* chain (LPWhitepaper → LiteratureForge → writing-III) as a richer end-to-end case.
- Alternative: include LPWhitepaper in MD-B5 *as-is* with an explicit note that the validated configuration is pre-migration; re-validate post-migration in Campaign E.

**Net**: MD-B5 should not block on the LiteratureForge migration. Use the three stable v0.3 consumers; treat LPWhitepaper as a post-migration validation target. MD-B5 remains a DG-D criterion; this reframe only changes *which* vaults it exercises. DG-D stays 9/11 (MD-X2 is non-gating).

## §6 Campaign E seed — III.aDNA "generalized writing-III" (mission deliverable 6; OUTLINE only)

A future III.aDNA campaign (provisional **Campaign E**) that generalizes the writing-quality loop and re-points LPWhitepaper. **Not chartered here** — seed outline:

- **E-goal**: lift genre-specific assumptions out of III canonical into LiteratureForge writing packs, leaving a domain-general "writing-III" that LiteratureForge consumes; then execute the LPWhitepaper migration (§3) end-to-end.
- **Plausible phases**: (E1) define the writing-III boundary as a ratified III ADR (formalize §2); (E2) extract `whitepaper_communication` → LiteratureForge writing pack + decide canonical-shim disposition; (E3) author the LiteratureForge↔writing-III federation contract (the bridge_pack the forge's P4-03 ADR pins against); (E4) migrate LPWhitepaper's `iii/` → `literatureforge/` wrapper, preserving the local learning store + in-flight campaigns; (E5) ≥3-vault re-validation incl. the new LPWhitepaper chain (folds the deferred MD-B5 LPWhitepaper case).
- **Dependency**: E cannot complete until LiteratureForge has a built whitepaper submodule (its execution campaign, chartered at genesis P5, has produced the submodule). E and the LiteratureForge execution campaign are co-dependent and should be sequenced together.
- **Boundary discipline**: graduation still pulls corrections *up* into canonical III; LiteratureForge writing packs are downstreams that never push automatically (Standing Rule 1 preserved).

## §7 Status & next steps
- **Status**: PROPOSAL. Ratification: LiteratureForge genesis P4 ADRs (forge-side boundary + federation contract) + III.aDNA Campaign E ADR (III-side boundary).
- **No III ADR authored at MD-X2** (per mission §6 + consumption-only precedent).
- **Carried into the vault** at fork as `what/context/md_x2_cross_vault_architecture_note.md`.
