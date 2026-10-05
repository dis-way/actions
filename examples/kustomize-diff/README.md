# kustomize-diff example manifests

Throwaway manifests that give `.github/workflows/kustomize-diff.yml` something
real to build. `.github/workflows/kustomize-diff-example.yml` runs the reusable
workflow against `overlays/*` on every pull request that touches either this
directory or the workflows themselves, so the diff comment can be reviewed
end to end without wiring up a separate repository.

```
base/                 a Namespace, ConfigMap, Deployment (two containers) and Service
overlays/dev/         one replica, LOG_LEVEL=debug
overlays/prod/        three replicas, a higher CPU request, plus an HPA
```

## Seeing a comment

Open a pull request that changes anything under this directory. Good one-line
edits, each of which lands somewhere different in the comment:

| Edit | Shows up as |
| --- | --- |
| `newTag` in an overlay's `kustomization.yaml` | a row in the **Container images** table |
| `count` in `overlays/prod/kustomization.yaml` | a `spec.replicas` row on the Deployment |
| `LOG_LEVEL` in `base/configmap.yaml` | a `data.LOG_LEVEL` row on both ConfigMaps |
| `FEATURE_FLAGS` in `base/configmap.yaml` | an **Other changes** block (multi-line values do not fit a table) |
| remove `hpa.yaml` from `overlays/prod/kustomization.yaml` | the HPA marked **removed**, with its YAML |
| add a new file to an overlay's `resources` | that resource marked **added**, with its YAML |

## Removing all of this

Delete `examples/kustomize-diff/` and
`.github/workflows/kustomize-diff-example.yml`. Nothing else refers to them.
