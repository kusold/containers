# hermes-sandbox

kagent's `acp-sandbox-hermes` (AgentHarness `backend: hermes` runtime)
with one change: an `acp-shim` wrapper that appends `model.base_url` —
rockymtn's Agent Gateway, `https://ai.svc.rockymtn.org/v1` — to
`~/.hermes/config.yaml` before exec'ing the real shim.

kagent 0.10.3's `hermesConfigPrelude` writes that config with only
`model/default/provider/api_mode`, dropping the ModelConfig's
`openAI.baseUrl`; hermes's `openai-api` overlay then pins
`api.openai.com` and every chat turn hangs (wrong endpoint, and not an
allowed actor egress). `AgentHarness.spec.env` doesn't reach the
container either — substrate 0.0.9's workload specs plumb only
`name/image/command`, and ActorTemplate specs are immutable — so the
wrapper is the one reliable injection point between the prelude's
config write and hermes reading it.

The wrapper is a no-op once a `base_url` shows up in the config
(upstream fix lands) — at that point the consumer can pin the stock
image again.

Consumer: rockymtn `apps/kagent/hermes/harness.yaml`
(`spec.substrate.workloadImage`).
