# renovate: datasource=github-releases packageName=andrewbanchich/shreddit versioning=semver-coerced
ARG SHREDDIT_VERSION=1.0.2

FROM rust:1.86.0-slim-bullseye@sha256:405e5b0fe9f74098d19f3a7b65368c9cd4323758dd6b2d6305a35df8d2cc0a2a AS builder
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
FROM debian:bullseye-slim@sha256:7aafeb23eaef5d5b1de26e967b9a78f018baaac81dd75246b99781eaaa2d59ef
COPY --from=builder --chown=nobody:nogroup /shreddit /shreddit
USER nobody:nogroup
ENV PATH="/shreddit:$PATH"
CMD [ "/shreddit/shreddit" ]
