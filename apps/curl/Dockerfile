FROM debian:trixie-20260623@sha256:d07d1b51c39f51188e60be9b64e6bf769fa94e187f092bc32b91305cfa34ba5a
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
