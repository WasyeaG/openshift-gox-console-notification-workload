# ConsoleNotification Workload Design

## Purpose

This repository contains only the OpenShift ConsoleNotification workload implementation.

The goal is to keep workload-specific configuration separate from reusable GitOps automation and reusable ApplicationSet orchestration.

## Responsibilities

This repository is responsible for:

- defining the ConsoleNotification Helm chart
- providing environment-specific workload values
- rendering the OpenShift ConsoleNotification resource
- validating workload configuration

This repository is not responsible for:

- cluster selection
- ApplicationSet generation
- GitHub Actions orchestration
- OpenShift authentication
- deployment pipeline logic

## Environment Configuration

Each environment has its own values file:

```text
values-sbx.yaml
values-stg.yaml
values-prd.yaml
```

The values control:

- notification name
- notification text
- banner location
- text color
- background color
- optional link configuration

## Environment Mapping

```text
sbx
 |
 +--> values-sbx.yaml
      |
      +--> GOX Sandbox

stg
 |
 +--> values-stg.yaml
      |
      +--> GOX Staging

prd
 |
 +--> values-prd.yaml
      |
      +--> GOX Production
```

## GitOps Integration

The reusable ApplicationSet template references this repository as a workload source.

The ApplicationSet layer determines:

- which cluster is selected
- which target revision is used
- which destination namespace receives the workload

This repository determines only what workload is rendered.

## Validation

Each environment is validated using:

```bash
helm lint charts/console-notification \
  -f charts/console-notification/values-<environment>.yaml
```

and:

```bash
helm template console-notification-<environment> \
  charts/console-notification \
  -f charts/console-notification/values-<environment>.yaml
```

Validation has been completed successfully for sandbox, staging, and production-like configurations.

## Design Principle

Reusable automation, ApplicationSet orchestration, and workload implementation remain separate.

This allows the same GitOps framework to support additional workloads without duplicating workflow or ApplicationSet logic.
