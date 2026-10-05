FROM caddy:2.11.6-builder@sha256:f5b1a66449d305280e559dba0ab9f7ce9a2a14c527c79d7c6fb48abc8f895818 AS builder

RUN xcaddy build \
    --with github.com/tailscale/caddy-tailscale@fd3f49d73216641b9cbe9167bbb05250c0ffc6d6

# checkov:skip=CKV_DOCKER_3:upstream image creates a non-root user
# checkov:skip=CKV_DOCKER_2:upstream image creates healthcheck
FROM caddy:2.11.6@sha256:3422ce6de165df66534f9b9ba50efaf457114ec961763cc52f5dbdaac2972d73
COPY --from=builder /usr/bin/caddy /usr/bin/caddy
