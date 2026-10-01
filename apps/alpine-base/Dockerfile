# Minimal shared Alpine base: upstream Alpine with all OS packages upgraded,
# so security updates land in one shared layer beneath all Alpine-based
# images in this repo instead of N per-image upgrade layers.
#
# ALPINE_BASE_VERSION is the image's own version, consumed by the release
# workflow's tag derivation (:3, :3.24, :3.24.0): MAJOR.MINOR tracks the
# Alpine release, PATCH bumps on deliberate content changes. NOT a dependency
# pin; no renovate annotation on purpose. When renovate automerges an Alpine
# minor bump below (e.g. 3.24 -> 3.26), bump this ARG to match in a
# follow-up (published tags float by digest regardless).
FROM alpine:3.24@sha256:294b683cb724975bec92580e1e685676bd4b50bda910ddb8c51d4cabeaec77e6

ARG ALPINE_BASE_VERSION=3.24.0

# checkov:skip=CKV_DOCKER_3:base image - child images create their own non-root users
# checkov:skip=CKV_DOCKER_2:healthcheck is not required (base image)

# Upgrading OS packages is this image's entire purpose.
# hadolint ignore=DL3017
RUN apk upgrade --no-cache
