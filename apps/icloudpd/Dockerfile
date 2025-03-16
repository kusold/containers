# renovate: datasource=docker packageName=icloudpd/icloudpd versioning=docker
ARG ICLOUDPD_VERSION=1.27.1@sha256:4c8b85493b57d8d92cc10eea1c972ddeee42e54d7ab2922fbab3648cd5c0d0fb

FROM icloudpd/icloudpd:${ICLOUDPD_VERSION}
RUN apk add --no-cache \
    curl # Used in the notification scripts

RUN adduser -D icloudpd
USER icloudpd

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=10 CMD [ "curl -f http://localhost:8080 || exit 1" ]
