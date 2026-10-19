# Kubernetes Manifests for Stardog Cloud MCP

These manifests are used by the GitHub Actions workflow in `.github/workflows/deploy-aks.yml`.

## Required GitHub Secrets

Add these secrets in your repository Settings → Secrets and variables → Actions:

- `AZURE_CREDENTIALS`: JSON output from:
  ```bash
  az ad sp create-for-rbac --name stardog-mcp-github --role contributor \
    --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP> \
    --sdk-auth
  ```
- `ACR_NAME`: Your Azure Container Registry name (e.g. `stardogmcpregistry`)
- `AKS_RESOURCE_GROUP`: Resource group containing the AKS cluster
- `AKS_CLUSTER_NAME`: Name of your AKS cluster

## Notes

- The workflow builds the image with `az acr build` (no local Docker needed on the runner).
- Image tag is the commit SHA; the `k8s-deploy` action updates the image in the manifests.
- Auth tokens are **not** stored in the cluster; clients pass them via MCP request headers.
