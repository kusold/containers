# renovate: datasource=docker packageName=ghcr.io/jmbannon/ytdl-sub versioning=docker
ARG YTDL_SUB_VERSION=2026.03.13@sha256:f9dacc45f48322a41e61d91035afe6dd83b1d95da45eb79ca23309fe04cdb52a

# checkov:skip=CKV_DOCKER_2:don't need a healthcheck for a cli tool
FROM ghcr.io/jmbannon/ytdl-sub:${YTDL_SUB_VERSION}

# Add POT Plugin - https://github.com/coletdjnz/yt-dlp-get-pot - https://github.com/Brainicism/bgutil-ytdlp-pot-provider
RUN apk add --no-cache py3-pip py3-setuptools \
    && python3 -m pip install --break-system-packages bgutil-ytdlp-pot-provider \
    && ytdl-sub -h

RUN adduser -D ytdlsub
USER ytdlsub
