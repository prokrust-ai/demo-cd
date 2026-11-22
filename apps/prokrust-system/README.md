# prokrust-system

Requires `prokrust-crd`, cert-manager, Argo Workflows, Kubernetes 1.35–1.37 and ARM64 nodes.
The Argo CD Application uses the externally managed `prokrust` AppProject and manual sync.

`values.yaml` contains the demo cluster's IPv6 network rules, AWS IRSA role and
Argo Workflows controller identity. Artifact access uses IRSA; registry and licensing
use the existing `harbor-reader` and `prokrust-license-credential` Secrets.
Adapt these settings when using another cluster.

Enable a bundled connector with `connectors.<provider>.enabled` in `values.yaml`.
Demo enables BigQuery, GCS, Iceberg, Paimon, S3 and Synthetic. The providers share
the Demo pull Secret, ARM64 placement and IPv6 API endpoint through YAML aliases.
Override a provider’s controller or runtime settings in its map when needed.

Each enabled provider installs its controller and descriptor ConfigMap in
`prokrust-system`. The system chart configures the Operator and Web API from the
same provider map. Upgrade the system chart pin to consume new bundled releases.
Keep the corresponding connection CRDs enabled in the CRD Application.

HTTPRoutes, TLS, ExternalSecrets and environment-specific NetworkPolicies are managed
in the private deployment repository. Browser access uses `https://demo.prokrust.ai`
and OIDC/session Secrets `prokrust-demo-ory` and `prokrust-browser-sessions`.
Workflow links use the existing `/argo` endpoint.
Register `https://demo.prokrust.ai/auth/callback/sso` as the OIDC client's redirect URI.

See the [chart configuration](https://github.com/prokrust-ai/helm-charts/tree/main/charts/prokrust-system).
