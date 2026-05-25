---
plan_id: plan_web_design_deep_review_charter
type: plan
title: "Web-Design Deep-Review — campaign charter authoring (planning mission)"
owner: stanley
status: planned
mission_class: campaign_planning_research
campaign: null   # standalone — designs a not-yet-opened campaign (no III campaign is currently open; Campaign D closed 2026-05-25)
designs_campaign: campaign_web_design_deep_review   # proposed slug; proposed letter Campaign F (E reserved for generalized writing-III) — see §9 Q1
disposition: standalone_planning
created: 2026-05-25
updated: 2026-05-25
last_edited_by: agent_argus
authored_at_session: session_stanley_20260525_iii_adna_web_design_deep_review_planning_authoring
boundary_posture: boundary_preserving   # III stays semantic; mechanical audits (Lighthouse/axe/CWV) stay in SiteForge — see §1
evidence_method: inspection_grade        # review existing Lighthouse/Playwright reports on disk; no live builds/audit runs
sequencing: "standalone; executes on operator gate. Not Campaign E (generalized writing-III stays reserved/gated on LiteratureForge.aDNA forge-BUILD)."
forward_seeds: "feeds the III thesis 'III-tuned sites get better and better'; the new web traps + corrections become inputs the SiteForge iii/ wrapper consumes; relates to (does not block) Campaign E"
tags: [plan, campaign_planning, web_design, siteforge, boundary_preserving, inspection_grade, standalone_planning, anti_slop, learning_store_growth, planned]
---

# Web-Design Deep-Review — Campaign-Planning Mission

> **Status note**: This is a *planning-mission card* authored 2026-05-25. It scopes what the **future executing session** will research and produce — namely the **charter for a campaign** that deep-reviews and improves III's web-design review system. It is NOT executed yet, and it does **not** open or charter that campaign. Per operator decision it is a **standalone planning mission** (no III campaign is currently open — Campaign D closed at DG-D 11/11). When the campaign it designs is later opened, this card becomes that campaign's P0 charter-authoring record.

## §1 Context & Why

Campaign D closed 2026-05-25 (DG-D 11/11, `v0.3.0`, on `origin`). With the federation-aware airlock + RLHF/adaptive loop shipped, the operator wants to turn III's improvement engine on its **own web-design review capability** — so that Astro sites built and reviewed under III get measurably better over successive cycles.

The reality the campaign must work within (established by recon):

- **III's web-design review is *semantic* today.** `what/context/core_domain_packs/context_iii_domain_packs_web_design.md` defines **5 traps** (Visual Hierarchy · Template Voice · Accessibility Theater · SEO Stuffing · Design Token Drift). The pack's composite is 4.00, but two axes are weak: **`source_diversity = 3`** (no ADR/skill/external citations) and **`graduation_yield = 1`**. Six web-themed canonical corrections exist (**C-012…C-016, C-027**). The **5-voice reviewer orchestration** (Critic / Design / UX / SEO / Brand) lives in the *SiteForge consumer wrapper*, not III core.
- **Mechanical audit already exists — in SiteForge, not III.** `SiteForge.aDNA/what/artifacts/sf_m02_quality_gate_framework.md` defines 10 gates including **Gate 3 Lighthouse ≥95**, **Gate 4 axe WCAG AA (zero violations)**, **Gate 7 Core Web Vitals**. Live Lighthouse configs + Playwright suites are already on disk for the built sites.
- **The real gap** is therefore *not* "III lacks a Lighthouse subroutine." It is that **III's semantic advice does not yet learn from the audit signals SiteForge already produces**, and its web pack is thin on source diversity and graduation evidence. III-tuned sites can pass every robot gate and still read as slop / weak design — that residue is exactly where new III traps should live.

**Boundary posture (operator-locked, boundary-preserving)**: III **stays semantic**. The campaign mines the gap between "passes the robots (Lighthouse/axe green)" and "still reads as slop / weak design," turning that residue into new III traps + corrections. Mechanical audit execution **remains in SiteForge**. III does **not** gain an audit runtime and authors **no** audit-execution ADR. An *audit-signal-informed* review note (III reads SiteForge's existing Lighthouse/axe JSON as **input** to its semantic traps, never running the audits itself) is documented as the integration seam — designed as a future option, not built in this campaign.

## §2 The Campaign Being Planned — Vision (what this mission charters)

A **boundary-preserving deep-review / improve campaign** for III's web-design surface. When executed, the campaign it charters will ship:

- a **`web_design` pack v-next** — new traps that target the "passes-robots-but-still-weak" residue, plus **`source_diversity` remediation** (add ADR / skill / external citations — directly raising the pack's weakest axis);
- **new canonical-candidate corrections** seeded from real-site evidence, carrying ≥2-vault provenance (ScienceStanley + wga + ContextCommons) so they can clear the ADR-003 graduation gate;
- an **audit-signal-informed review note** — the documented seam by which III *reads* SiteForge's Lighthouse/axe outputs as input to its semantic traps (proposal only; ingest seam not built);
- the **ADR-002 consumer-wrapper sweep** plan (the 5 active `iii/` wrappers minor-bump to consume the revised pack).

The thesis: each pass tightens III's web advice, the SiteForge wrapper consumes it, and the next generation of III-tuned Astro sites is better than the last.

## §3 Objectives (what the executing session produces)

- **O1 — External SOTA research**: modern web / Astro design best practices + evaluation frameworks, framed for III's *semantic* layer (not as audit tooling). Cover: **Core-Web-Vitals-as-design-signal** (CLS ↔ layout discipline; LCP ↔ hero / image strategy; INP ↔ interaction design) — i.e. what a CWV number *implies about design intent*; **WCAG 2.2 semantic gaps** beyond what axe auto-detects (the "accessibility theater" residue); **structured-data / SEO naturalness** (the line between valid JSON-LD and keyword stuffing); **Astro 6 idioms** (islands, view transitions, content collections, image optimization) and their *design* implications; and the **"AI-slop web aesthetic" tells** (generic hero + 3-card grid + gradient-blob + lorem-ish copy) given III's anti-slop mandate. → concise findings dossier with citations.
- **O2 — Internal lattice pattern-mine** (reuse before invent): map current coverage — the 5 web traps + 6 web corrections (C-012…C-016, C-027) + `SiteForge.aDNA/what/artifacts/sf_m02_quality_gate_framework.md` (10 gates) + `SiteForge.aDNA/what/context/siteforge/siteforge_reviewers.yaml` (5 voices) + `SiteForge.aDNA/iii/what/context/context_iii_domain_packs_iss.md` (ISS pack) + any VideoForge `web_design` reuse + CanvasForge visual traps. → coverage map (what's covered, by whom, at which layer).
- **O3 — Empirical site review (inspection-grade)**: review the 3 built sites — `ScienceStanley.aDNA/site/`, `wga.aDNA/site/`, `ContextCommons.aDNA/site/` — together with their **existing** Lighthouse configs / reports and Playwright suites already on disk (no live builds or audit runs). For each site build a **gap ledger** classifying findings as: *machine-caught* (Lighthouse/axe flagged) · *III-trap-caught* (an existing web trap covers it) · *neither-caught* (the residue — the campaign's target). → gap ledger across the 3 sites.
- **O4 — Gap analysis + new-trap / correction candidates**: synthesize O1–O3 into concrete proposed additions to the `web_design` pack — candidate traps such as **CWV-as-design-debt**, **AI-slop-hero**, **responsive-breakpoint-neglect**, **motion / interaction-design-absence** (final set chosen at execution); the **`source_diversity` remediation** (specific ADR / skill / external cites to add); **correction candidates** with their ≥2-vault evidence rows; and the **audit-signal-ingest note** (how III would consume SiteForge audit JSON without running it — proposal). → pack-revision spec + candidate list.
- **O5 — Execution-campaign charter draft**: a full charter for the deep-review/improve campaign — Mission / Predecessor / Scope / Phases (P0–P4) / Mission Table / Decision-Gate criteria / Risk Register, boundary-preserving (no III audit runtime). Modeled on the Campaign D charter skeleton. → charter draft ready for operator approval.
- **O6 — Cross-vault architecture note + forward-seed**: the **III ↔ SiteForge boundary statement** (III consumes audit signals; SiteForge owns audit execution); the **ADR-002 §3 minor-bump sweep** plan for the 5 active wrappers when the pack revises; the **ADR-003 graduation path** for the new web corrections (using the SS + wga + CC empirical evidence as the ≥2-vault basis); and how this work feeds the III thesis "III-tuned artifacts get better" and **relates to (does not block)** Campaign E. → architecture note (a proposal, not a ratified ADR).

## §4 Method (for the executing session)

- **Phase A — Research & Mine (parallelizable)**: (A1) external SOTA dossier per O1 (WebSearch / WebFetch); (A2) internal coverage map per O2 (Explore the III web pack + corrections + SiteForge gate framework + reviewers + ISS pack + VideoForge/CanvasForge); (A3) empirical gap ledger per O3 — read the 3 site dirs + their existing Lighthouse/Playwright reports by inspection.
- **Phase B — Synthesize (O4)**: turn the gap ledger + dossier + coverage map into the pack-revision spec — new-trap candidates, `source_diversity` remediation, correction candidates with evidence, and the audit-signal-ingest note.
- **Phase C — Charter & Architecture (O5 + O6)**: author the execution-campaign charter draft + the cross-vault architecture note; confirm the campaign letter + trap-priority + graduation-timing decisions with the operator (AskUserQuestion) before finalizing.

The executing session SHOULD use Explore/Plan subagents for Phase A and SHOULD reach operator alignment on §9's open questions before finalizing the charter draft.

## §5 Deliverables

1. External SOTA web/Astro design research dossier — `what/artifacts/wdr_external_sota_dossier.md` (or similar).
2. Internal lattice web-review coverage map — `what/artifacts/wdr_internal_coverage_map.md`.
3. Empirical gap ledger across ScienceStanley + wga + ContextCommons (inspection-grade) — `what/artifacts/wdr_empirical_gap_ledger.md`.
4. `web_design` pack-revision spec + new-trap/correction candidates (incl. `source_diversity` remediation + audit-signal-ingest note) — `what/artifacts/wdr_web_design_pack_revision_spec.md`.
5. **Execution-campaign charter draft** (boundary-preserving; P0–P4; mission table + DG criteria) — the primary output; `how/campaigns/campaign_web_design_deep_review/campaign_web_design_deep_review.md` (created at execution).
6. Cross-vault architecture note + forward-seed — `what/artifacts/wdr_cross_vault_architecture_note.md`.

## §6 Out of Scope (for this planning mission)

- **No edits to the live `context_iii_domain_packs_web_design.md`** — the pack revision is a *spec / proposal* (O4); the actual pack edit happens inside the execution campaign after the charter is approved.
- **No III ADR ratified** — the architecture note (O6) is a proposal; ADRs (if any) ratify in the execution campaign.
- **No audit runtime built into III** (boundary-preserving) — and the audit-signal-ingest seam is documented as a future option, not built.
- **No live audit runs / no site builds** (inspection-grade) — only existing reports on disk are read.
- **No consumer-wrapper edits** — the SiteForge `iii/` sweep (and the other 4 wrappers) is *planned*, not executed.
- **No learning-store mutation** — canonical jsonl md5 `5adb0dfa38d9224649c3b2cba83852ae` stays invariant; **zero graduation fires** (the new corrections are *candidates* in a spec, not appended entries).
- **No lattice-yaml / MANIFEST / root-CLAUDE / workspace-router changes.**
- **Does NOT open or charter the campaign** — it produces the charter *draft* for operator approval.
- **Not Campaign E** — generalized writing-III stays reserved/gated on `LiteratureForge.aDNA` reaching its forge-BUILD phase.

## §7 Reuse Map (read these; cite in the charter draft)

- **III web-design surface**: `what/context/core_domain_packs/context_iii_domain_packs_web_design.md` (5 traps; the `source_diversity=3` / `graduation_yield=1` weak axes); `what/context/core_domain_packs/iii_corrections_canonical.jsonl` (web corrections C-012…C-016, C-027); `what/context/core_domain_packs/context_iii_introspect_checks.md` (7 checks); `how/skills/skill_iii_review.md` (web dispatch + multi-voice orchestration trigger).
- **SiteForge mechanical layer + federation**: `SiteForge.aDNA/what/artifacts/sf_m02_quality_gate_framework.md` (10 gates incl. Lighthouse/axe/CWV); `SiteForge.aDNA/what/context/siteforge/siteforge_reviewers.yaml` (5 voices); `SiteForge.aDNA/iii/CLAUDE.md` (federation_ref pin `v0.3.0 / a309ad4`; `packs_used`); `SiteForge.aDNA/iii/what/context/context_iii_domain_packs_iss.md` (ISS pack).
- **Empirical corpus (inspection-grade)**: `ScienceStanley.aDNA/site/` + `.lighthouserc.cjs` + `tests/` (44 Playwright tests); `wga.aDNA/site/` + `lighthouserc.json` + Lighthouse reports (live at wga-site.vercel.app); `ContextCommons.aDNA/site/`.
- **Contracts**: `what/decisions/adr_002_consumer_federation_contract.md` (minor-bump wrapper sweep); `what/decisions/adr_003_learning_store_ownership.md` (graduation: frequency ≥3, acceptance ≥80%, ≥2-vault for elevated scrutiny).
- **Structure / conventions**: `how/missions/plan_literatureforge_genesis_planning.md` (MD-X2 — this card's structural template); the Campaign D charter under `how/campaigns/campaign_d_federation_adaptive_loop/` (charter skeleton); `how/templates/template_mission.md`; `how/templates/template_campaign.md`; `how/skills/skill_session_close_ceremony.md`.

## §8 Exit Gate

This planning mission (when executed) closes when: all 6 deliverables (§5) are authored + the operator has an execution-campaign charter draft to approve or modify. At that point the operator decides whether to open the campaign (P0). Hard invariants hold throughout execution: canonical jsonl md5 `5adb0dfa…` invariant; no consumer-wrapper / ADR / lattice-yaml mutation; no live audit runs. **For the present (card-authoring) session**, the gate is simply: card authored + committed with valid frontmatter, structure conforms to MD-X2, all §7 paths verified to exist, `status: planned`.

## §9 Open Questions for the Executing Session

1. **Campaign letter / slug** — propose **Campaign F**, slug `campaign_web_design_deep_review` (E is reserved/gated for generalized writing-III). Confirm letter, or run un-lettered with the descriptive slug.
2. **New-trap count + priority** — how many new traps to target (proposed candidates: CWV-as-design-debt · AI-slop-hero · responsive-breakpoint-neglect · motion/interaction-design-absence) and which to prioritize given the empirical gap ledger.
3. **Audit-signal-ingest seam** — operator chose boundary-preserving now. Keep the ingest seam as a *documented future option* (this campaign), or schedule a later campaign/ADR to actually build it?
4. **Graduation timing** — do the new web corrections graduate to canonical *within* this campaign (using SS + wga + CC as the ≥2-vault evidence) or stay candidates pending more cross-vault frequency?
5. **Scope of empirical review** — all 3 sites equally, or weight toward the live site (wga) and the most-instrumented site (ScienceStanley, 44 Playwright tests)?

## Notes

Authored as a standalone planning mission per operator decision (2026-05-25), structurally modeled on MD-X2 (`plan_literatureforge_genesis_planning.md`). Three operator decisions are locked into this card: **boundary-preserving** (III stays semantic; audits stay in SiteForge), **author-the-card-only** (execution is a separate gated session), **inspection-grade** (existing reports, no live runs). The card is `status: planned`; executing it produces the charter that, once approved, opens the campaign.

## AAR

_(empty — populated only when this mission is executed and closed.)_
