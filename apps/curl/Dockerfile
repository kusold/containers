# curl on the shared Debian runtime base: upgraded OS packages,
# ca-certificates, and curl itself all come from the tier.
#
# The base pin (tag + digest) is the image's single renovate-managed
# dependency — digest updates automerge as the tier's :13 floats, and the
# release workflow derives the published tag (:13) from this same line, so
# published tags follow base majors automatically.
FROM ghcr.io/kusold/debian-base-runtime:13@sha256:f1c79a4201eef82e0f752d26335feb0bec71aa3f47f504bf6b093b4370736ef0

# checkov:skip=CKV_DOCKER_2:healthcheck is not required

USER nobody
VOLUME [ "/data" ]
WORKDIR /data
