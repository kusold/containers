# kubectl + rclone on Alpine: a tools image for GitOps jobs that exec into
# pods and ship the output to S3-compatible storage. First consumer:
# rockymtn's pg-main nightly logical backups (pg_dump via kubectl exec,
# upload via rclone). The job SCRIPT deliberately stays in the consumer's
# manifest — this image only changes when the tools do.
#
# Versioning: KUBECTL_RCLONE_VERSION is the image's own semver, consumed by
# the release workflow's tag derivation — bump the MINOR when a tool version
# moves (kubectl/rclone), the PATCH for base-image bumps or rebuilds.
# Consumers pin tag + digest and let renovate follow.
FROM alpine:3.24@sha256:294b683cb724975bec92580e1e685676bd4b50bda910ddb8c51d4cabeaec77e6

# The image's own semver — NOT a dependency pin; no renovate comment on
# purpose. The release workflow greps this line for the published tags.
ARG KUBECTL_RCLONE_VERSION=1.1.0

# checkov:skip=CKV_DOCKER_2:healthcheck is not required (batch job tools image)

# renovate: datasource=github-releases packageName=kubernetes/kubernetes extractVersion=^v?(?<version>.*)$
ARG KUBECTL_VERSION=v1.37.1
# renovate: datasource=github-releases packageName=rclone/rclone
ARG RCLONE_VERSION=1.75.1
ARG TARGETARCH

# Both downloads are checksum-verified: kubectl against dl.k8s.io's sidecar
# .sha256, rclone against the release's SHA256SUMS asset. No exec of
# unverified bytes.
RUN apk add --no-cache ca-certificates curl unzip \
    && curl -fsSL "https://dl.k8s.io/release/${KUBECTL_VERSION}/bin/linux/${TARGETARCH}/kubectl" \
        -o /usr/local/bin/kubectl \
    && curl -fsSL "https://dl.k8s.io/release/${KUBECTL_VERSION}/bin/linux/${TARGETARCH}/kubectl.sha256" \
        -o /tmp/kubectl.sha256 \
    && echo "$(awk '{print $1}' /tmp/kubectl.sha256)  /usr/local/bin/kubectl" | sha256sum -c - \
    && curl -fsSL "https://github.com/rclone/rclone/releases/download/v${RCLONE_VERSION}/rclone-v${RCLONE_VERSION}-linux-${TARGETARCH}.zip" \
        -o /tmp/rclone.zip \
    && curl -fsSL "https://github.com/rclone/rclone/releases/download/v${RCLONE_VERSION}/SHA256SUMS" \
        -o /tmp/rclone-SHA256SUMS \
    && RCLONE_ZIP="rclone-v${RCLONE_VERSION}-linux-${TARGETARCH}.zip" \
    && echo "$(awk -v f="${RCLONE_ZIP}" '$2 == f {print $1}' /tmp/rclone-SHA256SUMS)  /tmp/rclone.zip" \
        | sha256sum -c - \
    && unzip -j -o /tmp/rclone.zip "*/rclone" -d /usr/local/bin \
    && chmod 0755 /usr/local/bin/kubectl /usr/local/bin/rclone \
    && rm -rf /tmp/* \
    && kubectl version --client=true \
    && rclone version | head -2

USER nobody
WORKDIR /data
