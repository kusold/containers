# renovate: datasource=docker packageName=caddy versioning=docker
ARG CADDY_VERSION=2.10.0

FROM caddy:${CADDY_VERSION}-builder AS builder

RUN xcaddy build \
    --with github.com/tailscale/caddy-tailscale@fd3f49d73216641b9cbe9167bbb05250c0ffc6d6

# checkov:skip=CKV_DOCKER_3:upsteam image creates a non-root user
# checkov:skip=CKV_DOCKER_2:upsteam image creates healthcheck
FROM caddy:${CADDY_VERSION}
COPY --from=builder /usr/bin/caddy /usr/bin/caddy
