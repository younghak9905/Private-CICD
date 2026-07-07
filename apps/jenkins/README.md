# Jenkins

## Current Version

```text
Release: jenkins
Namespace: jenkins
Chart: jenkins 5.9.29
App version: 2.555.3
Controller image: 2.555.3-jdk21
```

## Update Vendored Chart

```bash
helm repo add jenkins https://charts.jenkins.io
helm repo update

rm -rf apps/jenkins/charts/jenkins

helm pull jenkins/jenkins \
  --version 5.9.29 \
  --untar \
  --untardir apps/jenkins/charts
```

## Render And Diff

```bash
helm template jenkins apps/jenkins -n jenkins > /tmp/jenkins-rendered.yaml
grep -R "image:" /tmp/jenkins-rendered.yaml
kubectl -n jenkins diff -f /tmp/jenkins-rendered.yaml
```

## Notes

- Keep the release name `jenkins` and namespace `jenkins`.
- Confirm existing admin Secret behavior before first sync.
- Do not store Jenkins credentials in Git.
- Start with manual sync; enable automated prune only after diff is stable.

