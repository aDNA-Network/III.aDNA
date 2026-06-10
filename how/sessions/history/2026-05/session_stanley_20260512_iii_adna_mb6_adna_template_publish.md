---
type: session
session_id: session_stanley_20260512_iii_adna_mb6_adna_template_publish
created: 2026-05-12
updated: 2026-05-12
operator: stanley
agent: argus
campaign: campaign_b_iii_federation
mission: MB-6
status: closed
closed: 2026-05-12
tags: [session, iii, federation, adna_template, mb6]
---

# MB-6 — adna-template publish of `skill_iii_setup.md`

## Mission

Author `skill_iii_setup.md` (consumer onboarding skill for adding an `iii/` wrapper to a new vault), land it at `III.aDNA/how/skills/skill_iii_setup.md` (canonical), and publish to `.adna/how/skills/skill_iii_setup.md` (base template — public repo, additive-only per Campaign B R5). Update the workspace `~/aDNA/CLAUDE.md` Framework Ecosystem section to point at the new skill and refresh the III.aDNA maturity column.

## Scope

- **In scope**: author the skill; publish to adna template; one targeted edit to workspace CLAUDE.md; flip Campaign B registers (charter MB-6 row + DG-B box + Phase Plan P3; STATE.md; MANIFEST frontmatter).
- **Out of scope**: any wrapper file changes at existing consumers (MB-1..MB-5 wrappers stay byte-identical); no git tag bump (no canonical surface change — skill is additive); no remote pushes (Stanley pushes when ready).

## Inputs

- 5 live wrapper precedents — `lattice-labs/iii/CLAUDE.md` (MB-1, lateral domain_pack + learning_store_local), `SiteForge.aDNA/iii/CLAUDE.md` (MB-2, reviewer_registry), `VideoForge.aDNA/iii/CLAUDE.md` (MB-3, bridge_pack), `CanvasForge.aDNA/iii/CLAUDE.md` (MB-4, bridge_pack + local_skill + learning_store_local), `wga.aDNA/iii/CLAUDE.md` (MB-5, minimal baseline).
- ADR-002 §1 + §1a — federation_ref schema + `kind:` enum (5 values) + extension policy. Amended at MB-7 2026-05-12.
- ADR-003 §2..§4 — learning-store ownership + canonical-jsonl-as-read-only + graduation ceremony + correction-entry schema. Amended at MB-7 2026-05-12.
- `skill_iii_review.md` — style reference for skill frontmatter + section layout.
- `.adna/how/skills/skill_project_fork.md` — style reference for adna-template-published onboarding skills.

## Procedure

1. Author canonical `skill_iii_setup.md` at III.aDNA.
2. Copy to `.adna/how/skills/`.
3. Edit workspace CLAUDE.md Framework Ecosystem section.
4. Flip Campaign B charter (MB-6 row + DG-B + P3).
5. Update MANIFEST.md frontmatter (`mb6_closed: 2026-05-12`).
6. Update STATE.md (Current Phase summary, Campaigns table P3, mission queue MB-6 row, What's Working, Blockers, frontmatter `last_session`, new Latest Direction § for MB-6).
7. Move session to `how/sessions/history/2026-05/`.
8. Commit III.aDNA + .adna; report to operator.

## Closure invariants (target at session end)

- `III.aDNA/how/skills/skill_iii_setup.md` exists with full skill body
- `.adna/how/skills/skill_iii_setup.md` byte-identical to canonical
- Workspace `CLAUDE.md` Framework Ecosystem section references the new skill
- Campaign B charter MB-6 row ✅ COMPLETE 2026-05-12; DG-B MB-6 box ✅; Phase Plan P3 row updated
- STATE.md current phase reflects MB-6 ✅; only MB-8 + DG-B gate remain in P3
- No git tag bump
- No remote push

## Closure

**Closed**: 2026-05-12.

**Deliverables landed**:
1. `~/aDNA/III.aDNA/how/skills/skill_iii_setup.md` — 395-line canonical consumer-onboarding skill (federation_ref skeleton; packs decision table; `kind:` enum walkthrough for all 5 values per ADR-002 §1a; 10-step procedure; 4 wrapper variants; Worked Precedents table covering all 5 live MB-1..MB-5 wrappers).
2. `~/aDNA/.adna/how/skills/skill_iii_setup.md` — byte-identical published copy at adna base template (`LatticeProtocol/adna` public remote). `diff -q` clean.
3. `~/aDNA/CLAUDE.md` Framework Ecosystem section — "Consumer integration pattern" paragraph gained a pointer at the new skill; III.aDNA maturity column rewritten Genesis → Production with full closure timeline.
4. Campaign B charter — DG-B MB-6 box ✅; mission table MB-6 row ✅; Phase Plan P3 updated; frontmatter `mb6_closed: 2026-05-12`.
5. III.aDNA `MANIFEST.md` frontmatter `mb6_closed: 2026-05-12`.
6. III.aDNA `STATE.md` — Current Phase summary, Campaigns table, Campaign B mission queue row, What's Working bullet, Blockers paragraph, frontmatter `last_session`, new Latest Direction § — all updated.

**Verification gates** (5/5 green):
1. Canonical file exists at 395 lines ✅
2. Adna template copy byte-identical ✅
3. Workspace CLAUDE.md references `skill_iii_setup.md` (2 hits) ✅
4. Campaign B charter MB-6 row ✅ COMPLETE 2026-05-12 ✅
5. STATE.md Current Phase + Campaigns table + mission queue all reflect MB-6 ✅ ✅

**No git tag bump** — additive skill addition; no canonical surface (lattice yaml v1.2.1 / ADRs / canonical jsonl md5) touched.

**No remote push** this session. Local commits in III.aDNA + adna repos; workspace CLAUDE.md is not in a git repo. Stanley pushes when ready (mirroring the v0.1.0 / v0.2.0 III.aDNA release cadence).

**Next session**: MB-8 LPWhitepaper wrapper (closes Campaign B P3 entirely; `skill_iii_setup.md` becomes its **first test in production**) OR MC-4 substrate enforcement (closes Campaign C P2). Stanley signals.
