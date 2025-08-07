# GitOps with Argo CD + Helm (Flask + Postgres + Redis)

## Repo Layout
```
charts/
  flask-app/
  postgres/
  redis/
apps/
  app-of-apps.yaml
  flask-app.yaml
  postgres.yaml
  redis.yaml
```

## Quick Start

1. Push this repo to GitHub (replace `REPLACE_WITH_YOUR_GIT_REPO` in all `apps/*.yaml` files with the HTTPS URL).
2. Install Argo CD:
   ```bash
   kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```
3. Create the **App of Apps**:
   ```bash
   kubectl apply -n argocd -f apps/app-of-apps.yaml
   ```
4. Validate:
   ```bash
   kubectl get apps -n argocd
   kubectl get pods -n flask
   kubectl get svc -n flask
   ```

## Update Image Tag (Auto-Deploy Demo)
Edit `charts/flask-app/values.yaml`:
```yaml
image:
  repository: your-dockerhub-username/flask-app
  tag: "v2"
```
Commit and push. Argo CD will auto-sync and roll out the new version.

## Notes
- Adjust database credentials in `charts/postgres/values.yaml` and reference them in `charts/flask-app/values.yaml` as needed.
- If you have ingress, add an `ingress.yaml` template under `charts/flask-app/templates/`.
