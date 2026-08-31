# engineering-config

Central engineering and security configuration repository for `SStonkSS1` repositories.

## Purpose

This repository acts as the **single source of truth** for centralized security workflows and engineering automation across participating repositories.

## Central Workflows

### GitHub Actions Security (`.github/workflows/zizmor.yml`)

Runs [zizmor](https://github.com/zizmorcore/zizmor) to audit GitHub Actions workflow definitions for security risks (e.g. untrusted inputs, code injection, privilege escalation, unpinned actions, overly permissive tokens).

#### Security Invariants

- **Full SHA Pinning:** All third-party GitHub Actions are pinned by immutable full commit SHA.
- **Explicit Analyzer Version:** The `zizmor` engine version is explicitly pinned (currently `v1.30.0`) rather than floating on `latest`.
- **Least Privilege:** Top-level workflow permissions default to `{}` with only `contents: read` granted at the job level.
- **Safe Checkout:** Checkout explicitly disables persistent credentials (`persist-credentials: false`).
- **No Privileged Execution:** Runs purely on `pull_request` (or `workflow_call`); never uses `pull_request_target`.
- **Zero False-Positive on No Inputs:** Configured with `fail-on-no-inputs: false` so repositories without GitHub Actions workflows succeed safely.
- **Hosted Linux Runner:** Standard GitHub-hosted `ubuntu-latest` runner.

## How Repositories Consume Reusable Workflows

Each participating repository contains only a minimal caller workflow at `.github/workflows/zizmor.yml`:

```yaml
name: Security

on:
  pull_request:
    branches:
      - main

permissions:
  contents: read

jobs:
  zizmor:
    uses: SStonkSS1/engineering-config/.github/workflows/zizmor.yml@<FULL_COMMIT_SHA> # main
    permissions:
      contents: read
```

The `# main` comment provides the branch lineage hint required by Renovate to track and propose automated SHA updates over time.

## Visibility & Sharing Rationale

This repository is public to permit unauthenticated `impostor-commit` audits by zizmor across private repositories without requiring cross-repository personal access tokens. It contains strictly generic, non-sensitive workflow automation.

Current pins (resolved from GitHub Releases at rollout, not copied from older README examples):

- `actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1` # v7.0.1
- `zizmorcore/zizmor-action@70fb788f84895a7701f5643d103d587e460b5c99` # v0.6.3
- zizmor analyzer `version: v1.30.0`

See `docs/phase-1-path.md` and `docs/rollback.md`.
