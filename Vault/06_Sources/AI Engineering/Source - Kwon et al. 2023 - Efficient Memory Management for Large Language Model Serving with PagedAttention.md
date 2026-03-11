---
title: Source - Kwon et al. 2023 - Efficient Memory Management for Large Language Model Serving with PagedAttention
type: source
status: active
topic: LLM Inference and Serving
course: AI Engineering
tags: [source, paper, vllm, pagedattention, kv-cache]
prerequisites: []
related:
  - "[[Topic - LLM Inference Path, KV Cache, and Batching]]"
  - "[[Source - vLLM - Features, Architecture, Metrics, and Tuning]]"
sources:
  - "https://arxiv.org/abs/2309.06180"
last_updated: 2026-03-11
review_status: researched
priority: high
---

# Source - Kwon et al. 2023 - Efficient Memory Management for Large Language Model Serving with PagedAttention

## Source details

- **Authors:** Woosuk Kwon, Zhuohan Li, Siyuan Zhuang, Ying Sheng, Lianmin Zheng, Cody Hao Yu, Joseph E. Gonzalez, Hao Zhang, Ion Stoica
- **Venue/date:** SOSP 2023; arXiv submission Sep. 12, 2023
- **Link:** https://arxiv.org/abs/2309.06180
- **Source type:** primary research paper

## Why this source matters

This is the primary source for the design argument behind **PagedAttention** and the original vLLM system. It is the best source in this topic cluster for understanding why KV cache management, not just raw model math, becomes a first-order systems problem in LLM serving.

## Key claims

- High-throughput LLM serving needs large enough batches, but serving systems struggle because per-request KV cache memory is large and changes dynamically over time.
- Poor KV-cache management wastes memory through fragmentation and redundant duplication, which directly limits feasible batch size.
- PagedAttention treats KV cache management more like paged virtual memory so the system can avoid large contiguous allocations and reduce waste.
- The paper claims two central outcomes:
  1. near-zero waste in KV-cache memory, and
  2. 2-4x higher throughput at comparable latency versus the state-of-the-art systems used in the paper's evaluation.

## Useful examples

The best reusable mental model from this paper is the **virtual memory analogy**. The point is not that GPU memory literally becomes OS virtual memory. The point is that fixed-size blocks and indirection help the serving engine manage fast-changing request lengths without paying the usual fragmentation penalty.

That mental model is durable because it explains:
- why long-context serving is hard,
- why concurrency is constrained by cache pressure,
- and why scheduler design and memory layout are tightly linked.

## Caveats and limitations

- This is a 2023 paper, so its comparative baselines are historically important but not the same as comparing against every modern inference engine in 2026.
- The reported throughput gains are real for the evaluation setup in the paper, but they should not be read as guaranteed production gains for every workload.
- The paper explains the original vLLM design, not every later feature or scheduler policy found in current vLLM releases.

## What to verify later

- How modern TensorRT-LLM, TGI, SGLang, and newer vLLM releases compare on the same traffic mix.
- How later systems handle newer hybrid attention architectures and more advanced scheduling features.
- Which workload shapes still show the strongest benefit from the original PagedAttention design versus newer serving optimizations.

## Related notes

- [[Topic - LLM Inference Path, KV Cache, and Batching]]
- [[Source - vLLM - Features, Architecture, Metrics, and Tuning]]
