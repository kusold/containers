FROM caddy:2.11.7-builder@sha256:1ab914bd604996ab195f6326665cd2ae5d3df62feba973e776c4c8d5967d57a5 AS builder

RUN xcaddy build \
    --with github.com/tailscale/caddy-tailscale@fd3f49d73216641b9cbe9167bbb05250c0ffc6d6

# checkov:skip=CKV_DOCKER_3:upstream image creates a non-root user
# checkov:skip=CKV_DOCKER_2:upstream image creates healthcheck
FROM caddy:2.11.7@sha256:f2a1290d0463aad60660d4ec134943f183ee2a5f6c3eb7bf32dd984f2f020772
COPY --from=builder /usr/bin/caddy /usr/bin/caddy
