# renovate: datasource=docker packageName=icloudpd/icloudpd versioning=docker
ARG ICLOUDPD_VERSION=1.32.3@sha256:af2bf40cb2c1d42051793b4c3c04c825950697d7fedcd12bd8455d6952395801

FROM icloudpd/icloudpd:${ICLOUDPD_VERSION}
# Upgrade pre-installed packages (openssl/libcrypto ship with known CVEs in
# the pinned upstream base) and add curl for the notification scripts.
RUN apk upgrade && apk add --no-cache \
    curl

RUN adduser -D icloudpd
USER icloudpd

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=10 CMD [ "curl -f http://localhost:8080 || exit 1" ]
