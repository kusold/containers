# Upstream shreddit version, used for the source download AND tag
# derivation: the release workflow greps this ARG (uppercased dir name +
# _VERSION) for the published tags (:1, :1.1, :1.1.11), so this one
# renovate-managed pin drives the build and the tags — they cannot drift.
# renovate: datasource=github-releases packageName=andrewbanchich/shreddit versioning=semver-coerced
ARG SHREDDIT_RS_VERSION=1.1.11

# Distroless pin: ARGs are stage-scoped, so this must be declared before the
# first FROM for the runtime FROM below to expand it.
# renovate: datasource=docker packageName=gcr.io/distroless/cc-debian13 versioning=docker
ARG DISTROLESS_CC_VERSION=nonroot@sha256:08476c244888bfb425bae1ae4dd7c17f093e972aab8426883d97f3c6f280f7c1

FROM rust:1.99.1@sha256:09027f873d45b8ecd1dc86f1fa13b7a62b875c961f017a0e331267b350430137 AS builder
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
