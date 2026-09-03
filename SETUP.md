# Setup

## 1. Bootstrap the Integration Device

Before copying a starter workflow, follow the
[one-time bootstrap guide](https://docs.semafore.io/integrations/github-actions/#bootstrap-once).
You will need:

- a one-time SemaFore service token with the `bootstrap` capability; and
- a temporary fine-grained GitHub personal access token or GitHub App
  installation token with repository **Secrets: write** permission.

Store them as `SEMAFORE_BOOTSTRAP_TOKEN` and
`SEMAFORE_GITHUB_SECRET_TOKEN`, run the bootstrap workflow once, then remove the
workflow and revoke or delete both temporary credentials. Bootstrap creates the
`SEMAFORE_DEVICE_KEY` repository secret. Do not edit or copy that value into a
workflow file.

## 2. Store the Runtime Token

Issue a separate SemaFore service token with the capability needed by the
workflow:

- `notify` for the three notification examples;
- `execute` for the security audit-event example; or
- both capabilities if the same repository uses both kinds.

Store the runtime token as the `SEMAFORE_TOKEN` repository or organisation
secret. Restrict an organisation secret to only the repositories that need it.

At this point the repository should have:

| Secret | Used by |
| --- | --- |
| `SEMAFORE_TOKEN` | Notify or execute mode at runtime. |
| `SEMAFORE_DEVICE_KEY` | Notify mode; created by bootstrap. |

The audit-event workflow does not need `SEMAFORE_DEVICE_KEY` because it sends
metadata through execute mode rather than encrypted message content.

## 3. Copy and Customise a Workflow

Copy one of the YAML files from `workflows/` into your repository's
`.github/workflows/` directory. Keep the `@v1` Action reference, then customise
the trigger, `target`, and message template.

For example:

```bash
mkdir -p .github/workflows
curl -fsSL \
  https://raw.githubusercontent.com/Attomus/semafore-starter-workflows/main/workflows/notify-on-deploy.yml \
  -o .github/workflows/semafore-deploy.yml
```

Commit the copied workflow and trigger it once. Confirm the Action run succeeds
and that an approved mobile device in the target SemaFore organisation receives
the notification.

See the [full integration guide](https://docs.semafore.io/integrations/github-actions/)
for supported template placeholders, target formats, execute parameters, and
troubleshooting.
