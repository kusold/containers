# renovate: datasource=docker packageName=icloudpd/icloudpd versioning=docker
ARG ICLOUDPD_VERSION=1.28.2@sha256:b5fb50a0e541ecb1160afeb2773aef7f0948a6a035c429ace13f1e971657488c

FROM icloudpd/icloudpd:${ICLOUDPD_VERSION}
RUN apk add --no-cache \
    curl # Used in the notification scripts

RUN adduser -D icloudpd
USER icloudpd

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=10 CMD [ "curl -f http://localhost:8080 || exit 1" ]
