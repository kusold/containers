# gdrive-archive

`python3` + `rclone` on Alpine — a tools image for GitOps jobs that need a
real scripting language and cloud-storage moves.

First consumer: [rockymtn](https://github.com/kusold/rockymtn)'s
`gdrive-archive` CronJob — a python script reads a TOML drive catalog
(one block per purchased digital-download bundle) and `rclone copy`-ies
each Google Drive folder into TrueNAS, maintaining per-drive
`metadata.json` sidecars and a volume-root `_catalog.json`. The job
script and catalog live in the consumer's manifest tree on purpose —
script changes shouldn't need an image release, so this image only
changes when the tools do.

## Versioning

`GDRIVE_ARCHIVE_VERSION` is the image's own SemVer (the release workflow
derives the published tags from it):

- **minor** — a tool version moved (rclone; python via base bumps).
- **patch** — base image bump or rebuild, tools unchanged.

## Tool pins

rclone is downloaded pinned by `ARG` and checksum-verified at build time
(against the release's `SHA256SUMS` asset). python3 comes from the Alpine
base — stdlib only, no pip: the consumer's script parses TOML with
`tomllib` (Python 3.11+). Renovate bumps the rclone pin via its
`_VERSION=` custom manager (minor/patch automerge).
