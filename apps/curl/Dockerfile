FROM debian:trixie@sha256:fd8f5a1df07b5195613e4b9a0b6a947d3772a151b81975db27d47f093f60c6e6
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
