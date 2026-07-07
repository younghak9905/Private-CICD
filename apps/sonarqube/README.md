# SonarQube

## Current Version

```text
Release: sonarqube
Namespace: sonarqube
Chart: sonarqube 2026.3.0
App version: 2026.3.0
Community image: 26.5.0.122743-community
```

## Update Vendored Chart

```bash
helm repo add sonarqube https://SonarSource.github.io/helm-chart-sonarqube
helm repo update

rm -rf apps/sonarqube/charts/sonarqube

helm pull sonarqube/sonarqube \
  --version 2026.3.0 \
  --untar \
  --untardir apps/sonarqube/charts
```

## Required Secrets

```bash
kubectl -n sonarqube get secret sonarqube-db-secret
kubectl -n sonarqube get secret sonarqube-monitoring-passcode
kubectl -n sonarqube get secret harbor-regcred
```

## Render And Diff

```bash
helm template sonarqube apps/sonarqube -n sonarqube > /tmp/sonarqube-rendered.yaml
grep -R "image:" /tmp/sonarqube-rendered.yaml
kubectl -n sonarqube diff -f /tmp/sonarqube-rendered.yaml
```

## Notes

- SonarQube is not a blue/green workload.
- `initSysctl.enabled=true` requires privileged init container support.
- Start with manual sync; enable automated prune only after diff is stable.

