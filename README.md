# Prokrust deployment example

- `apps/<app-name>/`: Kustomize sources and Helm values.
- `.argocd/`: Argo CD application definitions.
- `generated/*.yaml`: child Application definitions for the non-recursive parent.
- `generated/<app-name>/manifests.yaml`: rendered Kubernetes manifests.

Each pull request renders every app with Kustomize 5.8.1 and Helm 3.19.0,
using `--enable-helm --helm-kube-version 1.35.0`. Generated changes are committed
to the same branch. Fork and Dependabot PRs must include generated changes because
their workflow tokens are read-only.
