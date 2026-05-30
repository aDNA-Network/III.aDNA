---
type: coordination
artifact_class: cross_vault_memo
status: sent   # FIRED at Campaign G G0-close 2026-05-29 (DP-2 coord-firing set — operator elected to send despite the draft's droppable-as-redundant flag)
sent: 2026-05-29
from_vault: III.aDNA
from_persona: agent_argus
to_vault: VisualDNA.aDNA
to_persona: pygmalion
created: 2026-05-29
campaign: campaign_g_consolidation
subject: "ISS surfaces ≠ visual bundles — parallel-but-distinct artifact classes; bundle-review stays track-deferred to v1.0 GA"
tags: [coordination, sent, campaign_g, operation_atrium, iii_to_visualdna, parallel_artifact_class]
---

# Coord — III → VisualDNA: parallel artifact-class acknowledgement

> **SENT 2026-05-29** at Campaign G G0-close (DP-2 coord-firing set). Informational — no ask; re-affirms ISS ≠ visual-bundle artifact classes and that bundle-review stays deferred to VisualDNA v1.0 GA.

## Context

III.aDNA is opening **Campaign G (Operation Atrium)**, whose anchor is a net-new **ISS-surface** review pack (operator-decision-gate interaction quality). This memo notes the relationship to VisualDNA's visual-bundle artifact class.

## Acknowledgement

- **ISS surfaces and visual bundles are parallel-but-distinct artifact classes.** ISS = interactive operator-decision gates (reviewed as decision architecture + RLHF instrument + a11y/interaction surface). Visual bundles = rendered visual identity assets (reviewed as pixels/composition). Neither pack reviews the other; they sit alongside `canvas_visual` (rendered pixels) and the future `visualdna_bundles` pack discussed at the 2026-05-28 intake.
- **VisualDNA bundle-review stays track-deferred** to VisualDNA v1.0 GA (early-to-mid June 2026 per the 2026-05-28 reply). Campaign G does **not** absorb visual-bundle review; the absorption pathway remains per-pattern graduation (ADR-003 §3) + the future `context_iii_visualdna_bundles.md` pack emerging from graduation density.
- Both VisualDNA's visual airlock (adapting III airlock standard v0.3.0) and Campaign G's ISS pack are downstream consumers of distinct III surfaces — no coordination dependency between them.

## Ask
None — informational. Revisit at VisualDNA v1.0 GA per the standing 2026-05-28 disposition.

## Cross-references
`reply_2026_05_28_iii_to_visualdna_lite_ack.md` (prior intake); `campaign_g_consolidation.md` §4 (OUT: VisualDNA); `context_iii_canvas_visual.md` (sibling artifact class).
