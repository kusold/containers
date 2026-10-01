# alpine-base

Minimal shared Alpine base — upstream `alpine:3.24` with every OS package
upgraded at build time, so security updates land in **one shared layer**
beneath all Alpine-based images in this repository instead of N per-image
upgrade layers. Patch the OS once here and every child image inherits it.

## Tiers

- `alpine-base` — every OS package upgraded, nothing else added.
- `alpine-base-runtime` — `alpine-base` plus `ca-certificates`, `curl`, and
  `unzip`.

## Versioning

`ALPINE_BASE_VERSION` is the upstream `alpine` tag + digest in one
renovate-managed pin (the release workflow derives the published tags from
it):

- Digest updates automerge while Alpine 3.24 is current — the published
  `:3`/`:3.24` float to the new digest.
- An Alpine minor bump (e.g. `3.24` → `3.26`) automerges as a single PR and
  moves the published tags to `:3`/`:3.26`.
- The `3` → `4` major arrives as a review-required PR.

Weekly rebuilds republish the same tags with a new digest — the digest is
the rebuild record, not the version.

## Consuming

Pin the **MAJOR tag + digest** in the child Dockerfile:

```dockerfile
FROM ghcr.io/kusold/alpine-base:3@sha256:...
```

Why the major and nothing tighter:

- A `:3@digest` pin receives automerged digest-only PRs from Renovate, so
  OS patches and Alpine minors flow in without editing the child's tag.
- The `3` → `4` jump is the only update that waits for review.
- `:3.24` exists but tracks the minor line — pinning it costs a tag-bump PR
  in every child on each Alpine minor.

## Operations

After the first publish, make the ghcr package public (Packages →
kusold/alpine-base → settings). New packages start private, which silently
breaks anonymous pulls and Renovate digest tracking.
