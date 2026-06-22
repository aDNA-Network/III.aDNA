---
type: wrapper
wrapper: git
created: 2026-06-22
updated: 2026-06-22
status: active
last_edited_by: agent_stanley
tags: [wrapper, git, federation, git_provider, github, public, p_released, wave_2]
---

# `git/` — III.aDNA consumer wrapper (federates Git.aDNA)

Federates **Git.aDNA** (Grace Hopper) for platform-agnostic git/forge/CI-CD ops. Applied at the Git.aDNA **P6 Wave 2** public-flip (2026-06-22, DP5-gated). Canonical schema: `Git.aDNA/what/specs/spec_gitops_provider_abstraction.md` §7. Git-Ops doctrine block lives in `../CLAUDE.md` under `## Git-Ops`.

```yaml
federation_ref:
  source_vault: Git.aDNA
  source_skill: how/skills/skill_git_provider_config.md
  version: "0.1.0"
  version_policy: minor
  pinned_at_commit: "6b7a559"            # Git.aDNA HEAD at apply
  binds_adrs: [adr_004, adr_006, adr_007, adr_008, adr_009, adr_011, adr_013]
  verbs_exposed: [create-repo, set-remote, push, open-pr, cut-release, configure-mirror, port-ci]
  local_extensions:
    - kind: non_contract_verb            # used at this flip
      name: set-visibility               # gitops_set_visibility (ADR-013 D4 release open-flow; contract unchanged)
git_provider:
  host: github.com             # released-FOSS public home (ADR-013 D3)
  backend: github
  org: aDNA-Network
  visibility: public           # P-released — flipped private→public at Wave 2 (2026-06-22)
  class: P
  lfs: false
  remotes:
    origin: https://github.com/aDNA-Network/III.aDNA.git   # unchanged by the flip (visibility-only; no host move)
    mirror:                    # n/a — GitHub IS the public home (ADR-013 D3)
    upstream:                  # n/a
```

> **Public-flip provenance (2026-06-22):** Git.aDNA P6 Wave 2 flipped this repo (`aDNA-Network/III.aDNA`) GitHub-private → **GitHub-public** via `gitops_set_visibility` — III is a **production Framework** (the III quality standard; live consumer wrappers across the fleet), so GitHub-public is its ADR-013 home. The flip is **visibility-only** — `origin` is unchanged, so there is **no `rollback` remote and no Home §C shim** (the reverse is a one-command re-privatize). Companion files: `hooks/pre-push.gitleaks.sh` (ADR-011 D2) + `.gitleaks.toml` (the hook resolves it via `git/.gitleaks.toml`; III carries a scoped allowlist for 10 confirmed false-positives — `idempotency_key` field-name prose + vendored Obsidian-plugin minified JS).
