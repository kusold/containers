# renovate: datasource=docker packageName=icloudpd/icloudpd versioning=docker
ARG ICLOUDPD_VERSION=1.33.1@sha256:8307c4878b36917b7b02b62487be829d3f9de6c75c38384a999de3929dd92b5c

FROM icloudpd/icloudpd:${ICLOUDPD_VERSION}
# Upgrade pre-installed packages (openssl/libcrypto ship with known CVEs in
# the pinned upstream base) and add curl for the notification scripts.
RUN apk upgrade && apk add --no-cache \
    curl

RUN adduser -D icloudpd
USER icloudpd

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=10 CMD [ "curl -f http://localhost:8080 || exit 1" ]
