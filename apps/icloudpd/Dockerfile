# renovate: datasource=docker packageName=icloudpd/icloudpd versioning=docker
ARG ICLOUDPD_VERSION=1.33.0@sha256:086e191b031bc779bcd9e698e6704264efab3bd5ee7bebd1ac029ea9899b6b38

FROM icloudpd/icloudpd:${ICLOUDPD_VERSION}
# Upgrade pre-installed packages (openssl/libcrypto ship with known CVEs in
# the pinned upstream base) and add curl for the notification scripts.
RUN apk upgrade && apk add --no-cache \
    curl

RUN adduser -D icloudpd
USER icloudpd

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=10 CMD [ "curl -f http://localhost:8080 || exit 1" ]
