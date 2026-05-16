# Agent Notes

This repository packages a local MLX sidecar that can run directly or behind OpenClaw.

Use the toy model path by default:

```text
DELTA_MEM_RUNTIME=mlx
DELTA_MEM_MODEL_PATH=mlx-community/Qwen2.5-0.5B-Instruct-4bit
DELTA_MEM_MODEL_ID=qwen2.5-0.5b-mlx-test
```

OpenClaw integration is optional. When used, it routes through OpenAI-compatible chat completions and requires:

```text
X-OpenClaw-Session-Key: <stable logical session key>
```

Do not add private planning notes, local status logs, machine caches, or downloaded model weights to this repo.

Do not vendor OpenClaw plugins by default. Reference optional plugin material from docs instead.
