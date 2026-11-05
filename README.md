# delta-mem-mlx-sidecar

Research workspace for running and evaluating delta-mem on Apple Silicon through
a local MLX/OpenAI-compatible sidecar.

The sidecar lives in `delta-mem-sidecar/`. It supports fake, official upstream,
MLX backbone, and MLX delta-mem runtime tracks. Session state is isolated by the
generic header:

```text
X-Delta-Mem-Session-Key: <stable logical session key>
```

## Where this stands (November 2026)

A research workspace frozen in May 2026, not maintained. The sidecar runs
and the benchmarks ran, but they were small local runs, not paper-grade:
flat on paper-style probes, about 1.07x on a local LoCoMo slice, and up to
1.3x only on short retrieved-snippet preloads in small fixtures
(`wiki/Benchmark-Findings.md`). The MLX and OpenClaw APIs it targets have
moved. Use it as a starting point for an OpenAI-compatible MLX sidecar
that injects hidden state, not as a dependency.

Better options today:

- Serving MLX models behind an OpenAI-compatible API: mlx-lm's built-in
  `mlx_lm.server`, https://github.com/ml-explore/mlx-lm;
  mlx-openai-server, https://github.com/cubist38/mlx-openai-server, for
  multimodal; oMLX, https://omlx.ai; LM Studio; or Ollama's MLX backend.
- Memory for OpenClaw agents: the built-in Markdown memory,
  https://docs.openclaw.ai/concepts/memory, or the Mem0 plugin,
  https://github.com/mem0ai/mem0.
- δ-mem itself: the paper, https://arxiv.org/abs/2605.12357, and the
  released adapter.

## Quick Start

```sh
cd delta-mem-sidecar
python3.11 -m venv .venv
. .venv/bin/activate
pip install -e ".[mlx,test]"
uvicorn delta_mem_sidecar.app:create_app --factory --host 127.0.0.1 --port 8765
```

## Benchmarks

- `benchmarks/session_memory_eval.py`: compact session-memory probes.
- `benchmarks/session_replay_eval.py`: replay one sanitized transcript/history
  file and run behavior probes.
- `benchmarks/transcript_toolbelt.py`: export, sanitize, probe, run, and report
  batches of JSONL transcripts.
- `benchmarks/full_delta_mem_bench.py`: compare Qwen3-4B MLX backbone with and
  without the released delta-mem adapter.

Benchmark notes are in `wiki/Benchmark-Findings.md`.
