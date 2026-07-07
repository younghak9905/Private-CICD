# Nexus

## Current Version

```text
Release: nexus
Namespace: nexus
Chart: custom local chart
App version: 3.93.2
```

## Render And Diff

```bash
helm template nexus apps/nexus -n nexus > /tmp/nexus-rendered.yaml
grep -R "image:" /tmp/nexus-rendered.yaml
kubectl -n nexus diff -f /tmp/nexus-rendered.yaml
```

## Notes

- Nexus is maintained as a local custom chart, not a vendored upstream chart.
- Keep PVC storageClass `csi-cinder-sc-retain`.
- Do not expose Nexus publicly unless an authenticated private ingress policy is in place.

