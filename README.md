# SemaFore Starter Workflows

SemaFore is secure business messaging for organisations. Message content is
end-to-end encrypted so SemaFore's servers route ciphertext without holding the
keys needed to read it.

This repository contains copy-paste GitHub Actions workflows for common build,
deployment, release, and security events. Each example uses the
[SemaFore GitHub Action](https://github.com/Attomus/semafore-github-action) to
send encrypted notifications or record bounded audit metadata.

## Workflows

| Workflow | Trigger | Result |
| --- | --- | --- |
| [`notify-on-deploy.yml`](./workflows/notify-on-deploy.yml) | A workflow named `deploy` completes | Sends its branch, commit, and conclusion to the organisation. |
| [`notify-on-pr-merge.yml`](./workflows/notify-on-pr-merge.yml) | A pull request merges to `main` | Sends the pull request number, title, and author. |
| [`notify-on-release.yml`](./workflows/notify-on-release.yml) | A GitHub Release is published | Sends the release tag, name, and URL. |
| [`audit-event-on-security-alert.yml`](./workflows/audit-event-on-security-alert.yml) | A `security_alert` repository dispatch is received | Records source, severity, alert identifier, and URL as audit metadata. |

### Security alert dispatch

GitHub does not emit a `repository_dispatch` event automatically when a
Dependabot or code-scanning alert opens. Connect the alert webhook to a small
relay that authenticates to the target repository's dispatch endpoint and
sends this non-secret payload:

```json
{
  "event_type": "security_alert",
  "client_payload": {
    "alert_id": "42",
    "severity": "high",
    "source": "dependabot",
    "alert_url": "https://github.com/OWNER/REPOSITORY/security/dependabot/42"
  }
}
```

The workflow treats every payload field as untrusted text and serialises it
with `jq`. Do not include advisory contents, credentials, or other secrets in a
dispatch payload.

## Get Started

1. Follow [SETUP.md](./SETUP.md) to bootstrap the integration device and add
   `SEMAFORE_TOKEN` and `SEMAFORE_DEVICE_KEY` as GitHub Actions secrets.
2. Copy one file from `workflows/` into `.github/workflows/` in your repository.
3. Adjust the trigger, target, and template for your repository.

The [GitHub Actions integration guide](https://docs.semafore.io/integrations/github-actions/)
contains the complete inputs reference, execute request shapes, security model,
and troubleshooting guidance. See [semafore.io](https://semafore.io/) for the
SemaFore product overview.

## Security

Never put service tokens or device keys in workflow YAML, repository variables,
dispatch payloads, or logs. Keep them in GitHub Actions secrets and grant each
SemaFore service token only the capabilities its workflow needs.

Report suspected vulnerabilities privately as described in
[SECURITY.md](./SECURITY.md).

## Licence

Apache-2.0. See [LICENSE](./LICENSE).
