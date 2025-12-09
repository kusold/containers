FROM debian:trixie@sha256:0d01188e8dd0ac63bf155900fad49279131a876a1ea7fac917c62e87ccb2732d
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
