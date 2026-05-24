---
type: idea
created: 2026-05-23
updated: 2026-05-23
status: open
priority: medium
last_edited_by: agent_stanley
source_mission: node.aDNA campaign_lattice_compliance_upgrade M19.5 (Finding J)
target_subsystem: III.aDNA Campaign D MD-B3 (cross-vault RLHF aggregation)
tags: [backlog, iii, agentic_critic, image_evaluation, methodology, multi_persona, prompt_fragments, node_adna, cross_vault]
---

# III domain pack candidate: agentic image evaluation (4-persona consensus + versioned prompt fragments)

## Source

`node.aDNA` campaign_lattice_compliance_upgrade M19.5 (Agentic-III Image-Evaluation Experiment, 2026-05-22 / 2026-05-23). 10-cycle closed-loop inspect → introspect → improve over Imagen 4 ULTRA + Bio-Digital Cozy Metaverse aesthetic with 4 agentic personas as critics; produced a reusable infrastructure stack + a critical methodology finding.

## What was built (node-local; reusable as a III pack)

Three Python modules at `node.aDNA/what/code/`:

1. **`prompt_fragments.py`** — versioned named-fragment library (immutable `Fragment` dataclass with `name + version + source + content + notes`). `compose_prompt(fragment_names, positive_mod, negative_mod) -> str` assembles prompts; `bump_version(version, level)` + `register_fragment_version(...)` enforce immutability + history. `CALIBRATED_<mission>` bundle pattern for downstream-consumer locking.
2. **`agentic_critics.py`** — 4 persona definitions (Argus Panoptes + Look-Closer R7 + Belonging Architect R3+R6+R12 + Design Critic), each with a system_prompt_template grounded in canonical voice/style anchors. `run_persona(persona, image_path, prompt_assembled) -> PersonaResponse` calls Gemini-vision with persona-specific system prompt; YAML-parses structured score + critique + fragment-modification hint.
3. **`agentic_cycle_m19_5.py`** — 10-cycle driver: generate → 4-persona inspect → consensus-fragment + weakest-dim introspect → fragment-bump improve → JSONL log → budget pre-flight. Idempotent `--resume` mode.

Architecturally clean: **fragments are immutable**, **personas are stateless**, **the driver is the only state-holder** (current runtime fragment registry + cumulative cost). All four pieces independently testable.

## What the experiment found

### Finding G (load-bearing — agentic ≠ operator)

The 4-persona agentic consensus did NOT match operator preference. The agents' favorite cycle (avg 96.2 across all 4) was beaten under operator's eye by the cycle_01 baseline (avg 94.0; v1.0.0 fragments untouched). The operator's pick became the locked CALIBRATED_M19_5 bundle = exactly the baseline, no overrides.

**Implication**: Rubric-driven optimization can over-engineer a working baseline. Any future agentic-III pack must either (a) include an "operator-eye proxy" persona weighted heavily, OR (b) treat the baseline as the test condition that any modification must DEMONSTRABLY beat (not just score-better against), OR (c) require operator-in-the-loop checkpoints at small cycle counts (e.g., every 3 cycles, not just at cycle 10).

### Finding H (operationally useful — accretion damage)

The III loop fixated on one fragment (`ss_embedded_diegetic_ui` cycles 5–10) and applied 6 successive modifications. Each cycle appended a clause; by cycle 10 the prompt was over-engineered and avg score fell from 96.2 (c3 peak) → 75.0 (c10). The single-fragment-bump-per-cycle rule + consensus-voting let one fragment monopolize attention.

**Fix candidates for the pack**:
- Cap modifications-per-fragment at N (e.g., 2) then force-rotate to a different fragment.
- Measure prompt-length elasticity: if prompt grew by ≥X chars in last N cycles without score gain, back off.
- Require consensus-voter rotation: if persona P voted fragment F at cycle N-1 and again at cycle N, weight that vote lower.

### Finding I (lower priority — robustness)

Design Critic persona had 2 unparseable YAML responses across 10 cycles (cycles 5 + 10; `mapping values are not allowed here`). Cause: free-text fields with literal colons broke unquoted YAML. Loop recovered (continued with the other 3 personas) but lost signal.

**Fix**: require persona responses to wrap free-text fields in double-quotes OR escape colons before YAML-parsing.

## Why this is a III.aDNA pack candidate

- **Multi-modal**: extends III beyond Markdown/CLAUDE.md inspection into image content evaluation — a natural III domain.
- **Cross-vault reusable**: any vault generating images (SS portraits + RareHarness clinical-art + CanvasForge panels + node.aDNA vault cards) needs structured image evaluation. The 4-persona + versioned-fragments + cycle-driver pattern transfers.
- **Calibrated against negative result**: Finding G is exactly the kind of "agentic loop did not improve baseline" evidence Campaign D MD-B3 cross-vault RLHF aggregation wants. The pack ships with documented limitations + Fix candidates H+I that any consumer can apply or extend.
- **Composable with existing III primitives**: re-uses III's V/F/Fmt/Ext rubric (re-anchoring Ext per domain), spec_iii_hitl_cycle_protocol's regenerate-with-feedback semantics, ADR-005 RLHF signal channel pattern.

## Proposed pack shape

`III.aDNA/what/context/core_domain_packs/image_content_evaluation/`:

```
README.md                    # Domain anchor + when to invoke
persona_roster.md            # Canonical 4-persona contract + extension pattern
fragment_library_spec.md     # Versioned named-fragment library API (Fragment dataclass + compose_prompt + bump_version)
cycle_driver_spec.md         # Closed-loop inspect/introspect/improve with accretion guard + voter rotation
yaml_response_schema.md      # PersonaResponse YAML schema + escape rules
examples/
  m19_5_lattice_network/     # The node.aDNA M19.5 run as a worked example (10 images, JSONL, ISS, outcome)
```

## Cross-vault sequencing

1. **Now (low-effort)**: file this idea as a III.aDNA backlog entry (this file).
2. **MD-B3 surface**: when Campaign D's cross-vault RLHF aggregation mission opens, this is candidate input — the 4 persona-as-critic JSONL rows + operator-rejection signal are RLHF data points already aligned with ADR-005.
3. **Pack promotion**: when ≥1 other vault uses the pattern (e.g., SS uses it for portrait-card generation, or CanvasForge for panel evaluation), promote the node-local code to `III.aDNA/what/context/core_domain_packs/image_content_evaluation/` as canonical.
4. **Apply Finding H + I fixes** before pack promotion — node.aDNA can run a M20a follow-up to validate fixes if budget permits.

## Affected files

- New: `III.aDNA/how/backlog/idea_agentic_image_evaluation_pack.md` (this file)
- Future promotion target: `III.aDNA/what/context/core_domain_packs/image_content_evaluation/` (TBD)
- Reference implementations (node.aDNA-local; remain at node until pack promotion):
  - `node.aDNA/what/code/prompt_fragments.py`
  - `node.aDNA/what/code/agentic_critics.py`
  - `node.aDNA/what/code/agentic_cycle_m19_5.py`
  - `node.aDNA/what/code/build_m19_5_range_sis.py`
- Worked example (run + data):
  - `node.aDNA/who/assets/pilot/agentic_m19_5/cycle_NN.jpg` × 10
  - `node.aDNA/iii/what/context/agentic_cycle_m19_5.jsonl`
  - `node.aDNA/how/gates/m19_5_range_review.{html,data.json,output.json}`

## Severity / urgency

Medium. Not blocking node.aDNA's compliance-upgrade campaign (M20 proceeds with CALIBRATED_M19_5 = baseline). Promotes naturally when MD-B3 opens or a second vault adopts the pattern.
