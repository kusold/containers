# curl on the shared Debian runtime base: upgraded OS packages,
# ca-certificates, and curl itself all come from the tier.
#
# The base pin (tag + digest) is the image's single renovate-managed
# dependency — digest updates automerge as the tier's :13 floats, and the
# release workflow derives the published tag (:13) from this same line, so
# published tags follow base majors automatically.
FROM ghcr.io/kusold/debian-base-runtime:13@sha256:885cfeee89ae6ceae7513e5bf150b20cc975326971ca143cd9715b911b1702d6

# checkov:skip=CKV_DOCKER_2:healthcheck is not required

USER nobody
VOLUME [ "/data" ]
WORKDIR /data
