# Upstream shreddit version, used for the source download AND tag
# derivation: the release workflow greps this ARG (uppercased dir name +
# _VERSION) for the published tags (:1, :1.1, :1.1.11), so this one
# renovate-managed pin drives the build and the tags — they cannot drift.
# renovate: datasource=github-releases packageName=andrewbanchich/shreddit versioning=semver-coerced
ARG SHREDDIT_RS_VERSION=1.1.11

# Distroless pin: ARGs are stage-scoped, so this must be declared before the
# first FROM for the runtime FROM below to expand it.
# renovate: datasource=docker packageName=gcr.io/distroless/cc-debian13 versioning=docker
ARG DISTROLESS_CC_VERSION=nonroot@sha256:3ee00291cf386ec52a552e6a858d555debd74776eb4bfa53fa3df2cd6677515e

FROM rust:1.99.1@sha256:931c2c0531a8b143e044f0f3914dfe802f15bb3d1955f0b657e9359327ac2ffc AS builder
ARG SHREDDIT_RS_VERSION

RUN mkdir -p /shreddit \
    && apt-get update \
    && apt-get install -y \
    curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /src

RUN curl --verbose --location --output shreddit.tar.gz https://github.com/andrewbanchich/shreddit/archive/refs/tags/v${SHREDDIT_RS_VERSION}.tar.gz \
    && tar -xzf shreddit.tar.gz --strip 1 \
    && rm -f shreddit.tar.gz \
    && cargo build --release --all-features \
    && mkdir -p /shreddit \
    && cp /src/target/release/shreddit /shreddit/shreddit \
    && ls -al /shreddit

# checkov:skip=CKV_DOCKER_2:don't need a healthcheck for a cli tool
# Distroless cc runtime: glibc + CA certs for the Reddit API, no shell or
# package manager. The tag + digest pin is the global ARG at the top of
# this file (renovate-managed); the published tags (:1, :1.1, :1.1.11)
# derive from SHREDDIT_RS_VERSION above — the upstream shreddit version,
# replacing the old :13 that tracked the debian-base major.
FROM gcr.io/distroless/cc-debian13:${DISTROLESS_CC_VERSION}
COPY --from=builder /shreddit/shreddit /usr/bin/shreddit
USER nonroot:nonroot
CMD [ "/usr/bin/shreddit" ]
