---
type: backlog_idea
status: adopted
priority: medium
created: 2026-05-20
updated: 2026-05-20
adopted_at: 2026-05-20
adopted_into: how/skills/skill_session_close_ceremony.md
adopted_at_commit:   # populated post-commit; see DG-C carry-forward interstitial close 2026-05-20
last_edited_by: agent_argus
filed_from: what/artifacts/mc4_5_alignment_recon_dossier.md §7 AAR Follow-up
tags: [backlog, skill, session_close, closure_ceremony, mc4_drift_lesson, dg_c_carry_forward, hygiene, adopted]
---

> **ADOPTED 2026-05-20** — promoted from backlog idea to canonical skill at `how/skills/skill_session_close_ceremony.md` via the DG-C carry-forward interstitial session (`session_stanley_20260520_iii_adna_skill_session_close_ceremony`). Per §Sequencing default ("DG-C carry-forward — keeps the lesson tightly bound to its origin and lands the mitigation before any Campaign D high-velocity work begins"), the skill landed BEFORE Campaign D charter opens. The interstitial itself executes the new 7-step protocol verbatim on its own session-close (the MC-4.5 self-reference pattern), making it the first canonical application of the skill it authors.


# Idea: `skill_session_close_ceremony.md`

## Problem

III.aDNA Campaign C MC-4 substrate-enforcement work landed on disk 2026-05-13 but **the close ceremony was skipped entirely**: Execution log not populated, SITREP not written, status not flipped active → completed, `ended/closed_at` timestamps not added, session file not moved active/ → history/, no commit fired, STATE.md not refreshed at the time. 7 days of disk-vs-git drift accumulated before MC-4.5 precondition check on 2026-05-20T20:31Z exposed the gap — operator-confirmed 2-commit cleanup at MC-4.5 (`c8ce621` MC-4 retroactive close + `8235837` MC-4.5 close). The cleanup added ~20-30 kT unplanned content-load to the MC-4.5 session that should otherwise have stayed within band.

The closure-ceremony steps are listed in `how/sessions/AGENTS.md` as protocol but are not enforced anywhere. When a session is high-velocity work (MC-4's substrate-implementation artifact was 490 lines of dense spec authoring), the close ceremony is the part that's easiest to skip — the work feels "done" at the last edit, but the metadata + git operations + audit-trail propagation are still pending.

This is generalizable across all III missions (and likely across the wider lattice). The MC-4.5 AAR §7 surfaced it as a load-bearing finding and queued `skill_session_close_ceremony.md` as a DG-C carry-forward candidate.

## Proposed Solution

Author `how/skills/skill_session_close_ceremony.md` codifying the **7-step close protocol**:

1. **Populate Execution log** — at minimum, mention the artifacts modified/created and any in-session pivots
2. **Write SITREP** — Completed / In progress / Next up / Blockers / Files touched (modified + created)
3. **Flip frontmatter `status: active → completed`**
4. **Add `ended:` + `closed_at:` timestamps** to frontmatter
5. **Move session file from `how/sessions/active/` → `how/sessions/history/YYYY-MM/`**
6. **`git add` + `git commit`** with conventional message (`campaign_<X>: M<N> CLOSE — <descriptor>`)
7. **Refresh STATE.md** — add CLOSED bullet at top of Current Phase section; demote prior bullet to concise form (optional Op-3 archive-on-close pattern from aDNA.aDNA M2.1)

Skill body includes:
- §Trigger conditions: "every session that closes a mission OR objective; explicit operator close request; fresh-agent session-handoff context"
- §Preconditions: working tree clean (no unrelated edits); all in-session edits complete
- §7-step recipe with bash snippets where helpful (e.g., `git mv` or `mv` depending on tracked status; `date -u +%Y-%m-%dT%H:%M:%SZ` for `closed_at` ISO timestamp)
- §Verification: post-commit `git log -1` matches message convention; `git status` clean; STATE.md `updated:` is current date
- §Anti-patterns: skip Execution log → drift risk; skip SITREP → next-agent cold-start without continuity; skip status flip → file stays in active/ blocking next session's collision check; skip commit → disk-vs-git drift (the MC-4 lesson)
- §Cross-references: `how/sessions/AGENTS.md` (protocol source) + MC-4 retroactive close commit `c8ce621` (canonical drift example) + aDNA.aDNA M2.3.5 close ceremony (canonical exemplar)

## Sequencing

Graduation moment options:

- **At DG-C close** (post-MC-5) — Campaign C carry-forward; skill drafted as part of DG-C AAR follow-up.
- **At Campaign D planning** — operator's "modular + adaptive + RLHF + improvement" framing has a natural seam for hygiene skills like this; could absorb as MD-0 or pre-Campaign-D infrastructure.
- **As a standalone post-DG-C interstitial** — pattern from MC-4.5 itself (planning-class single-session interstitial); ~0.5 sess to draft + ratify + cross-link.

Operator picks at MC-5 plan ratification OR DG-C close OR Campaign D planning. Default sequencing recommendation: **DG-C carry-forward** — keeps the lesson tightly bound to its origin and lands the mitigation before any Campaign D high-velocity work begins.

## Why now / why this idea matters

MC-4's 7-day drift cost ~20-30 kT of unplanned cleanup work + delayed every downstream commit by a week + masked the campaign's actual close-state from cross-vault agents during the gap. The closure-ceremony itself is small (~3-5 min wall-clock; ~5-10 kT) but easy to skip; codifying it as a skill makes the steps mechanical and habit-forming.

The mitigation is **demonstrated by MC-4.5 itself**: the MC-4.5 close ceremony executed all 7 steps in full (Activity log + SITREP + status flip + ended/closed_at + history move + commit `8235837` + STATE.md refresh). The MC-4.5 dossier §7 explicitly calls this out as Standing Order #8 self-reference — *the mission that surfaces the closure-discipline finding is itself the canonical exemplar of closure discipline*.

## Cross-references

- MC-4.5 dossier §7 AAR Follow-up (skill candidate surface): `what/artifacts/mc4_5_alignment_recon_dossier.md`
- MC-4 retroactive close commit (canonical drift example): commit `c8ce621` in this repo
- MC-4.5 close commit (canonical mitigation exemplar): commit `8235837` in this repo
- Existing session protocol: `how/sessions/AGENTS.md` (the protocol that needs the skill scaffold to be executable, not just documented)
- aDNA.aDNA M2.3.5 close ceremony precedent: `~/lattice/aDNA.aDNA/how/campaigns/campaign_adna_serious_tool_readiness/missions/mission_adna_str_p2_m23_5_push_readiness_review.md` + `~/lattice/aDNA.aDNA/how/campaigns/campaign_adna_serious_tool_readiness/missions/artifacts/aar_m23_5_push_readiness_review.md`
- aDNA.aDNA M2.1 Op-3 archive-on-close pattern (5th canonical instance precedent at M2.3.5; complements the close-ceremony steps with STATE.md router maintenance): `~/lattice/aDNA.aDNA/how/campaigns/campaign_adna_serious_tool_readiness/missions/artifacts/m21_obj4_archive_convention_design.md`

## Upstream graduation potential

If the skill proves valuable in III, the same closure-discipline applies to any aDNA vault. A v0.3+ or later cycle could promote `skill_session_close_ceremony.md` to `.adna/how/skills/` (the base template) so every new aDNA vault inherits it. Promotion follows the standard upstream-contribution flow per `.adna/how/skills/skill_upstream_contribution.md`.
