# Runtime tier on the shared Debian base: everything in debian-base
# (upgraded OS packages) plus the tooling nearly every runtime image needs:
# TLS roots and curl.
#
# DEBIAN_BASE_RUNTIME_VERSION is the parent pin (tag + digest) in one
# renovate-managed ARG, same pattern as the minimal tier's upstream pin:
# digest updates automerge while the parent's :13 tag floats, and the
# release workflow derives the published tag (:13) from this same value.
# renovate: datasource=docker packageName=ghcr.io/kusold/debian-base versioning=docker
ARG DEBIAN_BASE_RUNTIME_VERSION=13@sha256:4ca66fbda393f14cd6ee63cbc87b5a6c393880112836b35688e97a26addfbd2b
FROM ghcr.io/kusold/debian-base:${DEBIAN_BASE_RUNTIME_VERSION}

# checkov:skip=CKV_DOCKER_3:base image - child images create their own non-root users
# checkov:skip=CKV_DOCKER_2:healthcheck is not required (base image)

RUN apt-get update \
    && apt-get install -y ca-certificates curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
