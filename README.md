# engineering-config

Central engineering and security configuration repository for `SStonkSS1` repositories.

## Purpose

This repository acts as the **single source of truth** for centralized security workflows and engineering automation across participating repositories.

## Central Workflows

### GitHub Actions Security (`.github/workflows/zizmor.yml`)

Runs [zizmor](https://github.com/zizmorcore/zizmor) to audit GitHub Actions workflow definitions for security risks (e.g. untrusted inputs, code injection, privilege escalation, unpinned actions, overly permissive tokens).

#### Security Invariants

- **Full SHA Pinning:** All third-party GitHub Actions are pinned by immutable full commit SHA.
- **Explicit Analyzer Version:** The `zizmor` engine version is explicitly pinned (e.g., `v1.29.0`) rather than floating on `latest`.
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

permissions: {}

jobs:
  zizmor:
    uses: SStonkSS1/engineering-config/.github/workflows/zizmor.yml@<FULL_COMMIT_SHA>
```

Replace `<FULL_COMMIT_SHA>` with the current full commit SHA of the `main` branch in this repository.

## Sharing Access

This repository is private. Sharing is enabled via GitHub Actions settings:
- **Access Level:** `Accessible from repositories owned by the user 'SStonkSS1'` (`access_level: "user"`).
