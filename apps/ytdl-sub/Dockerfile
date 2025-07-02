# renovate: datasource=docker packageName=ghcr.io/jmbannon/ytdl-sub versioning=docker
ARG YTDL_SUB_VERSION=2025.07.02@sha256:9d9c50f5f7f37f072806724b22494b91f516bedab0dbc654706f0b4eda9ab81e

# checkov:skip=CKV_DOCKER_2:don't need a healthcheck for a cli tool
FROM ghcr.io/jmbannon/ytdl-sub:${YTDL_SUB_VERSION}

# Add POT Plugin - https://github.com/coletdjnz/yt-dlp-get-pot - https://github.com/Brainicism/bgutil-ytdlp-pot-provider
RUN apk add --no-cache py3-pip py3-setuptools \
    && python3 -m pip install --break-system-packages bgutil-ytdlp-pot-provider \
    && ytdl-sub -h

RUN adduser -D ytdlsub
USER ytdlsub
