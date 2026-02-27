# IoT Part 3 - GitHub Repository

This repository contains Kubernetes manifests for the wil42/playground application, managed by Argo CD.

## Files

- `deployment.yaml` - Kubernetes Deployment for the application
- `service.yaml` - Kubernetes Service (LoadBalancer) exposing port 8888

## Version Updates

To update the application version:

1. Edit `deployment.yaml`
2. Change the image tag from `wil42/playground:v1` to `wil42/playground:v2` (or vice versa)
3. Commit and push:
   ```bash
   git add deployment.yaml
   git commit -m "Update to v2"
   git push
   ```

Argo CD will automatically detect the change and sync the new version to your cluster.

## Verification

After deploying, verify the version:

```bash
# Check the deployed version
kubectl get deployment wil-playground -n dev -o jsonpath='{.spec.template.spec.containers[0].image}'

# Test the application
curl http://localhost:8888/
```

Expected output for v1: `{"status":"ok", "message": "v1"}`
Expected output for v2: `{"status":"ok", "message": "v2"}`
