# platform-gitops

GitOps repository for Argo CD managed deployments.

## Ubuntu prerequisites

```bash
sudo apt-get update
sudo apt-get install -y git curl
```

Install Helm on the Ubuntu management node if it is not installed:

```bash
curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 -o /tmp/get-helm-3.sh
chmod 700 /tmp/get-helm-3.sh
sudo /tmp/get-helm-3.sh
```

## Layout

```text
argocd/applications/
  springboot-demo.yaml

apps/
  springboot-demo/
    Chart.yaml
    values.yaml
    templates/
```

## Blue/green switch

`apps/springboot-demo/values.yaml` controls the active color.

```yaml
activeColor: blue
```

To stage a new version:

1. Set `colors.green.enabled=true`.
2. Set `colors.green.image.tag` to the new image tag.
3. Validate `springboot-demo-green`.
4. Switch `activeColor` to `green`.

Jenkins can update the default `image.tag` for simple validation, but production blue/green promotion should be a reviewed GitOps change.

## Render on Ubuntu

```bash
cd ~/sn/platform-gitops
helm template springboot-demo ./apps/springboot-demo -n springboot-demo
```

Server dry-run:

```bash
helm template springboot-demo ./apps/springboot-demo -n springboot-demo \
  | kubectl -n springboot-demo apply --dry-run=server -f -
```

## Push to Gitea from Ubuntu

Create an empty `platform-gitops` repo in Gitea first, then run:

```bash
cd ~/sn/platform-gitops
git init
git add .
git commit -m "feat(gitops): add springboot demo chart"
git branch -M main
git remote add origin https://gitea.example.com/gitea-admin/platform-gitops.git
git push -u origin main
```

If the remote already exists:

```bash
git remote set-url origin https://gitea.example.com/gitea-admin/platform-gitops.git
git push -u origin main
```
