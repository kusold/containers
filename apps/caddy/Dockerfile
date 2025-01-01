ARG VERSION=2.8.4

FROM caddy:${VERSION}-builder AS builder

RUN xcaddy build \
    --with github.com/tailscale/caddy-tailscale

FROM caddy:${VERSION}
COPY --from=builder /usr/bin/caddy /usr/bin/caddy
