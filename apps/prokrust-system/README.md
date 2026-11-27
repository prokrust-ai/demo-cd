# prokrust-system

Requires `prokrust-crd`, cert-manager, Argo Workflows, Kubernetes 1.35–1.37 and ARM64 nodes.
The Argo CD Application uses the externally managed `prokrust` AppProject and manual sync.

`values.yaml` contains the demo cluster's IPv6 network rules, AWS IRSA role and
Argo Workflows controller identity. Artifact access uses IRSA; registry and licensing
use the existing `harbor-reader` and `prokrust-license-credential` Secrets.
Adapt these settings when using another cluster.

The Kustomization pins each connector chart independently in Harbor. Change the
provider’s `version` to upgrade it. Keep its connection CRDs enabled in the CRD
Application. Shared controller and runtime settings live in
`connectors/common.yaml`. BigQuery’s identity and egress settings live in
`connectors/bigquery.yaml`. System discovers the registration ConfigMaps that the
connector charts install in its namespace.

The license deactivation Job runs before a cascading deletion of this System
Application. Ordinary syncs, individual resource pruning and orphaning the
Application preserve the activation.

HTTPRoutes, TLS, ExternalSecrets and environment-specific NetworkPolicies are managed
in the private deployment repository. Browser access uses `https://demo.prokrust.ai`
and OIDC/session Secrets `prokrust-demo-ory` and `prokrust-browser-sessions`.
Workflow links use the existing `/argo` endpoint.
Register `https://demo.prokrust.ai/auth/callback/sso` as the OIDC client's redirect URI.

See the [chart configuration](https://github.com/prokrust-ai/helm-charts/tree/main/charts/prokrust-system).
