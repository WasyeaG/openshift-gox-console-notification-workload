# ConsoleNotification Workload

This repository contains the OpenShift ConsoleNotification workload used by the reusable GitOps reference architecture.

The repository is intentionally workload-specific. It contains only the Helm chart and environment-specific configuration required to render OpenShift ConsoleNotification resources.

## Repository Purpose

This repository owns:

- the ConsoleNotification Helm chart
- workload-specific templates
- default workload values
- environment-specific values for sandbox, staging, and production-like environments

This repository does not contain:

- reusable GitHub Actions workflows
- reusable ApplicationSet templates
- cluster selection logic
- deployment orchestration logic

## Repository Structure

```text
charts/
└── console-notification/
    ├── Chart.yaml
    ├── values.yaml
    ├── values-sbx.yaml
    ├── values-stg.yaml
    ├── values-prd.yaml
    └── templates/
        └── console-notification.yaml

docs/
└── design.md
```

## Environment Configuration

The chart uses environment-specific values files.

| Environment | Notification Name | Text |
|---|---|---|
| sbx | `gox-sbx-console-notification` | `GOX Sandbox` |
| stg | `gox-stg-console-notification` | `GOX Staging` |
| prd | `gox-prd-console-notification` | `GOX Production` |

## Validation

Validate sandbox:

```bash
helm lint charts/console-notification \
  -f charts/console-notification/values-sbx.yaml
```

Render sandbox:

```bash
helm template console-notification-sbx \
  charts/console-notification \
  -f charts/console-notification/values-sbx.yaml
```

The same validation applies to `stg` and `prd`.

All three environment configurations have been validated successfully with `helm lint` and `helm template`.

## Integration

This workload is intended to be consumed by the reusable ApplicationSet template repository.

The reusable ApplicationSet is responsible for cluster selection and deployment orchestration, while this repository defines only the workload that is deployed.

## Clean-Room Implementation

This repository is a new workload implementation created for the reusable GitOps reference architecture.

Existing implementations may be reviewed conceptually only. No legacy chart templates or repository structures are copied into this repository.
