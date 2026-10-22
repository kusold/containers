# Runtime tier on the shared Alpine base: everything in alpine-base
# (upgraded OS packages) plus the common tooling: TLS roots, curl, unzip.
#
# ALPINE_BASE_RUNTIME_VERSION is the parent pin (tag + digest) in one
# renovate-managed ARG, same pattern as the minimal tier's upstream pin:
# digest updates automerge while the parent's :3 tag floats, and the
# release workflow derives the published tag (:3) from this same value.
# renovate: datasource=docker packageName=ghcr.io/kusold/alpine-base versioning=docker
ARG ALPINE_BASE_RUNTIME_VERSION=3@sha256:b7e9f769714b32f1209ce46f28a3ec48e3cd91aeed78229651d8a12187d26032
FROM ghcr.io/kusold/alpine-base:${ALPINE_BASE_RUNTIME_VERSION}

# checkov:skip=CKV_DOCKER_3:base image - child images create their own non-root users
# checkov:skip=CKV_DOCKER_2:healthcheck is not required (base image)

RUN apk add --no-cache ca-certificates curl unzip
