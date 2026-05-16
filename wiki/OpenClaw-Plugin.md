# Optional OpenClaw Plugin

This repo does not require an OpenClaw plugin.

Default integration should use `openclaw.example.json5` and register the sidecar as an OpenAI-compatible chat-completions provider.

Use a plugin only when you need a custom embedded harness rather than a model provider.

Expected pattern:

1. Install an OpenClaw embedded-harness plugin separately.
2. Configure that plugin to call this sidecar's `/v1` endpoint.
3. Keep plugin ownership and release cadence separate from this sidecar repo.

This keeps the public sidecar repo runnable without OpenClaw and avoids bundling gateway-specific plugin code.
