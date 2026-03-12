---
title: Topic - Latency Engineering for LLM Systems
type: topic
status: active
topic: Production Operations
course: AI Engineering
tags: [latency, ttft, tpot, serving]
prerequisites:
  - "[[Concept - Context Windows, Prefill, and Decode]]"
  - "[[Topic - LLM Inference Path, KV Cache, and Batching]]"
related:
  - "[[Technique - Latency Budget Worksheet]]"
  - "[[Topic - LLM Observability and Tracing]]"
sources:
  - "https://docs.vllm.ai/en/stable/design/metrics/"
  - "https://developer.nvidia.com/blog/llm-inference-benchmarking-performance-tuning-with-tensorrt-llm/"
  - "https://developer.nvidia.com/blog/streamlining-ai-inference-performance-and-deployment-with-nvidia-tensorrt-llm-chunked-prefill/"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: core
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---

# Topic - Latency Engineering for LLM Systems

## Purpose

Explain how to measure, allocate, and improve latency in LLM systems where prompt shape, scheduling, retrieval, and decode behavior all interact.

## Why it matters

In real applications, users experience latency as a product feature, not a neat infrastructure metric. Poor latency can come from queueing, retrieval overhead, long prompts, KV cache pressure, or slow tool calls. Improving one stage without measuring the whole path often just moves the bottleneck.

## 80/20 summary

- Measure latency as a path, not a single number.
- Search for auth time, retrieval, prompt assembly, TTFT, streaming duration, tool calls, and response delivery instead of only end-to-end time.
- Longer prompts often hurt TTFT; longer outputs often hurt streaming duration and cache pressure.
- Budget latency before unimuportant tuning.

## What to measure

At minimum, measure:

- queue time
- time to first token (TTFT)
- inter-token latency or tokens/second
- end-to-end latency
- retrieval duration
- tool call duration
- prompt token count and generated token count

## Where latency usually comes from

- Queueing under concurrency
- long system prompts or retrieved context
- inefficient retrieval or reranking
- slow decode behavior on long outputs
- tool calls that dominate the request path
- cache pressure that lowers concurrency
