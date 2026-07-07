# Harbor

## Current Version

```text
Release: harbor
Namespace: harbor
Chart: harbor 1.19.1
App version: 2.15.1
```

## Update Vendored Chart

```bash
helm repo add harbor https://helm.goharbor.io
helm repo update

rm -rf apps/harbor/charts/harbor

helm pull harbor/harbor \
  --version 1.19.1 \
  --untar \
  --untardir apps/harbor/charts
```

## Mirror Images

```bash
export HARBOR_VERSION=v2.15.1
export KCR=registry.cloud.kt.com/2e2koiqr

IMAGES=(
  goharbor/nginx-photon
  goharbor/harbor-portal
  goharbor/harbor-core
  goharbor/harbor-jobservice
  goharbor/registry-photon
  goharbor/harbor-registryctl
  goharbor/harbor-db
  goharbor/redis-photon
  goharbor/harbor-exporter
)

for img in "${IMAGES[@]}"; do
  docker pull docker.io/${img}:${HARBOR_VERSION}
  docker tag docker.io/${img}:${HARBOR_VERSION} ${KCR}/${img}:${HARBOR_VERSION}
  docker push ${KCR}/${img}:${HARBOR_VERSION}
done
```

## Required Secret

```bash
kubectl -n harbor get secret kcr-regcred
```

Create it manually if missing. Do not commit registry credentials to Git.

## Render And Diff

```bash
helm template harbor apps/harbor -n harbor > /tmp/harbor-rendered.yaml
grep -R "image:" /tmp/harbor-rendered.yaml
kubectl -n harbor diff -f /tmp/harbor-rendered.yaml
```

## Notes

- Keep `persistence.resourcePolicy=keep`.
- Keep `updateStrategy.type=Recreate` for RWO volumes.
- KT volumes should use 10Gi multiples.
- Replace `CHANGE_ME_USE_EXISTING_SECRET` with a proper Secret-based chart setting before first sync.
- Start with manual sync; enable automated prune only after diff is stable.

