# python3 + rclone on Alpine: a tools image for GitOps jobs that need a
# real scripting language and cloud-storage moves. First consumer:
# rockymtn's gdrive-archive CronJob — a python script reads a TOML drive
# catalog and rclone-copies purchased Google Drive folders to TrueNAS.
# The job script and catalog deliberately stay in the consumer's manifest
# tree — this image only changes when the tools do.
#
# Versioning: GDRIVE_ARCHIVE_VERSION is the image's own semver, consumed
# by the release workflow's tag derivation — bump the MINOR when a tool
# version moves (rclone/python), the PATCH for base-image bumps or
# rebuilds. Consumers pin tag + digest and let renovate follow.
FROM ghcr.io/kusold/alpine-base-runtime:3@sha256:7941a7dada72da6461b75b4acabd11d3716ff2cc909d7ee88cbe937e45260ced

# The image's own semver — NOT a dependency pin; no renovate comment on
# purpose. The release workflow greps this line for the published tags.
ARG GDRIVE_ARCHIVE_VERSION=1.0.1

# checkov:skip=CKV_DOCKER_2:healthcheck is not required (batch job tools image)

# renovate: datasource=github-releases packageName=rclone/rclone
ARG RCLONE_VERSION=1.75.1
ARG TARGETARCH

# python3 stays here (the only consumer); ca-certificates, curl, and unzip
# come from the base tier. rclone is pinned by ARG and checksum-verified
# against the release's SHA256SUMS asset; no exec of unverified bytes.
RUN apk add --no-cache python3 \
    && curl -fsSL "https://github.com/rclone/rclone/releases/download/v${RCLONE_VERSION}/rclone-v${RCLONE_VERSION}-linux-${TARGETARCH}.zip" \
        -o /tmp/rclone.zip \
    && curl -fsSL "https://github.com/rclone/rclone/releases/download/v${RCLONE_VERSION}/SHA256SUMS" \
        -o /tmp/rclone-SHA256SUMS \
    && RCLONE_ZIP="rclone-v${RCLONE_VERSION}-linux-${TARGETARCH}.zip" \
    && echo "$(awk -v f="${RCLONE_ZIP}" '$2 == f {print $1}' /tmp/rclone-SHA256SUMS)  /tmp/rclone.zip" \
        | sha256sum -c - \
    && unzip -j -o /tmp/rclone.zip "*/rclone" -d /usr/local/bin \
    && chmod 0755 /usr/local/bin/rclone \
    && rm -rf /tmp/* \
    && python3 --version \
    && rclone version | head -2

USER nobody
WORKDIR /data
