---
session_id: session_stanley_20260528_iii_adna_campaign_g_planning_charter
type: session
mission: plan_campaign_g_consolidation_charter
mission_class: campaign_planning_authoring
campaign: null                                       # standalone-interstitial; does NOT open Campaign G; mirrors plan_web_design_deep_review_charter 2026-05-25 + plan_literatureforge_genesis_planning (MD-X2 authoring) 2026-05-25 precedent
disposition: standalone_interstitial
status: completed
closed: 2026-05-28
created: 2026-05-28
operator: stanley
agent: agent_argus
boundary_posture: planning-only   # this session AUTHORS the planning card; no campaign opened, no pack content, no wrapper touched, no canonical mutation
evidence_method: structural        # mirrors plan_web_design_deep_review_charter.md + plan_literatureforge_genesis_planning.md frontmatter + section shapes
tags: [session, campaign_g_planning, operation_atrium, iss_surface_iii_pack_anchor, audit_ingest_seam_formalization, learning_store_pack_deepening, graduation_maturation_conditional, v0_5_0_target, planning_card_authoring, standalone_interstitial, non_gating, canonical_md5_invariant, zero_new_iii_adr, zero_consumer_wrapper_touch, eighteenth_canonical_close_ceremony]
---

# Session — Author Campaign G Planning-Mission Card (Operation Atrium)

## Goal

Author `how/missions/plan_campaign_g_consolidation_charter.md` with `status: planned` — the planning-mission card that, when **executed** in a future session, produces the Campaign G charter ("Operation Atrium"). Mirrors the standalone-interstitial precedent of `plan_web_design_deep_review_charter.md` (Campaign F's pre-charter card) and `plan_literatureforge_genesis_planning.md` (MD-X2 authoring): non-gating, no campaign tag opened, zero hard-invariant rotation.

## Operator intent (verbatim 2026-05-28)

> "Let's take a moment to review all of our campaign followups, open queue items, and potential next chapter items and comprehensively create a next campaign planning mission to be performed after this one that would integrate all these next step improvements into one strong campaign to bring us from .01 to .1 and then let's prioritize getting III support on a usability/readability/ui-ux design/objects/interaction/components context/personas/etc for the ISS system so the interaction surfaces our agents share with users are always as high quality / well composed / rich / useful for rlhf as possible."

## Pre-flight invariant snapshot

- `md5 what/context/core_domain_packs/iii_corrections_canonical.jsonl` → `5adb0dfa38d9224649c3b2cba83852ae`
- `wc -l iii_corrections_canonical.jsonl` → 28
- `lattice_iii_verification_oracle.lattice.yaml` `version: "1.2.5"`
- ADR file count → 10 (.md files in `what/decisions/`; 7 active III ADRs 000+001+002+003+005+007+008 + 3 legacy zeta-era + AGENTS.md)
- Active sessions → none (just `.gitkeep`)
- `git status` → clean
- III sits 6 commits ahead of `origin/main` post-VisualDNA coord intake (push operator-discretionary per workspace SR)

## Recon (parallel Explore + Plan agents at planning-mode entry)

- **ISS anatomy** (canonical skill `aDNA.aDNA/how/skills/skill_create_iss.md` + ADR-028 architecture AD-1..AD-10 + ADR-029 standard-touch ST-1..ST-6 + SiteForge `what/lib/iss/` README): single surface = gate identity + decision frame + 3–N options OR form fields + evidence panel (context/analysis/SWOT) + recommendation block (rationale/key_factors/risks/confidence/next_actions) + RLHF signal (schema v1.0) + persona/voice/skin cascade. 8 canonical HTML templates × 6 personas × 4 presets × 8-category adaptation guides.
- **ISS-adopter community DEFINITIVE state**: **2** live consumer wrappers (MoleculeForge `iss/CLAUDE.md` pinned at SiteForge `903f461` D10 close, `packs_used: []`; WilhelmAI `iss/CLAUDE.md`). RareHarness has **no** `iss/` directory yet (Explore-agent claim incorrect — verified at `ls /Users/stanley/aDNA/RareHarness.aDNA/iss/` returns "No such file or directory"). SiteForge HEAD at `59599c2`.
- **III has zero ISS coverage today** — no pack, no ADR, no wrapper, no backlog idea touching ISS. Verified across `what/context/core_domain_packs/`, `what/decisions/`, `how/backlog/`.
- **36-item inventory** consolidated into 5 themes: (1) graduation + cross-vault evidence maturation (C-029 ceremony, C-030..C-033 ≥2-vault evidence, Layer-2 AIRLOCK activations); (2) domain pack deepening (`learning_store` `source_diversity = 2`; `whitepaper_communication` Campaign E territory); (3) Campaign E forward-seed (gated on LiteratureForge.aDNA forge-BUILD); (4) ISS-surface III integration (anchor); (5) audit-signal ingest seam + VisualDNA bundle review (revisit v1.0 GA).

## Decisions ratified at planning-mode entry (Stanley 2026-05-28)

1. **This session AUTHORS the planning card only.** Campaign G is NOT chartered, NOT opened. The planning-mission EXECUTES in a future session per Standing Rule 7 human gate.
2. **Codename Operation Atrium** ratified (Plan-agent recommendation; sibling-callback to "Operation Tell"; atrium = where inputs meet + are composed before routing).
3. **Anchor priority = ISS-surface III pack** (T1). Per operator framing: "agent–operator interaction surfaces high-quality / well-composed / rich / useful for RLHF."
4. **4 IN tracks** (T1 ISS pack anchor; T2 audit-ingest seam doc-with-Trap-6-worked-example; T3 `learning_store` pack deepening MX-1 recipe replay; T4 graduation-maturation **conditional** on consumer-vault C-029 proposal).
5. **4 OUT** (Campaign E externally gated; VisualDNA absorption track-deferred to v1.0 GA; `whitepaper_communication` Campaign E territory; SiteForge ISS *runtime* — SiteForge owns).
6. **6 phases G0..G6 mirroring F0..F6** with 11 DG-G criteria mirroring DG-F scorecard shape.
7. **Version target `v0.5.0`** (clean minor bump preserves R3 documentation-grade posture; v1.0.0 deferred until executable enforcement lands).
8. **Zero new III ADR** in Campaign G (ISS pack is *consumed* pattern from SiteForge ADR-028/029; consumption-only discipline mirrors Campaign F's 9-of-11 D + 11-of-11 F precedent).
9. **Canonical jsonl md5 INVARIANT** in Campaign G **except** conditional DP-4 gate (if a consumer-vault C-029 proposal arrives, the canonical rotates; otherwise candidates-only continuation per Campaign F precedent).
10. **No outbound coord memos fired at planning-card authoring.** 4 memo intents documented in the card §8 fire at G0 of the executing campaign.

## Workflow

1. Read 5 structural template / reference files in parallel (read-only).
2. Author the planning-mission card (`how/missions/plan_campaign_g_consolidation_charter.md`) following the §-shape of `plan_web_design_deep_review_charter.md`:
   - Frontmatter (`kind`/`type`/`status`/`mission_class`/`campaign: null`/`designs_campaign`/`disposition: standalone_planning`/tags).
   - §1 Context & Why.
   - §2 The Campaign Being Planned — Vision.
   - §3 Objectives (O1..O6).
   - §4 Method (Phases A/B/C).
   - §5 Deliverables (numbered).
   - §6 Out of Scope.
   - §7 Reuse Map.
   - §8 Exit Gate.
   - §9 Open Questions for the Executing Session.
   - Notes.
3. Prepend STATE.md frontmatter `updated:` field + new Current Phase paragraph; demote prior to `**PRIOR — …**`.
4. Pre + post invariant verification.
5. Move this session file to `how/sessions/history/2026-05/`.
6. Single close commit with prefix `campaign_g_consolidation_charter_planning:`.

## Hard invariants (verify pre + post)

- Canonical jsonl md5 `5adb0dfa38d9224649c3b2cba83852ae` (28 entries) — INVARIANT.
- Lattice yaml `version: "1.2.5"` — UNCHANGED.
- `what/decisions/` ADR file count — UNCHANGED (10).
- All consumer wrappers (10 `iii/` carriers + 2 `iss/` carriers across the workspace) — UNTOUCHED.
- MANIFEST.md — UNTOUCHED (no version bump; no Release-History row).
- III root CLAUDE.md — UNTOUCHED (Campaign G is `planned` not `active`; Campaign State row stays off the table — Campaign F planning-card precedent).
- Workspace router `~/aDNA/CLAUDE.md` — UNTOUCHED (Standing Rule 7 surgical; planning cards do not promote into the router).
- No git tag cut.
- No `git push origin main` (operator-discretionary per workspace SR).

## Out of scope

- Executing the planning mission (Campaign G charter authoring happens in a future session).
- Authoring ISS-pack content (pack-content authoring is G1 of executing campaign per Campaign F F1 precedent).
- Firing the 4 outbound coord memos (fire at G0 of executing campaign).
- Reading the full SiteForge ISS library beyond README pin reference (deep recon is G0 of executing campaign).
- Touching any consumer wrapper / ADR / lattice yaml / canonical jsonl / MANIFEST / III root CLAUDE.md / workspace router.
- Cutting a `v0.5.0` git tag (G6 deliverable).
- `git push` to origin.

## AAR

- **Worked**:
  - Two parallel Explore agents at planning-mode entry (ISS surface anatomy + 36-item consolidated forward-queue inventory) returned tight, citation-grade reports under their <700/<1200-word ceilings; one Plan agent validated the campaign-card shape design (codename + 6 phases + 11 DG-G criteria + 5 risks + 4 outbound coord memos). Recon-to-card-author handoff was clean — no second exploration pass needed.
  - **Standalone-interstitial precedent reused cleanly**: `plan_web_design_deep_review_charter.md` (Campaign F pre-charter card) + `plan_literatureforge_genesis_planning.md` (MD-X2 authoring) both shipped with the same shape this session uses; the structural template absorbed Campaign G's multi-track-consolidation scope without §-shape distortion. Card sections §1-§9 + Notes mirror the precedent 1:1.
  - **Boundary discipline pre-locked at planning**: ADR-009 ISS-Surface Artifact Class candidate evaluated and **pre-disposed REJECTED** in the card (consumption-only ADR discipline; ISS is *consumed* pattern from SiteForge ADR-028/029, mirroring `web_design`'s lack of III-side structural ADR). Decision documented in card §6 so the executing session does not re-litigate at G0 — operator may override but defaults are crisp.
  - **Definitive ISS-adopter inventory** (2 wrappers: MoleculeForge + WilhelmAI; RareHarness has no `iss/` dir; verified by directory inspection) corrected the Explore agent's earlier 3-wrapper claim before it propagated into the card; card §1 + §7 + §9 carry the verified 2-wrapper number throughout. Avoids a stale-data trap-firing on the executing session's empirical gap ledger (Campaign F AAR follow-up F2 precedent re: ScienceStanley "44 tests" → actual 310).
  - **Cascade bookkeeping minimal + standalone-interstitial-conformant**: 3-file diff (planning card [new] + session [new] + STATE.md [edit]) + session-history move at close commit. MANIFEST.md UNTOUCHED (no version bump). III root CLAUDE.md UNTOUCHED (Campaign G is `planned` not `active` — Campaign State row stays off the table per Campaign F pre-charter precedent). Workspace router UNTOUCHED (Workspace SR-7 surgical).
  - **Eighteenth canonical close ceremony** absorbed cleanly as **third close ceremony on a single calendar day** (2026-05-28: MX-1 + VisualDNA coord intake + Campaign G planning-card). Demonstrates the close-ceremony skill scales to multi-close days without procedural drift.

- **Didn't**:
  - **No campaign opened, no charter authored**: by design per operator decision and standalone-interstitial precedent. Charter authoring (8 deliverables per card §5) executes in a future session at operator gate (DP-1). The 4 outbound coord memos remain UNDRAFTED (drafted at executing session's G0); 9 open questions in card §9 remain UNANSWERED (resolved at G0 with operator).
  - **No ISS-pack content authored**: anchor priority work begins at G1 of executing campaign. Trap selection from 9 candidate axes is itself a G0/DP-2 deliverable, not a planning-card deliverable.
  - **No `learning_store` pack edit**: T3 deepening (MX-1 recipe replay) is a G2 deliverable. Pack stays at composite 3.50 / `source_diversity = 2` until the executing campaign runs.
  - **No audit-ingest seam construction**: T2 documentation is a G2 deliverable. Even the schema-note skeleton is an O4 deliverable inside the executing campaign, not the planning card.
  - **No graduation fired**: T4 stays CONDITIONAL; canonical jsonl md5 INVARIANT preserved end-to-end. Even if a consumer-vault C-029 proposal exists today, the firing gate is DP-4 of executing campaign, not the planning card.

- **Finding**:
  Across Campaign F (DG-F close 2026-05-27) + MX-1 (2026-05-28) + VisualDNA coord intake (2026-05-28) + Campaign G planning-card (this session 2026-05-28), the standalone-interstitial mission pattern has compounded into a **mature consolidation primitive**. Each standalone preserves canonical hard-invariants while absorbing one previously-free-standing queue item into the next coherent push: MX-1 absorbed `vault_maintenance` from the post-DG-D queue; VisualDNA coord intake absorbed the parallel-framework signal; Campaign G planning-card now absorbs FOUR remaining queue items (T1+T2+T3+T4) into one not-yet-opened campaign. The "0.01 → 0.1" framing operationalizes as: each standalone-interstitial close + each campaign close shifts the queue from "scattered open ends" toward "coherent v0.5.0 push" without growing the canonical surface area. **The pattern is generalizable**: the standalone-interstitial + planning-card-then-execute discipline IS III's "small commits, big trajectories" recipe at the campaign-grain.

- **Change**:
  Two recipe-shape observations from this session for future planning-mission cards (especially multi-track consolidations like Campaign G):
  1. **Pre-dispose at the card, not the charter** — Campaign G's card pre-disposes ADR-009 REJECTED + codename Operation Atrium + version target v0.5.0 + audit-ingest doc-with-one-worked-example + graduation conditional. Default-crisp dispositions in the card with operator-override-at-G0 footnotes save the executing session from re-litigating settled questions. (Campaign F's planning card carried fewer pre-dispositions; the executing F0 spent a session on codename + trap-set ratification. Operation Atrium's G0 should be lighter.)
  2. **Definitive empirical numbers at planning-card time** — verify Explore-agent recon claims (2 vs 3 ISS adopters; pack composite scores; etc.) by direct inspection at planning before the number propagates into the card. Avoids re-work at executing session per Campaign F AAR Change item (ScienceStanley test-count drift).

- **Follow-up**:
  - **(a)** Open Campaign G at operator DP-1 gate — executes the card authored this session, produces the charter at `how/campaigns/campaign_g_consolidation/`, runs G0..G6 with `v0.5.0` release at DG-G close.
  - **(b)** When Campaign G opens: G0 ratifies codename (Operation Atrium default) + trap selection (≥5 from 9 candidate axes) + audit-ingest disposition (doc + one worked example default) + graduation conditional disposition (default) + version target (v0.5.0 default) + ADR-009 disposition (REJECTED default) — all 9 open questions in card §9 resolved at G0 with operator.
  - **(c)** Optional `git push origin main` (operator-discretionary; III sits 7 commits ahead post-Campaign-G-planning-card).
  - **(d)** Forward-seeded **Campaign E** (generalized writing-III) stays gated on LiteratureForge.aDNA forge-BUILD per MD-X2 §6 — **NOT** subsumed by Campaign G; Campaign G acknowledges via §G0 outbound coord memo to LiteratureForge.
  - **(e)** Revisit VisualDNA at v1.0 GA close (early-to-mid June 2026) per 2026-05-28 reply — Campaign G acknowledges via §G0 outbound coord memo to VisualDNA.
  - **(f)** SiteForge folds F4 pin into in-flight ISS commit at own cadence; Campaign G G4 wrapper-sweep may absorb the resolution.
  - **(g)** LPWhitepaper.aDNA wrapper re-pin at own cadence (likely concurrent with Campaign E).
  - **(h)** RareHarness.aDNA `iss/` directory provisioning (currently absent) — operator-discretionary; if provisioned by Campaign G G5, RareHarness becomes a 3rd validation target.
  - **(i)** Campaign F AAR follow-up (g) `template_session_bookkeeping.md` still standing — defer until next sub-1-hour bookkeeping-only commit slice.
  - **(j)** Hygiene: III root CLAUDE.md domain-pack-table descriptor "All org-vaults" (flagged stale at MX-1 close) — fold into Campaign G G6 cascade bookkeeping or earlier operator-discretionary cleanup.
