# delta-mem-mlx-sidecar

Research workspace for running and evaluating delta-mem on Apple Silicon through
a local MLX/OpenAI-compatible sidecar.

The sidecar lives in `delta-mem-sidecar/`. It supports fake, official upstream,
MLX backbone, and MLX delta-mem runtime tracks. Session state is isolated by the
generic header:

```text
X-Delta-Mem-Session-Key: <stable logical session key>
```

## Status (November 2026)

delta-mem-mlx-sidecar is an OpenAI-compatible chat-completions server for
Apple Silicon that runs MLX models and can preload hidden memory state
(`attention_state`) before generation, with a benchmark toolbelt for
session-memory probes and sanitized transcript replay. It ran the δ-mem
adapter against a frozen Qwen3-4B backbone and measured what different
memory preloads did.

The results, honestly: small local runs, not paper-grade. Flat on
paper-style synthetic probes, about 1.07x on a local LoCoMo slice, and up
to 1.3x when the preload was short retrieved snippets
(`wiki/Benchmark-Findings.md`). The clearest finding is that the relevance
and fact density of a preload matter more than its size.

Know before you build on it: active work paused in May 2026. MLX, mlx-lm
and OpenClaw have all moved since, so expect breakage at the model-loading
and OpenClaw-facing seams. The FastAPI surface and the benchmark scripts
are self-contained.

Issues and pull requests are welcome; replies may be slow.

Related: for serving MLX models behind an OpenAI-compatible API, mlx-lm's
`mlx_lm.server`, https://github.com/ml-explore/mlx-lm; mlx-openai-server,
https://github.com/cubist38/mlx-openai-server; oMLX, https://omlx.ai;
LM Studio; Ollama's MLX backend. For agent memory in OpenClaw, the
built-in Markdown memory, https://docs.openclaw.ai/concepts/memory, and
the Mem0 plugin, https://github.com/mem0ai/mem0. For the method, the
δ-mem paper, https://arxiv.org/abs/2605.12357.

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
