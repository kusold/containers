FROM debian:trixie-20260610@sha256:50d8c6e4413c3f5f5890208197bd439e60e4b0c6567326bfef8f3ede11d1645b
# Renovate doesn't recognize the date format
# https://docs.renovatebot.com/modules/versioning/debian/
# FROM debian:bookworm-20250203

# checkov:skip=CKV_DOCKER_2:healthcheck is not required

RUN apt-get update \
    && apt-get install -y curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

USER nobody
VOLUME [ "/data" ]
WORKDIR /data
