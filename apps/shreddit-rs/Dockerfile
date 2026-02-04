# renovate: datasource=github-releases packageName=andrewbanchich/shreddit versioning=semver-coerced
ARG SHREDDIT_VERSION=1.1.8

FROM rust:1.93.0@sha256:4c7eb947d7e078f5c076e086c7b75c36ea0ec7c685f2244b3d79306deb7e44b7 AS builder
ARG SHREDDIT_VERSION

RUN mkdir -p /shreddit \
    && apt-get update \
    && apt-get install -y \
    curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /src

RUN curl --verbose --location --output shreddit.tar.gz https://github.com/andrewbanchich/shreddit/archive/refs/tags/v${SHREDDIT_VERSION}.tar.gz \
    && tar -xzf shreddit.tar.gz --strip 1 \
    && rm -f shreddit.tar.gz \
    && cargo build --release --all-features \
    && mkdir -p /shreddit \
    && cp /src/target/release/shreddit /shreddit/shreddit \
    && ls -al /shreddit

# checkov:skip=CKV_DOCKER_2:don't need a healthcheck for a cli tool
FROM debian:trixie@sha256:f54909a654375e6d74d4986255ed9d3e0b7176461799195855030bc5826b5e4b
COPY --from=builder --chown=nobody:nogroup /shreddit /shreddit
USER nobody:nogroup
ENV PATH="/shreddit:$PATH"
CMD [ "/shreddit/shreddit" ]
