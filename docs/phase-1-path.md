# Phase 1 path printout

Captured 2026-08-31 before Phase 3–5 mutations. Do not treat this as a coding-agent stack change.

```text
OWNER TYPE: personal account (SStonkSS1). No GitHub organizations.
GITHUB PLAN: personal; REST `user.plan` is null and marketplace purchases are empty. Private-repository rulesets exist, so Pro-class repository rulesets are available. Organization-level rulesets are not available.
RULESETS AVAILABLE: yes (repository). no (user/org: GET /user/rulesets 404)
REQUIRED WORKFLOWS AVAILABLE: no (org-level required ruleset workflows require a GitHub organization)
CHOSEN ENFORCEMENT MODEL: reusable+required-check
SEMGREP ORG ADMIN: no (no GitHub org; Managed Scans private-app registration requires GitHub organization admin)
SEMGREP APPS: public= no  private= no
ACTIONS ALLOWLIST BLOCKS ZIZMOR: no (all inventoried repos: allowed_actions=all)
SIBLINGS: hk=present (local CLI) trivy=present (local CLI) renovate=present (SStonkSS1/renovate-config + per-repo .github/renovate.json) roborev=present (Dwellori reviews/handoffs; not installed in this task)
ENTITLEMENT HEADROOM: codescene=not onboarded / unknown cap  semgrep=not onboarded / Managed Scans blocked by missing GitHub org admin
DISPOSABLE TEST REPO: SStonkSS1/ci-quality-layer-fixture (created after this printout)
```

## Inventory notes

| Repo | Class | Actions | zizmor caller on default | Ruleset |
|---|---|---|---|---|
| Dwellori | active application | yes | no on `main`; yes on `feat/calendar-two-way-sync` | GitHub Actions Security **active** (`zizmor / zizmor`) |
| sierracc | active application | yes | yes (SHA `dfa16aba…`) | GitHub Actions Security **active** |
| stonks-dashboard | active application | yes (`ci.yml`) | no on `main` | GitHub Actions Security **active** (fail-closed until caller lands) |
| taskmoor | active application | **none** | n/a | GitHub Actions Security **active** (incorrect fail-closed; disable) |
| piccolo-os | active application | yes | no | none |
| renovate-config | infrastructure | none | n/a | none |
| engineering-config | central config | yes (this workflow) | self | none |

No merge queues. GHAS/code scanning not enabled on Dwellori. Hindsight process not running. CodeScene/Semgrep/zizmor are not MCP servers.
