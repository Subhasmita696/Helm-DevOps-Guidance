# Helm Usage and Kubernetes Integration for DevOps

## How to use Helm in DevOps

1. Install Helm on your local machine or CI environment
   - macOS: `brew install helm`
   - Linux: download the Helm binary from `helm.sh`

2. Create or use an existing chart
   - `helm create myapp` generates a starter chart structure
   - Use charts from public or private repositories

3. Customize values
   - Edit `values.yaml` for default settings
   - Create environment-specific override files, for example `values-dev.yaml` or `values-prod.yaml`

4. Deploy the chart to Kubernetes
   - `helm install myapp ./myapp-chart --values values-prod.yaml`
   - `helm upgrade --install myapp ./myapp-chart --values values-prod.yaml`

5. Manage lifecycle
   - Check deployment status: `helm status myapp`
   - View rendered manifests: `helm template myapp ./myapp-chart --values values-prod.yaml`
   - Roll back if needed: `helm rollback myapp 1`

## When to use Helm in DevOps

Use Helm when you need:
- Reusable application packaging for Kubernetes
- Environment-specific deployment configuration
- Release management with version history
- Standardized deployment workflows for multiple teams
- Easier onboarding with packaged charts instead of raw YAML

Ideal situations:
- You have microservices with similar deployment patterns
- You deploy the same app to dev, staging, and production
- You want a single source of truth for app configuration
- You want CI/CD to handle application deployment automatically

## Why use Helm for DevOps

- Standardization: Helm charts encapsulate Kubernetes resources and conventions.
- Reusability: One chart can deploy to many environments with different values.
- Maintainability: Templates keep YAML DRY and easier to update.
- Operational safety: Helm supports rollbacks and history for each release.
- DevOps automation: Helm integrates cleanly with CI/CD pipelines.

Benefits for DevOps teams:
- Faster deployments
- Lower chance of manual errors
- Better collaboration via shared chart repos
- Easier promotion of changes through staging and production

## Integrating Helm with Kubernetes

1. Prepare your Kubernetes cluster
   - Ensure `kubectl` is configured with the target cluster
   - Verify the cluster is reachable: `kubectl get nodes`

2. Package the chart
   - `helm package ./myapp-chart`
   - This creates `myapp-chart-0.1.0.tgz`

3. Add or use a chart repository
   - Add public repo: `helm repo add stable https://charts.helm.sh/stable`
   - Add private repo: `helm repo add myrepo https://myrepo.example.com/charts`
   - Update repos: `helm repo update`

4. Install or upgrade on Kubernetes
   - `helm install myapp ./myapp-chart --namespace dev --create-namespace`
   - `helm upgrade --install myapp ./myapp-chart --namespace dev --values values-dev.yaml`

5. Validate deployment
   - Kubernetes objects: `kubectl get all -n dev`
   - Helm release info: `helm status myapp -n dev`
   - Values used: `helm get values myapp -n dev`

6. Automate integration in CI/CD
   - Use pipeline steps to run `helm lint`, `helm package`, and `helm upgrade --install`
   - Store `values` files securely for different environments
   - Use Kubernetes service account credentials or kubeconfig in CI

## Example workflow

1. Developer updates chart templates or values.
2. CI runs `helm lint` and `helm template` for validation.
3. CI packages the chart and pushes it to a chart repository.
4. CD deploys the chart to Kubernetes with `helm upgrade --install`.
5. Operations monitors release status with Helm and `kubectl`.

## Quick reference

- `helm create <name>` - create a new chart
- `helm install <release> <chart>` - deploy a chart
- `helm upgrade --install <release> <chart>` - deploy or update
- `helm rollback <release> <revision>` - revert a release
- `helm status <release>` - inspect a release
- `helm get values <release>` - show applied values

## Summary
Helm is a DevOps tool that simplifies Kubernetes application deployments by packaging resources into charts, enabling environment-driven configuration, and integrating with Kubernetes and CI/CD. Use Helm to make Kubernetes deployment repeatable, versioned, and collaborative across teams.