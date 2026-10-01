# alpine-base

Minimal shared Alpine base — upstream `alpine:3.24` with every OS package
upgraded at build time, so security updates land in **one shared layer**
beneath all Alpine-based images in this repo instead of N per-image upgrade
layers. One patched layer on disk, and one place to fix a CVE for every
child image.

## Tiers

| Image | Contents |
| --- | --- |
| `alpine-base` | upgraded OS packages only |
| `alpine-base-runtime` | `alpine-base` + `ca-certificates`, `curl`, `unzip` |

## Versioning

`ALPINE_BASE_VERSION` is the image's own SemVer (the release workflow
derives the published tags from it):

- **major.minor** — tracks the Alpine release (`3.24`).
- **patch** — deliberate content changes.

Weekly rebuilds republish the same tags with a new digest — the digest is
the rebuild record, not the version. Renovate automerges upstream Alpine
minor bumps on the `FROM` line; bump `ALPINE_BASE_VERSION`'s minor to match
in a follow-up (published tags float by digest regardless).

## Consuming

Child images pin the **MAJOR tag + digest**:

```dockerfile
FROM ghcr.io/kusold/alpine-base:3@sha256:...
```

Renovate's docker versioning only proposes updates at the pinned tag's
precision, so a `:3@digest` pin produces automerged digest-only PRs (plus a
review-required major PR when Alpine 4 lands). Do not pin `:3.24` — every
base version bump would then generate tag-bump churn in every child.

## Operations

New ghcr packages are created **private** by default. After the first
publish, flip the package to public (GitHub → Packages → kusold/alpine-base
→ settings) or anonymous pulls and Renovate digest tracking silently break.
