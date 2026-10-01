# debian-base-runtime

Runtime tier on the shared Debian base — everything in
[`debian-base`](../debian-base) (upgraded OS packages) plus the tooling
nearly every runtime image needs: TLS roots (`ca-certificates`) and `curl`.
Child images get both tiers' patches in one shared layer stack.

## Tiers

| Image                 | Contents                                  |
| --------------------- | ----------------------------------------- |
| `debian-base`         | upgraded OS packages only                 |
| `debian-base-runtime` | `debian-base` + `ca-certificates`, `curl` |

## Versioning

`DEBIAN_BASE_RUNTIME_VERSION` is the parent pin (`ghcr.io/kusold/debian-base`
tag + digest) in one renovate-managed ARG (the release workflow derives the
published tag from it):

- Digest updates automerge as the parent's `:13` floats — the published
  `:13` of this image floats with it.
- A parent major (`:14`) arrives as a review-required PR; merging it moves
  this image's published tag to `:14` too.

Parent and published tags therefore always move in the same renovate PR;
there is no separate version to keep in sync.

Weekly rebuilds republish the same tags with a new digest — the digest is
the rebuild record, not the version.

## Consuming

Child images pin the **MAJOR tag + digest**:

```dockerfile
FROM ghcr.io/kusold/debian-base-runtime:13@sha256:...
```

Renovate's Docker versioning only proposes updates at the pinned tag's
precision, so a `:13@digest` pin produces automerged digest-only PRs. Pin
the major only — more precise tags generate tag-bump churn in every child.

## Operations

New ghcr packages are created **private** by default. After the first
publish, flip the package to public (GitHub → Packages →
kusold/debian-base-runtime → settings) or anonymous pulls and Renovate
digest tracking silently break.
