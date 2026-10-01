# Minimal shared Alpine base: upstream Alpine with all OS packages upgraded,
# so security updates land in one shared layer beneath all Alpine-based
# images in this repo instead of N per-image upgrade layers.
#
# ALPINE_BASE_VERSION is the upstream tag + digest in one renovate-managed
# pin (same pattern as the icloudpd/ytdl-sub version ARGs): digest updates
# and Alpine minor bumps (3.24 -> 3.26) automerge as a single PR, and the
# release workflow derives the published tags (:3, :3.24) from this same
# value — so base and tags always move together with nothing to sync by
# hand.
# renovate: datasource=docker packageName=alpine versioning=docker
ARG ALPINE_BASE_VERSION=3.24@sha256:294b683cb724975bec92580e1e685676bd4b50bda910ddb8c51d4cabeaec77e6
FROM alpine:${ALPINE_BASE_VERSION}

# checkov:skip=CKV_DOCKER_3:base image - child images create their own non-root users
# checkov:skip=CKV_DOCKER_2:healthcheck is not required (base image)

# Upgrading OS packages is this image's entire purpose.
# hadolint ignore=DL3017
RUN apk upgrade --no-cache
