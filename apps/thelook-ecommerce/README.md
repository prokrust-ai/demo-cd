# TheLook workspace

The `thelook-ecommerce` Argo CD Application installs the public workspace chart in
the existing `thelook-ecommerce` namespace and enrolls it with `prokrust-system`.
It uses manual sync with server-side apply and shared-resource protection.

The Demo profile uses ARM64, IPv6, regional STS, the existing S3 endpoint CIDRs,
and the namespace-local `harbor-reader` image pull Secret. Worker accounts use the
working-store IRSA role. The two Ballista accounts use the separate Ballista role.
The ephemeral-storage quota is 128 GiB requested and 256 GiB limited. Guest and
admin OIDC bindings use the system's `sso` provider and its role claim.

The workspace enables connector discovery for the six providers in the system chart.
Each provider grants its controller access to this namespace through its system
chart value `workspaceRbacBindings`.

BigQuery pods run as `prokrust-connector-runtime` and exchange a projected
service-account token directly with Google STS. The other five connectors each
use `prokrust-connector-runtime-<provider>`. All six accounts reference
`harbor-reader` and disable automatic API-token mounting. Only BigQuery projects
the configured Google workload identity audience. Its runtime can reach cluster
DNS and the approved Google API networks over HTTPS.

## Before syncing

- Apply the Demo-managed `prokrust` AppProject update. It authorizes Argo CD to
  target `thelook-ecommerce` and deploy `LimitRange`, `ResourceQuota` and
  `ProkrustAccessBinding` resources from this repository.
- Sync `prokrust-system` before this workspace when changing connector identities or token admission.
- Sync the private registry application and wait for the workspace's
  `harbor-reader` ExternalSecret to report Ready.

After sync, verify the enrollment hook completes and the workspace namespace
reports `prokrust.ai/enrollment-phase=Ready`. The workspace chart manages the default policy, S3 artifact store, and on-demand Ballista cluster. The existing Ballista service accounts keep their separate IRSA role.
