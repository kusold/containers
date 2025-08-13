# renovate: datasource=github-releases packageName=andrewbanchich/shreddit versioning=semver-coerced
ARG SHREDDIT_VERSION=1.1.1

FROM rust:1.89.0-slim-bullseye@sha256:c54b9eda43e86b2bb0b8576618f0b267ffc016b60f4afa7d85b40cb874aab38c AS builder
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
FROM debian:bullseye-slim@sha256:849d9d34d5fe0bf88b5fb3d09eb9684909ac4210488b52f4f7bbe683eedcb851
COPY --from=builder --chown=nobody:nogroup /shreddit /shreddit
USER nobody:nogroup
ENV PATH="/shreddit:$PATH"
CMD [ "/shreddit/shreddit" ]
