# renovate: datasource=docker packageName=icloudpd/icloudpd versioning=docker
ARG ICLOUDPD_VERSION=1.27.5@sha256:0a3f43cf4eef6a6f5b25b98d00887d48a068a42e6a5b35bd10cd941fb583a3db

FROM icloudpd/icloudpd:${ICLOUDPD_VERSION}
RUN apk add --no-cache \
    curl # Used in the notification scripts

RUN adduser -D icloudpd
USER icloudpd

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=10 CMD [ "curl -f http://localhost:8080 || exit 1" ]
