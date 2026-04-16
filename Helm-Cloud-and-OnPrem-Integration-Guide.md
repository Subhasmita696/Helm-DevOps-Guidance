# Helm Integration Guide for AWS, Azure, GCP, and On-Premises

This guide explains how to integrate Helm with Kubernetes clusters on AWS, Azure, GCP, and on-premises environments.

## Prerequisites

- Helm installed locally or in your CI/CD environment
- `kubectl` configured to access your Kubernetes cluster
- Kubernetes cluster running on the target platform

## 1. AWS Integration (EKS)

### Steps

1. Create or use an existing EKS cluster
   - Use AWS Console, `eksctl`, or Terraform
   - Example: `eksctl create cluster --name dev-cluster --region us-west-2`

2. Configure `kubectl`
   - `aws eks --region us-west-2 update-kubeconfig --name dev-cluster`
   - Verify access: `kubectl get nodes`

3. Install Helm chart
   - `helm install myapp ./myapp-chart --namespace dev --create-namespace`

### Optional AWS details

- Use AWS IAM Authenticator with EKS for access control
- Store values files in S3 or secrets manager for CI/CD usage
- Consider using AWS CodePipeline / CodeBuild for automation

## 2. Azure Integration (AKS)

### Steps

1. Create or use an existing AKS cluster
   - Use Azure CLI or Azure Portal
   - Example: `az aks create --resource-group myRG --name devAKS --node-count 2 --enable-managed-identity`

2. Configure `kubectl`
   - `az aks get-credentials --resource-group myRG --name devAKS`
   - Verify access: `kubectl get nodes`

3. Install Helm chart
   - `helm install myapp ./myapp-chart --namespace dev --create-namespace`

### Optional Azure details

- Use Azure Key Vault for secure configuration and secrets
- Use Azure DevOps pipelines or GitHub Actions for Helm deployment
- Use `az acr helm` for private chart repositories (deprecated for Helm v3, instead use OCI registries)

## 3. GCP Integration (GKE)

### Steps

1. Create or use an existing GKE cluster
   - Use Google Cloud Console, `gcloud`, or Terraform
   - Example: `gcloud container clusters create dev-cluster --zone us-central1-a`

2. Configure `kubectl`
   - `gcloud container clusters get-credentials dev-cluster --zone us-central1-a --project my-project`
   - Verify access: `kubectl get nodes`

3. Install Helm chart
   - `helm install myapp ./myapp-chart --namespace dev --create-namespace`

### Optional GCP details

- Use Google Cloud Build or Cloud Deploy for automated Helm releases
- Use Secret Manager for values and secrets
- Use Artifact Registry for OCI-based Helm charts

## 4. On-Premises Integration

### Common on-premises Kubernetes options

- VMware Tanzu Kubernetes Grid
- Red Hat OpenShift
- Rancher-managed clusters
- kubeadm or other self-managed clusters

### Steps

1. Ensure cluster access
   - Configure `kubectl` with the cluster's kubeconfig file
   - Example: `export KUBECONFIG=/path/to/kubeconfig`
   - Verify access: `kubectl get nodes`

2. Install Helm chart
   - `helm install myapp ./myapp-chart --namespace dev --create-namespace`

### Optional on-prem details

- Use local artifact repositories or private chart repos
- Use CI/CD systems such as Jenkins, GitLab CI, or Tekton on-prem
- Use local secret management tools like HashiCorp Vault or platform-native secrets

## 5. Common Helm Integration Patterns

### Use environment-specific values

- `values-dev.yaml`
- `values-staging.yaml`
- `values-prod.yaml`

Install with:

```bash
helm upgrade --install myapp ./myapp-chart --namespace dev --values values-dev.yaml
```

### Use private chart repositories

- AWS: S3-backed charts or OCI registry in ECR
- Azure: Azure Container Registry (ACR) with OCI support
- GCP: Artifact Registry with OCI support
- On-prem: Harbor, Nexus, or GitHub Packages with OCI

### Automate using CI/CD

- Validate charts: `helm lint ./myapp-chart`
- Render templates: `helm template myapp ./myapp-chart`
- Deploy: `helm upgrade --install myapp ./myapp-chart --values values.yaml`

## 6. Troubleshooting

- Check cluster connectivity: `kubectl get nodes`
- Review release status: `helm status myapp`
- Inspect rendered templates: `helm template myapp ./myapp-chart`
- View Helm history: `helm history myapp`

## Summary
This guide shows how Helm connects to Kubernetes across AWS EKS, Azure AKS, GCP GKE, and on-premises clusters. Use environment-aware values, chart repositories, and CI/CD automation to make Helm deployments reliable and repeatable across all platforms.
