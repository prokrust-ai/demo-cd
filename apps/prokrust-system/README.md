# prokrust-system

Requires `prokrust-crd`, cert-manager, Argo Workflows, Kubernetes 1.35–1.37 and ARM64 nodes.

`values.yaml` contains the demo cluster's IPv6 network rules, AWS IRSA role and
Argo Workflows identity/image. Artifact access uses IRSA; registry and licensing
use the existing `harbor-reader` and `prokrust-license-credential` Secrets.
The Application uses manual sync. Adapt these settings when using another cluster.

See the [chart configuration](https://github.com/prokrust-ai/helm-charts/tree/main/charts/prokrust-system).
