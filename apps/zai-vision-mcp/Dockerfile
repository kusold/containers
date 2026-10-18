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

FROM docker.io/library/node:22-alpine@sha256:0a7108bf6c7bf5de370ffb1a3ed6be93d405b43ff159f681a8d18c0e2bc2e402

ARG ZAI_VISION_MCP_VERSION
ARG SUPERGATEWAY_VERSION
# No lockfile: two pinned, dependency-light leaves (zod + the MCP SDK);
# --prefix keeps the server out of the global tree supergateway lives in.
RUN npm install --global supergateway@${SUPERGATEWAY_VERSION} \
 && npm install @z_ai/mcp-server@${ZAI_VISION_MCP_VERSION} --prefix /app

USER node

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
