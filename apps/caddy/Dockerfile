# Upstream caddy version, used solely for tag derivation: the release
# workflow greps this ARG for the published tags (:2, :2.11, :2.11.7).
# Renovate bumps it and the builder pin below together — both are depName
# "caddy" in this file, so the groupName "caddy" rule in renovate.json5
# bundles them into one PR and the tags can't drift from the binary inside.
# renovate: datasource=docker packageName=caddy versioning=docker
ARG CADDY_VERSION=2.11.7

# renovate: datasource=docker packageName=gcr.io/distroless/static-debian13 versioning=docker
ARG DISTROLESS_STATIC_VERSION=nonroot@sha256:e2e927ec666bae08560abb3c55d0659eceabb657f56b6782ab500a9fc7f555e3

FROM caddy:2.11.7-builder@sha256:b25f47453fa02f7e66c0828b4b1343658808b9000950b6825c4a1cb0c988f5f8 AS builder

RUN xcaddy build \
    --with github.com/tailscale/caddy-tailscale@fd3f49d73216641b9cbe9167bbb05250c0ffc6d6

# Default Caddyfile + welcome page, downloaded in the builder (distroless has
# no shell or wget) and copied into the runtime below. Pinned to the same
# caddyserver/dist commit upstream's official image uses — content-addressed
# by commit, which is exactly how upstream's Dockerfile verifies them.
ARG DIST_COMMIT=33ae08ff08d168572df2956ed14fbc4949880d94
RUN set -eux; \
    mkdir -p \
        /config/caddy \
        /data/caddy \
        /etc/caddy \
        /usr/share/caddy \
    ; \
    wget -O /etc/caddy/Caddyfile "https://github.com/caddyserver/dist/raw/${DIST_COMMIT}/config/Caddyfile"; \
    wget -O /usr/share/caddy/index.html "https://github.com/caddyserver/dist/raw/${DIST_COMMIT}/welcome/index.html"

# checkov:skip=CKV_DOCKER_2:distroless ships no shell or healthcheck binary to run a probe with
# Distroless static runtime: caddy and the tailscale plugin are pure Go, so
# the xcaddy-built binary is static and needs no libc; the base includes CA
# certs and tzdata but no shell or package manager. The distroless tag +
# digest pin is the global ARG at the top of this file — ARGs are
# stage-scoped, so it must be declared before the first FROM for the runtime
# FROM below to expand it — and is renovate-managed; the published tags
# derive from CADDY_VERSION above (upstream caddy bumps land via one grouped
# renovate PR that updates it and the builder pin together). USER nonroot
# (uid/gid 65532) cannot bind :80/:443 without the
# NET_BIND_SERVICE capability or a high-port site config — a consumer-side
# change.
FROM gcr.io/distroless/static-debian13:${DISTROLESS_STATIC_VERSION}

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
# /config and /data must exist and be writable by 65532 (distroless nonroot):
# caddy can't create them at runtime. Upstream chmods them 1777 for
# arbitrary-uid root runs; here they're simply owned by the nonroot user.
COPY --from=builder --chown=65532:65532 /config /config
COPY --from=builder --chown=65532:65532 /data /data
COPY --from=builder /etc/caddy/Caddyfile /etc/caddy/Caddyfile
COPY --from=builder /usr/share/caddy/index.html /usr/share/caddy/index.html

# See https://caddyserver.com/docs/conventions#file-locations for details
ENV XDG_CONFIG_HOME=/config
ENV XDG_DATA_HOME=/data

EXPOSE 80
EXPOSE 443
EXPOSE 443/udp
EXPOSE 2019

WORKDIR /srv

USER nonroot:nonroot

# Direct exec, no entrypoint: distroless has no shell to wrap arguments, so
# overriding the command means passing a full argv (e.g. K8s command:/args:).
CMD [ "caddy", "run", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile" ]
