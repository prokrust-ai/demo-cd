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

- Match the chart's declared supported system composition to the installed
  `prokrust-component-versions` ConfigMap. Workspace chart `0.22.0-rc.2` declares
  `0.22.0-rc.2`; the current Demo system reports `0.22.0-rc.4`.
- The private `prokrust` AppProject must allow the `thelook-ecommerce` destination,
  core `LimitRange` and `ResourceQuota`, and `platform.prokrust.ai/ProkrustAccessBinding`.
  Enrollment also uses the project's existing scoped cluster RBAC permissions
  and its `prokrust-system` destination.
- Provision `harbor-reader` in the workspace through Demo's private ExternalSecret
  configuration. The system namespace's Secret is separate.

After sync, verify the enrollment hook completes and the workspace namespace
reports `prokrust.ai/enrollment-phase=Ready`. This app provisions workspace
infrastructure and browser access. Optional tenant defaults (policy, store and
compute), connector workloads, and example data have separate lifecycles.
