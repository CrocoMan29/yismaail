# IoT Part 3 - GitHub Repository

This repository contains Kubernetes manifests for a demo application, managed by Argo CD.

## Files

- `deployment.yaml` - Kubernetes Deployment with nginx and ConfigMaps (ARM64 compatible)
- `service.yaml` - Kubernetes Service (LoadBalancer) exposing port 8888
- `HOW-TO-UPDATE.md` - Step-by-step guide for v1 → v2 update

## ARM64 Compatibility Note

This setup uses **nginx** instead of `wil42/playground` because:
- You're running on ARM64 architecture (M1 Mac via UTM)
- nginx supports multi-architecture (including ARM64)
- The version difference is shown via ConfigMap HTML content
- It demonstrates the same GitOps workflow

## Version Updates

To update the application version:

1. Edit `deployment.yaml`
2. Update the `app-html` ConfigMap content:
   - Change "Version 1" → "Version 2"
   - Change `"message": "v1"` → `"message": "v2"`
   - Update colors/styling (optional)
3. Update the version label: `version: v1` → `version: v2`
4. Commit and push:
   ```bash
   git add deployment.yaml
   git commit -m "Update to v2"
   git push origin main
   ```

See `HOW-TO-UPDATE.md` for detailed instructions with examples.

Argo CD will automatically detect the change and sync the new version to your cluster.

## Verification

After deploying, verify the version:

```bash
# Check the deployed version label
kubectl get pods -n dev -L version

# Test the application
curl http://localhost:8888/

# Or open in browser
xdg-open http://localhost:8888
```

Expected output for v1:
```html
<h1 style="color: #0066cc;">Version 1</h1>
...
<pre>{"status":"ok", "message": "v1"}</pre>
```

Expected output for v2:
```html
<h1 style="color: #cc0066;">Version 2</h1>
...
<pre>{"status":"ok", "message": "v2"}</pre>
```
