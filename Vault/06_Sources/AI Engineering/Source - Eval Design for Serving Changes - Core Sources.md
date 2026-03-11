---
title: Source - Eval Design for Serving Changes - Core Sources
type: source
status: active
topic: Evaluation and Production Operations
course: AI Engineering
tags: [source, evals, serving, observability, canarying]
related:
  - "[[Topic - Eval Design for Serving Changes]]"
  - "[[Topic - Quantization Tradeoffs for LLM Inference]]"
  - "[[Topic - LLM Inference Path, KV Cache, and Batching]]"
sources:
  - "https://docs.vllm.ai/en/stable/design/metrics/"
  - "https://docs.vllm.ai/en/stable/configuration/optimization/"
  - "https://docs.nvidia.com/nim/benchmarking/llm/latest/overview.html"
  - "https://docs.nvidia.com/nim/benchmarking/llm/latest/metrics.html"
  - "https://docs.nvidia.com/nim/benchmarking/llm/latest/parameters.html"
  - "https://docs.nvidia.com/nim/benchmarking/llm/latest/performance.html"
  - "https://mlcommons.org/2024/03/mlperf-llama2-70b/"
  - "https://sre.google/workbook/canarying-releases/"
  - "https://sre.google/static/pdf/canary_analysis.pdf"
last_updated: 2026-03-11
review_status: researched
priority: high
---

# Source - Eval Design for Serving Changes - Core Sources

## Why this source bundle matters

This bundle covers the three parts of serving eval that matter most in practice:
1. request-path metrics from the serving engine, 
2. benchmark-design guidance for latency and throughput, 
3. production rollout safety through canaries.

## Core source set

### 1. vLLM request-path metrics and tuning
- vLLM's metrics docs are unusually helpful because they expose queue intervals, prefill intervals, inter-token intervals, TTFT, inference/decode intervals, end-to-end latency, and KV-cache residency metrics.[^-vllm-metrics]
- vLLM's optimization docs connect those metrics back to tuning knobs such as chunked prefill, `max_num_batched_tokens`, and preemption behavior under KV-cache pressure.[^-vllm-opt]

### 2. NVIDIA benchmarking guidance
- NVIDIA's benchmarking overview distinguishes performance benchmarking from generic load testing and says cost/performance measurements should be interpreted against an acceptable accuracy target for the application.[^-nvidia-overview]
- The metrics page defines TTFT, end-to-end latency, ITL, TPS, and RPS as separate metrics because each answers a different operational question.[^-nvidia-metrics]
- The parameters page makes concurrency, request rate, input length, and output length explicit benchmark variables, which is useful because changing those variables changes the benchmark's meaning.[^-nvidia-params]
- NVIDIA's performance pages separate minimum-latency and maximum-throughput views, which is a good reminder not to collapse all serving behavior into one summary number.[^-nvidia-performance]

### 3. MLPerf server-style benchmarking
- MLPerf's server scenario is a useful design reference for interactive systems because it uses random arrivals and latency constraints instead of a simple offline throughput loop.[^-mlperf-server]

### 4. Google SRE canary guidance
- Google SRE's canary chapter is the durable argument for partial rollout: real traffic reveals defects that staging and offline tests miss, so expose only a small slice first.[^-google-canary]
- Google's Canary Analysis Service (CAS) paper shows the more mature version of that idea: automatic analysis of key metrics during production changes, with strict separation between change and analysis logic.[^-cas]

## Durable takeaways

- Good serving eval is layered: quality checks, serving-path metrics, and rollout safety all need explicit treatment.[^-nvidia-overview][^-google-canary]
- Queueing and preemption are part of the user experience, not just internal implementation detail.[^-vllm-metrics][^-vllm-opt]
- Benchmark results are only meaningful when workload shape, hardware, and metric definitions are held constant.[^-nvidia-params][^-nvidia-performance]
- Canarying is not optional for changes that affect user-facing latency, quality, or reliability under real traffic.[^-google-canary][^-cas]

## Caveats and limitations

- vLLM metrics are engine-specific; they are excellent for understanding vLLM but not a universal ontology for every server.[^-vllm-metrics]
- NVIDIA benchmarking docs are high-quality deployment guidance but still reflect NVIDIA's tooling and ecosystem choices.[^-nvidia-overview]
- MLPerf is a benchmark-design reference, not a full substitute for product-specific traffic replay.[^-mlperf-server]
- Canary analysis reduces rollout risk but still depends on choosing the right metrics and thresholds.[^-cas]

## Related notes

- [[Topic - Eval Design for Serving Changes]]
- [[Topic - Quantization Tradeoffs for LLM Inference]]
- [[Topic - LLM Inference Path, KV Cache, and Batching]]

[^-vllm-metrics]: vLLM docs, "Metrics." Last verified 2026-03-11. https://docs.vllm.ai/en/stable/design/metrics/
[^-vllm-opt]: vLLM docs, "Optimization and Tuning." Last verified 2026-03-11. https://docs.vllm.ai/en/stable/configuration/optimization/
[^-nvidia-overview]: NVIDIA NIM LLM Benchmarking, "Overview." Last updated Oct 28, 2025. Last verified 2026-03-11. https://docs.nvidia.com/nim/benchmarking/llm/latest/overview.html
[^-nvidia-metrics]: NVIDIA NIM LLM Benchmarking, "Metrics." Last updated Oct 28, 2025. Last verified 2026-03-11. https://docs.nvidia.com/nim/benchmarking/llm/latest/metrics.html
[^-nvidia-params]: NVIDIA NIM LLM Benchmarking, "Parameters and Best Practices." Last updated Oct 28, 2025. Last verified 2026-03-11. https://docs.nvidia.com/nim/benchmarking/llm/latest/parameters.html
[^-nvidia-performance]: NVIDIA NIM LLM Benchmarking, "Performance." Last verified 2026-03-11. https://docs.nvidia.com/nim/benchmarking/llm/latest/performance.html
[^-mlperf-server]: MLCommons, "Llama 2 70B: An MLPerf Inference Benchmark for Large Language Models." Mar. 27, 2024. Last verified 2026-03-11. https://mlcommons.org/2024/03/mlperf-llama2-70b/
[^-google-canary]: Google SRE Workbook, "Canary Release: Deployment Safety and Efficiency." Last verified 2026-03-11. https://sre.google/workbook/canarying-releases/
[^-cas]: ACM Queue / Google, "Canary Analysis Service." Last verified 2026-03-11. https://sre.google/static/pdf/canary_analysis.pdf
