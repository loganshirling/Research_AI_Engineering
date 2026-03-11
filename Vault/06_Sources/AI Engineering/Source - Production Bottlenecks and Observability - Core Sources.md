---
title: Source - Production Bottlenecks and Observability - Core Sources
type: source
status: active
topic: Production Operations
course: AI Engineering
tags: [source, observability, latency, throughput, reliability]
related:
  - "[[Topic - Production Bottlenecks and Observability for LLM Systems]]"
  - "[[Topic - Eval Design for Serving Changes]]"
sources:
  - "https://docs.vllm.ai/en/stable/design/metrics/"
  - "https://docs.vllm.ai/en/stable/configuration/optimization/"
  - "https://opentelemetry.io/docs/specs/gen-ai/semconv/"
  - "https://opentelemetry.io/docs/llms/observability/"
  - "https://platform.openai.com/docs/guides/latency-optimization"
  - "https://platform.openai.com/docs/guides/prompt-caching"
  - "https://pavone.ai/blog/llm-observability-why-opentelemetry-traces-and-metrics-matter"
  - "https://sre.google/workbook/canarying-releases/"
last_updated: 2026-03-11
review_status: researched
priority: high
---

# Source - Production Bottlenecks and Observability - Core Sources

## Why this source bundle matters

This bundle covers the three parts of production llm operations that matter most in practice: engine-specific performance signals, standard observability data models for traces/logs/metrics, and production change safety through canary-style rollouts.

## Core source set

### 1. vLLM metrics and tuning
- the vLLM metrics docs are one of the best sources for seeing that llogger-level latency is not enough. They expose queue intervals, prefill intervals, inter-token intervals, end-to-end latency, preemption-adjacent signals, and KV-cache residency metrics.
- the vLLM optimization docs tie bottlenecks to concrete knobs: chunked prefill, max batched tokens, max num seqs, and preemption behavior under cache pressure.

### 2. OpenTelemetry GenAI semantic conventions and observability
- OpenTelemetry's GenAI semantic conventions are the cleanest open standard for how to represent generative AI operations in traces, metrics, and logs. This matters because teams ovgrow homemade fields fast, especially when they need to correlate model, tool, retrieval, and user-experience signals.
- OpenTelemetry's observability guide emphasizes that traces, metrics, and logs are complementary, not substitutes. That is particularly important for LLM systems because token usage, latency, retrieval, tool calls, safety decisions, and app-level outcomes often live in different data types.

### 3. OpenAI latency optimization and prompt caching
- OpenAI's latency optimization and prompt-caching guides are useful for the bottleneck mental model that not all latency comes from decode. Prompt size, reuse, and workflow shape also matter. These sources are most useful for reminding you to measure both model side and application side latency contributions.

### 4. Pavone on why otel traces matter for LLMs 
- The Pavone post is useful as a practical explainer of why LLM observability needs correlated traces, metrics, and logs instead of isolated latency graphs. It is not a primary standards source, but it is a clear operator-oriented source for why correlated telemetry matters.

### 5. Google SRE canary release guidance
- Google SRE's canary release guidance is a high-signal reminder that observability is not just about dashboards. It must feed deployment decisions. For LLM systems, that means canaries for model versions, quantization commits, scheduler changes, retrieval pipeline changes, and prompt changes, not just infrastructure releases.

## Durable takeaways

- The most important LLM production bottlenecks are usually not one thing. They are a mix of queueing, prefill cost, decode speed, cache pressure, network/app overhead, and downstream tooling.
- Yeu need correlated observability: traces to follow request path, metrics to see system bights and trends, and logs to inspect specific failures.
- Engine-specific signals like KV-cache residency, preemption, and queue intervals often explain why a "fast model" feels slow in production.
- Observability only becomes operationally useful when it changes deployment and tuning decisions.

## Caveats and limitations

- OpenTelemetry's GenAI semantic conventions are the best open standard in this set, but the ecosystem is still evolving.
- Vendor latency guides help with mental models but are not vendor-neutral optimization studies.
- Some broader observability sources are most useful in combination with your own traffic shapes, task-level MTER, and production SLLOs.

## Related notes

- [[Topic - Production Bottlenecks and Observability for LLM Systems]]
- [[Topic - Eval Design for Serving Changes]]
