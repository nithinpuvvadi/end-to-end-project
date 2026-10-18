# Platform Architecture and Traffic Flow

## Application request path
Client
  -> DNS
  -> AWS Application Load Balancer
  -> Kubernetes routing
  -> Stable or canary application pods

## Deployment path
Developer
  -> GitHub
  -> GitHub Actions
  -> Test and build immutable container image
  -> Publish image to Amazon ECR
  -> Update desired configuration in Git
  -> Argo CD reconciles Kubernetes resources
  -> Argo Rollouts progresses or aborts the release
  -> AWS Load Balancer Controller updates supported ALB routing

## Observability path
Application and platform telemetry
  -> OpenTelemetry Collector
  -> Prometheus
  -> Grafana dashboards and alerts

## Architecture questions to resolve
- Which subnets host the load balancer and worker nodes?
- Which component owns each traffic-routing change?
- How will request success and latency be measured?
- Which SLO conditions allow release promotion?
- How will we distinguish an application failure from a dependency outage?
- How will we validate rollback and database compatibility?
