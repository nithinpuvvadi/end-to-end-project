# ADR-0001: Platform Ownership Boundaries

- Status: Accepted
- Date: 2026-10-18

## Context
The platform uses several automation and deployment tools.
Overlapping ownership can cause configuration drift.

## Decision
- Terraform owns AWS infrastructure resources.
- Ansible manages explicitly selected configuration tasks.
- GitHub Actions tests code and builds immutable application images.
- Amazon ECR stores container images.
- Argo CD reconciles Kubernetes configuration from Git.
- Argo Rollouts manages progressive application releases.
- AWS Load Balancer Controller reconciles supported routing resources.
- OpenTelemetry Collector gathers telemetry.
- Prometheus stores and queries metrics.
- Grafana visualizes operational and reliability data.

## Consequences
- Infrastructure and application delivery have separate workflows.
- Kubernetes desired state is version controlled.
- Traffic promotion has explicit ownership.
- Terraform and Argo CD must not compete to manage the same resources.
- Ansible must have a defined purpose rather than duplicate Terraform
  or Kubernetes functionality.
