---
title: Topic - LLM Inference Path, KV Cache, and Batching
type: topic
status: active
topic: LLM Inference and Serving
course: AI Engineering
tags: [inference, serving, kv-cache, batching, vllm]
prerequisites:
  - "[[AI Engineering - 80-20 Summary]]"
  - "[[AI Engineering - Roadmap]]"
related:
  - "[[Map - AI Engineering]]"
  - "[[Source - Kwon et al. 2023 - Efficient Memory Management for Large Language Model Serving with PagedAttention]]"
  - "[[Source - Hugging Face - KV Cache Strategies and Cache Explanation]]"
  - "[[Source - NVIDIA - Prefill, Decode, In-flight Batching, and Chunked Prefill]]"
  - "[[Source - vLLM - Features, Architecture, Metrics, and Tuning]]"
sources:
  - "https://arxiv.org/abs/2309.06180"
  - "https://huggingface.co/docs/transformers/en/kv_cache"
  - "https://huggingface.co/docs/transformers/cache_explanation"
  - "https://developer.nvidia.com/blog/streamlining-ai-inference-performance-and-deployment-with-nvidia-tensorrt-llm-chunked-prefill/"
  - "https://nvidia.github.io/TensorRT-LLM/advanced/gpt-attention.html"
  - "https://docs.vllm.ai/en/latest/design/arch_overview/"
  - "https://docs.vllm.ai/en/stable/design/metrics/"
  - "https://docs.vllm.ai/en/stable/configuration/optimization/"
last_updated: 2026-03-11
review_status: researched
priority: high
core_status: core
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---

# Topic - LLM Inference Path, KV Cache, and Batching

## Purpose

Build a practical mental model for what happens between a prompt arriving and tokens streaming back out, with special attention to prefill, decode, KV cache pressure, batching policy, and scheduler behavior.[^nvidia-phases][^vllm-arch]

## Why it matters

Serving quality is not just "how good the model is." In practice, user experience is strongly shaped by queueing, prefill cost, decode speed, and how much KV cache fits into GPU memory.[^nvidia-phases][^vllm-metrics][^pagedattention-paper]

## 80/20 summary

- A decoder-only LLM request usually splits into **prefill** and **decode**. Prefill processes the prompt and builds the KV cache; decode then generates tokens step by step while reusing that cache.[^nvidia-phases][^hf-cache-explained]
- **KV cache** reduces repeated compute during autoregressive generation, but it grows with sequence length and competes for GPU memory.[^hf-cache-explained][^pagedattention-paper]
- **Batching** is the main path to higher throughput, but static request-level batching wastes capacity when requests arrive at different times or have different prompt and output lengths.[^trt-ifb][^anyscale-cb]
- **Continuous batching** or **in-flight batching** lets the server add, remove, and rebalance requests between decode iterations instead of waiting for a whole batch to finish.[^trt-ifb][^anyscale-cb]
- **vLLM matters** because it combines continuous batching with PagedAttention-based KV cache management and operational metrics that make serving bottlenecks visible.[^pagedattention-paper][^vllm-metrics][^vllm-opt]

## The inference path in plain language

### 1. Request intake and scheduling

A serving engine receives the request, tokenizes the prompt, queues the work, and decides when it can run. vLLM's architecture overview is a useful model here: input processing, scheduling, model execution, and output processing are separate responsibilities.[^vllm-arch]

### 2. Prefill

During **prefill**, the model processes all input tokens and computes the KV cache that later decode steps will reuse.[^nvidia-phases] Long prompts often make prefill a large contributor to **time to first token (TTFT)**.[^nvidia-phases][^vllm-metrics]

### 3. Decode

During **decode**, the model generates output one token at a time while reusing cached keys and values from previous steps.[^hf-cache-explained] This is why decode performance is often measured with inter-token latency or generated tokens per second.[^vllm-metrics]

### 4. Scheduler decisions

In a real serving system, the scheduler decides which requests run next, which new requests can join the batch, and whether some requests must be preempted when cache pressure gets too high.[^vllm-arch][^vllm-opt]

## KV cache: the core tradeoff

The KV cache stores keys and values from previous tokens so the model does not need to recompute the full history at each decode step.[^hf-cache-explained] That is the speed win. The cost is memory.

The vLLM paper makes the systems consequence explicit: high-throughput serving is hard because per-request KV cache memory is large and changes dynamically as requests grow and shrink.[^pagedattention-paper] In practice, that means:

- longer prompts and longer generations consume more cache,
- more concurrent requests consume more cache,
- and KV cache pressure often becomes the real limit on concurrency even when weights already fit on the GPU.[^pagedattention-paper][^vllm-opt]

A durable mental model is: **weights decide whether the model loads; KV cache often decides whether the deployment serves well.**

## Static batching vs continuous batching

Traditional request-level batching runs a batch until it finishes. That works poorly for LLM traffic because prompt lengths, output lengths, and arrival times vary.[^anyscale-cb]

NVIDIA's TensorRT-LLM docs describe **in-flight batching** as continuous or iteration-level batching, where context-phase and generation-phase requests can be processed together.[^trt-ifb] The practical benefit is that after each decode iteration the scheduler can:
- add new requests,
- remove completed ones,
- and rebalance the next step.[^trt-ifb][^anyscale-cb]

This usually improves GPU utilization and helps throughput under realistic mixed traffic.[^trt-ifb][^anyscale-cb]

## Why prefill and decode feel different

NVIDIA's guidance separates the phases because they stress hardware differently: prefill is the expensive prompt-processing phase, while decode repeatedly processes the next token using previously built state.[^nvidia-phases] vLLM's tuning docs make the serving implication clear by describing chunked prefill as a way to mix **compute-bound prefill** with **memory-bound decode** more effectively.[^vllm-opt]

That explains why:
- long prompts often hurt TTFT more than steady-state token rate,
- decode-heavy workloads can look fast even when prompt-heavy traffic feels slow,
- and scheduler policy can change latency behavior even when the model is unchanged.[^nvidia-phases][^vllm-opt][^vllm-metrics]

## Why vLLM matters

The PagedAttention paper argues that existing systems waste KV-cache memory through fragmentation and duplication, which directly limits feasible batch size.[^pagedattention-paper] PagedAttention reduces that waste with a paging-style memory layout for KV cache.[^pagedattention-paper]

The current vLLM docs show that the project now combines that memory strategy with continuous batching, chunked prefill, prefix caching, quantization support, speculative decoding, and useful serving metrics.[^vllm-arch][^vllm-metrics][^vllm-opt]

## What to measure first

vLLM's metrics documentation provides a strong default measurement set:[^vllm-metrics]

- **Queue interval**
- **TTFT**
- **TPOT / inter-token latency**
- **End-to-end latency**
- **Prompt tokens/sec**
- **Generated tokens/sec**
- **GPU cache usage**
- **Prefix cache hit rate**
- **Preemptions**[^vllm-opt]

A practical rule: bad TTFT often points to prompt length, queueing, or prefill policy; bad streaming speed often points to decode scheduling, cache pressure, or batch shape.[^nvidia-phases][^vllm-opt][^vllm-metrics]

## Common mistakes

1. Treating "the model fits" as the same thing as "the model serves well."[^pagedattention-paper][^vllm-opt]
2. Looking only at average tokens/sec and ignoring queueing or p95/p99 behavior.[^vllm-metrics]
3. Assuming larger context is free instead of recognizing its cache and prefill costs.[^nvidia-phases][^pagedattention-paper]
4. Reading benchmark multipliers as universal promises instead of workload-specific evidence.[^pagedattention-paper][^anyscale-cb]

## What to read next

- Quantization tradeoffs
- Chunked prefill and prefix caching
- Eval design for serving changes
- Production observability for TTFT, TPOT, queueing, and cache pressure

## Supporting notes

- [[Source - Kwon et al. 2023 - Efficient Memory Management for Large Language Model Serving with PagedAttention]]
- [[Source - Hugging Face - KV Cache Strategies and Cache Explanation]]
- [[Source - NVIDIA - Prefill, Decode, In-flight Batching, and Chunked Prefill]]
- [[Source - vLLM - Features, Architecture, Metrics, and Tuning]]

[^pagedattention-paper]: Woosuk Kwon et al., "Efficient Memory Management for Large Language Model Serving with PagedAttention," SOSP 2023. Last verified 2026-03-11. https://arxiv.org/abs/2309.06180
[^hf-cache-explained]: Hugging Face Transformers docs, "Caching." Last verified 2026-03-11. https://huggingface.co/docs/transformers/cache_explanation
[^nvidia-phases]: NVIDIA Technical Blog, "Streamlining AI Inference Performance and Deployment with NVIDIA TensorRT-LLM Chunked Prefill," Nov. 15, 2024. Last verified 2026-03-11. https://developer.nvidia.com/blog/streamlining-ai-inference-performance-and-deployment-with-nvidia-tensorrt-llm-chunked-prefill/
[^trt-ifb]: NVIDIA TensorRT-LLM docs, "Multi-Head, Multi-Query, and Group-Query Attention" (in-flight batching section). Last verified 2026-03-11. https://nvidia.github.io/TensorRT-LLM/advanced/gpt-attention.html
[^vllm-arch]: vLLM docs, "Architecture Overview." Last verified 2026-03-11. https://docs.vllm.ai/en/latest/design/arch_overview/
[^vllm-metrics]: vLLM docs, "Metrics." Last verified 2026-03-11. https://docs.vllm.ai/en/stable/design/metrics/
[^vllm-opt]: vLLM docs, "Optimization and Tuning." Last verified 2026-03-11. https://docs.vllm.ai/en/stable/configuration/optimization/
[^anyscale-cb]: Anyscale engineering blog, "How continuous batching enables 23x throughput in LLM inference while reducing p50 latency," June 22, 2023. Last verified 2026-03-11. https://www.anyscale.com/blog/continuous-batching-llm-inference
