# renovate: datasource=docker packageName=ghcr.io/jmbannon/ytdl-sub versioning=docker
ARG YTDL_SUB_VERSION=2026.01.30@sha256:cdc70901e8b1698d715ec246f6f95be8617c09cd38b50eaf9a0dc8d57511e75a

# checkov:skip=CKV_DOCKER_2:don't need a healthcheck for a cli tool
FROM ghcr.io/jmbannon/ytdl-sub:${YTDL_SUB_VERSION}

# Add POT Plugin - https://github.com/coletdjnz/yt-dlp-get-pot - https://github.com/Brainicism/bgutil-ytdlp-pot-provider
RUN apk add --no-cache py3-pip py3-setuptools \
    && python3 -m pip install --break-system-packages bgutil-ytdlp-pot-provider \
    && ytdl-sub -h

RUN adduser -D ytdlsub
USER ytdlsub
