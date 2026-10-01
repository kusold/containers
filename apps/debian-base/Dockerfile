# Minimal shared Debian base: upstream Debian with all OS packages upgraded,
# so security updates land in one shared layer beneath all Debian-based
# images in this repo instead of N per-image upgrade layers.
#
# DEBIAN_BASE_VERSION is the upstream tag + digest in one renovate-managed
# pin (same pattern as the icloudpd/ytdl-sub version ARGs): digest updates
# automerge while Debian 13 is current, and the 13 -> 14 major arrives as a
# review-required PR. The release workflow derives the published tag (:13)
# from this same value, so base and tags always move in the same PR with
# nothing to sync by hand.
# renovate: datasource=docker packageName=debian versioning=docker
ARG DEBIAN_BASE_VERSION=13@sha256:9cc080028c43b27d2074d63a5f9caf7166d731494965616c1a6d2827a004585c
FROM debian:${DEBIAN_BASE_VERSION}

# checkov:skip=CKV_DOCKER_3:base image - child images create their own non-root users
# checkov:skip=CKV_DOCKER_2:healthcheck is not required (base image)

# Upgrading OS packages is this image's entire purpose.
# hadolint ignore=DL3005
RUN apt-get update \
    && apt-get -y upgrade \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
