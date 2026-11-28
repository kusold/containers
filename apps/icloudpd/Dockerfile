# renovate: datasource=docker packageName=icloudpd/icloudpd versioning=docker
ARG ICLOUDPD_VERSION=1.35.0@sha256:ea674c058a416d2d35d93d58616ee9ce0bfd1f64df1a24aa1f4d0bd4e98bb487

FROM icloudpd/icloudpd:${ICLOUDPD_VERSION}
# Upgrade pre-installed packages (openssl/libcrypto ship with known CVEs in
# the pinned upstream base) and add curl for the notification scripts.
RUN apk upgrade && apk add --no-cache \
    curl

RUN adduser -D icloudpd
USER icloudpd

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=10 CMD [ "curl -f http://localhost:8080 || exit 1" ]
