---
title: Source - NVIDIA - Prefill, Decode, In-flight Batching, and Chunked Prefill
type: source
status: active
topic: LLM Inference and Serving
course: AI Engineering
tags: [source, docs, nvidia, tensorrt-llm, batching, chunked-prefill]
prerequisites: []
related:
  - "[[Topic - LLM Inference Path, KV Cache, and Batching]]"
  - "[[Source - vLLM - Features, Architecture, Metrics, and Tuning]]"
sources:
  - "https://developer.nvidia.com/blog/streamlining-ai-inference-performance-and-deployment-with-nvidia-tensorrt-llm-chunked-prefill/"
  - "https://nvidia.github.io/TensorRT-LLM/advanced/gpt-attention.html"
last_updated: 2026-03-11
review_status: researched
priority: high
---

# Source - NVIDIA - Prefill, Decode, In-flight Batching, and Chunked Prefill

## Source details

- **Publisher:** NVIDIA
- **Pages used:** TensorRT-LLM chunked prefill blog post and TensorRT-LLM documentation on in-flight batching
- **Primary link:** https://developer.nvidia.com/blog/streamlining-ai-inference-performance-and-deployment-with-nvidia-tensorrt-llm-chunked-prefill/
- **Supporting link:** https://nvidia.github.io/TensorRT-LLM/advanced/gpt-attention.html
- **Source type:** vendor docs and engineering guidance

## Why this source matters

These NVIDIA materials are excellent for the operational picture of how serving engines behave under load. They separate **prefill** from **decode**, explain why the phases stress hardware differently, and give a concise official definition of **in-flight batching**.

## Key claims

- A request typically goes through two phases:
  - **prefill**, where all input tokens are processed and the KV cache is computed;
  - **decode**, where output tokens are generated one at a time using the intermediate state from prefill.
- NVIDIA describes prefill as compute-heavy and decode as lighter per step because it mainly processes the newly generated token after the heavy prompt-side work is done.
- TensorRT-LLM supports **in-flight batching**, also called continuous batching or iteration-level batching, so context-phase and generation-phase sequences can be interleaved in the same serving loop.
- Chunked prefill divides large prefills into smaller units so they can coexist more smoothly with decode work, improving GPU utilization and often improving latency behavior.

## Useful examples

The best practical takeaway is that serving problems often come from **mixing two very different workloads in one engine**:
- long, compute-heavy prompt ingestion,
- and short, repetitive decode steps.

Chunked prefill is important because it makes that mixed workload easier to schedule instead of letting one giant prefill monopolize the engine.

## Caveats and limitations

- This is NVIDIA-centric guidance, so the implementation details align with TensorRT-LLM's design and hardware assumptions.
- Some recommendations are optimized for NVIDIA GPUs and may not transfer directly to other hardware stacks.
- It explains useful serving mechanics, but it is not a neutral benchmark study across all inference engines.

## What to verify later

- How chunked prefill and in-flight batching differ in implementation across vLLM, TensorRT-LLM, TGI, and other servers.
- Which latency metrics matter most for your actual workload: TTFT, TPOT, or end-to-end.
- Whether your main bottleneck is prompt-heavy traffic, decode-heavy traffic, or queueing under concurrency.

## Related notes

- [[Topic - LLM Inference Path, KV Cache, and Batching]]
- [[Source - vLLM - Features, Architecture, Metrics, and Tuning]]
