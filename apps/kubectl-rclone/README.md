# kubectl-rclone

`kubectl` + `rclone` on Alpine — a tools image for GitOps jobs that exec
into Kubernetes pods and ship the output to S3-compatible storage.

First consumer: [rockymtn](https://github.com/kusold/rockymtn)'s pg-main
nightly logical backups (`pg_dump` via `kubectl exec` into the CNPG
primary, upload to MinIO via `rclone`). The job script lives in the
consumer's CronJob manifest on purpose — script changes shouldn't need an
image release, so this image only changes when the tools do.

## Versioning

`KUBECTL_RCLONE_VERSION` is the image's own SemVer (the release workflow
derives the published tags from it):

- **minor** — a tool version moved (kubectl / rclone). kubectl bumps are
  skew-sensitive (keep within one minor of the consuming cluster's API
  server), which is why the containers-repo renovate deliberately leaves
  `datasource=kubernetes` bumps unautomerged.
- **patch** — base image bump or rebuild, tools unchanged.

## Tool pins

Both binaries are downloaded pinned by `ARG` and checksum-verified at
build time (kubectl against dl.k8s.io's sidecar `.sha256`, rclone against
the release's `SHA256SUMS` asset). Renovate bumps the pins via its
`_VERSION=` custom manager — rclone minor/patch automerge; kubectl opens a
PR like everything else.
