# kagent's acp-sandbox-hermes with an acp-shim wrapper that appends
# model.base_url to ~/.hermes/config.yaml before exec'ing the real shim.
#
# Why: kagent 0.10.3's hermesConfigPrelude writes the sandbox's hermes
# config with only model/provider/api_mode — it drops the ModelConfig's
# openAI.baseUrl, so hermes's openai-api overlay pins api.openai.com and
# every chat turn hangs (wrong endpoint, and not an allowed actor
# egress). AgentHarness spec.env doesn't reach the container either:
# substrate 0.0.9's workload specs plumb only name/image/command. The
# wrapper runs after the prelude writes the config and before hermes
# reads it, and skips itself if a base_url ever appears upstream (then
# this image can be retired for the stock one).
#
# Versioning: HERMES_SANDBOX_VERSION is the image's own semver; the
# base ARG below is a tag+digest pin renovate manages in one PR.
# renovate: datasource=docker packageName=ghcr.io/kagent-dev/kagent/acp-sandbox-hermes versioning=docker
ARG HERMES_SANDBOX_BASE_VERSION=0.10.3@sha256:e107358cc9f3a04d84d4a765f583a79f7a965554fb3905f40a38b076214fa42f
FROM ghcr.io/kagent-dev/kagent/acp-sandbox-hermes:${HERMES_SANDBOX_BASE_VERSION}

# The image's own semver — NOT a dependency pin; no renovate comment on
# purpose. The release workflow greps this line for the published tags.
ARG HERMES_SANDBOX_VERSION=0.1.0

# checkov:skip=CKV_DOCKER_2:healthcheck lives in the workload (substrate probes the ACP port)

RUN mv /usr/local/bin/acp-shim /usr/local/bin/acp-shim.real
COPY --chmod=0755 acp-shim /usr/local/bin/acp-shim
