# renovate: datasource=docker packageName=icloudpd/icloudpd versioning=docker
ARG ICLOUDPD_VERSION=1.25.1

FROM icloudpd/icloudpd:${ICLOUDPD_VERSION}
RUN apk add --no-cache \
    curl # Used in the notification scripts
