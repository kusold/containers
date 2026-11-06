# curl on the shared Debian runtime base: upgraded OS packages,
# ca-certificates, and curl itself all come from the tier.
#
# The base pin (tag + digest) is the image's single renovate-managed
# dependency — digest updates automerge as the tier's :13 floats, and the
# release workflow derives the published tag (:13) from this same line, so
# published tags follow base majors automatically.
FROM ghcr.io/kusold/debian-base-runtime:13@sha256:d07245d822a21a03fde8a47cceda1dfa817cb33fe5879a11c5397a24bf48fe48

# checkov:skip=CKV_DOCKER_2:healthcheck is not required

USER nobody
VOLUME [ "/data" ]
WORKDIR /data
