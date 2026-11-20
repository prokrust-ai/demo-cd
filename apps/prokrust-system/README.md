# prokrust-system

Requires `prokrust-crd`, cert-manager, Argo Workflows, Kubernetes 1.35–1.37 and ARM64 nodes.

Before syncing, configure network access in `values.yaml` and create the referenced
registry, license and artifact credentials Secrets in `prokrust-system`.
Match `workflowIntegrity.argoControllerUsername` and `licensing.argoExecutorImage`
to your Argo Workflows installation. The Application uses manual sync until configured.

See the [chart configuration](https://github.com/prokrust-ai/helm-charts/tree/main/charts/prokrust-system).
