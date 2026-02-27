# How to Update from v1 to v2

When you want to demonstrate the GitOps v1→v2 update, edit `deployment.yaml` and make these changes:

## Change 1: Update the HTML Content in ConfigMap

Find the `app-html` ConfigMap section and change:

**FROM (v1):**
```yaml
data:
  index.html: |
    <!DOCTYPE html>
    <html>
    <head><title>IoT Playground</title></head>
    <body style="font-family: Arial; text-align: center; padding: 50px;">
      <h1 style="color: #0066cc;">Version 1</h1>
      <p style="font-size: 20px;">Status: <span style="color: green;">OK</span></p>
      <pre style="background: #f4f4f4; padding: 20px; border-radius: 5px;">{"status":"ok", "message": "v1"}</pre>
      <p style="color: #666;">Change this to v2 in your Git repo to see GitOps in action!</p>
    </body>
    </html>
```

**TO (v2):**
```yaml
data:
  index.html: |
    <!DOCTYPE html>
    <html>
    <head><title>IoT Playground</title></head>
    <body style="font-family: Arial; text-align: center; padding: 50px; background: #f0f8ff;">
      <h1 style="color: #cc0066;">Version 2</h1>
      <p style="font-size: 20px;">Status: <span style="color: green;">OK</span></p>
      <pre style="background: #e6f3ff; padding: 20px; border-radius: 5px;">{"status":"ok", "message": "v2"}</pre>
      <p style="color: #666;">✅ GitOps Update Successful! Argo CD automatically synced this change.</p>
    </body>
    </html>
```

## Change 2: Update the Version Label

Find the Deployment's template labels section and change:

**FROM:**
```yaml
    metadata:
      labels:
        app: wil-playground
        version: v1  # Change to v2 when updating
```

**TO:**
```yaml
    metadata:
      labels:
        app: wil-playground
        version: v2  # Updated!
```

## Quick Search & Replace

You can use these commands to update:

```bash
# Update Version 1 → Version 2
sed -i 's/Version 1/Version 2/g' deployment.yaml

# Update message v1 → v2
sed -i 's/"message": "v1"/"message": "v2"/g' deployment.yaml

# Update version label
sed -i 's/version: v1/version: v2/g' deployment.yaml

# Update color scheme
sed -i 's/#0066cc/#cc0066/g' deployment.yaml
sed -i 's/#f4f4f4/#e6f3ff/g' deployment.yaml

# Add background color
sed -i 's/padding: 50px;/padding: 50px; background: #f0f8ff;/g' deployment.yaml

# Update success message
sed -i 's/Change this to v2 in your Git repo to see GitOps in action!/✅ GitOps Update Successful! Argo CD automatically synced this change./g' deployment.yaml
```

Or just manually edit the file - it's clearer!

## Commit and Push

```bash
git add deployment.yaml
git commit -m "Update to v2"
git push origin main
```

Argo CD will detect the change within 3 minutes and automatically update your cluster!

## Verify

```bash
# Watch the update happen
watch kubectl get pods -n dev

# Test the new version (wait ~30 seconds after push)
curl http://localhost:8888/

# Or open in browser
xdg-open http://localhost:8888
```
