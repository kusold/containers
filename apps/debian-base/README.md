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

`DEBIAN_BASE_VERSION` is the upstream `debian` tag + digest in one
renovate-managed pin (the release workflow derives the published tag from
it):

- Digest updates automerge while Debian 13 is current — the published `:13`
  floats to the new digest.
- The `13` → `14` major arrives as a review-required PR; merging it moves
  the published tag to `:14`.

Base and published tags therefore always move in the same renovate PR;
there is no separate version to keep in sync.

Weekly rebuilds republish the same tags with a new digest — the digest is
the rebuild record, not the version.

## Consuming

Child images pin the **MAJOR tag + digest**:

```dockerfile
FROM ghcr.io/kusold/debian-base:13@sha256:...
```

Renovate's docker versioning only proposes updates at the pinned tag's
precision, so a `:13@digest` pin produces automerged digest-only PRs (plus a
review-required major PR when Debian 14 lands). Pin the major only — more
precise tags generate tag-bump churn in every child.

## Operations

New ghcr packages are created **private** by default. After the first
publish, flip the package to public (GitHub → Packages → kusold/debian-base
→ settings) or anonymous pulls and Renovate digest tracking silently break.
