# SonarQube PostgreSQL

## Current Version

```text
Release: sonarqube-postgresql
Namespace: sonarqube
Chart: postgresql 18.7.11
App version: 18.4.0
Image tag: 17.6.0-debian-12-r4
```

## Update Vendored Chart

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update

rm -rf apps/sonarqube-postgresql/charts/postgresql

helm pull bitnami/postgresql \
  --version 18.7.11 \
  --untar \
  --untardir apps/sonarqube-postgresql/charts
```

## Required Secret

```bash
kubectl -n sonarqube get secret sonarqube-db-secret
```

This Secret must contain `postgres-password` and `password`.

## Render And Diff

```bash
helm template sonarqube-postgresql apps/sonarqube-postgresql -n sonarqube > /tmp/sonarqube-postgresql-rendered.yaml
grep -R "image:" /tmp/sonarqube-postgresql-rendered.yaml
kubectl -n sonarqube diff -f /tmp/sonarqube-postgresql-rendered.yaml
```

## Notes

- Do not store database passwords in Git.
- Keep the release name `sonarqube-postgresql` to preserve service DNS.
- Start with manual sync; enable automated prune only after diff is stable.

