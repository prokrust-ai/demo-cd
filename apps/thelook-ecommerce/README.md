# TheLook workspace

The `thelook-ecommerce` Argo CD Application installs the public workspace chart in
the existing `thelook-ecommerce` namespace and enrolls it with `prokrust-system`.
It uses manual sync with server-side apply and shared-resource protection.

The Demo profile uses ARM64, IPv6, regional STS, the existing S3 endpoint CIDRs,
and the namespace-local `harbor-reader` image pull Secret. Worker accounts use the
working-store IRSA role. The two Ballista accounts use the separate Ballista role.
The ephemeral-storage quota is 128 GiB requested and 256 GiB limited. Guest and
admin OIDC bindings use the system's `sso` provider and its role claim.

## Before syncing

- Pin a workspace chart release that includes the enrollment fix. The current
  pin, `0.22.0-rc.2`, still rejects the Demo system's `0.22.0-rc.4`.
- Apply the Demo-managed `prokrust` AppProject update. It authorizes Argo CD to
  target `thelook-ecommerce` and deploy `LimitRange`, `ResourceQuota` and
  `ProkrustAccessBinding` resources from this repository.
- Sync the private registry application and wait for the workspace's
  `harbor-reader` ExternalSecret to report Ready.

After sync, verify the enrollment hook completes and the workspace namespace
reports `prokrust.ai/enrollment-phase=Ready`. This app provisions workspace
infrastructure and browser access. Optional tenant defaults (policy, store and
compute), connector workloads, and example data have separate lifecycles.
