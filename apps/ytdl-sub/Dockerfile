# renovate: datasource=docker packageName=ghcr.io/jmbannon/ytdl-sub versioning=docker
ARG YTDL_SUB_VERSION=2026.06.23@sha256:abbdc71622aa5485c379de0f2ce6cfcc891fdc595c47fe68c805db5d26be89f9

# checkov:skip=CKV_DOCKER_2:don't need a healthcheck for a cli tool
FROM ghcr.io/jmbannon/ytdl-sub:${YTDL_SUB_VERSION}

# Add POT Plugin - https://github.com/coletdjnz/yt-dlp-get-pot - https://github.com/Brainicism/bgutil-ytdlp-pot-provider
RUN apk add --no-cache py3-pip py3-setuptools \
    && python3 -m pip install --break-system-packages bgutil-ytdlp-pot-provider \
    && ytdl-sub -h

RUN adduser -D ytdlsub
USER ytdlsub
