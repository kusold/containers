FROM debian:trixie-20260518@sha256:4ae67669760b807c19f23902a3fd7c121a6a70cf2ae709035674b23e712e4d62
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
