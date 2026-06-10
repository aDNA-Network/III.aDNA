---
type: skill
lattice_type: skill
skill_type: agent
created: 2026-05-20
updated: 2026-05-20
mission_origin: dg_c_carry_forward_interstitial (filed from campaign_c_airlock_standard MC-4.5 AAR §7 Follow-up)
status: active
category: hygiene
trigger: "Every session that closes a mission OR objective; explicit operator close request ('close the session', 'wrap up', 'commit and close'); fresh-agent session-handoff context. Skip for pure-read sessions that mutated nothing; for in-flight sessions still working; for parked-not-closed sessions (different pattern — leave status: active and add a parking note)."
last_edited_by: agent_argus
tags: [skill, session, close, ceremony, hygiene, drift_mitigation, mc4_lesson, audit_trail]
federation:
  discoverable: true
  source_instance: III.aDNA
  version_policy: minor
fair:
  keywords: [session_close, audit_trail, git_hygiene, drift_mitigation, closure_protocol]
  license: Apache-2.0
---

# Skill: Session Close Ceremony

## Overview

The 7-step protocol for closing a session file at the end of a mission, objective, or operator-requested close. Codifies what `how/sessions/AGENTS.md` "On end" block (lines 41-44) describes at high level, in executable form with bash snippets, verification checks, and anti-patterns.

**Why this skill exists**: III.aDNA Campaign C MC-4 substrate-enforcement work landed cleanly on disk 2026-05-13 but the close ceremony was skipped entirely — Execution log not populated, SITREP not written, status not flipped active → completed, `ended/closed_at` timestamps not added, session file not moved active/ → history/, no commit fired, STATE.md not refreshed. 7 days of disk-vs-git drift accumulated before MC-4.5's precondition check on 2026-05-20T20:31Z exposed the gap. Cleanup cost ~20-30 kT of unplanned content-load (retroactive close commit `c8ce621` + MC-4.5 split commit `8235837`). The close ceremony itself is small (~3-5 min wall-clock; ~5-10 kT) but easy to skip — work feels "done" at the last edit, but metadata + git operations + audit-trail propagation are still pending. Codifying it as a skill makes the steps mechanical and habit-forming.

**Companion documents**:
- `how/sessions/AGENTS.md` — the protocol source; this skill is its operational elaboration
- `how/backlog/idea_skill_session_close_ceremony.md` — the originating idea card (`status: adopted` at first use of this skill)

**Canonical exemplars in this vault**:
- Commit `8235837` (MC-4.5 SINGLE-SESSION CLOSE — alignment recon) — mitigation exemplar; the mission that surfaced the discipline finding self-honored the protocol
- Commit `1ea1c4d` (campaign_c_airlock_standard: MC-5 + DG-C CLOSE) — second exemplar; combined-gate variant (mission close + DG gate close in one commit)

**Canonical drift example** (what this skill prevents):
- Commit `c8ce621` (campaign_c_airlock_standard: MC-4 SINGLE-SESSION CLOSE (retroactive)) — the 7-day-drift cleanup; do not repeat

## When to Use

**Use this skill when**:
1. **A session is closing because a mission is done** — the mission's deliverables are all on disk and the session file is ready for `status: active → completed`.
2. **An objective closes** (multi-mission unit; rare at III but consistent with aDNA template) — same ceremony at the objective-close session.
3. **A DG (Definitive Gate) closes** — combined-gate variant (mission close + DG close in one commit); see exemplar `1ea1c4d`.
4. **A planning-class single-session interstitial closes** — MC-4.5 / closure-skill-interstitial pattern; same ceremony.
5. **Operator explicitly requests close** — "close the session", "commit and wrap", "we're done here".
6. **Fresh-agent session-handoff context** — agent inherits a session in `active/`, finishes the work, and closes it on behalf of the originating session.

**Do NOT use this skill when**:
- Session is mid-mission and merely pausing for a context-window break — leave `status: active`, append a `## Parking Note` block with handoff notes, do not move file, do not commit-and-close (a future session will resume and run this skill at actual close).
- Session was pure-read (no files modified or created) — no audit-trail artifact required; can be deleted from `active/` rather than promoted to `history/` (operator discretion; default keep for analytics).
- Mission isn't actually finished but you're tempted to declare close — that's the parking-not-closing anti-pattern; leave `status: active` and be honest about it in the Activity Log.

## Prerequisites

- **Working tree state**: all in-session edits complete; no unrelated edits accumulated in the working tree (run `git status` and confirm only this session's changes appear).
- **Mission scope**: the mission or objective is actually closed (deliverables on disk; charter row flipped if applicable; DG criteria checked if applicable). The ceremony does NOT close the mission — it closes the *session*. Confirm mission closure first.
- **Session file location**: file is at `how/sessions/active/<session_id>.md` (not already in `history/` — that would be a re-close attempt).
- **Authority**: operator approval where required by project standing orders (e.g., DG-gate combined closes need operator sign-off per Campaign C precedent).

## The 7-Step Recipe

Execute these steps in order. Do not skip any. Do not commit before step 7's STATE.md refresh — that creates an audit-trail asymmetry.

### Step 1 — Populate the Execution log (Activity Log section)

The Activity Log section in the session file must show ≥3 lines covering what was actually done. Include:
- Artifacts created/modified (with paths)
- Key pivots or operator gates
- Notable findings or surprises

Minimum quality bar: a fresh agent reading the log alone (without the SITREP) should understand what happened in rough strokes.

```markdown
- **HH:MMZ** — Session opened. Plan at /Users/stanley/.claude/plans/<plan>.md approved.
- **HH:MMZ** — Authored `<artifact>` (~N lines); covers <subjects>.
- **HH:MMZ** — Updated `<file>` (frontmatter + body section).
- **HH:MMZ** — Verification pass: <checks> all green.
```

### Step 2 — Write the SITREP and Next Session Prompt

Append the SITREP section below the Activity Log. Use the structure from `how/sessions/AGENTS.md` §Session Closure:

```markdown
## SITREP

### Completed this session

- <bullet per deliverable>

### In progress

- <bullet per partial or carry-over; or "None — clean close" >

### Next up

- <recommended next mission OR operator-gated decision>

### Blockers

- <enumerate blockers OR "None">

### Files touched

**Created**:
- <path>

**Modified**:
- <path>

**External-vault**: <"ZERO foreign-vault mutations" OR enumerate>

## Next Session Prompt

> <Self-contained paragraph that a fresh agent can read and continue from. Include: what was accomplished, what remains, key context paths, recommended approach.>
```

### Step 3 — Flip frontmatter `status: active → completed`

```bash
# In an editor or via Edit tool — find the frontmatter status field:
status: active
# Change to:
status: completed
```

### Step 4 — Add `ended:` and `closed_at:` ISO timestamps

Use UTC ISO 8601. The bash one-liner:

```bash
date -u +%Y-%m-%dT%H:%M:%SZ
```

Add both fields to the frontmatter (some sessions name them `ended` + `closed_at` for the slight semantic difference: `ended` = when work ceased; `closed_at` = when the ceremony itself fired; for most sessions they're seconds apart and identical):

```yaml
ended: 2026-05-20T23:30Z
closed_at: 2026-05-20T23:30Z
```

### Step 5 — Move session file from `active/` to `history/YYYY-MM/`

```bash
# YYYY-MM bucket per session's started: date (not the close date if they differ)
mkdir -p how/sessions/history/2026-05/
mv how/sessions/active/<session_id>.md how/sessions/history/2026-05/<session_id>.md

# If the file was already git-tracked (rare — it's usually new this session), use git mv instead:
# git mv how/sessions/active/<session_id>.md how/sessions/history/2026-05/<session_id>.md
```

`mv` vs `git mv`: new sessions added this session are untracked; `mv` is fine. Long-running sessions tracked from a prior commit need `git mv` to preserve git history. When in doubt, run `git ls-files --error-unmatch <path>` — exit 0 means tracked (use `git mv`), exit 1 means untracked (use `mv`).

### Step 6 — `git add` + `git commit` with conventional message

The III.aDNA commit message convention is `<campaign_or_scope>: <mission_id> <ACTION> — <descriptor>`. Examples in service:

| Pattern | Example commit |
|---------|----------------|
| Mission close | `campaign_c_airlock_standard: MC-4 SINGLE-SESSION CLOSE (retroactive) — substrate enforcement implementation guidance` |
| Combined mission + DG | `campaign_c_airlock_standard: MC-5 + DG-C CLOSE — v0.2 validation complete; Campaign C end-to-end 9/9` |
| Planning-class interstitial | `campaign_c_airlock_standard: MC-4.5 SINGLE-SESSION CLOSE — alignment recon (planning-class interstitial mirroring aDNA.aDNA M2.3.5)` |
| Carry-forward / vault-level | `dg_c_carry_forward: skill_session_close_ceremony — closure-discipline mitigation (DG-C AAR follow-up #1)` |

```bash
git add how/sessions/history/2026-05/<session_id>.md \
        <other-files-touched-this-session>

git commit -m "$(cat <<'EOF'
<scope>: <mission_id> <ACTION> — <descriptor>
EOF
)"
```

Single commit per session-close by default. Two commits only when a retroactive cleanup is bundled (see MC-4.5 precedent: `c8ce621` MC-4 retroactive close + `8235837` MC-4.5 close).

### Step 7 — Refresh STATE.md

Two edits to STATE.md:

1. **Frontmatter**: update `last_session` to this session's ID, `prior_session` to the previous one, `updated:` to today's date.
2. **Current Phase section**: add a new top-bullet documenting this session's close (date + commit + 1-sentence summary); demote the previous top-bullet to one-line summary form (operator may instruct full removal at DG-class closes; default = preserve for audit).

Frontmatter edit:

```yaml
updated: 2026-05-20   # close-of-session date
last_session: session_<user>_<YYYYMMDD>_<descriptor>
prior_session: <previous session_id>
```

Current Phase bullet form (lead with the close marker, then a compact summary of what landed):

```markdown
**<Mission_ID> ✅ CLOSED 2026-05-20** (commit `<short_hash>`; <1-sentence summary of deliverable + impact>).
```

## Verification

Run all checks before considering the close complete:

```bash
# 1. Working tree clean
git status                              # expect: nothing to commit, working tree clean

# 2. Commit landed and matches convention
git log -1 --oneline                    # expect: <hash> <scope>: <mission_id> <ACTION> — <descriptor>

# 3. Session file moved out of active/
ls how/sessions/active/                 # expect: empty OR only other sessions (not this one)

# 4. Session file present in history/YYYY-MM/
ls how/sessions/history/$(date -u +%Y-%m)/<session_id>.md   # expect: file exists

# 5. STATE.md updated date matches today
grep "^updated: $(date -u +%Y-%m-%d)" STATE.md              # expect: 1 match

# 6. STATE.md last_session points at this session
grep "^last_session: <session_id>$" STATE.md                # expect: 1 match
```

For DG-class closes, add a pre-flight grep step from the Campaign B AAR Change #1 process improvement (validated at MC-5 close 2026-05-20):

```bash
# Zero unflipped checkboxes in the campaign master at DG close
grep -c '^- \[ \]' how/campaigns/<campaign>/<campaign>.md   # expect: 0

# Zero unfired `(pending)` markers outside Status Log / AAR body
grep '(pending)' how/campaigns/<campaign>/<campaign>.md     # expect: only methodological references, no real pending statuses

# Frontmatter status matches body assertions
grep "^status: completed" how/campaigns/<campaign>/<campaign>.md   # expect: 1 match
```

## Anti-patterns

| Skip | Failure mode |
|------|--------------|
| **Skip Execution log** | Drift risk — future audit can't reconstruct what was actually done; cold-start agents lack context |
| **Skip SITREP** | Next-agent cold-start without continuity; "what was finished?" becomes a code-archaeology question |
| **Skip status flip (`active → completed`)** | File stays in `active/` blocking next session's collision check; sessions accumulate stale and the active-set becomes untrustworthy |
| **Skip `ended` / `closed_at` timestamps** | Audit trail loses temporal precision; "when did this actually close?" gets answered with `git log` of the parent commit, which may differ by hours/days |
| **Skip history move** | Same as skip status flip — file in wrong directory; quasi-completed sessions accumulate |
| **Skip commit** | **The MC-4 lesson** — disk-vs-git drift; work that's "done on disk" is invisible to other agents until committed; can accumulate days or weeks before a precondition check exposes it |
| **Commit before STATE.md refresh** | Audit-trail asymmetry — the commit announces the close but STATE.md still says the session is active; next agent reading STATE.md alone gets stale state |
| **Declare close while parking** | Wrong protocol — parking is `status: active` + a parking note; closing implies the work scope is genuinely done |
| **Skip pre-flight grep at DG close** | DG can close with unflipped checkboxes still in the campaign master; Campaign B AAR Change #1 was filed because this happened pre-mitigation |

## Cross-references

- `how/sessions/AGENTS.md` — protocol source (this skill is its operational elaboration; AGENTS.md describes the steps at high level, this skill is the executable recipe)
- `how/backlog/idea_skill_session_close_ceremony.md` — originating idea card (`status: adopted` at first use of this skill)
- Commit `c8ce621` — MC-4 retroactive close (canonical drift example; do not repeat)
- Commit `8235837` — MC-4.5 SINGLE-SESSION CLOSE (canonical mitigation exemplar; self-honoring planning-class interstitial)
- Commit `1ea1c4d` — MC-5 + DG-C CLOSE (combined-gate variant exemplar)
- `~/aDNA/aDNA.aDNA/how/campaigns/campaign_adna_serious_tool_readiness/missions/mission_adna_str_p2_m23_5_push_readiness_review.md` — cross-vault precedent for planning-class single-session shape (M2.3.5 push-readiness review)
- `~/aDNA/aDNA.aDNA/how/campaigns/campaign_adna_serious_tool_readiness/missions/artifacts/aar_m23_5_push_readiness_review.md` — M2.3.5 AAR
- `how/templates/template_session.md` — session-file template (frontmatter + sections this skill operates on)

## Upstream graduation potential

This skill is currently III.aDNA-canonical. The closure-discipline lesson generalizes across any aDNA vault — every vault has session files, every vault risks the same drift. A future III.aDNA v0.3+ cycle could promote `skill_session_close_ceremony.md` to `.adna/how/skills/` (the base template) via the standard upstream-contribution flow at `~/aDNA/.adna/how/skills/skill_upstream_contribution.md`. Promotion candidates evaluated by: ≥ 3 in-vault canonical applications (achieved at first use post-adoption + accumulating across Campaign D sessions); zero in-skill drift findings during the trial period; explicit operator + agent co-ratification.
