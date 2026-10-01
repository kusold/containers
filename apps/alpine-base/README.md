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

`ALPINE_BASE_VERSION` is the upstream `alpine` tag + digest in one
renovate-managed pin (the release workflow derives the published tags from
it):

- Digest updates automerge while Alpine 3.24 is current — the published
  `:3`/`:3.24` float to the new digest.
- An Alpine minor bump (e.g. `3.24` → `3.26`) automerges as a single PR and
  moves the published tags to `:3`/`:3.26`.
- The `3` → `4` major arrives as a review-required PR.

Base and published tags therefore always move in the same renovate PR;
there is no separate version to keep in sync.

Weekly rebuilds republish the same tags with a new digest — the digest is
the rebuild record, not the version.

## Consuming

Child images pin the **MAJOR tag + digest**:

```dockerfile
FROM ghcr.io/kusold/alpine-base:3@sha256:...
```

Renovate's docker versioning only proposes updates at the pinned tag's
precision, so a `:3@digest` pin produces automerged digest-only PRs (plus a
review-required major PR when Alpine 4 lands). `:3.24` exists but is a
moving tag tied to the minor line — pinning it generates tag-bump PRs in
every child on each Alpine minor; pin the major instead.

## Operations

New ghcr packages are created **private** by default. After the first
publish, flip the package to public (GitHub → Packages → kusold/alpine-base
→ settings) or anonymous pulls and Renovate digest tracking silently break.
