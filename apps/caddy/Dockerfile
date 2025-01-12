# renovate: datasource=docker packageName=caddy versioning=docker
ARG CADDY_VERSION=2.8.4

FROM caddy:${CADDY_VERSION}-builder AS builder

RUN xcaddy build \
    --with github.com/tailscale/caddy-tailscale

# checkov:skip=CKV_DOCKER_3:upsteam image creates a non-root user
# checkov:skip=CKV_DOCKER_2:upsteam image creates healthcheck
FROM caddy:${CADDY_VERSION}
COPY --from=builder /usr/bin/caddy /usr/bin/caddy
