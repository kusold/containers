# Minimal shared Debian base: upstream Debian with all OS packages upgraded,
# so security updates land in one shared layer beneath all Debian-based
# images in this repo instead of N per-image upgrade layers.
#
# DEBIAN_BASE_VERSION is the image's own version, consumed by the release
# workflow's tag derivation (:13, :13.0, :13.0.0): MAJOR.MINOR tracks the
# Debian release, PATCH bumps on deliberate content changes. NOT a dependency
# pin; no renovate annotation on purpose. Weekly rebuilds republish the same
# tags with a new digest (the digest is the rebuild record, not the version).
FROM debian:13@sha256:9cc080028c43b27d2074d63a5f9caf7166d731494965616c1a6d2827a004585c

ARG DEBIAN_BASE_VERSION=13.0.0

# checkov:skip=CKV_DOCKER_3:base image - child images create their own non-root users
# checkov:skip=CKV_DOCKER_2:healthcheck is not required (base image)

# Upgrading OS packages is this image's entire purpose.
# hadolint ignore=DL3005
RUN apt-get update \
    && apt-get -y upgrade \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
