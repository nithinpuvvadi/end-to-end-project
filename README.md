# end-to-end-project
The project will simulate a production service with a real CI/CD pipeline and measurable reliability objectives.
Project: Production-grade DevOps platform on AWS EKS
GitHub repository
Application · Terraform · Ansible · Kubernetes · dashboards


GitHub Actions CI pipeline
Test → build image → scan → publish to Amazon ECR


Argo CD + Argo Rollouts
GitOps synchronization · canary traffic · promotion · rollback


AWS production infrastructure
VPC
Subnets, routing, IAM


EKS
Application pods



Terraform provisions infrastructure; Ansible manages selected host configuration.


Observability platform

OpenTelemetry
Collect


Prometheus
Store metrics


Grafana
Dashboards



SLIs · SLOs · error budget · release health · infrastructure health


For progressive delivery, Argo Rollouts supports staged traffic shifting and Prometheus-based release analysis; its AWS ALB integration can manage weighted traffic between stable and canary versions. 

Kubernetes Progressive Delivery Controller
+1



Our fixed toolset
Requirement	Tool we will use
Source code and version control	Git + GitHub
Infrastructure provisioning	Terraform
Configuration management	Ansible
Application CI	GitHub Actions
Container registry	Amazon ECR
Kubernetes platform	Amazon EKS
GitOps deployment	Argo CD
Canary and blue-green releases	Argo Rollouts
Traffic entry	AWS Load Balancer Controller + ALB
Telemetry collection	OpenTelemetry Collector
Metrics database	Prometheus
Dashboards and visualization	Grafana
Reliability evaluation	Prometheus queries, SLO rules and Grafana
One architectural decision is important: Terraform provisions the platform; Argo CD manages the Kubernetes application configuration. We will define clear ownership so Terraform and Argo CD do not fight over the same resources.
Ansible will have a real configuration-management responsibility, not an artificial task added just to say we used it.
The application we will use
We will start with Podinfo, a lightweight, open-source Kubernetes application. It is suitable for learning deployments, health checks, scaling, and progressive delivery without starting with a complicated microservices platform.
Official repository: Podinfo on GitHub.
We'll configure monitoring and load generation around it. Where necessary, we'll add a small test endpoint or controlled failure scenarios so we can measure request success, latency, and release regressions.
The seven implementation milestones
01

Design and Git foundation
Architecture diagram, repository structure, AWS account checks, Terraform state and cost controls.



02

AWS infrastructure
Terraform, VPC, public/private subnets, IAM, EKS, ECR and ALB prerequisites.



03

Configuration and CI
Ansible configuration-management task, GitHub Actions, tests, image build, scanning and publishing.



04

GitOps deployment
Install Argo CD, organize environment manifests, sync the application and separate CI from CD.



05

Progressive delivery
Argo Rollouts, ALB traffic shifting, canary gates, blue-green cutover and rollback drills.



06

Observability and SLOs
OpenTelemetry, Prometheus, Grafana dashboards, alert rules and error-budget tracking.



07

Production simulations
Introduce latency, errors, failed pods and bad releases; diagnose them from dashboards and recover.



The dashboards will drive our decisions
We will build four connected views in Grafana.









1. Application health
Request rate, error rate, p95/p99 latency, successful requests and saturation.












2. SLI / SLO / SLA and error budget
Availability objective, observed SLI, remaining error budget, burn rate and SLA target tracking.












3. Infrastructure and Kubernetes
Node capacity, pod restarts, CPU, memory, readiness, network and cluster health.












4. Release decision dashboard
Stable versus canary errors and latency, release stage, rollout status and rollback triggers.



These are the dashboard designs we will build toward, not dashboards that already exist in your environment.
Our initial example SLO can be 99.9% successful eligible requests over a rolling 30-day window. We will define precisely which requests count, what constitutes success, and how missing telemetry is handled before calculating the error budget. An SLA is a separate contractual commitment, not simply another name for an SLO.
