# alpine-base-runtime

Runtime tier on the shared Alpine base — everything in
[`alpine-base`](../alpine-base) (upgraded OS packages) plus the common
tooling: TLS roots (`ca-certificates`), `curl`, and `unzip`. Child images
get both tiers' patches in one shared layer stack.

## Tiers

| Image                 | Contents                                           |
| --------------------- | -------------------------------------------------- |
| `alpine-base`         | upgraded OS packages only                          |
| `alpine-base-runtime` | `alpine-base` + `ca-certificates`, `curl`, `unzip` |

## Versioning

`ALPINE_BASE_RUNTIME_VERSION` is the parent pin
(`ghcr.io/kusold/alpine-base` tag + digest) in one renovate-managed ARG
(the release workflow derives the published tag from it):

- Digest updates automerge as the parent's `:3` floats — the published `:3`
  of this image floats with it.
- A parent major (`:4`) arrives as a review-required PR; merging it moves
  this image's published tag to `:4` too.

Parent and published tags therefore always move in the same renovate PR;
there is no separate version to keep in sync.

Weekly rebuilds republish the same tags with a new digest — the digest is
the rebuild record, not the version.

## Consuming

Child images pin the **MAJOR tag + digest**:

```dockerfile
FROM ghcr.io/kusold/alpine-base-runtime:3@sha256:...
```

Renovate's Docker versioning only proposes updates at the pinned tag's
precision, so a `:3@digest` pin produces automerged digest-only PRs. Pin
the major only — more precise tags generate tag-bump churn in every child.

## Operations

New ghcr packages are created **private** by default. After the first
publish, flip the package to public (GitHub → Packages →
kusold/alpine-base-runtime → settings) or anonymous pulls and Renovate
digest tracking silently break.
