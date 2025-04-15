# renovate: datasource=docker packageName=icloudpd/icloudpd versioning=docker
ARG ICLOUDPD_VERSION=1.27.3@sha256:3c9e9d23b13103b9a19db6e0c050dc5d32074365dbcb4c63edc8d1ee3b395495

FROM icloudpd/icloudpd:${ICLOUDPD_VERSION}
RUN apk add --no-cache \
    curl # Used in the notification scripts

RUN adduser -D icloudpd
USER icloudpd

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=10 CMD [ "curl -f http://localhost:8080 || exit 1" ]
