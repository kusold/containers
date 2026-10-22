# z.ai vision MCP server (GLM vision models) for the rockymtn cluster's
# mcp namespace: the npm package is stdio-only, so supergateway bridges
# it to streamable HTTP (:8080/mcp) behind agentgateway
# (mcp.svc.rockymtn.org/zai-vision). The z.ai API key arrives as pod env
# (Z_AI_API_KEY), never baked in.
#
# Published tags derive from the server ARG below (:0, :0.1, :0.1.5);
# supergateway rides at its own ARG. Node 22 per the z.ai docs (>=22).
# renovate: datasource=npm packageName=@z_ai/mcp-server
ARG ZAI_VISION_MCP_VERSION=0.1.5
# renovate: datasource=npm packageName=supergateway
ARG SUPERGATEWAY_VERSION=4.2.0

# npm runs on the BUILD platform, not per-target: node SIGILLs under
# QEMU (the arm64 CI leg), and both packages are pure JS (zod, MCP SDK,
# express — no native addons), so the installed tree is portable. One
# --prefix tree so the .bin symlinks keep resolving after the COPY
# (copying a global bin out of its tree dereferences the symlink and
# breaks module resolution).
FROM --platform=$BUILDPLATFORM docker.io/library/node:22-alpine@sha256:0a7108bf6c7bf5de370ffb1a3ed6be93d405b43ff159f681a8d18c0e2bc2e402 AS deps

ARG ZAI_VISION_MCP_VERSION
ARG SUPERGATEWAY_VERSION
# No lockfile: two pinned, dependency-light leaves; one tree so shared
# deps dedupe.
RUN npm install --prefix /app \
      supergateway@${SUPERGATEWAY_VERSION} \
      @z_ai/mcp-server@${ZAI_VISION_MCP_VERSION}

FROM docker.io/library/node:22-alpine@sha256:0a7108bf6c7bf5de370ffb1a3ed6be93d405b43ff159f681a8d18c0e2bc2e402

COPY --from=deps /app /app
ENV PATH="/app/node_modules/.bin:${PATH}"

USER node

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
  CMD ["node", "-e", "fetch('http://127.0.0.1:8080/health').then(r=>process.exit(r.ok?0:1)).catch(()=>process.exit(1))"]

# Stateless bridge (no --stateful): each POST auto-initializes a fresh
# stdio child, so no session affinity — consumers see a stateless
# streamable-HTTP server. /mcp is supergateway's default path, and the
# health endpoint only exists if registered (there is no default).
CMD ["supergateway", \
     "--stdio", "node /app/node_modules/.bin/zai-mcp-server", \
     "--outputTransport", "streamableHttp", \
     "--port", "8080", \
     "--host", "0.0.0.0", \
     "--healthEndpoint", "/health"]
