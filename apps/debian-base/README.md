# debian-base

Minimal shared Debian base — upstream `debian:13` with every OS package
upgraded at build time, so security updates land in **one shared layer**
beneath all Debian-based images in this repo instead of N per-image upgrade
layers. One patched layer on disk, and one place to fix a CVE for every
child image.

## Tiers

| Image | Contents |
| --- | --- |
| `debian-base` | upgraded OS packages only |
| `debian-base-runtime` | `debian-base` + `ca-certificates`, `curl` |

## Versioning

`DEBIAN_BASE_VERSION` is the image's own SemVer (the release workflow
derives the published tags from it):

- **major.minor** — tracks the Debian release (`13.0` = Debian 13).
- **patch** — deliberate content changes.

Weekly rebuilds republish the same tags with a new digest — the digest is
the rebuild record, not the version.

## Consuming

Child images pin the **MAJOR tag + digest**:

```dockerfile
FROM ghcr.io/kusold/debian-base:13@sha256:...
```

Renovate's docker versioning only proposes updates at the pinned tag's
precision, so a `:13@digest` pin produces automerged digest-only PRs (plus a
review-required major PR when Debian 14 lands). Do not pin `:13.0` — every
base patch bump would then generate tag-bump churn in every child.

## Operations

New ghcr packages are created **private** by default. After the first
publish, flip the package to public (GitHub → Packages → kusold/debian-base
→ settings) or anonymous pulls and Renovate digest tracking silently break.
