# Rollback

Defined before activation of the 2026-08-31 quality/security layer pin bump.

| Control | Rollback |
|---|---|
| GitHub repository ruleset `GitHub Actions Security` | Set `enforcement` to `evaluate` or `disabled`. Restore prior JSON from `docs/snapshots/2026-08-31-pre-change/github-rulesets-and-workflows.json`. |
| Central zizmor workflow | Revert the introducing commit on `SStonkSS1/engineering-config` (`git revert`). Prior pins: `zizmorcore/zizmor-action@3dc1ecc9bcb9e94e9b2c709687979e1298497054` (# v0.6.2) and analyzer `v1.29.0`. |
| Reusable-workflow callers | Revert the caller commit in each participating repo, or retarget `@<prior engineering-config SHA>`. Prior caller SHA in production: `dfa16aba5fc258aab66354f7cd6ff2b1803dbf59`. |
| CodeScene | Uninstall **CodeScene Access** from selected repos, or set PR quality profile back to non-blocking / prior profile in the CodeScene UI. No CodeScene projects were created by API in this rollout. |
| Semgrep | Managed Scans were not enabled (no GitHub org admin). If later enabled: set Unified Policies remediation to Monitor / prior policy; uninstall public `semgrep-app` and the private code-access app. |
| Disposable test repo `ci-quality-layer-fixture` | Delete the repository. |

No organization-level secrets were added. No GitHub plan change. No allowlist change (`allowed_actions=all`).
