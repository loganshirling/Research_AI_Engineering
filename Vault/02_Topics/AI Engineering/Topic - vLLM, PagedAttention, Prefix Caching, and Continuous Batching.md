
---
title: Topic - vLLM, PagedAttention, Prefix Caching, and Continuous Batching
type: topic
status: active
topic: Advanced AI Systems
course: AI Engineering
tags: [vllm, pagedattention, prefix-caching, batching, serving]
prerequisites:
  - "[[Topic - LLM Inference Path, KV Cache, and Batching]]"
  - "[[Topic - Latency Engineering for LLM Systems]]"
related:
  - "[[Topic - Production Bottlenecks and Observability for LLM Systems]]"
  - "[[Source - Advanced Systems Expansion - Core Sources]]"
sources:
  - "https://arxiv.org/abs/2309.06180"
  - "https://docs.vllm.ai/en/latest/"
last_updated: 2026-03-12
review_status: researched
priority: high
core_status: expansion
practical_value: very_high
explanatory_power: high
difficulty: hard
must_know: true
---

# Topic - vLLM, PagedAttention, Prefix Caching, and Continuous Batching

## Purpose

Build a practical mental model for why modern LLM serving performance depends on memory management and scheduler behavior, not just raw GPU size.

## Why it matters

Many self-hosted systems become slow for reasons that look mysterious from the application layer:
- first-token latency spikes,
- throughput drops after concurrency rises,
- repeated prompts still cost too much,
- and large models feel unstable under mixed traffic.

vLLM matters because it makes those tradeoffs more visible and more controllable.

## 80/20 summary

The big ideas are:
- **PagedAttention** reduces wasted KV-cache space by managing cache blocks more like virtual memory.
- **Continuous batching** keeps the GPU busier by admitting new work while existing requests are still decoding.
- **Prefix caching** avoids repeating identical prompt work when shared prompt prefixes show up in real traffic.
- The main serving question is usually not “is the model fast?” but “where is latency really coming from: queueing, prefill, decode, or cache pressure?”

## What to understand

### PagedAttention
The KV cache is one of the main memory bottlenecks in serving. PagedAttention matters because fixed contiguous allocation wastes memory when sequence lengths vary. Block-based allocation makes utilization better and supports higher effective concurrency.

### Continuous batching
Static batches are too brittle for live traffic. Continuous batching improves throughput because the server can keep mixing new prefill work and ongoing decode work instead of waiting for a whole batch to finish.

### Prefix caching
Many production prompts have a stable system prompt, tool instructions, or long shared preamble. Prefix caching matters because repeated prompt prefixes are common in agent and chat systems, and caching them cuts repeated prefill work.

### Operational lesson
You should look at:
- queue time,
- time to first token,
- inter-token latency,
- cache usage,
- and preemption or scheduler pressure

before making hardware or model changes.

## Design defaults

- Treat serving as a scheduler and memory problem first.
- Measure prompt reuse before assuming prefix caching will help.
- Separate short-response and long-generation workloads when traffic shape is very mixed.
- Prefer diagnosing bottlenecks with traces and metrics before changing batch knobs.
- Do not confuse good benchmark throughput with good live tail latency.

## Common mistakes

1. Blaming the model when the scheduler is overloaded.
2. Measuring only end-to-end latency.
3. Adding hardware before understanding cache pressure.
4. Ignoring shared-prefix opportunities in agentic traffic.
5. Treating concurrency increases as free.

## What to read next

- [[Topic - Context Engineering, Prompt Caching, and Long-Context Tradeoffs]]
- [[Topic - Eval Flywheels, Human Review, and Regression Gates]]
- [[Practice - Advanced Systems Architecture Drills]]
