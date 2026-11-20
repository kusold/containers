# renovate: datasource=docker packageName=icloudpd/icloudpd versioning=docker
ARG ICLOUDPD_VERSION=1.34.0@sha256:1aa2f97167ec49659c2ec760d3c4599418c519981d61c94d60012e50049ad2c7

FROM icloudpd/icloudpd:${ICLOUDPD_VERSION}
# Upgrade pre-installed packages (openssl/libcrypto ship with known CVEs in
# the pinned upstream base) and add curl for the notification scripts.
RUN apk upgrade && apk add --no-cache \
    curl

RUN adduser -D icloudpd
USER icloudpd

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=10 CMD [ "curl -f http://localhost:8080 || exit 1" ]
