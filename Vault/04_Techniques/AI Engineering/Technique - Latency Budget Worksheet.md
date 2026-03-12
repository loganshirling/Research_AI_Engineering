---
title: Technique - Latency Budget Worksheet
type: technique
status: active
topic: AI Engineering
course: AI Engineering
tags: [latency, technique, ttft, throughput]
prerequisites:
  - "[[Topic - Latency Engineering for LLM Systems]]"
related:
  - "[[Topic - LLM Observability and Tracing]]"
sources:
  - "https://docs.vllm.ai/en/stable/design/metrics/"
  - "https://developer.nvidia.com/blog/streamlining-ai-inference-performance-and-deployment-with-nvidia-tensorrt-llm-chunked-prefill/"
last_updated: 2026-03-11
review_status: researched
priority: high
---

# Technique - Latency Budget Worksheet

## Purpose

Allocate a target end-to-end latency budget across the major phases of an LLM request.

## Worksheet

Start with a target such as:

- p50 end-to-end latency:
- p95 end-to-end latency:

Then budget each stage:

1. request intake and auth
2. retrieval and preprocessing
3. prompt assembly
4. time to first token
5. streaming duration
6. tool calls or validation
7. response delivery

## How to use it

- Measure actuals for each phase.
- Compare them against the budget.
- Fix the largest violations first.
- Re-run after every serving or prompt change.

## High-value questions

- Is the system prompt too large?
- Is retrieval sending too much context?
- Is queue time rising under concurrency?
- Is first token slow because prefill is too heavy?
- Is stream speed slow because decode or cache pressure is the bottleneck?
