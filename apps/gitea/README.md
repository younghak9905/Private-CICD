# Gitea

## Current Version

```text
Release: gitea
Namespace: gitea
Chart: gitea 12.6.0
Installed appVersion: 1.26.1
Configured image tag: 1.26.4
```

## Update Vendored Chart

```bash
helm repo add gitea-charts https://dl.gitea.com/charts/
helm repo update

rm -rf apps/gitea/charts/gitea

helm pull gitea-charts/gitea \
  --version 12.6.0 \
  --untar \
  --untardir apps/gitea/charts
```

## Render And Diff

```bash
helm template gitea apps/gitea -n gitea > /tmp/gitea-rendered.yaml
grep -R "image:" /tmp/gitea-rendered.yaml
kubectl -n gitea diff -f /tmp/gitea-rendered.yaml
```

## Notes

- Keep the release name `gitea` and namespace `gitea` to adopt the existing install.
- Do not store generated PostgreSQL or Gitea secrets in plain text.
- Confirm existing Secret and PVC names before first Argo CD sync.
- Start with manual sync; enable automated prune only after diff is stable.

