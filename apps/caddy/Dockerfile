# renovate: datasource=docker packageName=library/caddy versioning=docker
ARG CADDY_VERSION=2.8.4

FROM caddy:${CADDY_VERSION}-builder AS builder

RUN xcaddy build \
    --with github.com/tailscale/caddy-tailscale

FROM caddy:${CADDY_VERSION}
COPY --from=builder /usr/bin/caddy /usr/bin/caddy
