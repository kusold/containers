# renovate: datasource=github-releases packageName=andrewbanchich/shreddit versioning=semver-coerced
ARG SHREDDIT_VERSION=1.1.11

FROM rust:1.96.1@sha256:1f0dbad1df66647807e6952d1db85d0b2bda7606cb2139d82517e4f009967376 AS builder
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
FROM debian:trixie-20260623@sha256:d07d1b51c39f51188e60be9b64e6bf769fa94e187f092bc32b91305cfa34ba5a
COPY --from=builder --chown=nobody:nogroup /shreddit /shreddit
USER nobody:nogroup
ENV PATH="/shreddit:$PATH"
CMD [ "/shreddit/shreddit" ]
