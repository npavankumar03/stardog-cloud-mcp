# Kubernetes Manifests for Stardog Cloud MCP

## Production practice notes (test AKS)

- Service is ClusterIP (not LoadBalancer).
- Ingress expects the **nginx** Ingress controller.

### Install nginx Ingress controller (one-time on the cluster)
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.11.2/deploy/static/provider/cloud/deploy.yaml
```

### Get the Ingress external IP
```bash
kubectl get ingress stardog-cloud-mcp
# or
kubectl get svc -n ingress-nginx
```

### Enable real HTTPS later
1. Install cert-manager
2. Create a ClusterIssuer (Let's Encrypt)
3. Uncomment the `tls` section in `ingress.yaml` and set your domain
4. Point a DNS A record to the Ingress IP

The MCP endpoint will be available at `http://<INGRESS-IP>/mcp` (or `https://yourdomain/mcp` once TLS is configured).
