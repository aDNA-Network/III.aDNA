---
artifact_class: web_design_cross_vault_architecture_note
type: artifact
title: "WDR Deliverable 6 — Cross-vault architecture note (III↔SiteForge boundary + wrapper sweep + graduation path + Campaign E relation)"
mission: plan_web_design_deep_review_charter
designs_campaign: campaign_web_design_deep_review
created: 2026-05-25
updated: 2026-05-25
last_edited_by: agent_argus
status: proposal
hard_invariant: "PROPOSAL — not a ratified ADR; no contract change lands in this planning mission"
tags: [artifact, web_design, architecture_note, boundary, siteforge, adr_002, adr_003, graduation_path, campaign_e_relation]
---

# WDR-6 — Cross-Vault Architecture Note (PROPOSAL)

> **Status**: a *proposal*, not a ratified ADR. It states the III↔SiteForge boundary, the wrapper-sweep plan, and the graduation path that the future `campaign_web_design_deep_review` will execute. Nothing here changes a contract during this planning mission. Any ADR (if needed) ratifies inside the execution campaign, Stanley-gated.

## §1 The III ↔ SiteForge boundary (the load-bearing statement)

Two vaults, one web-quality system, a clean seam:

| | **SiteForge** (Forge.aDNA) | **III** (Framework.aDNA) |
|---|---|---|
| Owns | **Audit execution** — Gates 1–10 (Lighthouse, axe, CWV, CSP, responsive, meta, brand-screenshot). Runs the robots. | **Semantic judgment** — the `web_design` trap library + corrections. Reads what the robots produce; never runs them. |
| Produces | Pass/fail scores + report artifacts on disk (`lh-*.json`, `gate_*_*.json`, screenshots). | Ranked semantic findings + improvement orders + learning-store deltas. |
| Voice layer | 5-voice reviewer registry (VCRIT/DESIGN/UX/SEO/BRAND) — consumer-side composition orchestrating III pack findings. | The traps the voices apply. |

**Directionality**: signals flow **SiteForge → III as input**, never the reverse. III consumes audit signals to *attribute design intent*; it does not gate builds, run Lighthouse, or own thresholds. This is the operator-locked boundary-preserving posture. Trap 6 (CWV-as-Design-Debt) is the *only* trap that reads SiteForge artifacts, and it reads them as **already-on-disk evidence** — the documented ingest seam (WDR-4 §5), not a runtime.

**Why the boundary holds even as III's web review deepens**: every mechanical concern (a CWV number, an axe rule, meta presence, responsive overflow) is already gated by SiteForge. III's additions (Trap 6, Trap 7, the hardenings) are all *design-decision interpretation* of those signals or of the code/content — the residue the robots provably miss (~30%/~57% axe ceiling; Gate 8 manual-only). No duplication; no audit runtime.

## §2 ADR-002 §3 minor-bump wrapper sweep plan

Pack content changes (new traps + hardenings) = a **minor** version bump: III `v0.3.0 → v0.4.0`. Per ADR-002 §3, `version_policy: minor` consumers **review on minor bump** and re-pin explicitly.

**Sweep scope (verified 2026-05-25)** — all **5 active wrappers carry `web_design`** and are pinned at `v0.3.0`:

| Wrapper | Carries `web_design`? | Current pin | Sweep action |
|---|---|---|---|
| `SiteForge.aDNA/iii/` | yes (primary consumer; 5-voice registry) | v0.3.0 / a309ad4 | review + re-pin v0.4.0 (highest priority — owns the empirical sites) |
| `VideoForge.aDNA/iii/` | yes (title-card/thumbnail/caption UI) | v0.3.0 | review + re-pin or defer (web_design used for UI register only) |
| `CanvasForge.aDNA/iii/` | yes (deck/comic/marketing UI) | v0.3.0 | review + re-pin or defer (same — UI register) |
| `wga.aDNA/iii/` | yes | v0.3.0 | review + re-pin (wga is an empirical corpus site) |
| `LPWhitepaper.aDNA/iii/` | yes | v0.3.0 | review + re-pin or defer (whitepaper surface — least web-exposed) |
| `lattice-labs/iii/` | **no** (does not carry web_design) | v0.1.0 | **untouched** — tracked-deferred per genesis-first migration discipline |

**Sweep discipline** (execution campaign F4): bump III to v0.4.0 + CHANGELOG entry first; then each consumer agent reads the v0.3.0→v0.4.0 diff and decides re-pin vs defer (consumer explicitly approves per Standing Rule 5 / ADR-002 §3). Forward-stub discipline (Standing Rule 2) — no active consumer breaks. VideoForge/CanvasForge/LPWhitepaper may legitimately *defer* (their web_design use is UI-register-only, less affected by the site-design traps) — deferral is a valid ADR-002 outcome, recorded per-consumer.

## §3 ADR-003 graduation path for the new corrections (candidates-only)

Per operator decision (2026-05-25), the 5 correction candidates (`C-029+` provisional, WDR-4 §4) ship as `graduation_candidate: true` and **do not graduate within the campaign**. The path on record for a *later* gate:

- **ADR-003 §3 standard ceremony**: `frequency ≥ 3` across ≥2 sessions · `acceptance ≥ 0.80` · domain-general · not already canonical. The empirical corpus gives a *starting* evidence base but not yet the frequency: most candidates are **1-vault** on the current corpus (only CWV-as-design-debt reaches 2-vault: SS strong + wga weak). So none clear §3 today — correctly held as candidates.
- **The ≥2-vault provenance already banked**: C-029 `image_component_omits_dimensions` (SS+wga). The thin candidates (motion-absence, responsive-neglect, semantic-data-no-shared-source) need a 2nd independent vault before graduation — they wait.
- **ADR-003 §3.6 elevated-scrutiny** (≥50 cross-fork frequency) is not in play (volumes are tiny). Not relevant this campaign.
- **Canonical invariant**: until a future ceremony runs, `iii_corrections_canonical.jsonl` stays md5 `5adb0dfa38d9224649c3b2cba83852ae` (28 entries). The candidates live in the WDR-4 spec / a consumer local store (`learning_store_local` per ADR-002 §1a) — never the canonical jsonl. This matches the recent III spec-only discipline (MD-A1..A5 + MB4) where zero graduations fired.
- **Reinforced existing corrections** (C-012/C-015/C-016 gained fresh ≥2-vault provenance from the corpus) are noted as future re-graduation-review inputs — no action now.

**Domain-pack graduation note**: Trap 6 + Trap 7 are *static trap additions to the pack* (ADR-003 §6 — a file edit, Stanley-gated), distinct from *corrections graduation* (a jsonl append). The execution campaign's F1/F2 perform the §6 file edit; F3 holds the §3 corrections as candidates. Two different mechanisms — only the file edit happens in-campaign.

## §4 The "III-tuned artifacts get better" thesis + Campaign E relation

**The flywheel this campaign turns**:
1. III improves the `web_design` semantic surface (Trap 6/7 + hardenings).
2. SiteForge's `iii/` wrapper consumes the v0.4.0 pack (F4 sweep).
3. The next Astro site SiteForge builds is reviewed against the tighter surface → catches residue earlier.
4. New review cycles surface fresh corrections → frequency accrues → a *later* graduation gate fires → the canonical store grows.
5. The cycle repeats; each pass the advice is sharper and better-cited.

This is III's core thesis made measurable on the web surface: the `source_diversity` axis rises 3→4/5 *this* campaign; the correction count grows at a *future* gate as frequency accrues.

**Relation to Campaign E (generalized writing-III)** — *relates to, does not block*:
- Campaign E builds a **generalized writing-III** (supersedes the LPWhitepaper bespoke III layer; opens when `LiteratureForge.aDNA` reaches forge-BUILD per MD-X2 cross-vault note §6). It is a *different surface* (written artifacts, not web pages).
- **Shared pattern, not shared scope**: both campaigns instantiate the same flywheel (a domain pack + corrections that tighten over consumption cycles via the ADR-007 adaptive loop). Campaign F is a clean *worked precedent* for the per-domain deep-review/improve pattern that Campaign E will reuse for writing genres.
- **No dependency either direction**: Campaign F can run before, during, or after Campaign E. The `web_design` pack and the writing packs do not overlap. The letter ordering (F after E) reflects only reservation, not sequencing.

## §5 Summary of proposals (all deferred to execution-campaign ratification)
1. **Boundary**: III consumes audit signals; SiteForge owns audit execution. Documented seam, no runtime. (No ADR needed — restates the operator-locked posture + WDR-4 §5.)
2. **Wrapper sweep**: all 5 active wrappers review at III v0.3.0→v0.4.0; lattice-labs untouched. (ADR-002 §3 — no amendment; standard minor-bump.)
3. **Graduation**: 5 candidates held; none graduate in-campaign; canonical md5 invariant; later §3 gate when frequency accrues. (ADR-003 §3 — no amendment.)
4. **No new ADR is required by this campaign** — it operates entirely within ADR-002 §3 + ADR-003 §3/§6 + Standing Rules 6/7. (If F4 surfaces a genuinely new `audit_signal_ref` wrapper field, *that* would need an ADR-002 amendment — explicitly deferred, since the ingest seam is documented-only this campaign.)

## Cross-references
- Pairs with [[wdr_web_design_pack_revision_spec]] (the content) + [[campaign_web_design_deep_review]] (the charter that executes this).
- Contracts: `what/decisions/adr_002_consumer_federation_contract.md` §3/§1a; `what/decisions/adr_003_learning_store_ownership.md` §3/§3.6/§6.
- Campaign E seed: MD-X2 cross-vault note (`what/artifacts/md_x2_cross_vault_architecture_note.md`) §6.
