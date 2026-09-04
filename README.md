# actions

Repository for GitHub Actions to manage DIS resources.

## Layout

Actions live in two namespaces:

| Namespace | Origin |
| --- | --- |
| `terraform/`, `flux/` (repo root) | DIS-native actions, developed here |
| `altinn/` | Copied from [`Altinn/altinn-platform`](https://github.com/Altinn/altinn-platform)'s `actions/` folder |

The two sets share action names but are **not** interchangeable — they were
developed independently and differ in inputs and behaviour. Most notably
`terraform/init` and `altinn/terraform/init` use different Azure state backends
and different state-file naming, and `altinn/flux/setup-flux-acr` exports
federated credentials for the Azure SDK while `flux/setup-flux-acr` does not.
Pick one namespace per workflow and stay in it.

### `altinn/` actions

| Action | Purpose |
| --- | --- |
| [`altinn/terraform/plan`](altinn/terraform/plan) | `init` + `plan`, uploads plan artifact, writes PR summary |
| [`altinn/terraform/apply`](altinn/terraform/apply) | `init` + `apply` of a previously uploaded plan |
| [`altinn/terraform/init`](altinn/terraform/init) | Install Terraform, resolve state path from OIDC branch/environment, `fmt`, `init`, `validate` |
| [`altinn/terraform/write-terraform-summary`](altinn/terraform/write-terraform-summary) | Renders the job summary / PR comment |
| [`altinn/flux/build-push-image`](altinn/flux/build-push-image) | Builds and pushes a Flux OCI artifact |
| [`altinn/flux/retag-image`](altinn/flux/retag-image) | Re-tags an existing OCI artifact in ACR |
| [`altinn/flux/verify-syncroot`](altinn/flux/verify-syncroot) | Validates a Flux syncroot folder structure |
| [`altinn/flux/setup-flux-acr`](altinn/flux/setup-flux-acr) | Installs the Flux CLI and authenticates to ACR |

Only the actions that repositories **outside** `altinn-platform` consume were
copied — `terraform/plan`, `terraform/apply`, `flux/build-push-image`,
`flux/retag-image` and `flux/verify-syncroot` — plus the three they call
internally (`terraform/init`, `terraform/write-terraform-summary`,
`flux/setup-flux-acr`).

Deliberately **not** copied:

- `terraform/plan-only` and `terraform/apply-only` — only used by
  `altinn-platform`'s own `k6tests-rg-deploy` workflow.
- `terraform/azure-app-token` — no consumers anywhere, and this repo already
  has its own copy at [`terraform/azure-app-token`](terraform/azure-app-token).
- `generate-k6-manifests` — its binary is compiled into
  `ghcr.io/altinn/altinn-platform/k6-action-image` from `altinn-platform`'s
  `infrastructure/` folder, so moving the action without that folder would give
  a copy whose Go sources have no effect at runtime.

These are otherwise verbatim copies; only the cross-references between them were
repointed at `dis-way/actions/altinn/...`. They are duplicated here ahead of
being removed from `altinn-platform`, so upstream remains the source of truth
until that migration happens.

## Referencing an action

Pin to a commit SHA and note the branch in a trailing comment so Renovate keeps
it current:

```yaml
- uses: dis-way/actions/altinn/terraform/plan@<hash> # main
```
