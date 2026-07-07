# Argo CD

## Current Version

```text
Release: argocd
Namespace: argocd
Chart: argo-cd 10.1.2
App version: v3.4.4
```

## Update Vendored Chart

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update

rm -rf apps/argocd/charts/argo-cd

helm pull argo/argo-cd \
  --version 10.1.2 \
  --untar \
  --untardir apps/argocd/charts
```

## Required Follow-up

Replace `CHANGE_ME_REDIS_TAG` in `values.yaml` with the mirrored Redis image tag used by the live install.

## Render And Diff

```bash
helm template argocd apps/argocd -n argocd > /tmp/argocd-rendered.yaml
grep -R "image:" /tmp/argocd-rendered.yaml
kubectl -n argocd diff -f /tmp/argocd-rendered.yaml
```

## Notes

- Argo CD self-management should start with manual sync.
- Be careful with prune/self-heal on Argo CD itself.
- Do not store repo credentials or admin passwords in Git.

