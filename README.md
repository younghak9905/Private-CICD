# platform-gitops

GitOps repository for the saejong CI/CD platform.

## Layout

```text
bootstrap/
  root-app.yaml

applications/
  argocd.yaml
  gitea.yaml
  harbor.yaml
  jenkins.yaml
  nexus.yaml
  sonarqube-postgresql.yaml
  sonarqube.yaml
  springboot-demo.yaml

apps/
  argocd/
  gitea/
  harbor/
  jenkins/
  nexus/
  sonarqube-postgresql/
  sonarqube/
  springboot-demo/
```

## Bootstrap

Apply the root app once:

```bash
kubectl apply -f bootstrap/root-app.yaml
```

`root-app` watches `applications/` and creates the individual Argo CD Applications.

## Vendored Charts

Infrastructure apps use vendored Helm charts under each `apps/<name>/charts/` directory.

Each app README includes the exact `helm pull --untar` command required to refresh the vendored chart.

Before first sync, populate the chart directories, for example:

```bash
helm pull harbor/harbor \
  --version 1.19.1 \
  --untar \
  --untardir apps/harbor/charts
```

## Sync Policy

Existing stateful infrastructure starts with manual sync in its Application manifest.

Enable automated sync and prune only after:

```bash
helm template <release> apps/<name> -n <namespace> > /tmp/<name>.yaml
kubectl -n <namespace> diff -f /tmp/<name>.yaml
```

## Secrets

Do not store plain secrets in this repository.

Use one of:

```text
- manually pre-created Kubernetes Secrets
- Sealed Secrets
- SOPS
- External Secrets Operator
```

Current manifests assume required Secrets already exist in their namespaces.

## App Flow

```text
springboot-demo source repo
-> Jenkins
-> SonarQube
-> Harbor
-> platform-gitops values update
-> Argo CD
-> Kubernetes
```

