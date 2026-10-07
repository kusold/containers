# curl on the shared Debian runtime base: upgraded OS packages,
# ca-certificates, and curl itself all come from the tier.
#
# The base pin (tag + digest) is the image's single renovate-managed
# dependency — digest updates automerge as the tier's :13 floats, and the
# release workflow derives the published tag (:13) from this same line, so
# published tags follow base majors automatically.
FROM ghcr.io/kusold/debian-base-runtime:13@sha256:087debd23f9ac35792dd48339aaddf6e747c3ba7027680b331848202d4d9a182

# checkov:skip=CKV_DOCKER_2:healthcheck is not required

USER nobody
VOLUME [ "/data" ]
WORKDIR /data
