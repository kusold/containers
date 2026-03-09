# renovate: datasource=docker packageName=ghcr.io/jmbannon/ytdl-sub versioning=docker
ARG YTDL_SUB_VERSION=2026.03.09@sha256:e2a22413c4172121a9b46cb174417a60bcb7a67ecc7c2c2589149d1319392376

# checkov:skip=CKV_DOCKER_2:don't need a healthcheck for a cli tool
FROM ghcr.io/jmbannon/ytdl-sub:${YTDL_SUB_VERSION}

# Add POT Plugin - https://github.com/coletdjnz/yt-dlp-get-pot - https://github.com/Brainicism/bgutil-ytdlp-pot-provider
RUN apk add --no-cache py3-pip py3-setuptools \
    && python3 -m pip install --break-system-packages bgutil-ytdlp-pot-provider \
    && ytdl-sub -h

RUN adduser -D ytdlsub
USER ytdlsub
