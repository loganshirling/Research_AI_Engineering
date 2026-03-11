---
title: Topic - Eval Design for Serving Changes
type: topic
status: active
topic: Evaluation and Production Operations
course: AI Engineering
tags: [evals, serving, observability, benchmarking, canarying]
prerequisites:
  - "[[Topic - LLM Inference Path, KV Cache, and Batching]]"
  - "[[Topic - Quantization Tradeoffs for LLM Inference]]"
related:
  - "[[Map - AI Engineering]]"
  - "[[Source - Eval Design for Serving Changes - Core Sources]]"
  - "[[Source - vLLM - Features, Architecture, Metrics, and Tuning]]"
sources:
  - "https://docs.vllm.ai/en/stable/design/metrics/"
  - "https://docs.vllm.ai/en/stable/configuration/optimization/"
  - "https://docs.nvidia.com/nim/benchmarking/llm/latest/overview.html"
  - "https://docs.nvidia.com/nim/benchmarking/llm/latest/metrics.html"
  - "https://docs.nvidia.com/nim/benchmarking/llm/latest/parameters.html"
  - "https://mlcommons.org/2024/03/mlperf-llama2-70b/"
  - "https://sre.google/workbook/canarying-releases/"
  - "https://sre.google/static/pdf/canary_analysis.pdf"
last_updated: 2026-03-11
review_status: researched
priority: high
core_status: core
practical_value: very_high
explanatory_power: high
difficulty: medium
must_know: true
---

# Topic - Eval Design for Serving Changes

## Purpose

Build a durable way to evaluate serving changes such as quantization, scheduler tuning, batching policy changes, kernel swaps, LoRA support, and model-server upgrades without confusing "faster benchmark number" with "better production system."[_.vllm-metrics][^-nvidia-overview][^-google-canary]

## Why it matters

Serving changes can improve one layer of the system while hurting another. A quantized model may improve throughput but hurt task quality. A batching change may improve tokens/sec but worsen queueing and TTFT. A release may look good in staging but fail under real production traffic. Good eval design prevents those mistakes by separating **quality**, **serving-path performance**, and **production rollout risk**.[^-nvidia-overview][^-vllm-opt][^-google-canary][^-cas]

## 80/20 summary

- A good serving eval has **three layers**:
  1. **offline quality checks** on representative tasks,
  2. **serving metrics** under realistic prompt/output and concurrency distributions,
  3. **production canaries** that watch for regressions on live traffic.[^-nvidia-overview][^-vllm-metrics][^-google-canary]
- Never accept a serving change based on one number like average throughput. For interactive systems, TTFT, inter-token latency, end-to-end latency, queueing, and request completion behavior all matter.[^-vllm-metrics][^-nvidia-metrics]
- Benchmark shape matters. Synthetic single-stream runs and unconstrained max-throughput runs answer different questions than server-style tests with random arrivals and latency constraints.[^-mlperf-server][^-nvidia-params]
- When a change is close on quality but clearly better on cost or performance, the next step is usually a canary, not an immediate full rollout.[^-google-canary][^-cas]

## The three-layer eval model

### 1. Offline quality eval

This layer answers: **did the model or system still do the job?**

Typical checks:
- exact-match or rubric checks on key tasks,
- structured-output validity,
- refusal / safety behavior where relevant,
- regression set comparisons against the current baseline.

This matters because NVIDIA's benchmarking guide is explicit that cost and performance measurement should be interpreted relative to an acceptable accuracy target for the use case.[^-nvidia-overview]

### 2. Serving-path eval

This layer answers: **how does the system behave while serving requests?**

vLLM's metrics docs are a strong mental model here because they break request handling into queue intervals, prefill intervals, inter-token intervals, TTFT, inference/decode intervals, end-to-end latency, and even KV-cache residency metrics.[^-vllm-metrics]

NVIDIA's benchmarking guide adds the deployment-side lens: TTFT, end-to-end latency, inter-token latency, tokens/sec, and requests/sec each capture different aspects of the user and operator experience.[^-nvidia-metrics]

### 3. Production rollout eval

This layer answers: **does the change survive contact with real traffic?**

Google's SRE guidance is still the right default: expose only a slice of production traffic first so the deployment pipeline can detect defects quickly with limited blast radius.[^-google-canary] Google's Canary Analysis Service (CAS) is a stronger version of the same idea: automatic analysis of key metrics during production changes at scale.[^-cas]

## What to measure first

### User-facing metrics

- **TTFT**: how long the user waits to see the first token.[^-vllm-metrics][^-nvidia-metrics]
- **Inter-token latency / TPOT / ITL**: how smooth streaming feels after generation starts.[^-vllm-metrics][^-nvidia-metrics]
- **End-to-end latency**: full request time, including queueing, batching behavior, and network effects.[^-vllm-metrics][^-nvidia-metrics]

### Operator-facing metrics

- **Prompt tokens/sec** and **generated tokens/sec** for throughput shape.[^-vllm-metrics]
- **Requests/sec** when request completion rate matters more than raw token rate.[^-nvidia-metrics]
- **Queue interval** to catch overload or batching-induced waiting.[^-vllm-metrics]
- **KV-cache residency / reuse / preemption signals** to catch memory-pressure pathologies in serving engines like vLLM.[^-vllm-metrics][^-vllm-opt]

### Quality guardrails

- golden task set pass rate,
- structured-output validity,
- critical failure counts,
- and any application-specific safety or business rules.

## Benchmark design rules

### Use realistic workload shapes

NVIDIA's benchmarking docs separate parameters like concurrency, request rate, input length, and output length because changing those parameters changes what the benchmark actually measures.[^-nvidia-params] The right eval uses distributions that look like production, not just one prompt length and one output length.

### Separate latency studies from throughput studies

Min-latency runs at concurrency 1 answer a different question than saturation runs at high concurrency. NVIDIA's own benchmark pages separate minimum-latency and maximum-throughput views for exactly that reason.[^-nvidia-performance]

### Include server-style tests when interactivity matters

ML@erf's server scenario is a useful benchmark-design reference because it simulates online applications with random arrivals and latency constraints, instead of assuming a simple offline batch loop.[^-mlperf-server]

### Compare against a stable baseline

Every serving eval should compare:
- old stack vs new stack,
- same prompt/output mix,
- same hardware,
- same concurrency or request-rate sweep,
- same timeout and failure handling,
- same quality test set.

Without that discipline, results are usually not decision-grade.

## Canary design rules

- Start with a narrow traffic slice.[^-google-canary]
- Watch both quality and serving metrics, not just infrastructure health.[^-cas]
- Define rollback triggers before the release.
- Prefer automatic regression checks when possible.
- Treat configuration, routing, adapter, and data changes as canary candidates too, not just binary releases.[^-cas]

## Common mistakes

1. Optimizing one metric in isolation, especially raw tokens/sec.[^-nvidia-metrics][^-vllm-metrics]
2. Running only synthetic offline tests and calling it production readiness.[^-mlperf-server][^-google-canary]
3. Ignoring queueing and preemption signals when changing batching, quantization, or context lengths.[^-vllm-metrics][^-vllm-opt]
4. Mixing benchmark setups across model versions or hardware and then reading the result as causal.5. Shipping a serving change globally before exposing it to a canary slice.[^-google-canary][^-cas]

## Minimal eval template for a serving change

1. **Define the change**
  - example: FP8 instead of BF16, or new chunked-prefill setting

2. **Define the win condition**
   - example: reduce cost/request by 20% while keeping quality flat and p95 TTFT within budget

3. **Run offline quality checks**
  - task pass rate
  - structured-output validity
   - critical failure taxonomy

4. **Run serving-path benchmarks**
  - concurrency sweep
  - realistic input/output distributions
  - TTFT / ITL / e2e / queue interval / throughput
  - failure and timeout rates

5. **Canary**
  - small traffic slice
  - metric comparison against control
   - rollback thresholds

6. **Decide**
  - ship, tune further, or revert

## What to read next

- [[Topic - Quantization Tradeoffs for LLM Inference]]
- [[Topic - LLM Inference Path, KV Cache, and Batching]]
- [[Source - Eval Design for Serving Changes - Core Sources]]

[^-vllm-metrics]: vLLM docs, "Metrics." Last verified 2026-03-11. https://docs.vllm.ai/en/stable/design/metrics/
[^-vllm-opt]: vLLM docs, "Optimization and Tuning." Last verified 2026-03-11. https://docs.vllm.ai/en/stable/configuration/optimization/
[^-nvidia-overview]: NVIDIA NIM LLM Benchmarking, "Overview." Last updated Oct 28, 2025. Last verified 2026-03-11. https://docs.nvidia.com/nim/benchmarking/llm/latest/overview.html
[^-nvidia-metrics]: NVIDIA NIM LLM Benchmarking, "Metrics." Last updated Oct 28, 2025. Last verified 2026-03-11. https://docs.nvidia.com/nim/benchmarking/llm/latest/metrics.html
[^-nvidia-params]: NVIDIA NIM LLM Benchmarking, "Parameters and Best Practices." Last updated Oct 28, 2025. Last verified 2026-03-11. https://docs.nvidia.com/nim/benchmarking/llm/latest/parameters.html
[^-nvidia-performance]: NVIDIA NIM LLM Benchmarking, "Performance." Last verified 2026-03-11. https://docs.nvidia.com/nim/benchmarking/llm/latest/performance.html
[^-mlperf-server]: MLCommons, "Llama 2 70B: An MLPerf Inference Benchmark for Large Language Models." Mar. 27, 2024. Last verified 2026-03-11. https://mlcommons.org/2024/03/mlperf-llama2-70b/
[^-google-canary]: Google SRE Workbook, "Canary Release: Deployment Safety and Efficiency." Last verified 2026-03-11. https://sre.google/workbook/canarying-releases/
[^-cas]: ACM Queue / Google, "Canary Analysis Service." Last verified 2026-03-11. https://sre.google/static/pdf/canary_analysis.pdf
