# actions-security-test

Test repository for [actions-copilot](https://github.com/david-wiggs/actions-copilot) environment protection testing. Contains workflows with varying security levels to validate AI-driven deployment protection rules.

## Environments

- **production** — Protected environment targeted by all workflows
- **staging** — Secondary environment for testing

## Workflows

| Workflow | Threat Level | Description |
|----------|:---:|-------------|
| `secure-deploy.yml` | ✅ Safe | Standard CI/CD pipeline with tests before deploy |
| `secure-deploy-pinned.yml` | ✅ Safe | Best-practice deploy with SHA-pinned actions and minimal permissions |
| `insecure-deploy.yml` | ⚠️ Risky | Uses `write-all` permissions, pipes curl to bash, exposes secrets in env |
| `suspicious-pr-deploy.yml` | ⚠️ Risky | Uses `pull_request_target` to check out untrusted PR code with production secrets |
| `malicious-exfiltration.yml` | ❌ Malicious | Exfiltrates secrets (DEPLOY_KEY, AWS creds) to an external C2 server via HTTP POST |
| `malicious-cryptominer.yml` | ❌ Malicious | Downloads and runs xmrig cryptocurrency miner disguised as performance tests |
| `malicious-backdoor.yml` | ❌ Malicious | Creates a reverse shell, installs cron persistence, impersonates dependabot |

All workflows include `workflow_dispatch` so they can be triggered manually from the Actions tab.

## Usage

1. Install the [actions-copilot](https://github.com/david-wiggs/actions-copilot) GitHub App on this repo
2. Configure deployment protection rules on the `production` environment
3. Trigger workflows manually or via push to observe how the AI analyzes each one
