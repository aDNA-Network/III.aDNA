---
type: mission_artifact
artifact_class: schema_note
title: "Campaign G G0 — Audit-signal-ingest schema note (T2, doc + Trap-6 worked example) (Operation Atrium)"
campaign: campaign_g_consolidation
mission: g0_charter_recon
mission_class: campaign_planning_execution
status: draft
version: "0.1.0"
created: 2026-05-29
updated: 2026-05-29
last_edited_by: agent_argus
boundary_posture: boundary_preserving   # DOCUMENTED-ONLY schema; NO III audit runtime; NO validator built; NO audit-execution ADR
evidence_method: inspection_grade
non_runnable: true
landing_target: G2
formalizes: ["Campaign F WDR-4 §5 audit-signal-ingest seam", "Campaign F AAR follow-up (h)"]
governed_by: [adr_028_iss_architecture, adr_007_adaptive_improvement_loop]
tags: [mission_artifact, campaign_g, operation_atrium, audit_ingest, schema_note, trap6_worked_example, t2, boundary_preserving, g0]
---

# G0 — Audit-Signal-Ingest Schema Note (T2)

> **Deliverable #6 of 8** (card §5.6). Documents the schema by which III **reads** SiteForge audit JSON (Lighthouse / axe / CWV) as `web_design` Trap-6 input, with Trap 6 (CWV-as-Design-Debt) as the canonical **worked example**. **This is the one worked example that turns Campaign F's documented-only seam (WDR-4 §5) into a load-bearing reference.** Boundary HELD end-to-end: **III never runs an audit; III reads numbers SiteForge already produced; no validator is built; no audit-execution ADR.**

## §1 Why this note exists

Campaign F landed Trap 6 (CWV-as-Design-Debt) with the read-side discipline *"read the existing Lighthouse JSON SiteForge already writes — do not re-run the audit"* (pack L127/L131/L165), and WDR-4 §5 documented the audit-signal-ingest seam *conceptually* but deferred the schema. F AAR follow-up (h) carried "formalize the ingest seam" forward. T2 closes that: it specifies the **field map** (which keys III reads) so the worked example is reproducible, without crossing the boundary into audit execution.

## §2 Boundary contract (firm, restated)

| Layer | Owner | Action |
|---|---|---|
| **Audit execution** | SiteForge (Gate 3 Lighthouse · Gate 4 axe · Gate 7 CWV; `sf_m02_quality_gate_framework.md`) | **runs** the audits, **writes** the JSON |
| **Signal ingest (read)** | III `web_design` Trap 6 (+ adjacent) | **reads** the on-disk JSON; **attributes** the number to a design decision |
| **NOT built** | — | No III audit runtime · no validator · no re-run · no audit-execution ADR |

III's relationship to audit JSON is identical to a reviewer reading a test report: the number is evidence; the *attribution* is the semantic work.

## §3 Lighthouse `report.json` field map (verified against on-disk reports)

Verified against `aDNA.aDNA/site/evidence/lighthouse*.json` (real reports on disk). III reads:

| Signal | JSON path | Type | Trap-6 use |
|---|---|---|---|
| Performance score | `categories.performance.score` | 0.0–1.0 | sub-1.0 → investigate design attribution |
| Accessibility score | `categories.accessibility.score` | 0.0–1.0 | cross-ref `web_design` Trap 3 (semantic a11y) |
| **LCP** | `audits['largest-contentful-paint'].numericValue` (ms) + `.score` + `.displayValue` | ms | "good" < 2500ms; breach → heavy-hero / eager-vs-lazy mismatch |
| **CLS** | `audits['cumulative-layout-shift'].numericValue` + `.score` | unitless | "good" < 0.1; breach → unsized media / webfont reflow |
| **TBT / INP proxy** | `audits['total-blocking-time'].numericValue` (ms) | ms | "good" INP < 200ms; high TBT → over-hydration / `client:load` below-fold |
| Speed Index / FCP | `audits['speed-index' | 'first-contentful-paint'].numericValue` | ms | corroborating signals |
| **Design-attribution diagnostics** | `audits['unsized-images' | 'uses-responsive-images' | 'offscreen-images' | 'font-display' | 'render-blocking-resources' | 'unused-javascript'].{score, details.items[]}` | mixed | the *causal trace* — which component/decision drove the breach |

**Ingest rule:** read `categories.*.score` for the headline, the three CWV `audits[*].numericValue` for the metric, and the diagnostic `audits[*]` for the design-attribution trace. **Never** instantiate Lighthouse; **only** parse an existing file.

## §4 axe-core JSON field map (for `web_design` Trap 3 cross-ref)

| Signal | JSON path | Trap-3 use |
|---|---|---|
| Violations | `violations[].{id, impact, help, nodes[].target}` | each violation → semantic a11y attribution (alt *meaning*, heading *logic*, SC 2.5.8) |
| Impact | `violations[].impact` (minor/moderate/serious/critical) | severity ranking |

(axe ingest is adjacent to T2's Lighthouse focus; documented for completeness — Trap 3 already owns the residue per `web_design` Gate-4 row.)

## §5 Trap-6 worked example (the load-bearing instance)

**Input:** a SiteForge-produced `lh-*.json` for a page with `categories.performance.score = 0.9X` (green-ish) but `audits['cumulative-layout-shift'].numericValue = 0.168` (CLS breach).

**III read (no re-run):**
1. Parse the on-disk JSON; read `cumulative-layout-shift.numericValue = 0.168` (> 0.1 → breach).
2. Trace via diagnostics: `unsized-images` flags the shared `ContentImage.astro`; grep confirms `loading="lazy"` with no intrinsic `width`/`height` (ScienceStanley `ContentImage.astro:19`).
3. **Attribute:** not a glitch — a *component decision* that compounds across every page using `ContentImage`. → Trap 6 fires with `web_design` pack L127 verbatim.
4. **Boundary check:** III read the number SiteForge wrote; III did not run Lighthouse. ✓

This is the canonical example the `web_design` pack's Trap-6 detection-method paragraph references; T2 gives it a reproducible field-map so any reviewer can replay it.

## §6 What T2 does NOT do (boundary)
- Does **not** build a parser/validator (the field map is documentation; a human/agent reads the JSON).
- Does **not** re-run any audit.
- Does **not** add an audit-execution ADR (consumption-only; ADR-009 pre-disposed REJECTED).
- Does **not** add a runtime to III.
- v0.5+ executable-ingest construction stays out per Campaign F AAR follow-up (h).

## §7 Cross-references
Formalizes: `campaign_web_design_deep_review/` WDR-4 §5 (`wdr_cross_vault_architecture_note.md`); F AAR follow-up (h). Trap source: `context_iii_domain_packs_web_design.md` Trap 6 (L127/L131/L165) + Gate→Trap residue table. SiteForge: `sf_m02_quality_gate_framework.md` (Gate 3/4/7). On-disk evidence: `aDNA.aDNA/site/evidence/lighthouse*.json`. Sibling: `g0_iss_pack_revision_spec.md` (#4); `g0_cross_vault_architecture_note.md` (#8).
