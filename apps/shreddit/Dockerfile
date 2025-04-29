# renovate: datasource=github-releases packageName=andrewbanchich/shreddit versioning=semver-coerced
ARG SHREDDIT_VERSION=1.1.0

FROM rust:1.86.0-slim-bullseye@sha256:8b4fb6e91904117e77ff7fb19d63dc83c6f1ed3caa7915b9390e5380b19cb55d AS builder
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
FROM debian:bullseye-slim@sha256:fdd75562fdcde1039c2480a1ea1cd2cf03b18b6e4cb551cabb03bde66ade8a5d
COPY --from=builder --chown=nobody:nogroup /shreddit /shreddit
USER nobody:nogroup
ENV PATH="/shreddit:$PATH"
CMD [ "/shreddit/shreddit" ]
